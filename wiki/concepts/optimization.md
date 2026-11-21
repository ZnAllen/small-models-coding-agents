---
title: "小模型的效率優化技術"
slug: optimization-techniques-small-models
category: concept
tags: [optimization, quantization, pruning, reinforcement-learning, small-language-model]
summary: "彙整提升小模型 Coding Agent 效能的三大類技術：量化剪枝（GPTQ/AWQ/FP8）、RL 後訓練（GRPO/PPO），以及合成數據訓練管道，並比較各方法的 effectiveness 與 trade-offs。"
confidence: medium
created: 2026-11-21
last_updated: 2026-11-21
see_also: [[small-model-tool-calling-problems]], [[hybrid-systems]]
---

# 小模型的效率優化技術

即使經過 fine-tuning，原生 SLM 仍然可能因 hardware 限制而無法部署。本節系統性地探討三類優化技術及其在 Coding Agent 場景中的适用性。

## 一、量化（Quantization）

### 1.1 主流方法比較

| 方法 | -bit 精度 | 速度增益 | 精度損失 | 適用場景 |
|------|----------|---------|---------|---------|
| **FP16/BF16** | 16-bit | baseline | 幾乎無 | 基準線，reference |
| **INT8** | 8-bit | ~1.5× | 輕微 | 大多數 SLM 的可接受下限 |
| **GPTQ** | 4-bit | ~3× | 中等 | 需要平衡 speed/accuracy |
| **AWQ** | 4-bit | ~3× | 較 GPTQ 小 | 对 weight distribution 敏感模型 |
| **FP8** | 8-bit (E4M3/E5M2) | ~1.8× | 最小 | NVIDIA GPU 硬體加速 |
| **INT4/IQ4_XS** | 4-bit | ~4× | 較大 | 極端 edge case |

### 1.2 量化對 Function Calling 的影響

根據 T3 research seed 方向的未竟研究：
- INT8 量化對 FC accuracy 影響通常 <2pp（在 SFT 後的模型上）
- GPTQ-4bit 可能導致 complex nested call 的 accuracy 下降 5–10pp
- AWQ 在保持重要 weight 的情况下，FC 性能損耗小於同等 bit 的 GPTQ
- FP8 是 emerging standard，尤其適合 Llama/Mistral 家族

### 1.3 實用建議

```
部署場景                推薦量化等級
─────────────────────────────────────────────
桌面/伺服器 (GPU)       INT8 或 FP8
邊緣裝置 (NPU)          AWQ-4bit 或 GPTQ-4bit
極端受限裝置            IQ4_XS (需 extensive testing)
```

## 二、剪枝（Pruning）

### 2.1 Structured Pruning
- 移除整個 attention head 或 FFN 層
- 對 MoE 模型特別有效（已有 sparse activation）
- DeepSeek-Coder MoE 架構已是天然的 pruned 版本

### 2.2 Unstructured Pruning
- 逐參數移除不重要的 weight
- 需要特殊硬體才能有效率執行
- 目前在 SLM 領域應用有限

### 2.3 與量化的組合
- 先剪枝再量化（prune-then-quantize）通常是最佳順序
- 組合減速可達 4–6×，精度損失可控在 3–5pp 內

## 三、RL 後訓練（RL Post-Training）

### 3.1 原理
在完成 SFT 之後，使用 reinforcement learning（主要是 GRPO 或 PPO）進一步優化模型的 tool-use behavior。

### 3.2 Reward Signal 設計
```python
def compute_reward(completion, ground_truth):
    rewards = []
    
    # Format correctness (+0.3)
    if parses_as_valid_json(completion):
        rewards.append(0.3)
    
    # Tool selection accuracy (+0.4)
    if completion.tool_name == ground_truth.tool_name:
        rewards.append(0.4)
    
    # Parameter correctness (+0.3)
    if matches_schema(completion.arguments, ground_truth.args):
        rewards.append(0.3)
    
    return sum(rewards)  # Max 1.0
```

### 3.3 穩定性挑戰
根據 [[arxiv:2602.03022|STAR]] 的研究：
- 直接對小模型使用 RL 極度不穩定
- Reward signal 容易爆炸或消失
- 建議搭配 similarity-guided teacher refinement

### 3.4 GRPO vs PPO
| 特性 | GRPO | PPO |
|------|------|-----|
|  sample efficiency | 高 | 中 |
|  variance | 低 | 高 |
|  implementation complexity | 較簡 | 較複雜 |
|  在小模型上的表現 | 較穩定 | 需仔細調參 |

## 四、合成數據訓練管道

### 4.1 ToolACE Pipeline（参考 [[arXiv:2506.23394]]）
```
Seed API pool (26,507 APIs)
    ↓
Multi-agent dialog synthesis
    ↓
Dual-layer verification (rule-based + model-based)
    ↓
Train SLM on verified trajectories
    ↓
Iterate: re-synthesize failures
```

### 4.2 STAR 的合成數據方法（[[arXiv:2602.03022]]）
- Teacher model 生成 synthetic trajectories
- Similarity-based filtering 確保 diversity
- 錯誤軌跡被標記並用於 contrastive training

## 五、綜合決策樹

```
需要部署小模型 Coding Agent？
    ↓
hardware constraint?
    ├─ 無特別限制 → SFT-only（ simplest ）
    ├─ 記憶體緊張 → AWQ-4bit 或 FP8
    └─ 極端限制 → GPTQ-4bit + pruning
    
performance not sufficient?
    ├─ Format errors dominant → Template Training + Guided Decoding
    ├─ Tool selection poor → RL post-training (GRPO preferred)
    ├─ Multi-step failing → Plan-Execute decoupling
    └─ General capability → Synthetic data self-evolution loop
```

## 六、注意事項

1. **不要為了 speed 犧牲可靠性**：Coding Agent 的 tool call 錯誤比 slow response 更昂貴
2. **量化前務必在目標 benchmark 上測試**：不同模型家族對量化的敏感度差異很大
3. **RL 不是銀彈**：需配合穩定的 training infrastructure 與好的 reward design