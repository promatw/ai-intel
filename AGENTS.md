# AGENTS.md

## 專案定位

本 repo 是 AI / CoWoS 情報網站，目標是以繁體中文整理 AI 基礎設施、CoWoS/先進封裝、半導體供應鏈、模型實驗室與市場訊號。

## 發布規則

- 文章放在 `content/posts/`。
- Hugo front matter 使用 TOML `+++`。
- 週報 slug：`YYYY-MM-DD-ai-weekly`。
- 月報 slug：`YYYY-MM-monthly`。
- 分類優先使用：`ai-infrastructure`、`advanced-packaging`、`semiconductor-supply-chain`、`model-labs`、`market-signal`、`longform-translation`。
- 狀態檔固定放在 `data/`，不得改名。
- `data/published_urls.json` 是跨週報/月報去重資料庫。

## Remote Task 規則

- GitHub API 寫入邏輯沿用 `docs/autopost_skill.md`。
- 翻譯與摘要由 Claude/Codex 任務本身直接執行，不在 Python 程式碼內呼叫 Anthropic API。
- 週報步驟六沿用步驟一讀到的 state，不重新讀取。
- 月報固定 2 次 commit：月報本身一次，來源 draft + state + URLs 一次。
- 不要在 repo 內提交 token、API key 或 `.env`。

## Obsidian

發布文章的 metadata 可同步輸出到：

- `obsidian/articles/`
- `obsidian/metadata/`

預期對應本機 vault：`G:\我的雲端硬碟\secondbrain`。
