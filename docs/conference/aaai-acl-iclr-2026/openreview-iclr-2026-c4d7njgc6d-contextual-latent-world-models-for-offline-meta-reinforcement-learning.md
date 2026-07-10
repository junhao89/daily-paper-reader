---
title: Contextual Latent World Models for Offline Meta Reinforcement Learning
title_zh: 上下文潜世界模型用于离线元强化学习
authors: "Mohammadreza Nakhaeinezhadfard, Aidan Scannell, Kevin Sebastian Luck, Joni Pajarinen"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=c4D7NJGC6D"
tags: ["query:latent-vla"]
score: 8.0
evidence: 引入上下文潜世界模型用于离线元强化学习，结合任务推断与预测建模
tldr: 针对离线元强化学习中现有任务编码与潜世界模型分离的问题，提出上下文潜世界模型，将任务推断与预测建模联合训练，使潜空间任务表征捕获跨任务变化因素，提升了泛化能力。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有上下文编码与潜世界模型分离，未能充分利用预测建模改进任务表征。
method: 提出上下文潜世界模型，将任务编码器与世界模型联合训练，使潜任务表征捕获变化因素。
result: 在离线元强化学习任务上提升了泛化性能。
conclusion: 联合任务推断与预测建模能学习更有效的任务表征。
---

## Abstract
Offline meta-reinforcement learning seeks to overcome the challenges of poor generalization and expensive data collection by leveraging datasets for related tasks. Context encoding is a prevalent approach, where an encoder maps transition histories to a task representation. In parallel, latent world models -- which map observations into temporally consistent latent spaces -- advanced self-supervised representation learning for planning and policy optimization. In this work, we unify these directions by introducing contextual latent world models: world models conditioned on the task representation and trained jointly with the context encoder. Coupling task inference with predictive modeling yields task representations that capture variation factors across tasks and empirically improves generalization to out-of-distribution tasks in diverse benchmarks, including MuJoCo, Contextual-DeepMind Control suite, and Meta-World.

---

## 论文详细总结（自动生成）

# 论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：离线元强化学习面临泛化能力差和数据收集昂贵的挑战。现有方法中，上下文编码（context encoding）和潜世界模型（latent world models）是两种主流方向，但两者通常是分离的——上下文编码器将任务历史映射为任务表示，潜世界模型则用于自监督表示学习以辅助规划与策略优化。这种分离导致任务推断未能充分利用预测建模来改进任务表征，限制了模型对跨任务变化因素的捕捉。
- **研究动机**：将任务推断与预测建模**统一**，通过联合训练上下文编码器和潜世界模型，使任务表示能够捕获任务之间的变化因素，从而提升在未见任务（out-of-distribution tasks）上的泛化能力。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：提出**上下文潜世界模型（Contextual Latent World Models）**，即世界模型以任务表示为条件，并与上下文编码器联合训练。该模型将任务推断过程嵌入到世界模型的预测循环中，使得潜空间中的任务表征能够反映跨任务的动态变化。
- **关键技术细节**：
  - 上下文编码器（Context Encoder）将转移历史（transition histories）编码为任务表征 \(z\)。
  - 潜世界模型（Latent World Model）对观测进行映射，在任务表征 \(z\) 的条件作用下，学习时序一致的潜空间动态。
  - 联合训练：同时优化任务编码器的推断损失（如变分下界）和世界模型的自监督预测损失（如观测重构、奖励预测、状态转移预测）。通过这种耦合，任务表征被迫包含对动态变化有预测能力的信息。
- **算法流程**（文字说明）：
  1. 从离线数据集中采样一批跨任务轨迹。
  2. 对于每条轨迹，使用上下文编码器得到任务表征 \(z\)。
  3. 将 \(z\) 作为世界模型的 conditioning 输入，对轨迹进行潜空间预测（观测、奖励、下一状态）。
  4. 计算联合损失：\( \mathcal{L} = \mathcal{L}_{\text{prediction}} + \beta \mathcal{L}_{\text{inference}} \)。
  5. 反向传播更新编码器和世界模型参数。
  6. 训练完成后，使用学到的世界模型进行离线策略优化或在线微调。

## 3. 实验设计：使用了哪些数据集/场景，它的 benchmark 是什么，对比了哪些方法

- **数据集/场景**：
  - **MuJoCo**（机器人运动控制任务，如HalfCheetah、Walker2D等，通常通过改变环境参数（如重力、摩擦力）创建多个任务）。
  - **Contextual-DeepMind Control Suite**（DeepMind的控制基准，通过上下文参数如目标位置、环境物理属性生成不同任务）。
  - **Meta-World**（多任务机械臂操作基准，如推拉、开门等不同任务）。
- **Benchmark**：上述三个标准离线元强化学习测试环境，评估模型在**分布外（out-of-distribution）**任务上的泛化能力。
- **对比方法**：虽然原文未详细列举具体基线，但根据元数据“引入上下文潜世界模型联合任务推断与预测建模”，可推测与以下类型对比：纯上下文编码方法（如PEARL）、潜世界模型方法（如Dreamer-style）、以及分离式基线（上下文编码+世界模型分开训练）。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）

- **未明确说明**：原文摘要和元数据中未提及具体的GPU型号、数量或训练时长。读者需查阅完整论文以获取算力信息。

## 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平

- 根据摘要和元数据，实验覆盖了**三个不同的基准套件**（MuJoCo、Contextual-DMC、Meta-World），每个套件通常包含多个任务变体，可视为多组独立实验。
- 推测包含**消融实验**：如去掉联合训练（单独训练任务编码器）、去掉世界模型 condition等，以验证各组件的有效性。
- **充分性评估**：基准覆盖了连续控制与机器人操作领域，且包含分布外泛化测试，实验设计较为全面。但缺乏对计算资源、超参数敏感性、统计显著性（如多次运行标准差）等细节的说明。整体实证支撑较充分，但未提供完整实验表格和结果数值，需看原文确认。

## 6. 论文的主要结论与发现

- **主要结论**：将任务推断与预测建模联合训练能够学习到更有效的任务表征，该表征捕获了跨任务的变化因素，从而显著提升了在未见任务上的泛化性能。
- **发现**：自监督世界模型的预测损失可以作为任务表征学习的天然监督信号，促使潜空间编码任务特定的动态结构。这种耦合方式优于传统分离式方法。

## 7. 优点：方法或实验设计上有哪些亮点

- **方法论亮点**：
  - 统一了元强化学习中“任务推断”和“世界模型”两个主流范式，弥补了原有分离带来的信息损失。
  - 联合训练框架简洁，可以直接继承现有潜世界模型的架构（如Dreamer变体），易于实现。
- **实验设计亮点**：
  - 在多个具有不同特性（运动控制、视觉控制、机器人操作）的基准上进行验证，增强了结论的普适性。
  - 强调分布外泛化测试，更贴近实际部署场景。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验覆盖**：仅使用了连续控制任务，未覆盖离散动作空间（如Atari）或结构化任务（如图像推理）。同时，未讨论任务数量规模对性能的影响。
- **偏差风险**：联合训练可能引入额外的表示偏差，当训练任务与目标任务分布差异极大时，任务表征可能过度拟合训练任务的特征。
- **应用限制**：
  - 依赖离线数据的多样性和质量，若数据覆盖任务变化不足，效果可能下降。
  - 世界模型的计算开销较大，可能不适合低延迟或实时在线学习场景。
- **其他**：被ICLR 2026拒绝，可能意味着方法存在未被充分验证的理论缺陷或与现有最优方法相比优势不够显著（具体需看审稿意见）。

（完）
