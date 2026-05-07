# 全球封測最前線｜正式文章來源清單 v1

建立日期：2026-05-03  
用途：作為「全球封測最前線」正式內容抓取、翻譯、摘要、分類與後續自動化排程的來源白名單。

## 使用原則

1. 第一階段先人工／半自動挑文，不直接啟動週期排程。
2. 每個分類先完成 2-3 篇正式文章，確認內容格式、引用方式與分類邏輯。
3. 優先使用公開可讀來源；付費牆內容只可做標題與公開摘要層級的觀察，不公開全文翻譯。
4. 官方新聞稿、公司 IR、產業協會與技術媒體分開處理：官方來源適合確定事件，媒體來源適合脈絡與趨勢。
5. 每篇文章至少保留：原文 URL、來源、分類、摘要、重要性判讀、是否值得 NotebookLM、是否適合電子報。

## 四大分類

| 分類 slug | 中文分類 | 內容範圍 |
|---|---|---|
| `advanced-packaging` | 先進封裝 | CoWoS、2.5D/3D IC、chiplet、HBM 整合、hybrid bonding、glass substrate、fan-out、panel-level packaging |
| `advanced-testing` | 先進測試 | ATE、wafer probe、system-level test、burn-in、probe card、test interface、HPC/AI 晶片測試 |
| `equipment` | 設備 | bonding、lithography、inspection/metrology、dicing/grinding、cleaning、backend automation、封裝與測試設備 |
| `materials` | 材料 | ABF substrate、glass core、underfill、TIM、molding compound、photoresist、bonding materials、封裝基板與化材 |

## Tier 1：正式監測來源

這一層優先放進 Codex 抓取流程。每次人工挑文也先從這裡開始。

| 來源 | URL | 主要分類 | 使用方式 | 備註 |
|---|---|---|---|---|
| TSMC Newsroom | https://pr.tsmc.com/ | 先進封裝、設備、材料 | 官方事件確認 | CoWoS、先進封裝、產能、供應鏈投資 |
| ASE Technology | https://www.aseglobal.com/en/news | 先進封裝、先進測試 | 官方事件確認 | OSAT 觀點，適合追蹤封裝測試服務與產能 |
| Amkor Newsroom | https://amkor.com/newsroom/ | 先進封裝、先進測試 | 官方事件確認 | 先進封裝、測試、區域產能布局 |
| JCET News | https://www.jcetglobal.com/en/news/ | 先進封裝、先進測試 | 官方事件確認 | 中國 OSAT 龍頭，需留意地緣政治與客戶結構 |
| SEMI News | https://www.semi.org/en/news-resources | 設備、材料、市場訊號 | 產業協會與市場訊號 | 設備市場、材料、區域投資與政策 |
| Semiconductor Engineering | https://semiengineering.com/ | 先進封裝、先進測試、設備、材料 | 技術脈絡整理 | 優先抓 Advanced Packaging、Test/Inspection、Manufacturing 類文章 |
| 3D InCites | https://www.3dincites.com/ | 先進封裝、材料 | 技術與產業脈絡 | 3D integration、heterogeneous integration、advanced packaging 社群媒體 |
| TrendForce News | https://www.trendforce.com/news/ | 先進封裝、設備、市場訊號 | 快訊與市場訊號 | HBM、CoWoS、hybrid bonding、設備供需變化 |
| DIGITIMES Asia | https://www.digitimes.com/ | 先進封裝、設備、材料 | 台灣供應鏈訊號 | 常有台灣供應鏈消息；需注意付費牆 |
| EE Times | https://www.eetimes.com/ | 先進封裝、先進測試、設備 | 技術媒體 | 適合做趨勢脈絡與技術背景補充 |
| Semiconductor Digest | https://www.semiconductor-digest.com/ | 先進封裝、設備、材料 | 技術媒體 | 封裝、製程、檢測與材料新聞 |
| Yole Group | https://www.yolegroup.com/ | 先進封裝、設備、材料、市場訊號 | 報告摘要與市場框架 | 多為報告導流；可整理公開摘要，不搬運付費內容 |

## 台日公司第一手來源 v2

建立日期：2026-05-04  
完整主表：`docs/company_source_map_v2_2026-05-04.md`

這一層用於第 2/3 週擴充 50-100 家公司來源池。它不是自動排程白名單，而是「公司官網、News、IR、R&D、Products」的第一手來源候選池。正式文章與電子報優先從這裡挑來源，再由使用者審查後發布。

| 來源 | URL | 主要分類 | 使用方式 | 備註 |
|---|---|---|---|---|
| GPM 均豪精密 | https://www.gpmcorp.com.tw/ | 設備 | Products / News | 半導體、AOI、研磨、貼合、G2C+。 |
| C SUN 志聖工業 | https://www.csun.com.tw/ | 設備 | Products / News / IR | 半導體製程設備、真空壓膜、烘烤、光熱製程。 |
| 群翊工業 GP-Line | https://gp-line.com/ | 設備 | Products / News | 修正 Gemini 原 `shintech.com.tw`；此站明確標示先進封裝、FOPLP、TGV。 |
| Eternal 長興材料 | https://www.eternal-group.com/ | 材料 | Products / News / IR | 電子材料、封裝樹脂、光阻、材料應用。 |
| Shiny Chemical 勝一化工 | https://www.shinychem.com.tw/ | 材料 | Products / News / IR | 電子級溶劑、清洗液、剝離液；新聞有台積電供應商獎。 |
| Kinsus 景碩科技 | https://www.kinsus.com.tw/ | 材料 | Product / Investors | SiP、PBGA、FCCSP、CSP、FCBGA。 |
| Daxin 達興材料 | https://www.daxinmat.com/ | 材料 | Product / Technology / IR | 半導體材料、TBDB、PI。 |
| San Fu 三福化工 | https://www.sfchem.com.tw/ | 材料 | Products / IR | 顯影液、蝕刻液、剝離劑、研磨液代工。 |
| Everlight Chemical 永光化學 | https://www.ecic.com/ | 材料 | Products / News | PSPI、光阻、電子化學材料，需再找深頁。 |
| Tokyo Electron TEL | https://www.tel.com/ | 設備 | News / Products | 塗佈、開發、蝕刻、prober / bonding 相關設備。 |
| ULVAC | https://www.ulvac.co.jp/ | 設備 | News / Products / R&D | 真空、濺鍍、RDL、半導體製造裝置。 |
| TOWA | https://www.towajapan.co.jp/ | 設備 | Products / IR | molding、compression molding、singulation。 |
| JSR Corporation | https://www.jsr.co.jp/ | 材料 | Products / News / IR | photoresist、thick film resist、封裝用材料，需再找深頁。 |

待人工或備援確認：

| 來源 | URL | 狀態 | 備註 |
|---|---|---|---|
| GPTC 弘塑科技 | https://www.gptc.com.tw/# | canonical 已採用 | 使用者已確認；Gemini 給的 `https://www.grand-process.com/` 不採用，僅保留待查。 |
| Unimicron 欣興電子 | https://www.unimicron.com/ | 官網正確，抓取需備援 | 今天自動抓取遇到 502。 |
| Nanya PCB 南電 | https://www.nanyapcb.com.tw/ | 官網正確，抓取需備援 | 今天自動抓取內容空。 |
| Chemleader 化學領航 | http://www.chemleader.com.tw/ | 待驗證 | 濕製程化學品候選。 |
| CWTC 長華科技 | https://www.cwtg.com.tw/ | 待驗證 | 導線架 / QFN / 封裝支撐。 |
| Eternal Material 國精化學 | https://www.eternal-material.com/ | 待驗證 | UV 固化樹脂與電子化學品。 |
| Topco 崇越科技 | https://www.topco-global.com/ | 待驗證 | 半導體材料代理、散熱材料、研磨液。 |
| Niching 利機企業 | https://www.niching.com.tw/ | 待驗證 | CUF、銀膠、封裝工具。 |
| Crustal 晶化科技 | https://www.crustal.com.tw/ | 待驗證 | ABF 增層膜候選。 |

## 先進封裝來源

| 來源 | URL | 追蹤重點 | 搜尋備援 |
|---|---|---|---|
| TSMC Newsroom | https://pr.tsmc.com/ | CoWoS、SoIC、先進封裝產能 | `CoWoS SoIC advanced packaging site:tsmc.com` |
| ASE Technology | https://www.aseglobal.com/en/news | SiP、fan-out、advanced packaging services | `advanced packaging ASE Technology site:aseglobal.com` |
| Amkor | https://amkor.com/newsroom/ | advanced packaging campus、OSAT 產能 | `advanced packaging test Amkor site:amkor.com` |
| JCET | https://www.jcetglobal.com/en/news/ | 2.5D/3D、chiplet、OSAT 動態 | `advanced packaging JCET chiplet site:jcetglobal.com` |
| 3D InCites | https://www.3dincites.com/ | 3D integration、heterogeneous integration | `3D integration advanced packaging site:3dincites.com` |
| Semiconductor Engineering | https://semiengineering.com/ | chiplet、hybrid bonding、glass substrate | `advanced packaging chiplet hybrid bonding site:semiengineering.com` |
| TrendForce | https://www.trendforce.com/news/ | HBM、CoWoS、供需與市場訊號 | `CoWoS HBM advanced packaging site:trendforce.com` |
| Yole Group | https://www.yolegroup.com/ | advanced packaging market、back-end equipment | `advanced packaging Yole Group 2026` |

## 先進測試來源

| 來源 | URL | 追蹤重點 | 搜尋備援 |
|---|---|---|---|
| Advantest News | https://www.advantest.com/en/news/ | ATE、HPC/AI test、probe card partnership | `Advantest HPC AI test probe card site:advantest.com` |
| Teradyne News | https://www.teradyne.com/news/ | ATE、UltraFLEX、AI/HPC test | `Teradyne semiconductor test AI site:teradyne.com` |
| FormFactor News | https://www.formfactor.com/news/ | probe card、wafer probe、test interface | `FormFactor probe card AI test site:formfactor.com` |
| Technoprobe News | https://www.technoprobe.com/ | probe card、high-end test interface | `Technoprobe probe card semiconductor test` |
| Cohu News | https://www.cohu.com/news/ | test handlers、inspection、SLT | `Cohu semiconductor test handler AI site:cohu.com` |
| Chroma ATE News | https://www.chromaate.com/en/news | test system、Taiwan test ecosystem | `Chroma ATE semiconductor test site:chromaate.com` |
| Semiconductor Engineering | https://semiengineering.com/ | test, inspection, reliability | `semiconductor advanced test inspection site:semiengineering.com` |
| IEEE Spectrum | https://spectrum.ieee.org/ | 技術背景與工程脈絡 | `semiconductor test advanced packaging site:spectrum.ieee.org` |

## 設備來源

| 來源 | URL | 追蹤重點 | 搜尋備援 |
|---|---|---|---|
| Applied Materials News | https://www.appliedmaterials.com/us/en/about/news.html | hybrid bonding、advanced packaging、metrology | `Applied Materials hybrid bonding advanced packaging` |
| Lam Research News | https://newsroom.lamresearch.com/ | wafer-level process、packaging adjacency | `Lam Research advanced packaging hybrid bonding` |
| KLA News | https://www.kla.com/about/newsroom | inspection、metrology、defect control | `KLA advanced packaging inspection metrology` |
| Tokyo Electron News | https://www.tel.com/news/ | equipment roadmap、process tools | `Tokyo Electron advanced packaging equipment` |
| ASMPT SEMI Solutions | https://semi.asmpt.com/en/news-center/ | TCB、hybrid bonding、die attach | `ASMPT hybrid bonding TCB advanced packaging` |
| BE Semiconductor Industries | https://www.besi.com/investor-relations/news/ | hybrid bonding、die attach | `BESI hybrid bonding HBM packaging` |
| EV Group News | https://www.evgroup.com/company/news/ | wafer bonding、lithography、advanced packaging | `EV Group hybrid bonding advanced packaging` |
| SUSS MicroTec News | https://www.suss.com/en/news | bonding、temporary bonding、lithography | `SUSS MicroTec hybrid bonding advanced packaging` |
| DISCO News | https://www.disco.co.jp/eg/news/ | dicing、grinding、backend process | `DISCO dicing grinding advanced packaging AI` |
| Onto Innovation News | https://ontoinnovation.com/company/news | inspection/metrology for advanced packaging | `Onto Innovation advanced packaging inspection metrology` |
| Camtek News | https://www.camtek.com/news/ | inspection/metrology, advanced packaging | `Camtek advanced packaging inspection` |

## 材料來源

| 來源 | URL | 追蹤重點 | 搜尋備援 |
|---|---|---|---|
| Ajinomoto Group News | https://www.ajinomoto.com/news | ABF、build-up film、substrate materials | `Ajinomoto ABF semiconductor packaging` |
| Resonac News | https://www.resonac.com/news | packaging materials、panel-level interposer | `Resonac semiconductor packaging materials JOINT` |
| NAMICS News | https://www.namics.co.jp/en/news/ | underfill、encapsulation、electronic materials | `NAMICS underfill semiconductor packaging` |
| Henkel Electronics | https://www.henkel-adhesives.com/ | die attach、underfill、thermal materials | `Henkel semiconductor packaging materials underfill` |
| DuPont Electronics News | https://www.dupont.com/electronic-materials.html | lithography、CMP、interconnect、packaging materials | `DuPont advanced packaging materials` |
| Shin-Etsu Chemical News | https://www.shinetsu.co.jp/en/news/ | semiconductor materials、silicone、photoresist | `Shin-Etsu semiconductor packaging materials` |
| AGC News | https://www.agc.com/en/news/ | glass substrate、materials consortium | `AGC glass substrate semiconductor packaging` |
| Mitsui Chemicals News | https://jp.mitsuichemicals.com/en/release/ | electronic materials、packaging materials | `Mitsui Chemicals semiconductor packaging materials` |
| Ibiden News | https://www.ibiden.com/news/ | package substrate、ABF substrate | `Ibiden package substrate AI semiconductor` |
| Unimicron News | https://www.unimicron.com/ | package substrate、advanced substrate | `Unimicron ABF substrate AI packaging` |

## 觀察但不優先自動抓取

| 來源 | 原因 | 使用規則 |
|---|---|---|
| The Information | AI 與供應鏈內容有價值，但常有付費牆 | 只做公開標題與公開摘要層級，不公開翻譯付費內容 |
| Nikkei Asia | 半導體供應鏈價值高，但付費牆比例高 | 人工挑文，僅摘要公開可見部分 |
| Reuters / Bloomberg / WSJ | 事件確認與資本市場訊號強，但授權限制高 | 只做短摘要與原文連結，不全文翻譯 |
| SemiAnalysis | 深度高但付費內容多 | 可做公開文章摘要；付費文僅做個人研究，不公開搬運 |

## 第一批正式文章建議

先不用自動排程，人工從 Tier 1 來源挑 8-12 篇，替換目前測試文章。

| 分類 | 建議第一批主題 |
|---|---|
| 先進封裝 | CoWoS 產能、hybrid bonding、HBM 封裝、glass substrate / panel-level packaging |
| 先進測試 | AI/HPC ATE、probe card 供應鏈、system-level test、burn-in 與熱測試 |
| 設備 | TCB / hybrid bonding equipment、inspection/metrology、dicing/grinding、backend automation |
| 材料 | ABF substrate、underfill、TIM、glass core、panel-level organic interposer materials |

## 自動化前檢查表

| 檢查項目 | 狀態 |
|---|---|
| 是否公開可讀 | 待逐站測試 |
| 是否有 RSS | 待逐站測試 |
| 是否容易被 403/付費牆擋住 | 待逐站測試 |
| 是否適合 WebFetch | 待逐站測試 |
| 是否需要 WebSearch 備援 | 預設需要 |
| 是否適合全文翻譯公開 | 預設不公開全文，只做摘要與引用 |
| 是否適合 NotebookLM | 長文、報告、技術白皮書優先 |
| 是否適合電子報 | 高訊號、跨分類、有明確產業意義者優先 |
## Wet process international equipment sources - pending deep validation (2026-05-07)

Source: Expert photo table from 2026-05-07 wet process review. These are first-pass source targets for the next round; deep-page crawl suitability has not been verified yet.

| Company | URL | Category | Source type to test | Topic fit | Validation status |
|---|---|---|---|---|---|
| ACM Research | https://www.acmr.com/ | Equipment | Products / News / IR | Single-wafer wet clean, megasonic clean, advanced packaging flux clean | Pending deep-page validation |
| SCREEN Semiconductor Solutions | https://www.screen.co.jp/s/s | Equipment | Products / News / Technology | Wet cleaning equipment, single-wafer clean, repeatable clean recipes | Pending deep-page validation |
| Tokyo Electron | https://www.tel.com/ | Equipment | Products / News / Technology | Process integration, clean / etch / deposition adjacency, advanced packaging equipment | Pending deep-page validation |
| Pactech | https://www.pactech.com/ | Equipment | Products / News | Modular wet bench, pilot-line wet process, flux handling | Pending deep-page validation |
| SCHMID Group | https://schmid-group.com/ | Equipment | Products / News | Panel-level wet process, RDL / embedded trace / panel equipment | Pending deep-page validation |


