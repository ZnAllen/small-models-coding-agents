---
title: "Small Models, Big Tasks: An Exploratory Empirical Study on Small Language Models for Function Calling"
slug: small-models-big-tasks-fc-study
arxiv: "2504.19277"
version: v1
authors:
  - "Ishan Kavathekar"
  - "Raghav Donakanti"
  - "Ponnurangam Kumaraguru"
  - "Karthik Vaidhyanathan"
year: 2025
venue: "EASE 2025 AI Models and Data Evaluation track"
subjects: ["cs.AI", "cs.SE"]
date_submitted: "2025-04-27"
doi: "https://doi.org/10.48550/arXiv.2504.19277"
tags: [function-calling, small-language-model, empirical-study, benchmark]
source_url: "https://arxiv.org/abs/2504.19277"
pdf_url: "https://arxiv.org/pdf/2504.19277"
html_url: "https://arxiv.org/html/2504.19277v1"
abstract: |
  Function calling is a complex task with widespread applications in domains such as information retrieval, software engineering and automation. For example, a query to book the shortest flight from New York to London on January 15 requires identifying the correct parameters to generate accurate function calls. Large Language Models (LLMs) can automate this process but are computationally expensive and impractical in resource-constrained settings. In contrast, Small Language Models (SLMs) can operate efficiently, offering faster response times, and lower computational demands, making them potential candidates for function calling on edge devices. In this exploratory empirical study, we evaluate the efficacy of SLMs in generating function calls across diverse domains using zero-shot, few-shot, and fine-tuning approaches, both with and without prompt injection, while also providing the finetuned models to facilitate future applications. Furthermore, we analyze the model responses across a range of metrics, capturing various aspects of function call generation. Additionally, we perform experiments on an edge device to evaluate their performance in terms of latency and memory usage, providing useful insights into their practical applicability. Our findings show that while SLMs improve from zero-shot to few-shot and perform best with fine-tuning, they struggle significantly with adhering to the given output format. Prompt injection experiments further indicate that the models are generally robust and exhibit only a slight decline in performance. While SLMs demonstrate potential for the function call generation task, our results also highlight areas that need further refinement for real-time functioning.
related_articles:
  - "small-model-tool-calling-problems"
  - "small-model-code-gen-benchmarks"
created: 2026-11-21
last_updated: 2026-11-21