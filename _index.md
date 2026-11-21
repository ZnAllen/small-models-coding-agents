---
title: "小模型做 Coding Agent 的問題及解決方式"
slug: small-models-coding-agents
status: active
created: 2026-11-21
last_updated: 2026-11-21
description: "探討使用小型語言模型（7B 以下）建構 Coding Agent 所面臨的挑戰與實際解決方案"
tags: [coding-agent, small-language-model, llm, ai-agents]
---

# 小模型做 Coding Agent 的問題及解決方式

這是一個專注於探索如何讓小型語言模型（Small Language Models, SLMs，通常指 7B 參數以下的模型）有效執行 Coding Agent 任務的知識庫。

## 目標

- 了解小模型的優勢與限制
- 歸納常見問題與對應解法
- 累積實務經驗與最佳實踐

## 主題範圍

| 子主題 | 狀態 | 說明 |
|--------|------|------|
| [[model-capabilities]] | ✅ 已編譯 | 不同大小模型的能力邊界與選擇指南 |
| [[prompt-engineering]] | ✅ 已編譯 | 針對小模型優化的提示工程技巧 |
| [[tool-use]] | ⚠️ 內含於 P1–P6 | 小模型的工具呼叫能力與限制（詳見概念文） |
| [[memory-context]] | ✅ 已編譯 | 上下文窗口與記憶管理策略（ToolRAG、壓縮、滑動視窗） |
| [[evaluation]] | ✅ 已編譯 | 如何評估小模型 Coding Agent 的性能（詳見 benchmark 文） |
| [[hybrid-systems]] | ✅ 已編譯 | 大小模型協同架構設計（Plan-Execute、Ensemble） |
| [[optimization]] | ✅ 已編譯 | 量化、剪枝、RL 後訓練等效率優化技術 |

## 已編譯文章

| 標題 | 類型 | 摘要 |
|------|------|------|
| [[小模型工具呼叫的核心問題]] | Concept | sub-7B 模型 FC 的六大核心問題與解法方向 |
| [[小模型 Coding Agent Benchmark 數據總覽]] | Reference | Code Gen / FC / Agent 三維度 benchmark 數據表 |
| [[小模型能力邊界與選擇指南]] | Concept | 0.5B–7B 各級距模型在 Code Gen、FC、推理維度的表現差異 |
| [[面向小模型的提示工程策略]] | Concept | 結構化模板、引導解碼、分階段提示等實證有效技巧 |
| [[小模型的記憶與上下文管理策略]] | Concept | ToolRAG、記憶壓縮、滑動視窗等上下文管理框架 |
| [[大小模型協同架構設計]] | Topic | Plan-Execute 解耦、Multi-Agent Ensemble 等 hybrid 模式 |
| [[小模型的效率優化技術]] | Concept | 量化、剪枝、RL 後訓練、合成數據管道的系統性比較 |

## 快速導覽

- **Raw Sources**: `raw/articles/`, `raw/papers/`, `raw/notes/`
- **Compiled Articles**: `wiki/concepts/`, `wiki/topics/`, `wiki/references/`
- **Ideas**: `inventory/ideas/`
