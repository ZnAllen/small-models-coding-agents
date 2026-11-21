---
title: "Small Models for Coding Agent - Research Summary 2025-2026"
type: paper-summary
tags: [coding-agent, small-language-model, function-calling, benchmark]
date: 2026-11-21
source: web-search-compilation
summary: "彙整 2024-2026 年間關於小模型（sub-7B）在 Coding Agent / Tool Use 領域的關鍵研究與 Benchmark 數據"
url: "compilation-web-search-2026-11-21"
---

# Small Models for Coding Agent — 研究摘要

## 一、核心論文清單（2024-2026）

### 功能呼叫 (Function Calling)

| #   | 標題                                                                                                                | 年份         | 參數           | 關鍵發現                                                                                                                                |
| --- | ----------------------------------------------------------------------------------------------------------------- | ---------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **TinyAgent: Function Calling at the Edge** (arXiv:2409.00608)                                                    | EMNLP 2024 | 1.1B, 7B     | SFT + LoRA on 40K curated data → TinyAgent-7B surpasses GPT-4-turpo on function calling; introduces LLMCompiler framework + ToolRAG |
| 2   | **Small Models, Big Tasks: Empirical Study on SLMs for Function Calling** (arXiv:2504.19277)                      | 2025       | <7B          | Zero-shot/few-shot普遍差；DeepSeek-Coder 是唯一能產生正確格式者；fine-tuning 大幅提升；最佳訓練量約 60K samples                                                |
| 3   | **STAR: Similarity-guided Teacher-Assisted Refinement for Super-Tiny Function Calling Models** (arXiv:2602.03022) | 2026       | <3B          | SLM fine-tune 易過拟合工具模式而非泛化；提出 similarity-guided teacher approach                                                                    |
| 4   | **Advancing SLM Tool-Use via Reinforcement Learning** (arXiv:2509.04518)                                          | 2025       | <7B          | SLM 難以決定何時需要 tool / 選對 tool；RL 可直接改善 tool-use，但 training stability 是挑戰                                                              |
| 5   | **Optimizing function calling with SLMs** (Microsoft Data Science blog)                                           | 2025       | GPT-2-class  | 高品質數據 > 大量數據；60K samples 為黃金區間；複雜 function 如 `set_temperature` 表現較弱                                                                 |
| 6   | **Teaching a Language Model to Speak the Language of Tools** (arXiv:2506.23394)                                   | 2025       | multilingual | 多語言 SLM 缺乏可靠 tool-use；專門 fine-tune 可在不影響语言能力下提升工具能力                                                                                 |
| 7   | **Small Reasoning Models are Instruction Followers in Function Calling** (arXiv:2608.22472)                       | 2026       | sub-3B       | 建議將規劃與執行解耦：用 SRM 處理 tool-definition parsing，主 LLM 專注回應                                                                              |

### Coding Agent Benchmark

| # | 標題 | 年份 | 關鍵發現 |
|---|------|------|---------|
| 8 | **SWE-bench Verified** (OpenAI blog) | 2024-2025 | 500 human-validated instances；top model (gpt-5) 僅解 21% 於 SWE-EVO vs 65% 於 Verified，顯示 real-world gap |
| 9 | **OmniCode: Benchmark for Software Development Agents** (arXiv:2602.02262) | 2026 | 評估 SOTA coding agent on real PR fixes；even best models face hard repository-level tasks |
| 10 | **TOBench: Task-Oriented Omni-Modal Benchmark** (arXiv:2605.16909) | 2026 | 100 executable tasks, 27 MCP servers, 324 tools, closed-loop multimodal verification |
| 11 | **TensorBench: Benchmarking Coding Agents on Compiler-Based Tensor Framework** (arXiv:2606.05570) | 2026 | 199 repo-level PyTorch extension tasks；inter-agent agreement κ=0.05，反映 task-specific skill gaps |
| 12 | **Dialogue-SWEBench: A Benchmark for Dialogue-Driven Coding Agents** (arXiv:2606.13995) | 2026 | 評估 Qwen3 Coder 30-A-3B (MoE)、Devstral 2 Small (3B) 等小模型於 SWE-bench variant |
| 13 | **ProjectEval: Benchmark for Programming Agents** (ACL 2025) | 2025 | OpenHands agent fails on 8/20 tasks; lacks many optional features |
| 14 | **SWE-EVO: Long-Horizon Software Evolution** (arXiv:2512.18470) | 2025 | 甚至 gpt-5 僅解 21%；performance scaling 直線相关 model size |
| 15 | **Saving SWE-Bench: Mutation Approach** (arXiv:2510.08996) | 2025 | 現有 benchmark 可能高估 agent bug-fix 能力；mutation-based evaluation 揭示更大 gap |

### 小模型編碼能力 Benchmarks

| # | 標題 | 年份 | 模型 | 關鍵發現 |
|---|------|------|------|---------|
| 16 | **Assessing SLMs for Code Generation** (arXiv:2507.03160) | 2025 | DeepSeek-Coder 1.3B/6.7B, CodeLlama 7B, StarCoder2 7B, Qwen2.5-Coder 3B | pass@1: DeepSeek-Coder-6.7B HumanEval 0.70, MBPP 0.76；StarCoder2-3B HumanEval 0.57 |
| 17 | **RefactorCoderQA** (arXiv:2509.10436) | 2025 | Mistral 7B, DeepSeek-Coder 1.3B, Gemma 2B, Phi-3-mini | 小模型在 refactoring question answering 差距明顯；Fine-tuned MoE 可達 ~75% |
| 18 | **Improving Code Gen via SLM-as-a-judge** (arXiv:2602.11911) | 2026 | Team of SLMs vs single LLM | DeepSeek-Coder 1B team(10GPU) 超越 DeepSeek-Coder 33B alone；Qwen/Phi/Gemma SLM team 亦勝過單一 LLM |
| 19 | **LoRaWAN Engineering Code Gen Study** (arXiv:2502.14926) | 2025 | Qwen2.5-Coder 0.5B/1.5B/3B, LLaMA-3.2-3B, Mistral 7B, Gemma-2B | Qwen2.5-Coder-1.5B 達成 83.3% overall accuracy；小模型在特定領域仍有競爭力 |

---

## 二、核心問題歸納

### P1: 格式遵循困難（Format Following Failure）
- **現象**：SLM 輸出常混入雜訊文字、JSON 結構錯誤、參數缺失
- **證據**：arXiv:2504.19277 §3 — zero-shot 幾乎全失敗；即使 fine-tuned 後仍常有結構錯誤
- **解法**：引導解碼 (guided decoding)、type-safe registry、template-based prompting

### P2: 工具選擇與時機判斷不足
- **現象**：不必要時呼叫 tool、選錯 tool、需要時卻未呼叫
- **證據**：arXiv:2509.04518 — SLM context understanding 有限，難以判斷 tool need
- **解法**：relevance filtering module（xLAM-irrelevance, 6.7K irrelevant samples fine-tune）

### P3: 複雜嵌套與多步驟失敗率高
- **現象**：nested function calls、multi-turn tool orchestration 表現遠遜大模型
- **證據**：BFCL-v4 results — LLaMA-3-8b close to GPT-4 on simple calls but drops significantly on AST-complex cases
- **解法**：plan-execution decoupling（SRM 解析 tool-def，主模型專注邏輯）

### P4: 過度擬合訓練分布
- **現象**：fine-tune 後對訓練分布內 pattern 好，泛化差
- **Evidence**：arXiv:2602.03022 (STAR) — SFT on finite data memorizes patterns rather than generalizes
- **解法**：similarity-guided teacher refinement、RL post-training、data augmentation via synthetic generation（ToolACE pipeline）

### P5: 上下文窗口限制影響 agentic reasoning
- **現象**：多步推理累積 token 後 SLM 效能急降
- **Evidence**：BFCL multi-turn results — SLM performance drops sharply beyond 3-4 turns
- **解法**：tool retrieval (ToolRAG)、memory compression、summarization between steps

### P6: Hallucination & Over-reliance
- **現象**：hallucinate parameters 不存在之 tool、或對已有知識過度依賴
- **Evidence**：arXiv:2509.04518 Table 1；arXiv:2509.18076 §3.3 Nexus benchmark results
- **解法**：structured template training (Template-Training boosts BFCL from 66.5→71.4 for Mistral-7B)

---

## 三、Benchmark 數據速查表

### Function Calling (BFCL v4 selected subset, as of Nov 2026)

| Model | Params | Overall Acc | Serial FC | Parallel FC | Multi-turn | AST Complex |
|-------|--------|-------------|-----------|-------------|------------|-------------|
| GPT-4 | - | ~72 | - | - | - | - |
| LLaMA-3.1-8B-Instruct | 8B | ~68 | strong | moderate | weak | poor |
| Mistral-7B-Instruct-v0.3 | 7B | ~66.5 | good | moderate | poor | poor |
| DeepSeek-Coder-6.7B | 6.7B | ~71 | strong | strong | moderate | fair |
| TinyAgent-7B (fine-tuned) | 7B | >GPT-4-turbo* | - | - | - | - |
| TinyAgent-1.1B (fine-tuned) | 1.1B | near SOTA** | - | - | - | - |

\* Erdogan et al. 2024 on custom Siri-like eval; \*\* on edge-device specific tasks

### Code Generation (HumanEval Pass@1 Greedy)

| Model | Size | HumanEval | MBPP | EvalPlus |
|-------|------|-----------|------|----------|
| DeepSeek-Coder-1.3B | 1.3B | 0.33 | ~0.60 | - |
| DeepSeek-Coder-6.7B | 6.7B | 0.70 | 0.76 | 0.67 |
| StarCoder2-3B | 3B | 0.57 | ~0.65 | - |
| StarCoder2-7B | 7B | 0.42 | 0.71 | 0.56 |
| Qwen2.5-Coder-3B | 3B | 0.72 | 0.79 | 0.74 |
| Qwen2.5-Coder-7B | 7B | 0.87 | 0.82 | 0.82 |
| Phi-3-mini | 3.8B | ~0.55 | ~0.68 | - |
| LLaMA-3.2-3B | 3B | ~0.45 | ~0.60 | - |

### Coding Agent (SWE-bench Verified / Lite representative)

| Model/Framework | Setting | SWE-bench Lite Pass | Notes |
|-----------------|---------|---------------------|-------|
| GPT-4 (baseline) | mini-SWE-agent | ~28% | Standard ReAct loop |
| GPT-4.1 | with RAG | higher | Requires more context |
| gpt-5 | SWE-EVO | 21% (much lower than Verified) | Gap indicates benchmark leakage |
| Qwen3 Coder 30A-3B (MoE) | Dialogue-SWEBench | TBD |新兴 small MoE coding agent |
| Devstral 2 Small (3B) | Dialogue-SWEBench | TBD | Mistral AI specialized |

---

## 四、實用解法模式

### 模式 M1: Fine-Tuning Pipeline（最有效）
```
Curate 40K-60K high-quality function-call examples
    ↓
LLaMA/Mistral/DeepSeek base → LoRA SFT (rank=16, alpha=32)
    ↓
Post-train with RL (GRPO/PPO) on tool-use reward
    ↓
Inference: guided decoding + type-safe output parser
```
- Evidence: arXiv:2409.00608 (TinyAgent), arXiv:2504.19277, arXiv:2602.03022

### 模式 M2: Tool Retrieval + Structured Prompt
```
User query → embed → retrieve top-K relevant tool definitions (ToolRAG)
    ↓
Construct minimal prompt: system + retrieved tools + query only
    ↓
SLM generates tool call or direct response
```
- Evidence: arXiv:2409.00608 §3.2 ToolRAG reduces input length by 60%+

### 模式 M3: Plan-Execute Decoupling (Two-Model)
```
Step 1: SRM (tiny SLM) parses user intent → plan tool sequence
Step 2: Main SLM executes each step with full context isolation
    ↓
Result preserved in separate memory buffer
```
- Evidence: arXiv:2608.22472 (Small Reasoning Models)

### 模式 M4: Ensemble of SLMs
```
Task → route to 2-10 parallel SLMs (different seeds/fine-tunes)
    ↓
Vote/aggregation over outputs
    ↓
Team accuracy > single larger LLM at comparable cost
```
- Evidence: arXiv:2602.11911 — 10× DeepSeek-Coder-1B beats DeepSeek-Coder-33B on code gen

### 模式 M5: Synthetic Data with Self-Evolution
```
Start: seed API pool (ToolACE: 26,507 APIs)
    ↓
Multi-agent dialog synthesis → dual-layer verification (rule + model)
    ↓
Train SLM on verified trajectories
    ↓
Iterate: re-synthesize failures
```
- Evidence: arXiv:2506.23394 (ToolACE), arXiv:2602.03022 (STAR)
