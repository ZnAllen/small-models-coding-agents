---
title: "RefactorCoderQA: Benchmarking LLMs for Multi-Domain Coding Question Solutions in Cloud and Edge Deployment"
slug: refactorcoderqa-slm-evaluation
arxiv: "2509.10436"
version: v2
authors:
  - "Shadikur Rahman"
  - "Aroosa Hameed"
  - "Gautam Srivastava"
  - "Syed Muhammad Danish"
year: 2025
venue: "IEEE Transactions on Services Computing (submitted)"
subjects: ["cs.CL"]
date_submitted: "2025-09-12"
doi: "https://doi.org/10.48550/arXiv.2509.10436"
tags: [code-generation, refactoring, benchmark, small-language-model, moe]
source_url: "https://arxiv.org/abs/2509.10436"
pdf_url: "https://arxiv.org/pdf/2509.10436"
html_url: "https://arxiv.org/html/2509.10436v2"
abstract: |
  To optimize the reasoning and problem-solving capabilities of Large Language Models (LLMs), we propose a novel cloud-edge collaborative architecture that enables a structured multi-agent prompting framework. This framework comprises three specialized components: GuideLLM, a lightweight model deployed at the edge to provide methodological guidance; SolverLLM, a more powerful model hosted in the cloud and responsible for generating code solutions; and JudgeLLM, an automated evaluator for assessing solution correctness and quality. To evaluate and demonstrate the effectiveness of this architecture in realistic settings, we introduce RefactorCoderQA, a comprehensive benchmark designed to evaluate and enhance the performance of LLMs across multi-domain coding tasks. Motivated by the limitations of existing benchmarks, RefactorCoderQA systematically covers multiple technical domains, including Software Engineering, Data Science, Machine Learning, and Natural Language Processing, using authentic coding challenges sourced from Stack Overflow. We propose RefactorCoder-MoE, a fine-tuned mixture-of-experts (MoE) code language model based on DeepSeek-Coder-7B-Instruct, adapted to the RefactorCoderQA benchmark using QLoRA for domain-specific coding question answering. Extensive experiments demonstrate that RefactorCoder-MoE achieves strong and competitive performance, significantly outperforming all evaluated open-source and commercial baselines, with an overall accuracy of 76.84%.
related_articles:
  - "small-model-code-gen-benchmarks"
created: 2026-11-21
last_updated: 2026-11-21