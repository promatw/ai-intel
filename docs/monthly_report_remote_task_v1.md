# AI / CoWoS 情報｜月報 Remote Schedule Task v1

## 版本記錄

| 版本 | 日期 | 主要變更 |
|---|---|---|
| v1 | 2026-05-02 | 由月報 v4 改寫為 AI / CoWoS 情報版；掃描 `ai-weekly` 來源；slug 改為 `YYYY-MM-monthly`；分類改為 AI infrastructure、advanced packaging、semiconductor supply chain、model labs、market signal。 |

## 與原月報 v4 功能對照

| v4 功能 | AI / CoWoS v1 對應 |
|---|---|
| 讀取 `monthly_state.json` 防重複 | 沿用 |
| 讀取 `published_urls.json` 排除已發布 URL | 沿用 |
| 掃描上個月週報來源 | 改為掃描 `ai-weekly` |
| Claude 萃取條目並去重 | 沿用，改為產業情報脈絡 |
| 月報先 commit | 沿用 |
| 來源 draft + state + urls 合併第二次 commit | 沿用 |
| 固定 2 次 commit | 沿用 |

## 任務說明

- 執行頻率：每月 1 號 08:00（UTC+8）
- 任務目標：彙整上個月 AI / CoWoS 週報，去重後整理為月報。
- GitHub Repo：`promatw/ai-intel`
- 發布路徑：`content/posts/YYYY-MM-monthly.md`

## 任務程式碼骨架

```python
import os
import urllib.request
import urllib.error
import json
import base64
import re
from datetime import datetime, timedelta
from calendar import monthrange

GH_TOKEN = os.environ.get("GH_TOKEN", "")
GH_REPO = "promatw/ai-intel"
GH_BRANCH = "main"
if not GH_TOKEN:
    raise SystemExit("GH_TOKEN is required in the execution environment")

GH_API = "https://api.github.com"

def gh_request(method, path, data=None):
    url = f"{GH_API}{path}"
    headers = {
        "Authorization": f"Bearer {GH_TOKEN}",
        "Accept": "application/vnd.github+json",
        "X-GitHub-Api-Version": "2022-11-28",
        "User-Agent": "promatw-ai-intel-monthly/1.0",
    }
    body = json.dumps(data).encode() if data else None
    if body:
        headers["Content-Type"] = "application/json"
    req = urllib.request.Request(url, data=body, headers=headers, method=method)
    try:
        with urllib.request.urlopen(req, timeout=30) as r:
            return json.loads(r.read().decode()) if r.status != 204 else {}
    except urllib.error.HTTPError as e:
        print(f"[HTTP {e.code}] {method} {url} -> {e.read().decode()[:300]}")
        return None
    except Exception as e:
        print(f"[ERROR] {method} {url} -> {e}")
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

def gh_list_dir(path):
    res = gh_request("GET", f"/repos/{GH_REPO}/contents/{path}?ref={GH_BRANCH}")
    return res if isinstance(res, list) else []

def gh_multi_commit(file_updates, message):
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

today = datetime.now()
first_day = today.replace(day=1)
last_month = first_day - timedelta(days=1)
year = last_month.year
month = last_month.month
_, last_day_num = monthrange(year, month)
this_month_key = f"{year}-{month:02d}"

monthly_state_path = "data/monthly_state.json"
state_str, state_sha = gh_get_file(monthly_state_path)
monthly_state = json.loads(state_str) if state_str else {}
if monthly_state.get("last_published_month") == this_month_key:
    raise SystemExit(f"{this_month_key} already published")

pub_urls_path = "data/published_urls.json"
pub_str, pub_sha = gh_get_file(pub_urls_path)
pub_db = json.loads(pub_str) if pub_str else {"urls": []}
published_urls = set(pub_db.get("urls", []))

all_files = gh_list_dir("content/posts")
target_prefix = f"{year}-{month:02d}-"
source_files = sorted([
    f for f in all_files
    if f["name"].startswith(target_prefix)
    and "ai-weekly" in f["name"]
    and f["name"].endswith(".md")
], key=lambda f: f["name"])
if not source_files:
    raise SystemExit("No ai-weekly source files found")

source_contents = []
for f in source_files:
    content, sha = gh_get_file(f["path"])
    if content:
        source_contents.append({"name": f["name"], "path": f["path"], "sha": sha, "content": content})

print("請閱讀下列來源，萃取不重複產業情報條目。")
print("第一層：標題或主題相似者只保留資訊量最高的一則。")
print("第二層：排除 published_urls 既有 URL。")
print("每則輸出 BLOCK_START / BLOCK_END，包含：標題、分類、來源、摘要、閱讀原文 URL、是否適合 NotebookLM。")
for url in sorted(published_urls):
    print(f"PUBLISHED: {url}")
for sc in source_contents:
    print(f"--- {sc['name']} ---")
    print(sc["content"])
    print(f"--- END {sc['name']} ---")

# Claude/Codex 在此填入 unique_blocks 後，繼續執行以下發布段落。

SECTIONS = [
    ("AI 基礎設施", ["GPU", "TPU", "datacenter", "data center", "networking", "power", "cooling", "infrastructure", "算力"]),
    ("CoWoS / 先進封裝", ["CoWoS", "advanced packaging", "HBM", "SoIC", "InFO", "封裝", "封測"]),
    ("半導體供應鏈", ["TSMC", "OSAT", "supply chain", "capacity", "capex", "設備", "材料", "供應鏈"]),
    ("模型實驗室", ["OpenAI", "Anthropic", "Gemini", "DeepMind", "Meta", "Claude", "model", "模型"]),
    ("市場訊號", ["revenue", "forecast", "price", "demand", "market", "財報", "價格", "需求"]),
]

def classify(block):
    b = block.lower()
    for section, keywords in SECTIONS:
        if any(k.lower() in b for k in keywords):
            return section
    return "其他觀察"

def block_to_html(block):
    lines = [line.strip() for line in block.splitlines() if line.strip()]
    title = next((line.replace("🔹", "").strip() for line in lines if line.startswith("🔹")), "未命名條目")
    source = next((line for line in lines if line.startswith("來源")), "")
    link = next((line for line in lines if "http" in line), "")
    url_match = re.search(r"https?://[^\s)）]+", link)
    url = url_match.group(0).rstrip("。，、；：") if url_match else ""
    body = "<br>".join(line for line in lines if not line.startswith("🔹") and not line.startswith("來源") and "http" not in line)
    return f"<h3>{title}</h3>\n<p>{source}</p>\n<p>{body}</p>\n<p><a href=\"{url}\" target=\"_blank\" rel=\"noopener\">閱讀原文</a></p>"

section_map = {s[0]: [] for s in SECTIONS}
section_map["其他觀察"] = []
for block in unique_blocks:
    section_map[classify(block)].append(block)

slug = f"{year}-{month:02d}-monthly"
post_title = f"{year}年{month}月 AI / CoWoS 情報月報"
html = f"<p><strong>本月報範圍：</strong>{year}年{month}月1日至{year}年{month}月{last_day_num}日，彙整 {len(source_contents)} 篇週報，去重後保留 {len(unique_blocks)} 則情報。</p>\n"
for section in [s[0] for s in SECTIONS] + ["其他觀察"]:
    blocks = section_map.get(section, [])
    if not blocks:
        continue
    html += f"\n<h2>{section}</h2>\n"
    for block in blocks:
        html += block_to_html(block) + "\n<hr>\n"

md_body = (
    "+++\n"
    f"title = {json.dumps(post_title, ensure_ascii=False)}\n"
    f"date = {json.dumps(today.isoformat(), ensure_ascii=False)}\n"
    f"slug = {json.dumps(slug, ensure_ascii=False)}\n"
    "categories = [\"market-signal\"]\n"
    "draft = false\n"
    "+++\n\n"
    f"{html}"
)

monthly_ok = gh_multi_commit([{"path": f"content/posts/{slug}.md", "content": md_body}], f"feat(post): {post_title}")
if not monthly_ok:
    raise SystemExit("monthly commit failed")

post_url = f"https://promatw.github.io/ai-intel/posts/{slug}/"
file_updates = []
for sc in source_contents:
    content = sc["content"].replace("draft = false", "draft = true")
    file_updates.append({"path": sc["path"], "content": content})
monthly_state["last_published_month"] = this_month_key
monthly_state["published_at"] = today.isoformat()
monthly_state["post_url"] = post_url
file_updates.append({"path": monthly_state_path, "content": json.dumps(monthly_state, ensure_ascii=False, indent=2)})

new_urls = []
for block in unique_blocks:
    for match in re.findall(r"https?://[^\s)）]+", block):
        new_urls.append(match.rstrip("。，、；："))
pub_db["urls"] = list(dict.fromkeys(pub_db.get("urls", []) + new_urls))
pub_db["last_updated"] = today.strftime("%Y-%m-%d")
file_updates.append({"path": pub_urls_path, "content": json.dumps(pub_db, ensure_ascii=False, indent=2)})

batch_ok = gh_multi_commit(file_updates, f"chore: archive ai weekly sources and state {this_month_key}")
print("月報完成")
print(f"文章網址：{post_url}")
print(f"第二次 commit：{'成功' if batch_ok else '失敗'}")
print("總 commit 數：2")
```
