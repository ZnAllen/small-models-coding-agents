---
title: "小模型能力邊界與選擇指南"
slug: model-capabilities-boundaries
category: concept
tags: [small-language-model, model-selection, capabilities, sizing]
summary: "系統性梳理不同參數規模小模型的能力邊界，提供按任務類型選擇合適模型的實用指南，涵蓋 0.5B 至 7B 各級距模型在程式碼生成、工具呼叫、推理等維度的表現差異。"
confidence: medium-high
created: 2026-11-21
last_updated: 2026-11-21
see_also: [[small-model-code-gen-benchmarks]]
---

# 小模型能力邊界與選擇指南

小型語言模型（Small Language Models, SLMs）的能力並非線性縮放——不同參數規模在不同任務類型上有著顯著的能力斷層。理解這些邊界是建構有效 Coding Agent 的第一步。

## 一、參數規模分類

| 級距 | 代表模型 | 典型部署情境 | 適用任務類型 |
|------|---------|-------------|-------------|
| **極微型**（≤1B） | Qwen2.5-Coder-0.5B, DeepSeek-Coder-1.3B | 邊緣裝置、手機端 | 單一指令、簡單 API 呼叫、領域專精任務 |
| **小型**（1B–3B） | DeepSeek-Coder-1.3B, Phi-3-mini, LLaMA-3.2-3B | 筆記型電腦、嵌入式 | 基礎程式碼生成、單步工具呼叫 |
| **中型**（3B–7B） | Qwen2.5-Coder-3B/7B, Mistral-7B, StarCoder2-3B/7B | 雲端推論、效能工作站 | 多步推理、嵌套工具呼叫、較複雜程式碼生成 |
| **大型小型**（7B–8B） | LLaMA-3.1-8B, Qwen2.5-Coder-7B | 多數雲端部署 | 接近大模型水準的工具呼叫、多步編程 agent |

## 二、各維度能力分析

### 2.1 程式碼生成（Code Generation）

根據 [[arXiv:2507.03160|Assessing SLMs for Code Gen]] 及 HumanEval Pass@1 數據：

| 級距 | HumanEval Pass@1 | MBPP | 說明 |
|------|-----------------|------|------|
| 0.5B | ~0.40 | ~0.45 | 僅限極簡函式；需領域 fine-tune |
| 1.3B | ~0.33 | ~0.60 | DeepSeek-Coder 系列在此級距表現突出 |
| 3B | ~0.57–0.72 | ~0.65–0.79 | StarCoder2-3B 和 Qwen2.5-Coder-3B 為主流選擇 |
| 7B | ~0.82–0.87 | ~0.82 | Qwen2.5-Coder-7B 目前 SOTA，HumanEval 達 87.2% |

**關鍵觀察**：3B→7B 的性能提升幅度（~30pp HumanEval）遠大於 0.5B→3B（~20pp），顯示 7B 是目前實用的最低門檻。

### 2.2 工具呼叫（Function Calling）

根據 [[arXiv:2504.19277|Small Models, Big Tasks]] 與 BFCL-v4 數據：

| 級距 | 基本 FC | 多輪 FC | 嵌套 AST | 說明 |
|------|--------|--------|---------|------|
| ≤1B | 差（需 SFT） | 極差 | 幾乎不可能 | 即使 fine-tuned 也多步失敗 |
| 3B | 可接受（SFT 後） | 中等 | 差 | 需要 ToolRAG 輔助 |
| 7B | 良好（SFT 後） | 中等偏下 | 中等 | TinyAgent-7B 可超越 GPT-4-turbo 於特定 eval |

**核心問題**（詳見 [[小模型工具呼叫的核心問題]]）：
- P1 格式遵循困難：所有級距皆受影响，但 7B 較好
- P2 工具選擇錯誤：3B 以下尤甚
- P3 多步驟失敗：1B 模型在多輪後崩潰機率 >60%

### 2.3 規劃與推理（Planning & Reasoning）

| 級距 | 簡單規劃（2–3 步） | 複雜規劃（5+ 步） | 自我修正 |
|------|------------------|-----------------|---------|
| ≤1B | 偶爾成功 | 極低成功率 | 難以實現 |
| 3B | 需引導提示 | 部分可用 | 有限 |
| 7B | 可執行 | 中等成功率 | 基本可行 |

## 三、架構設計建議

### 3.1 單一模型方案

適合規模 ≤3B 的場景，但需配合以下優化：
- 強制使用 [[prompt-engineering|結構化提示工程]]
- 搭配 ToolRAG 減少上下文負擔
- 使用guided decoding 確保輸出格式正確

### 3.2 Plan-Execute 分離方案

針對 3B 以下模型（詳見 [[hybrid-systems]]）：
- SRM（小型推理模型，1–2B）負責解析意圖與生成計劃
- 主 SLM 專注於每步執行
- 此模式可彌補小模型多步規劃能力的不足

### 3.3 集成的方案（Ensemble）

根據 [[arXiv:2602.11911|Team of SLMs]]，10× DeepSeek-Coder-1B 團隊在程式碼生成上超越了單一 DeepSeek-Coder-33B，證明小模型集合策略的有效性。

## 四、任務－模型配對決策表

| 任務類型 | 推薦最小尺寸 | 必要技術 | 預期成功率 |
|---------|-------------|---------|-----------|
| 單一工具呼叫（已知 schema） | 1.5B | SFT + guided decoding | 70–80% |
| 多輪工具協調（3–5 步） | 7B 或 hybrid | ToolRAG + plan-decoupling | 40–60% |
| 單一函式程式碼生成 | 3B | Instruct tuning | 60–75% |
| 多文件 refactoring | 7B | SFT + RAG | 30–50% |
| 倉庫級 coding agent | 7B+ 或 MoE | 完整 framework + 長上下文 | <30%（當前 SOTA） |
| Edge 部署（終端裝置） | ≤1.1B | TinyAgent SFT + quantization | 侷限於指定領域 |

## 五、總結

小模型的「可用尺寸」取決於任務複雜度而非絕對參數量。實務上：
- **<3B** 應視為「專才」（domain-specific, single-step tasks）
- **3–7B** 是「通用 Coding Agent」的合理起點
- **>7B** 開始接近「實際可用」的 agent 能力，但仍需架構上的協助

未來趨勢（Qwen3 MoE、Devstral 2 Small 等新兴模型）正持續推高此邊界。