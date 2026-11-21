---
title: "SWE-EVO: Benchmarking Coding Agents in Long-Horizon Software Evolution Scenarios"
slug: swe-evo-long-horizon-benchmark
arxiv: "2512.18470"
version: v6
authors:
  - "Tue Le"
  - "Minh V. T. Thai"
  - "Dung Nguyen Manh"
  - "Huy Phan Nhat"
  - "Nghi D. Q. Bui"
year: 2025
venue: "arXiv preprint"
subjects: ["cs.SE", "cs.AI", "cs.MA"]
date_submitted: "2025-12-20"
doi: "https://doi.org/10.48550/arXiv.2512.18470"
tags: [coding-agent, benchmark, swe-bench, long-horizon, multi-step]
source_url: "https://arxiv.org/abs/2512.18470"
pdf_url: "https://arxiv.org/pdf/2512.18470"
html_url: "https://arxiv.org/html/2512.18470v6"
abstract: |
  Existing benchmarks for AI coding agents focus on isolated, single-issue tasks such as fixing a bug or adding a small feature. However, real-world software engineering is a long-horizon endeavor: developers interpret high-level requirements, coordinate changes across many files, and evolve codebases over multiple iterations while preserving functionality. We introduce SWE-EVO, a benchmark for this long-horizon software evolution challenge. Constructed from release notes of seven mature open-source Python projects, SWE-EVO comprises 48 tasks requiring multi-step modifications spanning an average of 21 files, validated against test suites averaging 874 tests per instance. Experiments reveal a striking capability gap: GPT-5.4 with OpenHands achieves only 25% on SWE-EVO versus 72.80% achieved by GPT-5.2 on SWE-Bench Verified, showing that current agents struggle with sustained, multi-file reasoning. We also propose Fix Rate, a metric capturing partial progress on these complex, long-horizon tasks.
related_articles:
  - "small-model-code-gen-benchmarks"
created: 2026-11-21
last_updated: 2026-11-21