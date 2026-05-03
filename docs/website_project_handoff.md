# 全球封測最前線｜網站與自動化交接文件

建立日期：2026-05-03  
用途：交接給新的 Codex 專案／新 Chat，繼續做網站、正式文章、自動抓取發布、NotebookLM、Obsidian 與電子報流程。

## 目前狀態

| 項目 | 狀態 |
|---|---|
| 本機資料夾 | `G:\我的雲端硬碟\ai-intel` |
| GitHub repo | `promatw/ai-intel` |
| GitHub Pages | https://promatw.github.io/ai-intel/ |
| 網站名稱 | 全球封測最前線 |
| 視覺風格 | 已沿用原 `promatw.github.io` 的 Hugo / PaperMod 客製樣式 |
| 分類 | 先進封裝、先進測試、設備、材料 |
| 測試文章 | 每分類 2 篇，共 8 篇假資料 |
| 部署 | GitHub Actions 成功 |

## 重要設計決策

1. 現階段先讓網站外觀、排版、連結方式與原 MM 網站一致，降低溝通成本。
2. 品牌視覺暫不重設；之後再討論顏色、圖片、logo、語氣與商業定位。
3. Claude 繼續維護原 MM 網站；Codex 負責「全球封測最前線」。
4. 自動化排程不要一開始就開，先用正式文章填滿每分類 2-3 篇。
5. 正式內容只公開摘要、脈絡整理與原文連結；不要公開搬運付費或授權不明全文。

## 目前 repo 重要檔案

| 檔案 | 用途 |
|---|---|
| `hugo.toml` | 站名、導覽、分類、PaperMod module 設定 |
| `assets/css/extended/blocksy-style.css` | 原站同款視覺樣式 |
| `layouts/_default/list.html` | 首頁與分類頁卡片列表 |
| `layouts/partials/header.html` | 藍色 header 與手機選單 |
| `layouts/partials/sidebar.html` | 搜尋、Recent Posts、文章分類側欄 |
| `content/categories/*/_index.md` | 四個分類頁 |
| `content/posts/*.md` | 目前 8 篇測試文章，之後替換為正式文章 |
| `docs/source_sites.md` | 正式來源白名單與分類規則 |
| `data/state.json` | 未來自動抓取輪替狀態 |
| `data/monthly_state.json` | 未來月報狀態 |
| `data/published_urls.json` | 跨報告 URL 去重資料庫 |

## 下一階段工作

### Phase 1：正式內容取代測試文章

目標：每個分類 2-3 篇正式文章，網站看起來像可公開營運的情報站。

流程：
1. 從 `docs/source_sites.md` 的 Tier 1 來源人工挑文。
2. 每篇文章建立 Markdown，保留原文 URL 與來源。
3. 每篇加入：摘要、重要性判讀、產業脈絡、後續追蹤問題。
4. 用正式文章替換目前測試文章。
5. 部署後檢查首頁、分類頁、文章頁與連結。

### Phase 2：Codex 半自動發布

先不要排程，只做「手動觸發」流程。

建議流程：
1. Codex 讀來源清單。
2. 針對指定分類抓 2-3 篇候選。
3. 輸出候選表：標題、URL、來源、分類、是否公開可讀、是否重複、推薦程度。
4. 使用者確認後，Codex 產生文章 Markdown。
5. Codex commit / push 到 GitHub Pages。
6. 同步更新 `data/published_urls.json`。

### Phase 3：Codex 自動化排程

等 Phase 2 跑順再做。不要直接複製 Claude Remote Scheduled Task 的排程模式；這個站應該先用 Codex 管理。

保留原穩定規則：
- GitHub API commit helper 不亂改。
- 不在 Python 裡呼叫 Anthropic API。
- `published_urls.json` 做跨文章去重。
- 月報固定 2 次 commit。
- WebFetch 失敗時使用 WebSearch 備援。

### Phase 4：Newsletter / 電子報

不建議現在馬上做前台訂閱表單，但應先規劃資料結構與流程。

建議架構：

| 元件 | 建議 |
|---|---|
| 訂閱表單 | Hugo 靜態頁嵌入表單，但不直接暴露寄信 API key |
| 後端端點 | Supabase Edge Function、Cloudflare Worker 或其他 serverless endpoint |
| 訂閱者資料庫 | Supabase `subscribers` table |
| 寄信服務 | Resend |
| 寄信觸發 | Codex 自動化或 GitHub Actions 呼叫 serverless endpoint |
| 內容來源 | 每週／每月已發布文章中標記 `newsletter_candidate = true` 的項目 |

資料表草案：

```sql
create table subscribers (
  id uuid primary key default gen_random_uuid(),
  email text not null,
  site text not null default 'advanced-packaging-frontline',
  status text not null default 'pending',
  source text,
  tags text[] default '{}',
  consent_at timestamptz,
  confirmed_at timestamptz,
  unsubscribed_at timestamptz,
  created_at timestamptz default now(),
  unique (email, site)
);
```

寄信流程：
1. 訂閱者填 email。
2. serverless endpoint 寫入 `status = pending`。
3. Resend 寄出確認信。
4. 使用者點確認連結後改為 `active`。
5. 每次寄信必須有 unsubscribe link。

注意：Resend 與 Supabase pricing 會變動。2026-05-03 查到 Resend Free 有 3,000 emails/month 與 100/day；Supabase 官方文件說 Free plan 可用於起步並提供 2 個 free projects。正式上線前需再次確認。

## 新 Chat 開場提示：網站專案

```text
我們要繼續做「全球封測最前線」網站。

請先閱讀 repo：G:\我的雲端硬碟\ai-intel
重點文件：
- docs/source_sites.md
- docs/website_project_handoff.md
- docs/project_instructions.md
- docs/autopost_skill.md

目前狀態：
- GitHub repo: promatw/ai-intel
- Pages: https://promatw.github.io/ai-intel/
- 視覺風格已比照原 promatw.github.io
- 四分類：先進封裝、先進測試、設備、材料
- 目前每類有 2 篇假測試文章

下一步：
1. 從 docs/source_sites.md 挑正式文章來源。
2. 每個分類先做 2-3 篇正式文章。
3. 不要先開排程。
4. 等正式文章格式跑順後，再規劃 Codex 端半自動與排程發布。
```