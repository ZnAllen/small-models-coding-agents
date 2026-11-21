---
title: "小模型的記憶與上下文管理策略"
slug: memory-context-small-models
category: concept
tags: [memory, context-management, small-language-model, toolrag, sliding-window]
summary: "探討小模型在多步 coding agent 任務中面臨的上下文窗口限制，以及 ToolRAG、記憶壓縮、滑動視窗等上下文管理策略的設計與實作要點。"
confidence: medium
created: 2026-11-21
last_updated: 2026-11-21
see_also: [[small-model-tool-calling-problems]]
---

# 小模型的記憶與上下文管理策略

小模型（特別是 <3B）在 multi-turn coding agent 任務中，最容易遇到的瓶頸不是單一推理能力，而是上下文窗口的快速耗盡與累积噪訊。本節深入探討此問題的成因與解決框架。

## 一、問題的根源

### 1.1 Token 消耗結構

一次典型的 coding agent turn 包含：
```
System Prompt    ≈ 500–2000 tokens  (工具定義、角色設定)
User Query       ≈ 50–500 tokens
History Buffer   ≈ 1000–5000 tokens (累積的 tool calls + outputs)
Response         ≈ 200–1000 tokens
─────────────────────────────────
Total per turn   ≈ 1800–8500 tokens
```

對於 4K context window 的 1B 模型，僅 1–2 步就會觸發截斷。

### 1.2 小模型的 context utilization 劣勢

根據 [[arXiv:2509.04518|Advancing SLM Tool-Use via RL]] 的研究：
- SLM 難以從冗長上下文中有效提取關鍵資訊（attention dilution）
- 超过 3–4 輪 conversation 後，性能急劇下降
- 历史訊息中的非相關內容會嚴重干擾當前決策

## 二、ToolRAG：動態工具檢索

### 2.1 核心思想
不再一次性將所有工具定義放入 prompt，而是根據當前 query 即時檢索相關的 tool definitions。

### 2.2 流程
```
User Query
    ↓
Embedding (轻量级 encoder)
    ↓
Vector DB Search (top-K 相似 tool definitions)
    ↓
Minimal Prompt = System + Top-K Tools + Current Query
    ↓
SLM generates response / tool call
```

### 2.3 效果
根據 [[arXiv:2409.00608|TinyAgent]]：
- 輸入長度減少 60% 以上
- 在 40+ 工具的 Siri-like 場景中保持可接受的 recall

### 2.4 實作要點
- **Embedding 模型選擇**：可使用 100M–300M 的小型 embedder（如 MiniLM）
- **K 值調整**：通常 K=3–5 為最佳平衡點
- **重新排序**：二次檢索（reciprocal rank fusion）可提升相關性

## 三、記憶壓縮與摘要

### 3.1 定期摘要（Periodic Summarization）
- 每 N 步對話後，觸發一次摘要生成
- 使用專門的 summarization prompt：「請總結過去 N 步的關鍵決定與結果」
- 保留：已完成的目標、關鍵發現、當前狀態
- 捨棄：中間嘗試的細節、已失敗的路徑

### 3.2 事件驅動摘要
- 只在重要事件後觸發摘要（如：工具呼叫失敗、目標達成）
- 減少不必要的摘要 overhead

### 3.3 向量化記憶（Vector Memory）
- 將每個 turn 的 key insights 存為向量
- 查詢時檢索最相關的歷史片段
- 類似 RAG，但用於自身 history 而非外部 knowledge

## 四、滑動視窗與 Context Isolation

### 4.1 滑動視窗策略
```
Turn 1: [sys][q1][r1]        → 完整保留
Turn 2: [sys][q1][r1][q2][r2] → 完整保留
Turn 3: [sys][...][q3][r3]    → 滾動移除最早的非關鍵片段
Turn 4: [sys][summary_1-3][q4][r4] → 替換為摘要
```

### 4.2 Plan-Execute 隔離
根據 [[arXiv:2608.22472|Small Reasoning Models are Instruction Followers]]：
- Planning phase 使用獨立 memory buffer
- Execution phase 每次只載入當前 step 所需的 context
- 避免 plan context 洩漏到 execution context

### 4.3 分段上下文（Segmented Context）
- 將 long-horizon 任務分割為 sub-goals
- 每個 sub-goal 有獨立 context window
- Sub-goal 之間通過 compact handoff messages 傳遞狀態

## 五、Context Window 的硬件考慮

| 模型級距 | 典型 Context Window | 建議部署策略 |
|---------|-------------------|------------|
| ≤1B | 2K–4K tokens | 必須配合 ToolRAG + 頻繁摘要 |
| 3B | 4K–8K tokens | ToolRAG 足以應對中等複雜度任務 |
| 7B | 8K–32K tokens | 可容納 10+ 步對話，但仍需策略性管理 |
| 7B+ (MoE) | 32K–128K tokens | 需評估 costs vs. performance tradeoff |

## 六、實戰建議

1. **起步策略**：先用 ToolRAG 將 prompt 控制在 1500 tokens 以內
2. **監控指標**：追蹤每次 turn 的實際 token 消耗與 context 利用率
3. **觸發閾值**：設定 context usage >70% 時自動觸發摘要
4. **fallback 機制**：當 context 耗盡且無法摘要時，回退到 plan-execute decoupling 模式

## 七、待研究方向

- 自適應 context window sizing（根據任務難度動態調整）
- 小模型自 summarization（而非依賴大模型生成摘要）
- Cross-turn attention compression 技術