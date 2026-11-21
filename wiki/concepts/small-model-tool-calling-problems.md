---
title: "小模型工具呼叫的核心問題"
slug: small-model-tool-calling-problems
category: concept
tags: [function-calling, tool-use, small-language-model, limitations]
summary: "歸納 sub-7B 模型在 function calling / tool use 上的六大核心問題：格式失敗、工具選擇錯誤、多步困難、泛化差、上下文受限、hallucination"
confidence: medium-high
created: 2026-11-21
last_updated: 2026-11-21
see_also: [[small-model-code-gen-benchmarks]]
---

# 小模型工具呼叫的核心問題

小型語言模型（SLM，通常指 <7B 參數）在工具呼叫（Function Calling / Tool Use）上表現顯著遜色於大模型，已有多項系統性研究確認以下六大類別問題。

## P1: 格式遵循困難 (Format Following Failure)

### 現象
SLM 輸出常混入雜訊文字、JSON 結構錯誤、引數缺失或不符合 schema。即使經過 fine-tuning，此類錯誤仍是最常見失敗原因。

### 實證證據
- [[arXiv:2504.19277|Small Models, Big Tasks (Kavathekar et al., 2025)]] §3: zero-shot/few-shot 幾乎全失敗；DeepSeek-Coder 是唯一能產生正確結構者
- Microsoft Data Science blog (2025): 訓練 60K samples 後 prediction error 快速下降，但複雜 function 如 `set_temperature`、`adjust_fan_speed` 仍有高錯誤率

### 解法方向
- **引導解碼 (Guided Decoding)**: 限制 token 生成空間為有效 JSON/AST
- **Type-safe Registry**: 在推理階段強制型別檢查與修復
- **Template-based prompting**: [[arXiv:2509.18076|Template-Training (2025)]] 使 Mistral-7B BFCL 分數從 66.5 提升至 71.4

## P2: 工具選擇與時機判斷不足

### 現象
SLM 在以下情境容易出錯：
- 不需要 tool 卻執意呼叫（false positive）
- 需要 tool 卻未偵測到（false negative）
- 選錯 tool（wrong tool selection）

### 實證證據
- [[arXiv:2509.04518|Advancing SLM Tool-Use via RL (2025)]]: SLM context understanding 有限，難以自主判斷 tool need
- xLAM-irrelevance dataset (6.7K filtered irrelevant samples): fine-tune 可改善 irrelevance detection

### 解法方向
- Irrelevance classification module（二元分類：need tool vs not）
- Two-stage prompting: 先判斷 intent type，再選擇 tool

## P3: 複雜嵌套與多步驟失敗率高

### 現象
nested function calls（函數內含函數）、multi-turn tool orchestration（多輪工具協調）表現遠遜大模型。BFCL-v4 AST evaluation 顯示 LLaMA-3-8b 於單次呼叫接近 GPT-4，但在多層 AST 結構上性能急降。

### 解法方向
- [[arXiv:2608.22472|Small Reasoning Models as Instruction Followers (2026)]]: Plan-Execute Decoupling — SRM 解析 tool-definition，主 SLM 專注邏輯執行
- Memory buffer 隔離各 step context，避免累积 token 污染

## P4: 過度擬合訓練分布

### 現象
SFT on finite data 易 memorize specific tool-use patterns，而非學習泛化規則。導致對未知 API 或新任務失效。

### 實證證據
- [[arXiv:2602.03022|STAR (2026)]]: 直接 RL training on small models 極度不穩定且低效
- [[arXiv:2504.19277]]: 超過 60K 訓練量性能不再提升，過拟合跡象明顯

### 解法方向
- Similarity-guided teacher refinement (STAR)
- Synthetic data self-evolution loops (ToolACE pipeline: 26,507 APIs → multi-agent dialog synthesis)
- RL post-training with carefully designed reward (tool correctness + format compliance)

## P5: 上下文窗口限制影響 Agentic Reasoning

### 現象
多步推理累積 token 後 SLM 效能急降，尤其在 multi-turn conversation 中。

### 解法方向
- **ToolRAG** ([[arXiv:2409.00608|TinyAgent, 2024]]): 動態檢索 top-K relevant tool definitions，減少 prompt 長度 60%+
- Context summarization between turns
- Sliding-window attention variants optimized for agentic loops

## P6: Hallucination & Over-Reliance on Prior Knowledge

### 現象
- Hallucinate parameters 不存在之 tool
- 對訓練時见过之 knowledge 過度依賴，忽略實際 tool schema
- 輸出 "討論 tool mechanics" 而非真正執行呼叫

### 解法方向
- Structured template training ([[arXiv:2509.18076]])
- Post-generation validation layer (schema check before execution)
- Contrastive training: paired examples of correct vs hallucinated calls

## 參考文獻

[[arXiv:2409.00608|TinyAgent: Function Calling at the Edge]] (Erdogan et al., EMNLP 2024 Demo)  
[[arXiv:2504.19277|Small Models, Big Tasks]] (Kavathekar et al., 2025)  
[[arXiv:2509.04518|Advancing SLM Tool-Use via RL]] (2025)  
[[arXiv:2602.03022|STAR]] (2026)  
[[arXiv:2608.22472|Small Reasoning Models are Instruction Followers]] (2026)  
[[arXiv:2509.18076|Improving LLM Function Calling via Guided-Structured Templates]] (2025)  
[[arXiv:2506.23394|Teaching a Language Model to Speak the Language of Tools]] (2025)  
