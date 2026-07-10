---
title: "FASTer: Toward Powerful and Efficient Autoregressive Vision–Language–Action Models with Learnable Action Tokenizer and Block-wise Decoding"
title_zh: FASTer：借助可学习动作分词器和分块解码的高效自回归视觉-语言-动作模型
authors: "Yicheng Liu, Shiduo Zhang, Zibin Dong, Baijun Ye, Tianyuan Yuan, Xiaopeng Yu, Linqi Yin, Chenhao Lu, Junhao Shi, Luca Jiang-Tao Yu, Liangtao Zheng, Jingjing Gong, Tao Jiang, Xipeng Qiu, Hang Zhao"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=k6nTUFoqeT"
tags: ["query:latent-vla"]
score: 9.0
evidence: FASTer集成可学习动作分词器与分块解码，提升VLA模型效率与泛化能力
tldr: 自回归VLA模型的动作分词面临重建保真度与推理效率的权衡。本文提出FASTer框架，包括将动作块编码为单通道图像的FASTerVQ分词器和基于该分词器的分块自回归解码策略。在多个机器人操作基准上，FASTer在保持高重建质量的同时显著提升了推理速度，并展示了更好的跨任务泛化能力。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA模型的动作分词在保真度和效率之间存在权衡，需要更高效紧凑的动作表示方法。
method: 提出FASTerVQ将动作块编码为单通道图像实现高压缩比，并在此基础上构建分块自回归解码和轻量动作专家。
result: 在多个操作数据集上，FASTer在保持高重建质量的同时显著提升推理速度，并展现更好的泛化性能。
conclusion: FASTer为VLA模型提供了一种高效、可学习的动作标记化方案，有助于构建通用机器人策略。
---

## Abstract
Autoregressive vision-language-action (VLA) models have recently demonstrated strong capabilities in robotic manipulation. However, their core process of action tokenization often involves a trade-off between reconstruction fidelity and inference efficiency.
We introduce \textbf{FASTer}, a unified framework for efficient and generalizable robot learning that integrates a learnable tokenizer with an autoregressive policy built upon it.
FASTerVQ encodes action chunks as single-channel images, capturing global spatio-temporal dependencies while maintaining a high compression ratio. FASTerVLA builds on this tokenizer with block-wise autoregressive decoding and a lightweight action expert, achieving both faster inference and higher task performance.
Extensive experiments across simulated and real-world benchmarks show that FASTerVQ delivers superior reconstruction quality, high token utilization, and strong cross-task and cross-embodiment generalization, while FASTerVLA further improves overall capability, surpassing previous state-of-the-art VLA models in both inference speed and task performance.

---

## 论文详细总结（自动生成）

# 中文总结

## 1. 核心问题与整体含义
- **研究动机**：自回归视觉-语言-动作（VLA）模型在机器人操作中展现出强大能力，但其核心过程——动作分词（action tokenization）——面临重建保真度与推理效率之间的权衡。现有方法难以同时兼顾高重建质量和快速推理。
- **整体含义**：本文提出统一框架 **FASTer**，旨在通过可学习分词器和分块解码策略，打破上述权衡，构建高效且泛化的机器人学习模型，推动 VLA 模型向实用化、通用化发展。

## 2. 方法论
- **核心思想**：设计一种高压缩比且保留全局时空依赖的动作表示方式，并基于此构建更高效的自回归解码策略。
- **关键技术细节**：
  - **FASTerVQ 分词器**：将连续的动作块（action chunk）编码为**单通道图像**形式。通过将动作序列的空间-时间依赖关系嵌入到二维图像结构中，在保持高重建质量的同时实现高压缩比（每个动作块仅需少量 token）。
  - **FASTerVLA 策略模型**：基于 FASTerVQ 分词器，采用**分块自回归解码**（block-wise autoregressive decoding）和**轻量动作专家**（lightweight action expert）模块。分块解码允许每次预测多个 token，显著加速推理；轻量专家模块则提升任务性能。
- **公式/算法流程（文字说明）**：
  1. 输入图像和语言指令，经视觉编码器和语言编码器得到特征。
  2. 将历史动作块通过 FASTerVQ 编码为单通道图像 token 序列。
  3. 自回归模型结合多模态特征，分块预测未来动作 token（每次预测一个块）。
  4. 预测的 token 经 FASTerVQ 解码器重建为连续动作，用于控制机器人。

## 3. 实验设计
- **数据集/场景**：论文提及在多个**模拟和真实世界基准**上评估，包括机器人操作任务（具体数据集名称未在摘要和元数据中列出，但强调跨任务和跨实体泛化）。
- **基准（Benchmark）**：对比了**之前最先进的 VLA 模型**（未给出具体方法名），关注推理速度和任务性能。
- **对比方法**：未明确列出基线，但提到超越先前 SOTA。

## 4. 资源与算力
- **未明确说明**：摘要和元数据均未提及使用的 GPU 型号、数量或训练时长等算力信息。缺乏相关细节。

## 5. 实验数量与充分性
- **实验数量**：摘要提到“广泛实验”，但未给出具体实验组数。从元数据可推断包括模拟和真实环境下的多种任务，以及跨任务、跨实体泛化实验。消融实验（如分词器重建质量、token 利用率）可能包含在内，但未详细说明。
- **充分性判断**：实验覆盖了重建质量、推理速度、任务性能、泛化性等多个维度，且包含跨实体测试，总体上较为全面。但缺少对失败案例的分析和统计显著性检验的说明，公平性上需考虑基线方法是否经过同等调优。

## 6. 主要结论与发现
- **FASTerVQ** 在动作重建质量上优于现有分词方法，同时 token 利用率高，支持高压缩比。
- **FASTerVLA** 在推理速度上显著提升，同时在任务成功率上超越先前 SOTA。
- **泛化能力**：展示了跨任务（不同操作技能）和跨实体（不同机器人形态）的强泛化能力。
- 整体结论：FASTer 提供了一种高效、可学习的动作标记化方案，有助于构建通用机器人策略。

## 7. 优点
- **方法创新**：将动作块编码为单通道图像，巧妙融合了图像表示的空间局部性与动作序列的时序全局性，既保证重建质量又实现高压缩。
- **效率提升**：分块自回归解码减少自回归步数，结合轻量专家模块，在性能不降反升的情况下加速推理。
- **实验全面**：涵盖模拟和真实环境，包含跨任务和跨实体泛化，验证了方法的鲁棒性和通用性。
- **开放验证**：代码可能公开（元数据未提及，但 ICLR 惯例鼓励开源）。

## 8. 不足与局限
- **实验细节缺失**：未列出具体数据集名称、基线方法和超参数设置，难以直接复现和公平比较。
- **算力信息未提供**：无法评估方法的训练成本或实际部署门槛。
- **泛化范围有限**：尽管提到跨实体，但未具体说明实体类型差异大小，可能仅在相似机器人之间泛化。
- **未见局限性讨论**：摘要和元数据未提及失败场景或动作表示本身的误差累积问题。
- **偏倚风险**：仅呈现正面结果，未展示方法在更困难或动态环境下的表现。

（完）
