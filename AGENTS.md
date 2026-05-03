# AGENTS.md

## 專案定位

本 repo 是「全球封測最前線」網站，目標是以繁體中文整理全球先進封裝、先進測試、設備與材料情報。

目前策略：先複用原 `promatw.github.io` 的 Hugo / PaperMod 客製版型，讓網站骨架與視覺完整度先到位；品牌色、圖片、logo 與商業定位之後再討論。

## 發布規則

- 文章放在 `content/posts/`。
- Hugo front matter 目前使用 YAML `---`，與原站內容格式一致。
- 分類固定使用：`advanced-packaging`、`advanced-testing`、`equipment`、`materials`。
- 文章至少保留原文 URL、來源、分類、摘要、重要性判讀。
- 正式內容公開摘要與脈絡整理，不公開搬運付費或授權不明全文。
- 狀態檔固定放在 `data/`，不得改名。
- `data/published_urls.json` 是跨文章、週報、月報的 URL 去重資料庫。

## 自動化規則

- 第一階段不要直接開週期排程；先人工／半自動完成每分類 2-3 篇正式文章。
- GitHub API 寫入邏輯沿用 `docs/autopost_skill.md` 的穩定做法。
- 翻譯與摘要由 Codex 任務本身直接執行，不在 Python 程式碼內呼叫 Anthropic API。
- 週報／批次發布步驟要沿用步驟一讀到的 state，不重新讀取。
- 月報固定 2 次 commit：月報本身一次，來源歸檔 + state + URLs 一次。
- 不要在 repo 內提交 token、API key 或 `.env`。

## 重要文件

- `docs/source_sites.md`：正式來源白名單與四分類來源。
- `docs/website_project_handoff.md`：交接給後續網站／自動化／電子報專案。
- `docs/business_model_handoff.md`：交接給新的商業模型 Chat。
- `docs/project_instructions.md`：本專案穩定規則。

## Obsidian

發布文章的 metadata 可同步輸出到：

- `obsidian/articles/`
- `obsidian/metadata/`

預期對應本機 vault：`G:\我的雲端硬碟\secondbrain`。