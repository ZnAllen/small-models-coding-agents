---
title: "Improving Large Language Models Function Calling and Interpretability via Guided-Structured Templates"
slug: improving-llm-fc-guided-structured-templates
arxiv: "2509.18076"
version: v1
authors:
  - "Hy Dang"
  - "Tianyi Liu"
  - "Zhuofeng Wu"
  - "Jingfeng Yang"
  - "Haoming Jiang"
  - "Tao Yang"
  - "Pei Chen"
  - "Zhengyang Wang"
  - "Helen Wang"
  - "Huasheng Li"
  - "Bing Yin"
  - "Meng Jiang"
year: 2025
venue: "EMNLP 2025 Main Conference"
subjects: ["cs.AI"]
date_submitted: "2025-09-22"
doi: "https://doi.org/10.48550/arXiv.2509.18076"
tags: [function-calling, prompt-engineering, guided-decoding, template-training]
source_url: "https://arxiv.org/abs/2509.18076"
pdf_url: "https://arxiv.org/pdf/2509.18076"
html_url: "https://arxiv.org/html/2509.18076v1"
abstract: |
  Large language models (LLMs) have demonstrated strong reasoning and tool-use capabilities, yet they often fail in real-world tool-interactions due to incorrect parameterization, poor tool selection, or misinterpretation of user intent. These issues often stem from an incomplete understanding of user goals and inadequate comprehension of tool documentation. While Chain-of-Thought (CoT) prompting has proven effective for enhancing reasoning in general contexts, our analysis reveals that free-form CoT is insufficient and sometimes counterproductive for structured function-calling tasks. To address this, we introduce a curriculum-inspired framework that leverages structured reasoning templates to guide LLMs through more deliberate step-by-step instructions for generating function callings. Experimental results show that our method reduces tool-use errors, achieving 3-12% relative improvements over strong baselines across diverse model series and approaches. Moreover, our framework enhances the robustness, interpretability, and transparency of tool-using agents, advancing the development of more reliable AI assistants for real-world applications.
related_articles:
  - "small-model-tool-calling-problems"
  - "prompt-engineering-small-models"
created: 2026-11-21
last_updated: 2026-11-21