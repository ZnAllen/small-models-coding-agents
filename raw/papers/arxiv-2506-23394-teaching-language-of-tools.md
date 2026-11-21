---
title: "Teaching a Language Model to Speak the Language of Tools"
slug: teaching-llm-language-of-tools
arxiv: "2506.23394"
version: v1
authors:
  - "Simeon Emanuilov"
year: 2025
venue: "arXiv preprint"
subjects: ["cs.IR", "cs.AI", "cs.CL"]
date_submitted: "2025-06-29"
doi: "https://doi.org/10.48550/arXiv.2506.23394"
tags: [function-calling, multilingual, synthetic-data, tool-acquisition, mcp]
source_url: "https://arxiv.org/abs/2506.23394"
pdf_url: "https://arxiv.org/pdf/2506.23394"
html_url: null
abstract: |
  External tool integration through function-calling is essential for practical language model applications, yet most multilingual models lack reliable tool-use capabilities in non-English languages. Even state-of-the-art multilingual models struggle with determining when to use tools and generating the structured outputs required for function calls, often exhibiting language confusion when prompted in lower-resource languages. This work presents a methodology for adapting existing language models to enable robust tool use in any target language, using Bulgarian as a case study. The approach involves continued training of the BgGPT model series (2.6B, 9B, 27B parameters) on a novel bilingual dataset of 10,035 function-calling examples designed to support standardized protocols like MCP (Model Context Protocol). The research introduces TUCAN (Tool-Using Capable Assistant Navigator), which achieves up to 28.75% improvement in function-calling accuracy over base models while preserving core language understanding, as verified on established Bulgarian benchmarks. Beyond accuracy gains, TUCAN models demonstrate production-ready response formatting with clean, parsable function calls, contrasting with the verbose and inconsistent outputs of base models. The models, evaluation framework, and dataset are released to enable replication for other languages. This work demonstrates a practical approach for extending tool-augmented capabilities beyond English-centric systems.
related_articles:
  - "small-model-tool-calling-problems"
  - "small-model-code-gen-benchmarks"
created: 2026-11-21
last_updated: 2026-11-21