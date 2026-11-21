---
title: "Improving Code Generation via Small Language Model-as-a-judge"
slug: improving-code-gen-slm-team
arxiv: "2602.11911"
version: v1
authors:
  - "Giuseppe Crupi"
  - "Rosalia Tufano"
  - "Gabriele Bavota"
year: 2026
venue: "ICSE 2026 (48th International Conference on Software Engineering)"
subjects: ["cs.SE"]
date_submitted: "2026-02-12"
doi: "https://doi.org/10.48550/arXiv.2602.11911"
tags: [code-generation, ensemble, small-language-model, llm-as-judge]
source_url: "https://arxiv.org/abs/2602.11911"
pdf_url: "https://arxiv.org/pdf/2602.11911"
html_url: "https://arxiv.org/html/2602.11911v1"
abstract: |
  Large language models (LLMs) have shown remarkable capabilities in automated code generation. While effective for mainstream languages, they may underperform on less common or domain-specific languages, prompting companies to develop in-house code generators. While open-source models can be trained for this, only LLMs with tens of billions of parameters match the performance of commercial tools, demanding costly training and deployment. Recent work proposed supporting code generation with smaller models (SLMs) by generating multiple candidate solutions and using another SLM to select the most likely correct one. The most recent work in this area is the one by Sun et al. [29] presenting RankEF, a T5 model trained to rank code solutions using both execution-based and non-execution-based information. However, Sun et al. do not assess the T5 ranker's classification accuracy, that is, how often it misjudges correct implementations as incorrect or vice versa, leaving open questions about the reliability of LMs as code correctness judges for other tasks (e.g., automated code review). Moreover, their experiments involve relatively old models, making it unclear the extent to which such a methodology would still help companies in cheaply training their own code generators with performance comparable to those of massive LLMs. We present a study addressing these limitations. We train several state-of-the-art SLMs as code correctness judges and assess their ability to discriminate between correct and wrong implementations. We show that modern SLMs outperform RankEF, even without exploiting execution-based information. When used as code rankers, they achieve higher performance gains than RankEF and perform competitively with LLMs 5-25x larger, at a fraction of the cost.
related_articles:
  - "small-model-code-gen-benchmarks"
created: 2026-11-21
last_updated: 2026-11-21