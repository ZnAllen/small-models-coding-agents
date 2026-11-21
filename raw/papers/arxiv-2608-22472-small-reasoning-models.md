---
title: "Small Reasoning Models are Instruction Followers in Function Calling"
slug: small-reasoning-models-instruction-followers
arxiv: "2608.22472"
version: v1
authors:
  - "Yalda Taheri"
  - "Mohammad Hassan Heydari"
  - "Erfan Naaman"
  - "Afsaneh Fatemi"
year: 2026
venue: "arXiv preprint"
subjects: ["cs.AI", "cs.CL"]
date_submitted: "2026-08-23"
doi: "https://doi.org/10.48550/arXiv.2608.22472"
tags: [function-calling, plan-execute-decoupling, small-language-model, reasoning, instruction-following]
source_url: "https://arxiv.org/abs/2608.22472"
pdf_url: "https://arxiv.org/pdf/2608.22472"
html_url: "https://arxiv.org/html/2608.22472v1"
abstract: |
  Function calling represents the core capability of agentic large language models (LLMs). Existing research has focused on enhancing LLMs function-calling accuracy through fine-tuning, reinforcement learning (RL), and multi-agent frameworks, particularly for native function-calling LLMs. This work demonstrates that LLMs achieve superior accuracy in function calling in instruction-following contexts (i.e., standard user-assistant interactions) rather than a tool calling context. We introduce Instruction-Followed Function Calling (IFFC), a novel framework that decouples function-calling logic from the primary LLM and delegates it to a dedicated smaller model operating within the instruction-following paradigm. Our method consistently outperforms both native function calling (NFC) and prompt-based function calling (PFC) baselines, with particularly strong gains on reasoning-oriented LLMs. Furthermore, we demonstrate that IFFC maintains robust performance under aggressive quantization, enabling efficient on-device deployment without significant accuracy degradation. This work establishes a new paradigm for reliable, resource-efficient function calling in edge-computing scenarios.
related_articles:
  - "small-model-tool-calling-problems"
  - "hybrid-systems-small-large-cooperation"
created: 2026-11-21
last_updated: 2026-11-21