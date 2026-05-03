# AI / CoWoS 情報網站 Project Instructions

## 任務背景

本專案管理 `promatw/ai-intel` 的自動發布流程。網站部署於 GitHub Pages：

- Repo: `promatw/ai-intel`
- Site: `https://promatw.github.io/ai-intel/`
- Content: `content/posts/`
- State: `data/state.json`, `data/monthly_state.json`, `data/published_urls.json`

## 穩定規則

1. GitHub API commit 函數沿用 `docs/autopost_skill.md`。
2. 翻譯、摘要、長文判斷由 Claude/Codex 任務直接執行，不在 Python 裡呼叫 Anthropic API。
3. 週報每次從 12 個來源輪替抓 3-5 篇，每網站最多 1 篇。
4. 去重以 URL 為準，合併 `seen_articles` 與 `published_urls.json`。
5. 週報 slug：`YYYY-MM-DD-ai-weekly`。
6. 月報 slug：`YYYY-MM-monthly`。
7. 月報固定 2 次 commit，避免 GitHub Actions build 互相排擠。
8. 包含 inline HTML 的 Markdown 不用 GitHub 網頁編輯器貼入，改用程式化 commit 或 Upload files。

## 文章定位

週報偏情報摘要與脈絡解讀；月報偏趨勢歸納。遇到長文或高價值文章，額外標記是否適合：

- 整篇翻譯
- NotebookLM source
- Obsidian 永久筆記
- 後續月報主題
