---
title: "TOBench: A Task-Oriented Omni-Modal Benchmark for Real-World Tool-Using Agents"
slug: tobench-multimodal-tool-benchmark
arxiv: "2605.16909"
version: v1
authors:
  - "Zhiqiang Liu"
  - "Wenhui Dong"
  - "Yilang Tan"
  - "Yuwen Qu"
  - "Haochen Yin"
  - "Chenyang Si"
year: 2026
venue: "arXiv preprint"
subjects: ["cs.AI"]
date_submitted: "2026-05-16"
doi: "https://doi.org/10.48550/arXiv.2605.16909"
tags: [coding-agent, multimodal, tool-use, benchmark, mcp, closed-loop]
source_url: "https://arxiv.org/abs/2605.16909"
pdf_url: "https://arxiv.org/pdf/2605.16909"
html_url: "https://arxiv.org/html/2605.16909v1"
github_url: "https://github.com/Pi3AI/TOBench"
abstract: |
  Tool-using agents are increasingly expected to operate across realistic professional workflows, where they must interpret multimodal inputs, coordinate external tools, inspect intermediate artifacts, and revise their actions before producing a final result. Existing benchmarks, however, often evaluate tool use, computer use, and multimodal reasoning in isolation, leaving a gap between benchmark settings and end-to-end omni-modal tool use in the real world. To address this gap, we introduce MM-ToolBench, a benchmark and evaluation harness for task-oriented omni-modal tool use. MM-ToolBench contains 100 executable tasks from two macro task families, Customer Service and Intelligent Creation, covering 20 subcategory slices and supported by 27 MCP servers with 324 tools. The central design of MM-ToolBench is closed-loop multimodal verification: agents must execute tools, inspect rendered or transformed artifacts, and self-correct when outputs fail task-specific requirements. To make such evaluation scalable and verifiable, MM-ToolBench couples MCP-based execution with task-specific grounded evaluators and a semi-automated construction pipeline for scenario discovery, task instantiation, evaluator synthesis, and human audit. Experiments on 15 contemporary agentic models show that MM-ToolBench remains highly challenging: Claude Opus 4.6, commonly regarded as one of the strongest coding-agent models, achieves only 32.0% task success, far below the 94.0% human benchmark. We envision MM-ToolBench as a practical foundation for evaluating and advancing next-generation omni-modal tool-using agents through closed-loop multimodal verification.
related_articles:
  - "small-model-code-gen-benchmarks"
created: 2026-11-21
last_updated: 2026-11-21