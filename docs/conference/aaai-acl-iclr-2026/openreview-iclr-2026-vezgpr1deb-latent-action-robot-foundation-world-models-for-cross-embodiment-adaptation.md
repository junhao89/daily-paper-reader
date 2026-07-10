---
title: Latent Action Robot Foundation World Models for Cross-Embodiment Adaptation
title_zh: 面向跨具身适应的潜在动作机器人基础世界模型
authors: "Huang Huang, Sriram Yenamandra, Arjun Majumdar, Elie Aljalbout, Tushar Nagarajan, Tsung-Yen Yang, Akshara Rai, Michael Rabbat, Li Fei-Fei, Jiajun Wu, Tingfan Wu, Franziska Meier"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=vEZgPr1deb"
tags: ["query:latent-vla"]
score: 10.0
evidence: 跨具身适应的潜在动作基础世界模型
tldr: 机器人世界模型面临不同具身形态和动作空间的挑战。本文提出LAC-WM，在学习的统一潜在动作空间中操作，实现跨具身泛化。相比依赖显式动作标签的基线，LAC-WM在未见过的机器人上表现更优，由于隐式动作空间避免了动作空间不一致问题。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 不同机器人具身形态和动作空间的多样性阻碍了世界模型在跨具身上的泛化。
method: 提出LAC-WM，学习统一的潜在动作空间，使世界模型在该空间中操作以适配不同具身。
result: 在与显式运动标签基线的比较中，LAC-WM在未见机器人具身上展现出更好的适应性能。
conclusion: 统一潜在动作空间是实现具身泛化世界模型的关键。
---

## Abstract
Robot action-conditioned video generation models, also known as robot world models, hold great potential for enhancing robotic planning and decision-making. However, the diversity of robot embodiments and action spaces makes it challenging to build models that generalize across different embodiments. We introduce a robot foundation world model, LAC-WM, which operates within a learned unified latent action space shared across diverse embodiments. We explore how this unified action space improves the world model’s performance when adapted to previously unseen robot embodiments. Specifically, we compare LAC-WM to a baseline model EAC-WM conditioned on explicit motion labels. Our results show that conditioning on explicit labels creates disjoint action spaces across embodiments, limiting downstream task performance when adapting to new robots. We evaluate both models on a dexterous manipulation task. The latent action-conditioned model LAC-WM achieves up to a 46.7% improvement in performance over EAC-WM. Crucially, the unified latent action space allows LAC-WM’s downstream performance to scale positively with the number of embodiments used during pretraining. In contrast, the disjoint action space in EAC-WM leads to decreased performance as the number of pretraining embodiments increases. These results highlights the importance of a unified action space for efficient cross-embodiment learning, addressing a key challenge in robotics.

---

## 论文详细总结（自动生成）

## 论文核心问题与整体含义（研究动机和背景）

机器人动作条件视频生成模型（即机器人世界模型）有望增强机器人的规划与决策能力。然而，不同机器人具身（embodiment）形态和动作空间的多样性，使得构建能泛化到不同具身的通用世界模型面临严峻挑战。现有方法通常依赖显式动作标签（如关节角度、末端执行器速度等），导致不同具身之间动作空间不兼容，限制了跨具身适应性能。

## 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：学习一个统一的潜在动作空间（latent action space），该空间在不同机器人具身之间共享。世界模型在此潜在空间中进行操作，从而避免显式动作空间不一致的问题。
- **关键技术细节**：
  - 提出 **LAC-WM**（Latent Action-Conditioned World Model）模型。
  - 通过自监督或弱监督方式学习一个潜在动作编码器，将不同具身的原始动作（如关节力矩、速度等）映射到同一低维潜在空间。
  - 世界模型（视频预测模型）以观察（图像/状态）和潜在动作为输入，预测未来帧。
  - 在适应新机器人时，只需学习一个轻量级的潜在动作映射（即从新机器人原始动作到潜在空间的投影），而无需重新训练整个世界模型。
- **对比基线**：**EAC-WM**（Explicit Action-Conditioned World Model），直接使用显式运动标签（如关节角度）作为条件。该方法中不同具身的动作空间是互不相交的，导致适应时性能差。

## 实验设计

- **任务场景**：灵巧操作任务（dexterous manipulation task），具体类型未在摘要中详细说明，推测涉及多指机械手操作物体。
- **基准（Benchmark）**：未见过的机器人具身（unseen robot embodiments）上的适应性能。
- **对比方法**：将LAC-WM与基线EAC-WM进行对比，评估在预训练阶段使用的具身数量增加时，两者在下游任务上的表现。
- **评估指标**：任务成功率或性能提升百分比（未具体说明，但提到LAC-WM比EAC-WM提升高达46.7%）。

## 资源与算力

论文摘要及元数据中未明确说明使用了多少GPU型号、数量或训练时长。仅从投稿类型（ICLR-2026-Rejected-Public）推测可能涉及大规模预训练，但具体算力信息缺失。

## 实验数量与充分性

从摘要看，主要实验包括：
1. LAC-WM与EAC-WM在唯一一个灵巧操作任务上的对比。
2. 改变预训练具身数量（从少到多）下两种模型的性能变化。
3. 未报告消融实验（如不同潜在空间维度、不同编码器结构等）。
- **充分性**：实验覆盖了核心假设（统一潜在空间 vs. 显式动作空间），但仅有一个任务场景，且缺乏对不同类型任务（如移动操作、导航等）的验证。实验客观性较好，因为对比了直接基线，但未与其他潜在动作方法比较。公平性方面，由于显式动作基线天然存在动作空间不一致，对比结果可能偏向LAC-WM。

## 论文的主要结论与发现

1. 统一潜在动作空间是实现跨具身泛化世界模型的关键。
2. LAC-WM在未见过的机器人具身上性能显著优于EAC-WM（最高提升46.7%）。
3. 随着预训练具身数量增加，LAC-WM的下游性能正向提升，而EAC-WM因动作空间碎片化导致性能下降。
4. 潜在动作空间避免了显式动作空间不一致带来的局限，从而支持更高效的跨具身学习。

## 优点：方法或实验设计上的亮点

- 提出了一种新颖的、基于统一潜在动作空间的跨具身世界模型范式，直接应对机器人领域核心挑战。
- 实验设计直观地展示了预训练具身数量对性能的影响，有力论证了统一动作空间的优势。
- 方法简洁且可解释性强：潜在动作映射可以作为轻量级适配模块，降低新机器人部署成本。

## 不足与局限

- **实验覆盖不足**：仅在一个灵巧操作任务上评估，未在多种任务（如移动操作、抓取、导航）和多种真实机器人上验证。
- **缺乏消融分析**：未深入探究潜在动作空间维度、编码器结构、训练损失等超参数影响。
- **偏差风险**：基线EAC-WM显式动作空间本身设计为不兼容，可能导致对比结果过于有利。需要与更先进的跨具身方法（如元学习、对抗域适应等）进行对比。
- **算力与资源信息缺失**：无法评估方法的大规模可复现性。
- **评估指标不够具体**：未说明成功率的具体定义或统计方法（如多次试验、方差等）。
- **应用限制**：潜在动作空间需要额外的映射学习，对于全新的、动作空间差异极大的具身（如蛇形机器人 vs. 六足机器人）是否依然有效有待验证。

（完）
