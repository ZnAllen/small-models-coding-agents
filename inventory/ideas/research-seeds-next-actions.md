---
title: "Research Seeds — Next Actions"
type: idea
tags: [coding-agent, research-plan, small-language-model]
created: 2026-11-21
status: candidate
---

# Research Seeds for Small Models in Coding Agent

## Pending Topics (尚未深入研究)

### T1: Hybrid System Architecture
- 大小模型協同：大模型做規劃、小模型做執行
- 參考：[[arXiv:2608.22472]] Plan-Execute Decoupling pattern
- 行動：實測 Qwen3-32B plan + Qwen2.5-Coder-3B execute on SWE-bench Lite

### T2: RL Post-Training for SLM Tool Use
- GRPO/PPO on function-calling reward signals
- Training stability concerns ([[arXiv:2602.03022]])
- 行動：復現 TinyAgent RL phase 並測量 gain

### T3: Quantization Impact on Tool Calling
- GPTQ / AWQ / FP8 對 FC 準確率的影響
- 行動：在 BFCL-v4 上測試 TinyAgent-7B 各量化等級

### T4: Multi-Agent SLM Collaboration
- [[arXiv:2602.11911]] Team-of-SLMs 模式延伸
- 行動：設計 4× DeepSeek-Coder-1.5B voting ensemble 對比單個 7B

### T5: Real-world Edge Deployment Case Study
- 選擇一個具體場景（如筆記型電腦 Siri-like assistant）
- 完整評估 latency、memory、accuracy tradeoff
- 行動：使用 TinyAgent framework 部署實際 demo

## Recommended First Step
從 **T1** 開始——先建立一個小型 hybrid system prototype，再逐步深入各子題。