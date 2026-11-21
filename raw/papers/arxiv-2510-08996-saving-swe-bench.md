---
title: "Saving SWE-Bench: A Benchmark Mutation Approach for Realistic Agent Evaluation"
slug: saving-swe-bench-mutation-evaluation
arxiv: "2510.08996"
version: v4
authors:
  - "Spandan Garg"
  - "Benjamin Steenhoek"
  - "Yufan Huang"
year: 2025
venue: "CAIN 2026 Research Track"
subjects: ["cs.SE", "cs.AI"]
date_submitted: "2025-10-10"
doi: "https://doi.org/10.48550/arXiv.2510.08996"
tags: [benchmark, swe-bench, mutation-testing, evaluation-integrity, agent-eval]
source_url: "https://arxiv.org/abs/2510.08996"
pdf_url: "https://arxiv.org/pdf/2510.08996"
html_url: "https://arxiv.org/html/2510.08996v4"
abstract: |
  Current benchmarks for evaluating software engineering agents, such as SWE-Bench Verified, are predominantly derived from GitHub issues and fail to accurately reflect how developers interact with chat-based coding assistants in integrated development environments (IDEs). We posit that this mismatch leads to a systematic overestimation of agent's capabilities in real-world scenarios, especially bug fixing. We introduce a novel benchmarking framework that transforms existing formal benchmarks into realistic user queries through systematic analysis of developer interaction patterns with chat-based agents. Our methodology is flexible and can be easily extended to existing benchmarks. In this paper, we apply our testing framework to SWE-Bench Verified, the TypeScript subset of Multi-SWE-Bench and a private benchmark, SWE-Bench C# and transform formal GitHub issue descriptions into realistic user-style queries based on telemetry analysis of a popular chat-based agent interactions. Our findings reveal that existing benchmarks significantly overestimate agent capabilities for some models by >50% over baseline performance for public benchmarks and ~10-16% for our internal benchmark. This work establishes a new paradigm for evaluating interactive chat-based software engineering agents through benchmark mutation techniques.
related_articles:
  - "small-model-code-gen-benchmarks"
created: 2026-11-21
last_updated: 2026-11-21