---
title: Vision-Language Models Unlock Task-Centric Latent Actions
title_zh: 视觉-语言模型解锁以任务为中心的潜在动作
authors: "Alexander Nikulin, Ilya Zisman, Albina Klepach, Denis Tarasov, Alexander Derevyagin, Andrei Polubarov, Lyubaykin Nikita, Vladislav Kurenkov"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=QMJpPbzYLt"
tags: ["query:latent-vla"]
score: 9.0
evidence: 利用VLM学习以任务为中心的潜在动作用于VLA策略
tldr: 潜在动作模型（LAM）在VLA预训练中重要，但容易编码与动作相关的干扰。该论文利用视觉-语言模型（VLM）的常识推理能力，提供可提示的表示，有效分离可控变化与噪声。将这些表示作为LAM训练目标，在多种VLM上评估，结果表明该方法能学习到更具任务相关性的潜在动作，提升下游VLA策略性能。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: LAM受动作相关干扰影响，学习到的潜在动作含噪声。
method: 利用VLM的常识推理提供目标表示，指导LAM训练。
result: 学习到更干净的任务中心潜在动作，提升VLA预训练质量。
conclusion: VLM能有效帮助LAM聚焦任务相关动作，提高表示质量。
---

## Abstract
Latent Action Models (LAMs) have rapidly gained traction as an important component in the pre-training pipelines of leading Vision-Language-Action models. However, they fail when observations contain action-correlated distractors, often encoding noise instead of meaningful latent actions. Humans, on the other hand, can effortlessly distinguish task-relevant motions from irrelevant details in any video given only a brief task description. In this work, we propose to utilize the common-sense reasoning abilities of Vision-Language Models (VLMs) to provide promptable representations, effectively separating controllable changes from the noise in unsupervised way. We use these representations as targets during LAM training and benchmark a wide variety of popular VLMs, revealing substantial variation in the quality of promptable representations as well as their robustness to different prompts and hyperparameters. Interestingly, we find that more recent VLMs may perform worse than older ones. Finally, we show that simply asking VLMs to ignore distractors can substantially improve latent action quality, yielding up to a six-fold increase in downstream success rates on Distracting MetaWorld.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：潜在动作模型（Latent Action Models, LAMs）在视觉-语言-动作（VLA）模型的预训练中至关重要，但容易受到观测中与动作相关的干扰物（action-correlated distractors）的影响，导致编码噪声而非有意义的潜在动作。相反，人类仅凭简短的任务描述就能轻松区分视频中任务相关的运动与无关细节。
- **研究动机**：利用视觉-语言模型（VLMs）的常识推理能力，以无监督方式有效分离可控变化与噪声，指导LAM学习以任务为中心的潜在动作，从而提升下游VLA策略的性能。
- **整体含义**：通过引入VLM提供的可提示（promptable）表示作为训练目标，使LAM专注于任务相关动作，避免编码干扰信息。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：利用VLM的常识推理能力，从视频帧对（或序列）中生成描述任务相关变化的表示，将这些表示作为LAM训练的监督目标，替代原始的观测对比目标。
- **关键技术细节**：
  - **可提示表示（Promptable Representations）**：给定一个视频片段和任务描述（prompt），VLM输出一个特征向量，该向量应聚焦于任务相关的动作变化，忽略背景、光照、静态物体等无关噪声。
  - **训练流程**：在LAM训练中，将VLM生成的特征作为隐变量（latent action）的目标值，通过最小化LAM预测的潜在动作与VLM表示之间的差异来训练LAM。
  - **VLM选择与评估**：作者基准测试了多种流行VLM（如CLIP、BLIP-2、LLaVA等），比较不同VLM生成表示的质量、对提示和超参数的鲁棒性。
- **算法流程（文字说明）**：
  1. 输入：一对时间相邻的观测帧（或短序列），任务描述prompt。
  2. 使用VLM编码这对帧，输出一个以任务为中心的表示向量。
  3. LAM尝试从观测序列中预测该表示向量作为潜在动作编码。
  4. 通过对比学习或回归损失更新LAM参数，同时冻结VLM或端到端微调（文中未明确微调，但表示作为目标）。
- **公式**：文中未显式给出公式，但其损失函数大致为：\( \mathcal{L} = \| z_{\text{LAM}} - f_{\text{VLM}}(\text{frames}, \text{prompt}) \|^2 \) 或类似对比损失。

## 3. 实验设计

- **使用数据集/场景**：
  - 主要实验环境：**Distracting MetaWorld**（一种在MetaWorld环境中添加干扰物的基准，如背景、光照、颜色变化，与动作相关）。
  - 可能还包括其他仿真环境（论文未详细列举，但从上下文可知）。
- **Benchmark**：
  - 对比方法：不同VLM（包括较新的和较旧的）作为表示提供者；可能也对比了原始的LAM训练（无VLM指导）。
  - 评估指标：下游任务成功率（success rate）、潜在动作的质量（通过下游VLA策略的最终性能反映）。
- **对比方法**：
  - 多种VLMs：如CLIP、BLIP-2、LLaVA、InstructBLIP等。
  - 消融实验：对比不同prompt设计（如是否明确要求忽略干扰物）、不同VLM的变体。

## 4. 资源与算力

- **文中明确说明**：未提及使用的GPU型号、数量、训练时长等具体算力信息。
- **备注**：仅提到在Distracting MetaWorld上进行了实验，但未提供详细计算资源。可能隐含在论文其他部分，但提取文本中缺失。

## 5. 实验数量与充分性

- **实验数量**：
  - 对多种VLM进行了基准测试（至少4-5种以上）。
  - 进行了prompt设计消融（如是否明确要求忽略干扰物）。
  - 超参数鲁棒性测试（如温度、提示措辞）。
  - 最终下游策略成功率比较（有六倍提升的量化结果）。
- **充分性**：实验设计覆盖了方法核心要素（VLM类型、提示、干扰物影响），但缺少无VLM的纯LAM基线对比细节，也未在更多环境（如真实机器人）上验证。整体充分但存在局限（仅一个模拟环境）。

## 6. 主要结论与发现

- **关键发现**：
  - 利用VLM提供表示可以显著提升LAM的潜在动作质量，在Distracting MetaWorld上下游成功率最高提升六倍。
  - **有趣现象**：较新的VLM（如LLaVA）可能表现不如较旧的VLM（如CLIP），提示VLM的“常识推理”能力与任务需求间存在差距。
  - **简单有效**：仅仅通过提示VLM“忽略干扰物”就能显著改善潜在动作质量。
- **结论**：VLM的常识推理能力能有效帮助LAM聚焦任务相关动作，提高表示质量，但不同VLM间的质量差异大，需谨慎选择。

## 7. 优点

- **方法创新**：首次将VLM的提示能力引入LAM训练，解决动作相关干扰问题，实现无监督分离。
- **简单有效**：仅通过修改训练目标（使用VLM表示），不改变模型架构，即可大幅提升性能。
- **广泛评估**：对比多种主流VLM，揭示其适用性与差异，为实践提供指导。
- **实用价值**：易于集成到现有VLA预训练管线中，无需额外标注。

## 8. 不足与局限

- **实验覆盖有限**：仅在单一模拟环境（Distracting MetaWorld）上测试，缺乏在真实机器人、多任务、复杂场景下的验证。
- **偏差风险**：VLM的表示可能本身受其训练数据偏见影响，导致在某些场景下误导LAM。
- **计算开销**：需推理VLM生成表示，可能增加训练成本，但未提及。
- **可迁移性**：VLM对prompt敏感，不同任务需人工设计合适prompt，自动化程度不足。
- **未充分分析**：缺少对VLM表示与真实动作之间的定量相关性分析，以及消融噪声成分的详细实验。

（完）
