---
title: "大小模型協同架構設計"
slug: hybrid-systems-small-large-cooperation
category: topic
tags: [hybrid-systems, plan-execute, small-language-model, large-language-model, architecture]
summary: "探討如何設計大小模型協同的 Coding Agent 架構，包括 Plan-Execute 分離模式、multi-agent ensemble 策略、以及具體的實作框架與效能權衡。"
confidence: medium
created: 2026-11-21
last_updated: 2026-11-21
see_also: [[small-model-tool-calling-problems]], [[model-capabilities]]
---

# 大小模型協同架構設計

純小模型方案在複雜任務上存在根本性的能力限制，而純大模型方案又面臨成本與 latency 的問題。大小模型協同（Hybrid Systems）是當前最務實的折衷途徑。

## 一、Plan-Execute 解耦架構

### 1.1 核心概念
根據 [[arXiv:2608.22472|Small Reasoning Models are Instruction Followers]]，sub-3B 的小模型在本質上是優秀的「指令Following者」而非「規劃者」。因此可以：
- 用小型推理模型（SRM, Small Reasoning Model）負責 parsing tool definition 與生成執行計劃
- 主 SLM 專注於逐階段執行程設定的邏輯

### 1.2 架構示意
```
┌─────────────────────────────────────────────┐
│              User Request                    │
└──────────────────┬──────────────────────────┘
                   ↓
┌─────────────────────────────────────────────┐
│          Planner (SRM, 1-2B)                 │
│  - Parse user intent                         │
│  - Generate step-by-step tool sequence       │
│  - Identify required parameters              │
└──────────────────┬──────────────────────────┘
                   ↓  (计划作为结构化数据)
┌─────────────────────────────────────────────┐
│         Executor Loop (Main SLM, 3-7B)      │
│                                              │
│  For each step in plan:                      │
│    ├── Load isolated context (no pollution) │
│    ├── Execute tool call                     │
│    ├── Capture output                        │
│    └── Store result in memory buffer         │
│                                              │
│  Check completion condition                 │
└──────────────────┬──────────────────────────┘
                   ↓
            Final Response
```

### 1.3 優勢
- **Context 隔離**：每步執行只有必要的最小 context，避免累积噪訊
- **角色專精**：SRM 不需要具備強大的 code gen 能力，只需理解 tool semantics
- **容錯性**：單步失敗不易影響全局，可 retry specific steps

### 1.4 挑戰
- Plan 品質直接決定整體上限
- 兩階段架構增加約 30–50% 的 latency
- SRM 的訓練需要額外的 quality control

## 二、Multi-Agent Ensemble 策略

### 2.1 團隊協作模式
根據 [[arXiv:2602.11911|Team of SLMs]]，多个小模型協作可以超越單一更大的模型：
- 10 × DeepSeek-Coder-1B 團隊 > DeepSeek-Coder-33B 單獨運作
- Qwen/Phi/Gemma 不同家族的 SLM team 同樣勝過單一 LLM

### 2.2 路由與投票機制
```
Input Task
    ↓
┌──────────────────────────────────────┐
│           Router (Heuristic / Small) │
│  - Analyze task type                  │
│  - Route to appropriate agent(s)     │
└──────────┬──────────────┬────────────┘
           ↓              ↓
    ┌──────────┐    ┌──────────┐
    │ Agent A  │    │ Agent B  │
    │(DeepSeek)│    │ (Qwen)   │
    └────┬─────┘    └────┬─────┘
         ↓               ↓
    ┌─────────────────────────────┐
    │      Aggregator / Voter     │
    │  - Majority vote            │
    │  - Quality-weighted merge   │
    │  - Self-consistency check   │
    └────────────┬────────────────┘
                 ↓
           Final Output
```

### 2.3 適用場景
- **Code generation**：多模型 voting 提高 correctness
- **Tool selection**：不同模型可能有不同的 tool preference patterns
- **Debugging**：ensemble 診斷比單模型更可靠

## 三、具體實作框架

### 3.1 TinyAgent Framework（參考 [[arXiv:2409.00608]]）
- LLMCompiler：支援 multi-stage tool orchestration
- ToolRAG：動態工具檢索
- 適用於 edge deployment 的完整 stack

### 3.2 自研 hybrid 架構的關鍵組件
1. **Task Classifier**（轻量级，<500M params）：判斷任務複雜度，決定使用 pure SLM 還是 delegated to larger model
2. **Plan Generator**（2–4B）：生成 structured execution plan
3. **Step Executor**（3–7B）：執行單步操作
4. **Memory Manager**：管理跨 step 的狀態流轉
5. **Error Recovery Module**：處理執行失敗與迴路重試

## 四、效能與成本的權衡

| 架構 | Latency | Cost | Accuracy | 適用規模 |
|------|---------|------|----------|---------|
| Pure SLM（7B） | 低 | 低 | 中等 | 簡單任務 |
| Plan-Execute（1B+7B） | 中高 | 中 | 高 | 中等複雜度 |
| Multi-Agent Ensemble（×10 1B） | 高（parallel） | 中 | 最高 | 關鍵任務 |
| Delegation（SLM→LLM fallback） | 可變 | 最低 | 可選 | 混合 workload |

## 五、未來方向

- **AutoML for hybrid sizing**：自動尋找最佳的 SLM + LLM 組合比例
- **Dynamic routing**：基於 runtime feedback 動態切換架構
- **Unified training**：同一模型同時優化 planning 與 execution 能力