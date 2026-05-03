---
name: autopost-github
description: 自動發布 Remote Schedule Task 的操作規則。當用戶要求產生、改寫、或除錯任何自動發布類 Remote Task（週報、月報、或其他定期發布任務）時，必須先讀取此 Skill，再開始任何產出。觸發關鍵詞：「產生週報」「產生月報」「寫一個新的自動發布任務」「把日報改成週報」「新增一個 Remote Task」「改寫 Remote Task」。
---

本 Skill 記錄「多發性骨髓瘤國際資訊摘要」自動發布系統（以及任何同架構衍生系統）的核心操作規則。所有規則來自實際踩坑經驗，必須嚴格遵守，不得自行發明新做法。

---

## 系統現況（2026-05）

- 發布平台：GitHub Pages（promatw.github.io）
- 狀態儲存：GitHub repo 的 `data/` 目錄 JSON 檔
- 週報 v7：每週一 08:00，抓取 5 個網站，commit 到 `content/posts/YYYY-MM-DD-mm-weekly.md`
- 月報 v4：每月 1 號 08:00，彙整上個月週報，commit 到 `content/posts/YYYY-MM-monthly.md`
- 狀態檔案：`data/state.json`（週報輪替）、`data/monthly_state.json`（月報防重複）、`data/published_urls.json`（跨報告去重）

---

## 規則一：新任務產生前必須比對現有任務功能點

在產生任何新的 Remote Task 之前，必須：

1. 列出週報 v7 的所有功能點
2. 逐項確認新任務是否有對應實作（✅ 沿用 / ⚠️ 調整 / ❌ 不適用）
3. 等用戶確認對照表無遺漏，才開始產生程式碼

**這條規則的存在原因**：週報 v1-v3 因為沒有對照現有任務，產生了在 Python 程式碼中呼叫 Anthropic API 做翻譯（錯誤）、把 WordPress 讀寫程式碼誤刪（錯誤）等問題。v4 才回到正確架構。

---

## 規則二：GitHub API 標準範本（已驗證，直接複製）

每次撰寫新 Remote Task，**直接複製以下函數，不得改寫**：

```python
GH_API = "https://api.github.com"

def gh_request(method, path, data=None):
    url = f"{GH_API}{path}"
    headers = {
        "Authorization": f"Bearer {GH_TOKEN}",
        "Accept": "application/vnd.github+json",
        "X-GitHub-Api-Version": "2022-11-28",
        "User-Agent": "promatw-task/1.0",
    }
    body = json.dumps(data).encode() if data else None
    if body:
        headers["Content-Type"] = "application/json"
    req = urllib.request.Request(url, data=body, headers=headers, method=method)
    try:
        with urllib.request.urlopen(req, timeout=30) as r:
            return json.loads(r.read().decode()) if r.status != 204 else {}
    except urllib.error.HTTPError as e:
        print(f"[HTTP {e.code}] {method} {url} → {e.read().decode()[:300]}")
        return None
    except Exception as e:
        print(f"[ERROR] {method} {url} → {e}")
        return None

def gh_get_file(path):
    res = gh_request("GET", f"/repos/{GH_REPO}/contents/{path}?ref={GH_BRANCH}")
    if res is None:
        return "", None
    return base64.b64decode(res["content"]).decode("utf-8"), res["sha"]

def gh_put_file(path, content_str, sha=None, message=""):
    body = {
        "message": message or f"chore: update {path}",
        "content": base64.b64encode(content_str.encode("utf-8")).decode("ascii"),
        "branch": GH_BRANCH,
    }
    if sha:
        body["sha"] = sha
    return gh_request("PUT", f"/repos/{GH_REPO}/contents/{path}", data=body)

def gh_multi_commit(file_updates, message):
    """多檔合併一次 commit，月報專用"""
    ref_res = gh_request("GET", f"/repos/{GH_REPO}/git/refs/heads/{GH_BRANCH}")
    if not ref_res:
        return False
    head_sha = ref_res["object"]["sha"]
    commit_res = gh_request("GET", f"/repos/{GH_REPO}/git/commits/{head_sha}")
    if not commit_res:
        return False
    tree_items = [{"path": f["path"], "mode": "100644", "type": "blob", "content": f["content"]} for f in file_updates]
    tree_res = gh_request("POST", f"/repos/{GH_REPO}/git/trees", data={"base_tree": commit_res["tree"]["sha"], "tree": tree_items})
    if not tree_res:
        return False
    new_commit = gh_request("POST", f"/repos/{GH_REPO}/git/commits", data={"message": message, "tree": tree_res["sha"], "parents": [head_sha]})
    if not new_commit:
        return False
    return gh_request("PATCH", f"/repos/{GH_REPO}/git/refs/heads/{GH_BRANCH}", data={"sha": new_commit["sha"]}) is not None
```

---

## 規則三：翻譯摘要由 Claude 直接執行，禁止呼叫 Anthropic API

Remote Task 中，翻譯與摘要必須由 Claude 在 Prompt 中直接執行。

**禁止**在 Python 程式碼中出現：
```python
# ❌ 絕對禁止
requests.post("https://api.anthropic.com/v1/messages", ...)
```

**正確做法**：在 Prompt 中直接指示 Claude 抓取並翻譯，Claude 執行時本身就具備這個能力。

---

## 規則四：步驟六禁止重新讀取狀態

步驟六直接沿用步驟一載入的變數，絕對不可重新呼叫任何讀取函數。

```python
# ❌ 禁止
state = json.loads(gh_get_file("data/state.json")[0])

# ✅ 正確：沿用步驟一已載入的 state
state["rotation_index"] = (rotation_index + 5) % 20
```

**這條規則的存在原因**：重新讀取有回傳空值或失敗的風險，會導致 state fallback 回預設值，輪替索引永遠停在 0。

---

## 規則五：月報固定 2 次 commit，禁止逐篇個別 commit

**這條規則的存在原因**：月報 v2 對 9 篇來源逐一呼叫 `gh_put_file`，觸發 9 次 GitHub Actions build，Actions 互相排擠，月報本身的 build 被推擠導致頁面 404。

**強制規定**：
- 第 1 次 commit：月報本身（單獨，用 `gh_put_file`）
- 第 2 次 commit：所有來源改 draft + state 更新 + published_urls 更新，合併為一次 `gh_multi_commit`
- 禁止對每篇來源個別呼叫 `gh_put_file`

---

## 規則六：跨報告去重機制（published_urls.json）

`data/published_urls.json` 是全系統共用的已發布 URL 資料庫。

**週報**：
- 步驟一讀取，與 seen_articles 合併為 `all_seen_urls`
- 重複時執行換文章迴圈（最多 3 輪：主欄位 → 其他分類 → WebSearch）
- 步驟六將新 URL 追加寫回

**月報**：
- 步驟一讀取
- 步驟四指示 Claude 排除已在資料庫中的 URL
- 步驟六將月報新 URL 與 draft 改動合併為同一次 `gh_multi_commit`

若 `published_urls.json` 不存在，自動略過跨報告去重，不報錯。

---

## 規則七：GitHub 檔案上傳禁止使用網頁編輯器貼入 HTML

**這條規則的存在原因**：GitHub 網頁編輯器在複製貼上時會過濾 `<a href="...">` 等 HTML 標籤，導致閱讀原文連結失效，且不會報錯，難以察覺。

**強制規定**：包含 inline HTML 的 Markdown 檔案，必須用 `Add file → Upload files` 上傳，或透過程式化 `gh_put_file` 寫入，禁止用網頁鉛筆編輯器手動貼入。

---

## 規則八：佔位符禁用

禁止在程式碼區塊內出現需要手動替換的佔位符（如 `GH_TOKEN_HERE`、`TODAY_DATE`、`SLUG_HERE`）。所有動態值必須在程式碼內自動取得或從設定區讀入。

---

## 規則九：成功判斷標準

```python
# 單檔 commit 成功
ok = result is not None and "content" in result

# 多檔 commit 成功
ok = gh_multi_commit(file_updates, message)  # 回傳 True/False
```

執行報告必須明確輸出每個關鍵步驟的成功或失敗，不得只輸出「任務完成」。

---

## 新 Task 撰寫完成後的檢查清單

| 檢查項目 | 確認 |
|---------|------|
| gh_request / gh_get_file / gh_put_file 與標準範本完全一致 | ☐ |
| 月報使用 gh_multi_commit，固定 2 次 commit | ☐ |
| 翻譯摘要由 Prompt 直接指示 Claude，無 Anthropic API 呼叫 | ☐ |
| 步驟六直接沿用步驟一的 state，未重新讀取 | ☐ |
| 步驟六將新 URL 追加寫回 published_urls.json | ☐ |
| 程式碼中無任何佔位符 | ☐ |
| 第一次執行前先手動 Run Now 確認，再排程 | ☐ |

