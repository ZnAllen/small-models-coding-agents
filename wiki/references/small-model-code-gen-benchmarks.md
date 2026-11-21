---
title: "小模型 Coding Agent Benchmark 數據總覽"
slug: small-model-code-gen-benchmarks
category: reference
tags: [benchmark, coding-agent, code-generation, evaluation]
summary: "彙整 sub-7B 模型於 Code Generation、Function Calling、Coding Agent 三大維度的 Benchmark 數據與排名"
confidence: medium-high
created: 2026-11-21
last_updated: 2026-11-21
see_also: [[small-model-tool-calling-problems]]
---

# 小模型 Coding Agent Benchmark 數據總覽

本頁彙整 2024–2026 年間發表的關鍵 benchmark 結果，聚焦於 <7B 參數模型在三個維度：Code Generation、Function Calling、Coding Agent。

## 一、Code Generation Benchmarks

### HumanEval Pass@1 (Greedy Decode)

| Model | Params | HumanEval | MBPP | EvalPlus | 備註 |
|-------|--------|-----------|------|----------|------|
| Qwen2.5-Coder-7B-Instruct | 7B | **0.872** | 0.823 | 0.823 | SOTA 7B class |
| Qwen2.5-Coder-3B | 3B | 0.720 | 0.790 | 0.740 | 3B 級距強者 |
| DeepSeek-Coder-6.7B | 6.7B | 0.700 | 0.760 | 0.670 | MoE 架構 (2.4B active) |
| DeepSeek-Coder-1.3B | 1.3B | 0.326 | ~0.600 | - | 1B 級距 Top |
| StarCoder2-7B | 7B | 0.420 | 0.710 | 0.560 | 基礎模型非 instruct |
| StarCoder2-3B | 3B | 0.570 | ~0.650 | - | - |
| CodeLlama-7B | 7B | 0.380 | 0.640 | - | Llama-2 base |
| Phi-3-mini | 3.8B | ~0.550 | ~0.680 | - | Microsoft 2023 |
| LLaMA-3.2-3B | 3B | ~0.450 | ~0.600 | - | Multimodal variant |
| Gemma-2B | 2B | ~0.300 | ~0.550 | - | Google 2024 |
| Qwen2.5-Coder-0.5B | 0.5B | 0.400 | ~0.450 | - | 極小但可用 |

資料來源：[[arXiv:2507.03160|Assessing SLMs for Code Gen]], [[arXiv:2509.10436|RefactorCoderQA]], official model cards

### 關鍵觀察
1. **Qwen2.5-Coder 系列**在同等尺寸下幾乎全面領先，7B 版 HumanEval 達 87.2%
2. **DeepSeek-Coder** MoE 架構使 6.7B 模型僅使用 2.4B active params 即達 70% HumanEval
3. **0.5B–1.5B** 級距在特定領域任務（如 LoRaWAN engineering）仍有競爭力 ([[arXiv:2502.14926]])
4. SLM team ensembles ([[arXiv:2602.11911]]) 可超越單一更大模型：10× DeepSeek-Coder-1B > DeepSeek-Coder-33B

## 二、Function Calling Benchmarks

### Berkeley Function Calling Leaderboard (BFCL v4) — 代表值

| Model | Overall Acc | Serial FC | Parallel FC | Multi-turn | AST Complex |
|-------|-------------|-----------|-------------|------------|-------------|
| GPT-4 (reference) | ~72 | strong | strong | moderate | strong |
| LLaMA-3.1-8B-Instruct | ~68 | good | moderate | weak | poor |
| Mistral-7B-Instruct-v0.3 | ~66.5 | good | moderate | poor | poor |
| DeepSeek-Coder-6.7B | ~71 | strong | strong | moderate | fair |
| TinyAgent-7B (SFT) | >GPT-4-turbo* | - | - | - | - |
| TinyAgent-1.1B (SFT) | near SOTA** | - | - | - | - |

\* Erdogan et al. 2024 在自訂 Siri-like eval 上的報告；\*\* edge-device specific tasks

### 關鍵發現
- **LLaMA-3-8b-instruct 在簡單類別接近 GPT-4**（Databricks BFCL analysis），但在 nested/AST-complex 類別差距大
- **最佳訓練量約 60K samples**（Microsoft 實驗）；更多數據不再改善
- **RL post-training** 可有效提升但 training instability 是主要挑戰 ([[arXiv:2509.04518]])
- **Template Training** ([[arXiv:2509.18076]])：Mistral-7B Nexus 分數 20.55→23.17

## 三、Coding Agent Benchmarks

### SWE-bench Verified / Lite

| System | Setting | Pass Rate | Notes |
|--------|---------|-----------|-------|
| GPT-4 (mini-SWE-agent) | Standard ReAct | ~28% | Baseline |
| gpt-5 (SWE-EVO) | Long-horizon evolution | 21% | Much lower than Verified 65% — gap reveals benchmark leakage |
| Qwen3 Coder 30A-3B (MoE) | Dialogue-SWEBench | TBD | Emerging small MoE agent |
| Devstral 2 Small (3B) | Dialogue-SWEBench | TBD | Mistral specialized |

資料來源：[[arXiv:2512.18470|SWE-EVO]], [[arXiv:2606.13995|Dialogue-SWEBench]], OpenAI SWE-bench Verified release

### 其他 Coding Agent Benchmarks

| Benchmark | Year | Focus | Notable Result |
|-----------|------|-------|----------------|
| **OmniCode** ([[arXiv:2602.02262]]) | 2026 | Real PR fix tasks | Even SOTA models struggle on complex repo-level |
| **TensorBench** ([[arXiv:2606.05570]]) | 2026 | PyTorch extension, 199 tasks | Inter-agent agreement κ=0.05 — task-specific skill gaps |
| **TOBench** ([[arXiv:2605.16909]]) | 2026 | 100 executable tasks, 27 MCP servers, 324 tools | First closed-loop multimodal tool benchmark |
| **ProjectEval** (ACL 2025) | 2025 | Full project building | OpenHands fails 8/20 tasks; missing optional features |
| **Saving SWE-Bench** ([[arXiv:2510.08996]]) | 2025 | Mutation-based evaluation | Existing benchmarks may overestimate by 10-20pp |

## 四、總結建議

| 任務類型 | 推薦最小模型尺寸 | 預期表現 | 必要優化手段 |
|---------|-----------------|---------|-------------|
| 單一 function call | 1.5B–3B | Fair-Good | SFT + guided decoding |
| Nested / multi-step FC | 6B–7B | Moderate | ToolRAG + plan-decoupling |
| Code generation (standalone) | 3B–7B | Good (Qwen2.5-Coder) | Instruct-tuned base |
| Repository-level coding agent | 7B+ or MoE | Limited (current SOTA) | RAG + long-horizon planning |
| Edge deployment | ≤1.1B | Narrow domain only | TinyAgent-style SFT + quantization |
