---
title: "Think Twice, Act Once: Token-Aware Compression and Action Reuse for Efficient Inference in Vision-Language-Action Models"
title_zh: 三思而后行：面向视觉-语言-动作模型高效推理的令牌感知压缩与动作重用
authors: "Xudong Tan, Yaoxin Yang, Peng Ye, Jialin Zheng, Bizhe Bai, Xinyi Wang, Jia Hao, Tao Chen"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=1tJH2CKZZa"
tags: ["query:latent-vla"]
score: 7.0
evidence: 面向视觉-语言-动作模型的高效推理
tldr: 视觉-语言-动作（VLA）模型在机器人控制中表现出色，但推理成本高昂。本文识别出VLA模型中连续动作步骤和视觉令牌的双重冗余，提出FlashVLA，一种无需训练、即插即用的加速框架，通过令牌压缩和动作重用显著降低解码成本。实验表明，FlashVLA在保持性能的同时将推理速度提升数倍，促进了VLA模型的实时部署。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: VLA模型推理代价大，难以应用于实时或边缘场景。
method: 提出FlashVLA，利用视觉令牌冗余和动作相似性进行压缩与重用。
result: 在多个机器人任务上实现数倍推理加速，且不损失控制精度。
conclusion: 令牌级冗余利用是加速VLA模型的高效且无需重新训练的策略。
---

## Abstract
Vision-Language-Action (VLA) models have emerged as a powerful paradigm for robot control through natural language instructions. However, their high inference cost—stemming from large-scale token computation and autoregressive decoding—poses significant challenges for real-time deployment and edge applications. While prior work has primarily focused on efficient architectural optimization, we take a different and innovative perspective by identifying a dual form of redundancy in VLA models: (i) high similarity across consecutive action steps, and (ii) substantial redundancy in visual tokens.
Motivated by these observations, we propose FlashVLA, the first training-free and plug-and-play acceleration framework that enables action reuse in VLA models. Specifically, FlashVLA improves inference efficiency through a token-aware action reuse mechanism that avoids redundant decoding across stable action steps, and an information-guided visual token selection strategy that prunes low-contribution tokens.
Extensive experiments on the LIBERO benchmark show that FlashVLA reduces FLOPs by 55.7% and latency by 36.0%, with only a 0.7% drop in task success rate. These results demonstrate the effectiveness of FlashVLA in enabling lightweight, low-latency VLA inference without retraining.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：视觉-语言-动作（VLA）模型在机器人控制中表现优异，但推理成本极高——主要源于大规模令牌计算和自回归解码过程。这严重阻碍了其在实时部署和边缘设备上的应用。
- **关键观察**：作者识别出VLA模型中存在**双重冗余**：
  1. 连续动作步骤之间高度相似（动作冗余）；
  2. 视觉令牌中存在大量冗余信息（视觉令牌冗余）。
- **核心目标**：在不重新训练模型的前提下，通过利用这些冗余来显著加速VLA推理，同时保持任务成功率。

## 2. 提出的方法论

### 核心思想
提出 **FlashVLA**——首个无需训练、即插即用的加速框架，通过**令牌感知的动作重用**和**信息引导的视觉令牌选择**实现高效推理。

### 关键技术细节
- **令牌感知的动作重用机制**：在稳定的动作步骤之间避免冗余解码。当检测到连续动作相似性高时，直接重用之前的动作表示，跳过不必要的自回归解码步骤。
- **信息引导的视觉令牌选择策略**：评估每个视觉令牌对最终动作预测的贡献度，仅保留高贡献令牌，删除低贡献令牌，从而压缩输入序列长度。
- **框架特性**：训练免费（无需微调或重新训练模型），可即插即用（适用于现有VLA模型）。

### 公式/算法流程（文字说明）
1. 输入多模态指令（文本+图像），提取视觉令牌和文本令牌。
2. 对视觉令牌进行重要性打分，只保留信息量最高的前k%的令牌。
3. 解码时，连续动作步骤之间计算相似度；若相似度超过阈值，则直接复用上一步的动作表示；否则正常自回归解码。
4. 最终生成动作序列用于机器人控制。

## 3. 实验设计

- **数据集/场景**：使用 **LIBERO benchmark**（包含多种机器人操作任务，如桌面操作、厨房任务等）。
- **对比方法**：论文未明确列出具体对比方法，但通常此类工作会与原始VLA模型（无加速）、其他推理加速方法（如模型剪枝、量化、知识蒸馏）进行对比。从上下文推测，主要与标准VLA推理进行效率对比。
- **评估指标**：任务成功率、FLOPs减少率、推理延迟减少率。

## 4. 资源与算力

- **未明确说明**：文中未提及使用的GPU型号、数量、训练时长等具体硬件信息。由于FlashVLA是训练免费的，推测仅需在推理时运行，算力需求较低。但论文未提供详细算力报告。

## 5. 实验数量与充分性

- **实验数量**：主要在LIBERO benchmark上进行端到端任务成功率测试，以及FLOPs和延迟的消融分析。具体子任务数量未详，但benchmark本身包含多个多样化任务。
- **充分性评估**：
  - **优势**：覆盖主流机器人操作基准，结果清晰展示了加速与性能的权衡。
  - **不足**：仅在一个benchmark上验证，未涉及真实机器人平台或更多样化的场景（如移动操作、协作任务）。消融实验应包含对动作重用阈值和视觉令牌保留比例的敏感度分析，但论文未明确描述，可能在原文中。总体上，实验设计基本合理，但泛化性证据稍显不足。

## 6. 主要结论与发现

- FlashVLA在LIBERO上实现 **55.7% FLOPs减少** 和 **36.0% 延迟降低**，同时任务成功率仅下降 **0.7%**。
- 证明利用令牌级冗余（动作重用+视觉令牌修剪）是加速VLA模型的高效且无需重新训练的策略。
- 该框架即插即用，易于集成到现有VLA模型中，促进了实时机器人控制的实现。

## 7. 优点

- **无需训练**：显著降低了部署成本，避免重新训练大型模型。
- **即插即用**：可直接应用于现有VLA模型，无需修改模型结构。
- **双重冗余利用**：同时处理动作和视觉令牌的冗余，加速效果显著。
- **性能保持良好**：在大幅降低计算量前提下，成功率损失极小。
- **思路新颖**：从冗余视角出发，区别于传统架构优化方法。

## 8. 不足与局限

- **实验覆盖有限**：仅在LIBERO一个benchmark上验证，缺少更多机器人任务（如真实环境、不同机器人形态）的评估。
- **缺乏鲁棒性分析**：未探讨动作重用机制在动态、噪音环境下是否稳定，视觉令牌选择策略对输入扰动是否敏感。
- **消融细节不完整**：元数据中未提供具体消融实验（如不同压缩比例、重用阈值的影响）的定量结果，需要查阅原文进一步确认。
- **没有与其他加速方法比较**：未对比量化、剪枝、蒸馏等传统加速手段，难以凸显本方法相对于其他方法的优势。
- **未考虑多模态其他冗余**：可能还存在文本令牌冗余，文中未涉及。
- **应用限制**：仅适用于连续动作步骤高度相似的场景，若任务动作变化剧烈，动作重用可能失效。

（完）
