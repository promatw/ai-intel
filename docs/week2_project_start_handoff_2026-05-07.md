# 全球封測最前線 Week2 專案開始交接

日期：2026-05-07  
狀態：Week1 已完成兩篇試作文章與第一批來源擴充，Week2 準備進入 50-100 家公司來源池整理與深頁驗證。

## 目前已完成

1. 第一篇設備文章已上架  
   URL: https://promatw.github.io/ai-intel/posts/2026-05-03-equipment-dicing-grinding-dispensing-thermal/

2. 第二篇濕製程與化學藥液文章已上架  
   URL: https://promatw.github.io/ai-intel/posts/2026-05-07-equipment-wet-process-chemicals/

3. 第二篇文章使用兩張代表圖，已放在：
   - `G:\我的雲端硬碟\ai-intel\static\images\posts\wet-process-equipment.png`
   - `G:\我的雲端硬碟\ai-intel\static\images\posts\wet-process-chemicals.png`

4. 來源清單已新增濕製程國際設備商待驗證名單：
   - ACM Research: https://www.acmr.com/
   - SCREEN Semiconductor Solutions: https://www.screen.co.jp/s/s
   - Tokyo Electron: https://www.tel.com/
   - Pactech: https://www.pactech.com/
   - SCHMID Group: https://schmid-group.com/

5. 已建立公開報導語氣規則，避免文章寫成協作摘要或審稿筆記：
   - `G:\我的雲端硬碟\ai-intel\docs\article_voice_guideline_2026-05-07.md`
   - Obsidian: `知識庫/全球封測最前線-公開報導語氣規則-2026-05-07.md`

## Week2 主要目標

Week2 不急著產出大量文章。優先建立可長期使用的來源池與驗證流程。

建議目標：

1. 整理 50-100 家先進封裝、測試、設備、材料相關公司。
2. 每家公司至少建立一個官方入口 URL。
3. 標記最適合的入口類型：News / IR / R&D / Products / Technology / Blog。
4. 對第一批高潛力來源做深頁驗證，看能不能抓到文章、產品頁或技術頁。
5. 記錄抓取難度、是否有付費牆、是否適合電子報、網站文章、Podcast。
6. 先不啟動全自動排程，維持手動試跑。

## Week2 建議優先工作

第一優先：濕製程國際設備商深頁驗證  
從 `docs/source_sites.md` 新增的五家開始：

- ACM Research
- SCREEN Semiconductor Solutions
- Tokyo Electron
- Pactech
- SCHMID Group

驗證內容：

- 官網是否可正常開啟。
- 是否有 Products / News / Technology / IR 頁。
- 是否能找到 wet clean、single wafer clean、megasonic、flux clean、panel-level wet process、advanced packaging 相關深頁。
- 是否適合放進下一篇文章候選或只先留在來源池。

第二優先：公司來源池擴充  
依照 `docs/company_source_map_v2_2026-05-04.md` 與 `Advanced_Packaging_Supply_Chain.md` 去重、補分類、補入口。

第三優先：Week2 候選主題表  
每個候選主題至少包含：

- 主題名稱
- 可支撐的公司
- 代表來源
- 適合分類：advanced-packaging / advanced-testing / equipment / materials
- 適合形式：網站文章 / 電子報 / Podcast
- 是否需要外部專家審閱

## 新 session 應讀取的檔案

請新 session 優先閱讀以下檔案：

1. `G:\我的雲端硬碟\ai-intel\docs\week2_project_start_handoff_2026-05-07.md`
2. `G:\我的雲端硬碟\ai-intel\docs\source_sites.md`
3. `G:\我的雲端硬碟\ai-intel\docs\company_source_map_v2_2026-05-04.md`
4. `G:\我的雲端硬碟\ai-intel\Advanced_Packaging_Supply_Chain.md`
5. `G:\我的雲端硬碟\ai-intel\docs\article_voice_guideline_2026-05-07.md`
6. `G:\我的雲端硬碟\ai-intel\docs\wet_process_chemicals_deep_validation_2026-05-04.md`
7. `G:\我的雲端硬碟\ai-intel\docs\week1_second_work_cycle_handoff_2026-05-04.md`
8. `G:\我的雲端硬碟\ai-intel\global_packaging_12_week_checklist.html`
9. `G:\我的雲端硬碟\ai-intel\docs\website_project_handoff.md`

Obsidian 參考：

- `知識庫/全球封測最前線-濕製程文章專家回饋與審稿版-2026-05-07.md`
- `知識庫/全球封測最前線-公開報導語氣規則-2026-05-07.md`

## 新 session 開場提示

可以直接貼這段：

```
我們要開始「全球封測最前線」Week2。請先閱讀：
G:\我的雲端硬碟\ai-intel\docs\week2_project_start_handoff_2026-05-07.md
G:\我的雲端硬碟\ai-intel\docs\source_sites.md
G:\我的雲端硬碟\ai-intel\docs\company_source_map_v2_2026-05-04.md
G:\我的雲端硬碟\ai-intel\Advanced_Packaging_Supply_Chain.md
G:\我的雲端硬碟\ai-intel\docs\article_voice_guideline_2026-05-07.md

Week2 先不要直接寫文章，先做來源池與深頁驗證。第一步請從濕製程國際設備商五家開始：ACM Research、SCREEN、TEL、Pactech、SCHMID。目標是確認它們的 Products / News / Technology 頁能不能抓到適合後續文章、電子報或 Podcast 的第一手資料。
```

## 寫作注意事項

公開文章不要寫成「我們討論後」「專家確認」「使用者提供」「一般讀者可以理解為」這類協作或審稿語氣。外部專家意見要消化成產業判斷，保留在內部記錄，不要直接放進公開正文。

來源揭露目前採取「每家公司一個代表 URL」方式：公開文章保留公司名稱與代表第一手 URL，完整來源地圖保存在 `docs/source_sites.md` 與 Obsidian。

