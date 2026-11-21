# Activity Log — Small Models for Coding Agents

## [2026-11-21] migrate | Migrated wiki from ~/wiki/ to local .wiki/

- Moved entire topic wiki to `E:\LLM-wiki\.wiki\topics\small-models-coding-agents/`
- Added hub-level `_index.md` at `E:\LLM-wiki\.wiki/_index.md`
- All internal links and relative paths remain valid (no absolute path references)

## [2026-11-21] research | Collect papers on SLM tool-calling limits and coding agent benchmarks

- Compiled 19 key papers (2024–2026) on arXiv covering function calling, code gen, and coding agent eval
- Wrote raw article: `research-summary-function-calling-benchmarks-2025-2026.md`
- Compiled two wiki articles:
  - `wiki/concepts/small-model-tool-calling-problems.md` — 6 core problem categories + solution patterns
  - `wiki/references/small-model-code-gen-benchmarks.md` — benchmark data tables for 3 dimensions
- Key finding: Qwen2.5-Coder-7B leads at 87.2% HumanEval; TinyAgent-7B surpasses GPT-4-turbo on edge function calling

## [2026-11-21] raw | 修正論文原始資料：從摘要改回 arXiv 原文摘錄

- 用戶指出 raw/papers/ 的 19 個檔案應為論文原文，而非 AI 撰寫的摘要
- 重新抓取每篇論文的 arXiv abs 頁面，以網頁上的原始 abstract 文字覆蓋原檔
- 完整保留 metadata：full author list、venue、DOI、source/pdf/html URL
- 未收錄於 arXiv 的來源（Microsoft blog、ACL 2025 proceedings）標註 null 並說明

## [2026-11-21] compile | 編譯缺失文章並補足論文原始資料

- 建立 19 篇論文 raw paper 來源檔於 `raw/papers/`，全數以中文撰寫 metadata
- 編譯 5 篇新增 wiki 文章（全部中文）：
  - `wiki/concepts/model-capabilities.md` — 小模型能力邊界與選擇指南
  - `wiki/concepts/prompt-engineering.md` — 面向小模型的提示工程策略
  - `wiki/concepts/memory-context.md` — 記憶與上下文管理策略
  - `wiki/topics/hybrid-systems.md` — 大小模型協同架構設計
  - `wiki/concepts/optimization.md` — 效率優化技術（量化/剪枝/RL）
- 更新 `_index.md`、`config.md`、`raw/_index.md`、`wiki/_index.md`
- 主題範圍從 2/7 篇文章增至 7/7，所有規劃中的 topic 均已編譯完成

## [2026-11-21] assignment | 製作教學 HTML 簡報與作業報告

- 基於現有 wiki（7 篇編譯文章 + 19 篇 raw papers）製作教學簡報
- 簡報檔案：`output/slm-coding-agent-presentation.html`（11 頁，Reveal.js 框架）
- 作業報告：`output/assignment-report.md`（含 loop/graph 說明與反思）
- 簡報結構：什麼是 SLM → 六大問題 → 尺寸對照表 → 四大解法 → 決策表 → 重點回顧 → Q&A
- 排版修訂（v2）：縮小字級與間距、表格 column 精簡、六問題改用 3×2 網格、決策表改為卡片格式、flow diagram 限寬 160px、source-note 加入 word-break 避免溢出
