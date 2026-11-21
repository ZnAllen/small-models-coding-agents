---
title: "Assessing Small Language Models for Code Generation: An Empirical Study with Benchmarks"
slug: assessing-slms-code-generation
arxiv: "2507.03160"
version: v4
authors:
  - "Md Mahade Hasan"
  - "Muhammad Waseem"
  - "Kai-Kristian Kemell"
  - "Jussi Rasku"
  - "Juha Ala-Rantala"
  - "Pekka Abrahamsson"
year: 2025
venue: "Journal of Systems and Software (submitted)"
subjects: ["cs.SE"]
date_submitted: "2025-07-03"
doi: "https://doi.org/10.48550/arXiv.2507.03160"
related_doi: "https://doi.org/10.1016/j.jss.2026.112815"
tags: [code-generation, benchmark, small-language-model, humaneval, empirical-study]
source_url: "https://arxiv.org/abs/2507.03160"
pdf_url: "https://arxiv.org/pdf/2507.03160"
html_url: "https://arxiv.org/html/2507.03160v4"
abstract: |
  The recent advancements of Small Language Models (SLMs) have opened new possibilities for efficient code generation. SLMs offer lightweight and cost-effective alternatives to Large Language Models (LLMs), making them attractive for use in resource-constrained environments. However, empirical understanding of SLMs, particularly their capabilities, limitations, and performance trade-offs in code generation remains limited. This study presents a comprehensive empirical evaluation of 20 open-source SLMs ranging from 0.4B to 10B parameters on five diverse code-related benchmarks (HumanEval, MBPP, Mercury, HumanEvalPack, and CodeXGLUE). The models are assessed along three dimensions: i) functional correctness of generated code, ii) computational efficiency and iii) performance across multiple programming languages. The findings of this study reveal that several compact SLMs achieve competitive results while maintaining a balance between performance and efficiency, making them viable for deployment in resource-constrained environments. However, achieving further improvements in accuracy requires switching to larger models. These models generally outperform their smaller counterparts, but they require much more computational power. We observe that for 10% performance improvements, models can require nearly a 4x increase in VRAM consumption, highlighting a trade-off between effectiveness and scalability. Besides, the multilingual performance analysis reveals that SLMs tend to perform better in languages such as Python, Java, and PHP, while exhibiting relatively weaker performance in Go, C++, and Ruby. However, statistical analysis suggests these differences are not significant, indicating a generalizability of SLMs across programming languages. Based on the findings, this work provides insights into the design and selection of SLMs for real-world code generation tasks.
related_articles:
  - "small-model-code-gen-benchmarks"
created: 2026-11-21
last_updated: 2026-11-21