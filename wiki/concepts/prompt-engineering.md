---
title: "面向小模型的提示工程策略"
slug: prompt-engineering-small-models
category: concept
tags: [prompt-engineering, small-language-model, function-calling, structured-prompts]
summary: "歸納針對小模型（sub-7B）優化的提示工程技巧，涵蓋結構化模板、引導解碼、分階段提示等策略，以及常見的陷阱與緩解方法。"
confidence: medium
created: 2026-11-21
last_updated: 2026-11-21
see_also: [[small-model-tool-calling-problems]]
---

# 面向小模型的提示工程策略

小型語言模型雖缺乏大模型天生的 instruction-following 能力，但透過精心設計的提示策略，仍可大幅提升其表現。本節歸納經實證有效的提示工程方法。

## 一、結構化模板（Structured Templates）

根據 [[arXiv:2509.18076|Guided-Structured Templates]] 的研究，模板訓練是最有效的提示工程方法之一。

### 原理
將 tool call 的輸出格式預先定義為固定模板，強制模型填入變量位置，而非自由生成 JSON。

### 範例
```
請根據以下工具和使用者請求，產生工具呼叫：

工具清單：
[1] get_weather(city: str) -> str: 查詢指定城市的天氣
[2] set_temperature(room: str, temp: float) -> None: 設定室內溫度

使用者請求：把客廳溫度調到 22 度

請依此格式回應：
<tool_call>
tool_id: [number]
arguments: {key: value}
</tool_call>
```

### 效果
- Mistral-7B Nexus 分數從 20.55 提升至 23.17
- BFCL 準確率從 66.5 提升至 71.4
- 大幅降低 JSON 格式錯誤率

## 二、引導解碼（Guided Decoding）

### 原理
在推理階段限制 token 生成空間，使其只能產生符合預定義 schema 的合法輸出。

### 實現方式
- **JSON Schema 引導**：將 OpenAPI/Swagger schema 轉換為 regex 約束
- **AST 導引**：將輸出限制為抽象語法樹的有效形式
- **ReAct 模式強制**：要求 model 必須遵守 `Thought → Action → Observation` 循環

### 工具選擇
- **Outlines**：Python library，支援 JSON Schema、regex、grammar 三種模式
- **lm-format-enforcer**：輕量級的 token-level 約束引擎
- **xgrammar**：針對 function calling 最佳化的解碼器

## 三、分階段提示（Two-Stage Prompting）

### 原理
將複雜任務拆分為多個簡單階段，每一階段只處理單一子問題，降低單次推理的認知負荷。

### 模式 A：意圖分類 → 工具選擇 → 參數填充
```
Stage 1 (Intent): 「使用者需求屬於哪一類別？A.查詢 B.設定 C.刪除」
Stage 2 (Tool Selection): 「對應类别使用哪個工具？」
Stage 3 (Parameter Filling): 「從對話中抽取所需參數」
```

### 模式 B：Plan → Execute（參見 [[hybrid-systems]]）
```
Stage 1 (Planning, SRM): 「為完成此任務需要哪些工具？順序為何？」
Stage 2 (Execution, Main SLM): 「依計畫執行每一步」
```

## 四、上下文管理策略

### 4.1 ToolRAG（動態檢索）
根據 [[arXiv:2409.00608|TinyAgent]]，對擁有上百個工具的系統：
- 不把所有工具定義放入 prompt（會淹沒 SLM）
- 使用 embedding 搜尋 top-K 相關工具定義
- 僅將選取的 K 個工具納入 prompt

### 4.2 上下文壓縮
- 每 N 步對話後，用 summarization prompt 壓縮歷史
- 保留結論性資訊，捨棄中間過程細節

### 4.3 滑動視窗注意機制
- 僅保留最近 M 步的 tool call/output
- 配合 memory buffer 存取早期結果

## 五、常見陷阱與緩解

| 陷阱 | 現象 | 緩解策略 |
|------|------|---------|
| 提示過長 | SLM 忽略尾段指示 | 限制 system prompt 長度 <500 tokens |
| 格式不一致 | 模型混合多种输出格式 | 強制使用 template + guided decoding |
| 過度依賴前置知識 | Hallucinate 不存在之工具 | 明確聲明「只使用提供的工具列表」 |
| 多步漂移 | 後續步驟偏離初始意圖 | Plan-Execute decoupling 隔離 context |
| 工具重複呼叫 | 無意義地反覆呼叫同一工具 | 在 prompt 中加入「避免重複」指令 |

## 六、小模型特化的 Few-Shot 策略

### 選擇示例的原則
1. **難度遞增**：從簡單→複雜排列 few-shot examples
2. **覆蓋边界案例**：包含 false positive/negative 的範例
3. **格式示範優先**：用正確的格式範例抵消模型天生傾向

### 數量建議
- 1B 模型：2–3 個 examples 足夠（太多反而混淆）
- 3B 模型：3–5 個 examples
- 7B 模型：5–10 個 examples

## 七、總結

小模型提示工程的核心原則是「降低推理負擔」——透過結構化輸出格式、分階段思考、以及上下文管控，讓小模型能在有限的計算預算內發揮最大潛力。最實用的起手式是：模板化提示 + guided decoding + ToolRAG。