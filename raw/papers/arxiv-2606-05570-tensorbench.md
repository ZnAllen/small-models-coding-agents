---
title: "TensorBench: Benchmarking Coding Agents on a Compiler-Based Tensor Framework"
slug: tensorbench-pytorch-agent-benchmark
arxiv: "2606.05570"
version: v1
authors:
  - "Bobby Yan"
  - "Fredrik Kjolstad"
year: 2026
venue: "arXiv preprint"
subjects: ["cs.CL", "cs.AI"]
date_submitted: "2026-06-04"
doi: "https://doi.org/10.48550/arXiv.2606.05570"
tags: [coding-agent, benchmark, pytorch, compiler, domain-specific]
source_url: "https://arxiv.org/abs/2606.05570"
pdf_url: "https://arxiv.org/pdf/2606.05570"
html_url: "https://arxiv.org/html/2606.05570v1"
abstract: |
  Repository-level coding benchmarks face a trade-off between task difficulty and evaluation reliability: tasks that challenge frontier models often involve large codebases with incomplete test coverage, while human review does not scale. We introduce TensorBench, a benchmark of 199 feature-addition and refactoring tasks on an open-source compiler-based tensor framework that extends PyTorch with first-class support for dense and sparse tensors. Tasks cover new sparse formats, dense optimization passes, IR transformations, scheduler changes, runtime components, and high-level numerical operators. TensorBench grades each run by applying the agent's patch and running the framework's test suite, which includes the pre-existing randomized regression tests and any tests the agent adds. For feature-addition tasks, a pass means that the patched repository preserves the tested pre-existing behavior and satisfies the agent-added checks for the requested feature. We evaluate seven coding agents spanning three frontier model families and one open-weight model. Pass rates under this criterion range from 64.8% for the strongest agent to 22.1% for the weakest. Agents pass different subsets of tasks: pairwise Cohen's kappa ranges from -0.07 to 0.43, with kappa = 0.05 for the two strongest agents.
related_articles:
  - "small-model-code-gen-benchmarks"
created: 2026-11-21
last_updated: 2026-11-21