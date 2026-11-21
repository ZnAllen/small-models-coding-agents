# 作業報告：用 LLM Wiki 製作教學 HTML 簡報

**學校：** __________　**學號：** __________　**系級：** __________　**姓名：** __________
**主題：小模型做 Coding Agent（Small Language Models as Coding Agents）**

---

## 步驟一：LLM Wiki 建立

### 主題與簡介

本作業建立的 LLM Wiki 主題為「**小模型做 Coding Agent 的問題及解決方式**」（slug: `small-models-coding-agents`）。內容聚焦於：小型語言模型（Small Language Models, SLMs；本作業以 7B 參數以下為範圍）在程式碼生成與工具呼叫任務中的能力、常見失敗原因，以及可行的改善策略。

建置時先討論並決定三項規格：以繁體中文撰寫、以原始論文摘要作為 raw 資料、將內容分成能力邊界／工具呼叫問題／提示工程／上下文管理／優化與協同架構等子題；確認範圍後才進行資料攝取與 Wiki 編譯。這樣可避免先生成大量內容後，才發現主題範圍或資料格式不符合需求。

### 資料攝取紀錄（Ingest）

共 ingest **19 篇資料來源**：17 篇 arXiv 論文與 2 篇非 arXiv 來源（Microsoft 技術文章、ACL 2025 proceedings）。各來源的 metadata 與原始摘要／來源內容存入 `raw/papers/`；Wiki 文章則由這些 raw 資料整理、交叉比對後編譯而成。

| 類別 | 數量 | 說明 |
|------|------|------|
| Function Calling 相關資料 | 8 篇 | 工具呼叫、提示模板與 RL 後訓練 |
| Code Generation 相關資料 | 4 篇 | 小模型程式碼生成與多模型協作 |
| Coding Agent Benchmark 相關資料 | 5 篇 | 長程軟體工程、PR 修正與多模態評測 |
| 技術部落格 / 會議論文 | 2 篇 | Microsoft blog、ACL 2025 proceedings |

### Wiki 成果

- **編譯文章**：7 篇（5 concept + 1 topic + 1 reference），全繁體中文撰寫
- **原始資料**：19 篇 raw paper 檔，含 metadata（作者、venue、DOI／來源連結與原始摘要或來源內容）
- **索引同步**：`_index.md`、`raw/_index.md`、`wiki/_index.md` 三處完全一致

**Wiki 路徑：** `E:\LLM-wiki\.wiki\topics\small-models-coding-agents\`

---

## 步驟二：教學 HTML 簡報

### 簡報檔案
`E:\LLM-wiki\.wiki\topics\small-models-coding-agents\output\slm-coding-agent-presentation.html`

### 簡報結構（11 頁，面向零基礎讀者）

| # | 標題 | Wiki 來源 |
|---|------|----------|
| 1 | 封面：小模型做 Coding Agent | — |
| 2 | 什麼是 SLM？（定義／優點／挑戰） | model-capabilities.md §一 |
| 3 | 六大核心問題 P1–P6 | small-model-tool-calling-problems.md |
| 4 | 不同尺寸 SLM 能力對照表 | benchmarks.md + model-capabilities.md §二 |
| 5 | 解法一：提示工程策略 | prompt-engineering.md |
| 6 | 解法二：Plan-Execute 解耦架構 | hybrid-systems.md §一 |
| 7 | 解法三：ToolRAG 與上下文管理 | memory-context.md |
| 8 | 解法四：量化、剪枝、RL 後訓練 | optimization.md |
| 9 | 任務－模型配對決策表 | model-capabilities.md §四 |
| 10 | 課程重點回顧 | 全篇綜合 |
| 11 | Q&A 提問時間 | — |

### 設計要點
- **框架**：Reveal.js v4.5.0（單一 HTML 檔，瀏覽器直接開啟即可播放）
- **字型**：Google Fonts Noto Sans TC（全繁體中文支援）
- **導航**：方向鍵 / 空白鍵 / 滑鼠點擊均可切換投影片
- **可追溯性**：每頁底部皆有 source note，點擊可直接開啟對應 wiki 文章
- **垂直置中**：所有頁面內容在視窗內上下置中，無溢出問題
- **全繁體字**：經子 Agent 驗證全文無簡體字殘留

---

## 步驟三：Loop / Graph 流程說明

### 整體 Workflow 圖

```
┌──────────────────────────────────────────────────────────────┐
│                    LLM Wiki × 簡報產生 Pipeline              │
└──────────────────────────────────────────────────────────────┘

  [Node 1] 取得資料 (Ingest)
       │  webfetch 逐頁抓取 arXiv abstract 原文
       ▼
  [Node 2] 整理知識庫 (Compile) ──────────────┐
       │   raw/papers/*.md  →  wiki/concepts/ │ 反饋循環
       │   wiki/topics/     →  wiki/references/│ 若素材不足→回到此節點
       │   index 同步更新    ↑                 │
       ▼                        │              │
  [Node 3] 提取重點 ────────────┘              │
       │   從 7 篇文章中 extract 簡報素材       │
       │   （六大問題、benchmark 數據、架構圖） │
       ▼                                      │
  [Node 4] 檢查內容 ════ feedback loop ════════┘
       │   事實準確？　引用完整？　語言一致？　slug 存在？
       │        ↓ 不通過
       │   → 回到 Node 3 重新提取
       │        ↓ 通過
  [Node 5] 產出簡報（HTML + Reveal.js）
       │
  [Node 6] 子 Agent 理解度驗證
       │   僅憑簡報回答 5 道問題，評估是否足以教學
       │        ↓ 有缺失 → 回到 Node 3 補充
       ▼
  [Node 7] 輸出最終簡報 + 產出本報告文件
```

### 各節點說明

| Node | 動作 | 輸入 | 輸出 | 重複頻率 |
|------|------|------|------|---------|
| 1 | webfetch 抓取 arXiv abstract 原文 | URL | `raw/papers/*.md` | 一次（新增論文時） |
| 2 | 編譯 wiki 文章 | raw papers | wiki articles + indexes | 按需（topic 擴充時） |
| 3 | 從 wiki 提取簡報素材 | wiki articles | slide draft | **是**（多次調整） |
| 4 | 逐頁檢查 consistency | slide draft | 檢核結果 | **是**（與 Node 3 形成 loop） |
| 5 | 生成 HTML 簡報 | approved content | presentation.html | 一次（定稿版） |
| 6 | 子 Agent 理解度驗證 | 簡報文字稿 | 正確率評估 | 必要時重跑 |
| 7 | 產生作業報告 | 全程紀錄 | assignment-report.md | 一次 |

### 哪些步驟會重複執行？

1. **Node 3 ↔ Node 4**（最頻繁）：每次檢查發現問題（如 slug 拼錯、missing source、簡體字殘留）即回到 Node 3 修正後重新檢查。本次執行中共經歷 3 輪 loop。
2. **Node 2 ↔ Node 3**（較少）：若發現某個 slide 缺乏足夠素材，需回頭編譯新的 wiki 文章以補充內容。
3. **Node 6 驗證失敗時**：若子 Agent 對某個概念出現錯誤理解，返回 Node 3 補強該 slide 的描述清晰度。

### 為什麼這樣設計？

1. **wiki 是單一字 source of truth**：簡報所有內容源自 wiki，不直接讀 paper PDF，確保跨頁一致性
2. **分開 Extract 與 Check**：提取時不受檢查標準干擾；獨立稽核可避免遺漏
3. **Node 6 子 Agent 驗證**：強制以最嚴苛的方式檢驗簡報的完整性——如果一個不了解主題的人無法從簡報理解，就代表需要改進
4. **每頁附 source note**：觀眾可追溯每一個 claim 到原始 wiki 文章，符合學術誠信要求

---

## 步驟四：反思

### 哪個節點最常出錯？

**Node 2（編譯階段）** 是最常出問題的節點，共發生兩類失誤：

**失誤一：違反 Raw is immutable 原則**
- 初版 `raw/papers/` 的 19 個檔案是我自己撰寫的英文摘要，而非論文的原始 abstract
- 使用者指出此一問題後，必須全部推倒重來，改用 webfetch 逐頁抓取 arXiv 網頁上的 original abstract text 覆蓋原檔
- 這是整個作業中最嚴重的失誤，造成約 1 小時的重工

**失誤二：Cross-reference slug 拼寫不一致**
- 例如 `small-model-code-gen-benchmarks` vs `small-model-code-gen-benchmark`（少了一個 s）
- 導致 wiki 內部的 see_also 鏈結失效，需要手動修復

### 把檢查往前移到了哪一個步驟？

在第一次失誤後，將檢查點前置到 **Node 2（編譯之前）**，新增三道守門機制：

1. **Check-before-write 規則**：每次編譯新文章前，先讀取現有的 `_index.md`，確認 slug 不存在衝突或拼寫錯誤
2. **原文比對**：webfetch 取得 arXiv abstract 後，立即核對 title 和 authors 是否與目標一致，才寫入 raw/papers/
3. **Index 一致性檢查**：建立 `_index.md` 時同步做一次 file count vs row count 的一致性比對，發現不符立即修復

### 有沒有因此修改 workflow / graph？

是的，作了三項具體調整：

1. **新增 Node 6（子 Agent 驗證節點）**：在產出簡報後增加一道「外部理解度測試」，讓另一個人工智慧只靠簡報內容回答問題，確認教學效果
2. **Raw papers 生成規則修改**：在 workflow 中明確定義 raw/ 目錄只能存放從外部來源抓取的原文，禁止 AI 自行生成摘要存入
3. **Log.md 追蹤強化**：所有重大修正（如 raw papers 重建）均在活動日誌中明確記載，方便事後追溯

這些調整使後續的 Node 3（提取重點）和 Node 4（檢查）變得更加順利，因為基礎資料的品質已經在第一關就被把關住了，避免了「垃圾進、垃圾出」的問題。

---

## 附錄：簡報內容理解度驗證結果（Node 6 執行情形）

已使用 general 子 Agent，僅憑簡報文字稿回答 5 道開放式問題：

| 問題 | 回答品質 | 判斷 |
|------|---------|------|
| Q1：SLM 定義與優缺點 | 完整列出 4 項優點 + 3 項缺點，全部對應 Slide 2 | ✅ 通過 |
| Q2：六大核心問題 P1–P6 | 精準對應 Slide 3 每一條，含關鍵數據（如 <40%） | ✅ 通過 |
| Q3：1B 模型多輪協調方案 | 正確指出簡報說需 7B 或 hybrid，不建議純 1B，並引用 Slide 9 表格 | ✅ 通過（且態度謹慎不臆測） |
| Q4：Plan-Execute 原理 | 清楚解釋 SRM Planner + Main SLM Executor 分工，列出三大優勢 | ✅ 通過 |
| Q5：邊緣裝置部署推薦 | 綜合 Slide 4/8/9 給出完整方案（TinyAgent SFT + 量化 + 引導解碼） | ✅ 通過 |

**結論：子 Agent 完全憑藉簡報內容即可正確理解整個主題，且在第 3 題主動指出簡報中的能力限制，未引入外部假設。**
