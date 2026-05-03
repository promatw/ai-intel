# 全球封測最前線 Project Instructions

## 任務背景

本專案管理 `promatw/ai-intel` 的網站與未來自動發布流程。網站部署於 GitHub Pages：

- Repo: `promatw/ai-intel`
- Site: `https://promatw.github.io/ai-intel/`
- Content: `content/posts/`
- State: `data/state.json`, `data/monthly_state.json`, `data/published_urls.json`

目前站名為「全球封測最前線」，四個正式分類為：

- `advanced-packaging`：先進封裝
- `advanced-testing`：先進測試
- `equipment`：設備
- `materials`：材料

## 穩定規則

1. GitHub API commit 函數沿用 `docs/autopost_skill.md`。
2. 翻譯、摘要、長文判斷由 Codex 任務直接執行，不在 Python 裡呼叫 Anthropic API。
3. 第一階段先人工／半自動發布正式文章，不直接啟動週期排程。
4. 去重以 URL 為準，合併 `seen_articles` 與 `published_urls.json`。
5. 批次發布 slug 建議：`YYYY-MM-DD-packaging-...`、`YYYY-MM-DD-testing-...`、`YYYY-MM-DD-equipment-...`、`YYYY-MM-DD-materials-...`。
6. 若未來建立週報，slug 使用：`YYYY-MM-DD-advanced-packaging-weekly` 或 `YYYY-MM-DD-osat-weekly`，需先再確認命名。
7. 若未來建立月報，slug 使用：`YYYY-MM-monthly`。
8. 月報固定 2 次 commit，避免 GitHub Actions build 互相排擠。
9. 包含 inline HTML 的 Markdown 不用 GitHub 網頁編輯器貼入，改用程式化 commit 或 Upload files。
10. 不公開全文翻譯付費或授權不明內容；公開內容以摘要、脈絡整理與原文連結為主。

## 文章定位

每篇正式文章應包含：

- 原文標題與 URL
- 來源網站
- 所屬分類
- 2-4 句繁體中文摘要
- 重要性判讀：為什麼這則消息值得追蹤
- 供應鏈位置：封裝、測試、設備、材料或跨分類
- 後續追蹤問題
- 是否適合 NotebookLM
- 是否適合電子報

## 電子報規則

電子報不是第一階段立即實作，但架構已在 `docs/website_project_handoff.md` 規劃。

原則：

- 前端不得暴露 Resend 或 Supabase service key。
- 訂閱表單需經 serverless endpoint。
- 建議使用 double opt-in。
- 每封信必須有 unsubscribe link。
- 寄信成本與 free tier 需在實作前重新查官方 pricing。

## 來源清單

正式來源以 `docs/source_sites.md` 為準。未列入白名單的來源可以人工評估，但不要直接加入自動化排程。