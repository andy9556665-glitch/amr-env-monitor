# PubMed 布林檢索式（每月文獻精選用）

共用區塊：

```
E = (wastewater[tiab] OR sewage[tiab] OR effluent*[tiab] OR sludge[tiab] OR river*[tiab]
     OR "surface water"[tiab] OR sediment*[tiab] OR soil[tiab] OR manure[tiab]
     OR aquaculture[tiab] OR livestock[tiab])
A = ("antibiotic resistance gene*"[tiab] OR ARGs[tiab] OR "antimicrobial resistance"[tiab]
     OR resistome[tiab] OR intI1[tiab])
```

| 類別 | 檢索式 |
|---|---|
| 1 化學檢測技術 | `(antibiotic*[tiab] OR pharmaceutical*[tiab]) AND ("LC-MS/MS"[tiab] OR HRMS[tiab] OR "non-target"[tiab] OR "suspect screening"[tiab] OR "solid-phase extraction"[tiab]) AND E` |
| 2 生物檢測技術 | `A AND (qPCR[tiab] OR "high-throughput qPCR"[tiab] OR "digital PCR"[tiab] OR metagenom*[tiab] OR "whole-genome sequencing"[tiab]) AND (method*[tiab] OR validation[tiab] OR comparison[tiab]) AND E` |
| 3 ARGs 機制 | `A AND ("horizontal gene transfer"[tiab] OR conjugat*[tiab] OR plasmid*[tiab] OR integron*[tiab] OR "mobile genetic element*"[tiab]) AND E` |
| 4 非抗生素共選 | `A AND (co-selection[tiab] OR "cross-resistance"[tiab] OR "quaternary ammonium"[tiab] OR disinfectant*[tiab] OR biocide*[tiab] OR "heavy metal*"[tiab] OR microplastic*[tiab])` |
| 5 環境殘留與來源 | `antibiotic*[tiab] AND (occurrence[tiab] OR residue*[tiab] OR fate[tiab] OR "source apportionment"[tiab]) AND E` |
| 6 ARGs 監測與指標 | `A AND (surveillance[tiab] OR monitoring[tiab] OR "wastewater-based"[tiab] OR indicator*[tiab]) AND E` |
| 7 污水處理與削減 | `A AND (removal[tiab] OR disinfection[tiab] OR ozonation[tiab] OR "advanced oxidation"[tiab] OR membrane*[tiab] OR "anaerobic digestion"[tiab]) AND (wastewater[tiab] OR sludge[tiab] OR manure[tiab])` |
| 8 風險評估與預警 | `A AND ("risk assessment"[tiab] OR "risk ranking"[tiab] OR prioriti*[tiab] OR "early warning"[tiab] OR threshold*[tiab] OR sentinel[tiab]) AND E` |
| 9 國際公告與政策 | `(A OR antibiotic*[tiab]) AND (policy[tiab] OR policies[tiab] OR regulation*[tiab] OR guideline*[tiab] OR governance[tiab] OR "action plan"[tiab] OR legislation[tiab]) AND E` |
| 10 我國法規與研究 | `(A OR antibiotic*[tiab]) AND Taiwan[tiab]` |

日期條件（E-utilities esearch 參數）：`datetype=crdt&mindate=上月/25&maxdate=本月/24`

## 每月精選規則

1. 10 類各自檢索，排除回顧、社論、預印本與環境面無關者。
2. 每類挑 3–8 篇最相關者；必須確認 efetch 取得的摘要可閱讀，讀不到摘要或無法撰寫摘要者放棄、改選下一篇。該類當月沒有合適新文獻就略過。
3. 同一篇命中多類時，只收錄一次，歸入最主要的類別。
4. 當月合計目標 8–20 篇；最少 6 篇、最多 25 篇。超過 25 篇時，優先保留與我國管理需求最相關者。
5. 選定但 AI 無法撰寫中文摘要的文獻（含撰寫被中斷者），改依 TEAM_WORKFLOW.md 寫入試算表、狀態「待撰寫」，由團隊撰寫，不放上網站。這類文獻仍計入當月篇數。
6. 分段存檔：每完成一個類別，就先把該類寫入 items.json 並 commit、push，同時寫入試算表，不要等全部完成才存。
