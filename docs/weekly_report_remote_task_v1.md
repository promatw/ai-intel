# AI / CoWoS 情報｜週報 Remote Schedule Task v1

## 版本記錄

| 版本 | 日期 | 主要變更 |
|---|---|---|
| v1 | 2026-05-02 | 由週報 v7 改寫為 AI / CoWoS 情報版；repo 改為 `promatw/ai-intel`；slug 改為 `YYYY-MM-DD-ai-weekly`；來源改為 AI infrastructure、CoWoS、先進封裝與半導體供應鏈；保留 state 與 published_urls 去重機制。 |

## 與原週報 v7 功能對照

| v7 功能 | AI / CoWoS v1 對應 |
|---|---|
| GitHub API commit | 沿用，repo 改為 `promatw/ai-intel` |
| `data/state.json` 輪替 | 沿用 |
| `data/published_urls.json` 跨報告去重 | 沿用 |
| 每次輪替抓 5 個網站 | 調整為 12 個來源中輪替抓 3-5 篇 |
| 每網站只收錄 1 篇 | 沿用 |
| WebFetch 優先、WebSearch 備援 | 沿用 |
| Claude 直接翻譯摘要 | 沿用，不呼叫 Anthropic API |
| `mm-weekly` slug | 改為 `ai-weekly` |
| 醫療免責說明 | 改為產業情報說明 |

## 任務說明

- 執行頻率：每週一 08:00（UTC+8）
- 任務目標：從 AI / CoWoS / 半導體來源輪替抓取新文章，翻譯整理為繁體中文情報週報，發布到 GitHub Pages。
- GitHub Repo：`promatw/ai-intel`
- 發布路徑：`content/posts/YYYY-MM-DD-ai-weekly.md`
- 站點 URL：`https://promatw.github.io/ai-intel/`

## 設定區

`GH_TOKEN` 必須由 Remote Task 執行環境提供，權限至少包含 repo contents read/write。不要把 token 寫入程式碼或 repo。

## 任務程式碼骨架

```python
import os
import urllib.request
import urllib.error
import json
import base64
import re
from datetime import datetime

GH_TOKEN = os.environ.get("GH_TOKEN", "")
GH_REPO = "promatw/ai-intel"
GH_BRANCH = "main"
if not GH_TOKEN:
    raise SystemExit("GH_TOKEN is required in the execution environment")

SITES = [
    {"index": 0, "name": "SemiAnalysis", "url": "https://www.semianalysis.com/", "search_query": "AI datacenter GPU CoWoS HBM site:semianalysis.com", "category": "ai-infrastructure", "lang": "en"},
    {"index": 1, "name": "NVIDIA Blog", "url": "https://blogs.nvidia.com/", "search_query": "AI datacenter GPU networking site:blogs.nvidia.com", "category": "ai-infrastructure", "lang": "en"},
    {"index": 2, "name": "TSMC Newsroom", "url": "https://pr.tsmc.com/", "search_query": "CoWoS advanced packaging AI site:tsmc.com", "category": "advanced-packaging", "lang": "en"},
    {"index": 3, "name": "TrendForce", "url": "https://www.trendforce.com/news/", "search_query": "CoWoS HBM advanced packaging site:trendforce.com", "category": "advanced-packaging", "lang": "en"},
    {"index": 4, "name": "DIGITIMES Asia", "url": "https://www.digitimes.com/", "search_query": "CoWoS AI semiconductor supply chain site:digitimes.com", "category": "semiconductor-supply-chain", "lang": "en"},
    {"index": 5, "name": "EE Times", "url": "https://www.eetimes.com/", "search_query": "AI chip CoWoS semiconductor site:eetimes.com", "category": "semiconductor-supply-chain", "lang": "en"},
    {"index": 6, "name": "Nikkei Asia", "url": "https://asia.nikkei.com/", "search_query": "TSMC Nvidia AI chip supply chain site:asia.nikkei.com", "category": "semiconductor-supply-chain", "lang": "en"},
    {"index": 7, "name": "Tom's Hardware", "url": "https://www.tomshardware.com/", "search_query": "AI GPU HBM CoWoS site:tomshardware.com", "category": "ai-infrastructure", "lang": "en"},
    {"index": 8, "name": "OpenAI News", "url": "https://openai.com/news/", "search_query": "OpenAI model infrastructure compute site:openai.com", "category": "model-labs", "lang": "en"},
    {"index": 9, "name": "Anthropic News", "url": "https://www.anthropic.com/news", "search_query": "Anthropic Claude model infrastructure site:anthropic.com", "category": "model-labs", "lang": "en"},
    {"index": 10, "name": "Google AI Blog", "url": "https://blog.google/technology/ai/", "search_query": "Gemini TPU AI infrastructure site:blog.google", "category": "model-labs", "lang": "en"},
    {"index": 11, "name": "IEEE Spectrum", "url": "https://spectrum.ieee.org/", "search_query": "AI chip semiconductor packaging site:spectrum.ieee.org", "category": "market-signal", "lang": "en"},
]

GH_API = "https://api.github.com"

def gh_request(method, path, data=None):
    url = f"{GH_API}{path}"
    headers = {
        "Authorization": f"Bearer {GH_TOKEN}",
        "Accept": "application/vnd.github+json",
        "X-GitHub-Api-Version": "2022-11-28",
        "User-Agent": "promatw-ai-intel-weekly/1.0",
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

print("AI / CoWoS 情報週報：讀取 state 與 published_urls")
today = datetime.now()
today_str = today.strftime("%Y-%m-%d")
state_path = "data/state.json"
state_str, state_sha = gh_get_file(state_path)
state = json.loads(state_str) if state_str else {"rotation_index": 0, "seen_articles": []}
rotation_index = state.get("rotation_index", 0)
seen_articles = state.get("seen_articles", [])
seen_urls = set(entry.split("||", 1)[1] for entry in seen_articles if "||" in entry)

pub_urls_path = "data/published_urls.json"
pub_str, pub_sha = gh_get_file(pub_urls_path)
pub_db = json.loads(pub_str) if pub_str else {"urls": []}
published_urls = set(pub_db.get("urls", []))
all_seen_urls = seen_urls | published_urls

this_week_sites = [SITES[i % len(SITES)] for i in range(rotation_index, rotation_index + 5)]
print("本週來源：")
for site in this_week_sites:
    print(f"- [{site['index']}] {site['name']} | {site['url']} | WebSearch: {site['search_query']}")

print("""
請直接執行抓取、翻譯與摘要，不要呼叫外部 LLM API。
對每個來源最多收錄 1 篇，先 WebFetch，失敗則 WebSearch。
抓到文章後必須比對 all_seen_urls；若重複，依序嘗試主要欄位、其他分類、WebSearch 三輪換文。

請將結果填入 collected，格式如下：
collected = [
  {"url": "https://...", "site": "來源名", "category": "advanced-packaging", "title": "繁中標題", "summary": "2-4 句繁中摘要", "notebooklm": true}
]
""")

# Claude/Codex 在此填入 collected 後，繼續執行以下發布段落。

def category_for(items):
    cats = []
    for item in items:
        cat = item.get("category") or "market-signal"
        if cat not in cats:
            cats.append(cat)
    return cats or ["market-signal"]

def item_to_html(item):
    title = item.get("title", "未命名情報")
    site = item.get("site", "未知來源")
    url = item.get("url", "")
    summary = item.get("summary", "")
    notebook = "適合放入 NotebookLM" if item.get("notebooklm") else "摘要追蹤"
    return (
        f"<h2>{title}</h2>\n"
        f"<p><strong>來源：</strong>{site}　｜　<strong>處理建議：</strong>{notebook}</p>\n"
        f"<p>{summary}</p>\n"
        f"<p><a href=\"{url}\" target=\"_blank\" rel=\"noopener\">閱讀原文</a></p>\n<hr>"
    )

slug = f"{today_str}-ai-weekly"
post_title = f"{today_str} AI / CoWoS 情報週報"
items_html = "\n".join(item_to_html(item) for item in collected)
categories = category_for(collected)
cat_toml = ", ".join(json.dumps(c, ensure_ascii=False) for c in categories)

md_body = (
    "+++\n"
    f"title = {json.dumps(post_title, ensure_ascii=False)}\n"
    f"date = {json.dumps(today.isoformat(), ensure_ascii=False)}\n"
    f"slug = {json.dumps(slug, ensure_ascii=False)}\n"
    f"categories = [{cat_toml}]\n"
    "draft = false\n"
    "+++\n\n"
    "<p><strong>本週重點：</strong>本週報由 AI 自動整理，聚焦 AI 算力、CoWoS、先進封裝、半導體供應鏈與模型實驗室動態。</p>\n\n"
    f"{items_html}\n"
)

result = gh_put_file(f"content/posts/{slug}.md", md_body, message=f"feat(post): {post_title}")
if not result or "content" not in result:
    raise SystemExit(f"commit failed: {result}")

new_seen = [f"{item['site']}||{item['url']}" for item in collected]
state["seen_articles"] = (seen_articles + new_seen)[-60:]
state["rotation_index"] = (rotation_index + 5) % len(SITES)
state_ok = gh_put_file(state_path, json.dumps(state, ensure_ascii=False, indent=2), sha=state_sha, message=f"chore(state): rotate ai weekly {rotation_index} to {state['rotation_index']}")

new_urls = [item["url"] for item in collected if item.get("url")]
pub_db["urls"] = list(dict.fromkeys(pub_db.get("urls", []) + new_urls))
pub_db["last_updated"] = today_str
pub_ok = gh_put_file(pub_urls_path, json.dumps(pub_db, ensure_ascii=False, indent=2), sha=pub_sha, message=f"chore(urls): add ai weekly urls {today_str}")

print("週報完成")
print(f"文章網址：https://promatw.github.io/ai-intel/posts/{slug}/")
print(f"狀態寫入：{'成功' if state_ok else '失敗'}")
print(f"URL DB 更新：{'成功' if pub_ok else '失敗'}")
```
