# 每週自動蒐整流程

本檔是每週一排程任務的作業說明。修改檢索詞、來源或分類規則時，直接編輯本檔即可，下次執行就會套用。

## 基本設定

| 項目 | 值 |
|---|---|
| GitHub repo | `andy9556665-glitch/amr-env-monitor`（分支 `main`） |
| 網站資料檔 | `data/items.json` |
| Google 試算表 | ID `1l8GsU9DsomdAtuYaPwD-OR9q1yhLHj29ig6P9hbWgn4`，工作表「彙整資料」 |
| 快訊信箱 | andy9556665@gmail.com |
| 蒐整區間 | 上次執行日（`items.json` 的 `updated_at`）至本次執行日；首次以 7 天為準 |
| 語言 | 繁體中文（臺灣用語），專有名詞保留英文 |

## 步驟

1. **取得現有資料**：clone repo，讀 `data/items.json`，以 `id`、`url`、`pmid`、`doi` 建立去重清單。
2. **讀 Gmail 快訊**：以下列查詢搜尋蒐整區間內的信件，讀取完整內容，擷取文獻題名、期刊、DOI 或連結：
   `from:(scopus.com OR elsevier.com OR clarivate.com OR webofscience.com OR ncbi.nlm.nih.gov OR nlm.nih.gov) after:YYYY/MM/DD`
   只讀取快訊信件，不處理、不更動其他信件。
3. **檢索 PubMed**：用 WebFetch 逐一開啟下列搜尋頁（`https://pubmed.ncbi.nlm.nih.gov/?term=<檢索詞>&sort=date&size=50`），一次一頁；遇到速率限制就等一下再試一次，仍失敗就略過並記下。只收發布日期在蒐整區間內者。
   - `antibiotic resistance genes wastewater`
   - `antibiotic resistance river OR sediment OR sludge`
   - `antibiotics occurrence wastewater LC-MS/MS OR non-target screening`
   - `co-selection OR quaternary ammonium OR biocide OR heavy metal antibiotic resistance`
   - `wastewater treatment removal antibiotic resistance genes`
   - `environmental antimicrobial resistance risk assessment OR surveillance`
4. **搜尋官方機構**：以 WebSearch 針對各機構搜尋蒐整區間內的新公告（關鍵字 antimicrobial resistance / antibiotic / AMR environment / wastewater），必要時 WebFetch 原文確認日期與內容：
   WHO、UNEP、FAO、WOAH、OECD、European Commission（DG ENV、DG SANTE、JRC）、EEA、ECDC、EMA、EFSA、US EPA、US CDC、UK（UKHSA、Environment Agency、DEFRA）、Health Canada / Environment and Climate Change Canada、日本環境省／厚生勞動省、臺灣環境部、衛福部疾管署、農業部。
5. **篩選與分類**：只收與「環境面」抗生素、化學物質與抗藥性相關者，純臨床個案、新藥研發等排除。每筆依下方 10 類擇一（主類），並給月報分類（政策與法規動態／監測與檢測技術／風險評估與管理案例／國內可參考方向）。
6. **撰寫摘要**：盡量讀取摘要或原文（PubMed 文章頁、Europe PMC、出版社頁、官方原文）。`basis` 填實際依據（摘要／全文／官方新聞稿／題名）。只依題名者，摘要句末加「（依題名整理，待回查摘要）」。不得杜撰數據。
7. **更新網站**：把新項目加到 `data/items.json` 的 `items`（`batch` = 本次執行日 YYYY-MM-DD，`review` = "待審"），更新 `updated_at`（含 +08:00）與 `coverage`。用 `python3 -m json.tool` 驗證後 commit 並 push 到 `main`。commit 訊息：`週更新 YYYY-MM-DD：新增 N 筆`。
8. **更新試算表**：用 Google Sheets 的 append 功能把新項目依下列欄位順序附加到「彙整資料」：
   ID、收錄批次、發布日期、資訊來源、來源類型、國家或區域、資料類型、文件名稱（中文）、文件名稱（原文）、類別編號、類別名稱、月報分類、內容摘要、重點摘譯、國內管理意涵、建議後續追蹤、關鍵字（以「、」連接）、原始連結、PMID、DOI、摘要依據、審查狀態、審查人（空白）、審查備註（空白）
9. **回報**：列出本週新增筆數、各類別筆數、重大政策發展（若有），以及無法讀取的來源。

## items.json 欄位

`id`（pmid-XXXX、doi 或英文短代碼）、`batch`、`pub_date`（YYYY-MM-DD）、`source`、`source_type`（官方機構／學術文獻）、`region`、`doc_type`（政策／法規／監測報告／技術指引／研究論文）、`title_en`、`title_zh`、`category`（1–10 數字）、`report_cat`、`summary`、`key_points`、`implication`、`follow_up`、`keywords`（陣列）、`url`、`pmid`、`doi`、`review`、`basis`

## 10 類收納標準

1. **化學方法的檢測技術**：抗生素、藥物及新興污染物之化學分析方法，含樣品前處理（SPE、冷凍乾燥等）、LC-MS/MS 標的分析、HRMS 疑似目標及非標的篩檢（NTS），以及 QA/QC、基質效應與樣品保存。僅報導濃度調查而未涉及方法者歸第 5 類。
2. **生物方法的檢測技術**：抗藥性菌及抗藥基因之生物／分子檢測方法，含培養法、PCR/qPCR、HT-qPCR、ddPCR、宏基因體、全基因體定序、生物資訊流程，以及採樣、保存與 QA/QC。以方法建立、比較或驗證為主要貢獻者。
3. **ARGs 機制研究**：抗藥基因與抗生素對應及抗藥機制（如 sul／磺胺類、tet／四環黴素類、bla／β-內醯胺類），以及可動遺傳因子（質體、轉位子、整合子）之水平基因轉移機制。
4. **非抗生素化學物質共選研究**：季銨鹽、消毒劑、殺生物劑、重金屬等造成抗藥性共選或交叉抗性，含共位、交叉抗性、共同調節機制，亞抑菌濃度與最低選擇濃度（MSC），金屬抗性基因與 ARGs 之連鎖。
5. **抗生素及化學物質環境殘留與污染來源**：於污水、放流水、污泥、河川、土壤及沉積物之殘留濃度、流布與宿命，以及醫療、畜牧、水產養殖、製藥、民生等污染來源之排放特性。
6. **ARGs 環境監測與指標應用**：環境介質中 ARGs 及抗藥菌之實際監測，含時空分布、處理前後變化、污水監測，以及 16S rRNA、intI1、sul1 等指標基因之應用；化學物質與 ARGs 相關性分析亦歸此。
7. **污水處理與抗藥性削減技術**：處理程序及技術（生物處理、消毒、高級氧化、薄膜、污泥處理等）對抗生素、化學物質、抗藥菌及 ARGs 之去除效能與影響因子，以及源頭減量與設施改善評估。僅報導處理前後變化而未評估技術者，歸第 5 類或第 6 類。
8. **風險評估、分級與預警機制**：ARGs 風險排序（人類活動關聯性、可移動性、病原宿主）、優先指標篩選、預警閾值、分級預警、哨兵點位設計及管理觸發條件。
9. **各國及國際組織公告、法規與政策**：國際組織（WHO、UNEP、FAO、WOAH、OECD、ECHA 等）及各國之行動計畫、指引、法規、監測制度及政策評論。
10. **我國法規、權責與國內研究基礎**：我國相關法規（水污染防治法、毒性及關注化學物質管理法、環境用藥管理法、動物用藥品管理法、藥事法等）、國家行動計畫、跨部會權責文件，以及國內環境抗生素與抗藥性研究、計畫或白皮書。
