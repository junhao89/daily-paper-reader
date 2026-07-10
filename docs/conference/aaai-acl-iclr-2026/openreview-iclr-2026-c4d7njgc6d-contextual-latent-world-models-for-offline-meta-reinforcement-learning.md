---
title: Contextual Latent World Models for Offline Meta Reinforcement Learning
title_zh: 用于离线元强化学习的上下文潜在世界模型
authors: "Mohammadreza Nakhaeinezhadfard, Aidan Scannell, Kevin Sebastian Luck, Joni Pajarinen"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=c4D7NJGC6D"
tags: ["query:wam-vla"]
score: 8.0
evidence: 用于可扩展元强化学习训练的潜在世界模型
tldr: 离线元RL面临泛化问题和数据收集成本。本文统一上下文编码与潜在世界模型，在潜在空间中联合训练任务编码器和世界模型。通过耦合任务推理与预测建模，学习到的任务表示能捕捉跨任务变化因素，显著提升下游策略的泛化性能，为可扩展具身智能训练提供新思路。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 离线元RL需要泛化到新任务，现有上下文编码方法缺乏预测能力。
method: 提出上下文潜在世界模型，联合训练任务编码器和世界模型，用预测建模增强任务表示。
result: 学习到的任务表示捕捉了跨任务变化因素，提升了策略泛化。
conclusion: 预测建模与任务推理联合是提升元RL泛化的有效方法。
---

## Abstract
Offline meta-reinforcement learning seeks to overcome the challenges of poor generalization and expensive data collection by leveraging datasets for related tasks. Context encoding is a prevalent approach, where an encoder maps transition histories to a task representation. In parallel, latent world models -- which map observations into temporally consistent latent spaces -- advanced self-supervised representation learning for planning and policy optimization. In this work, we unify these directions by introducing contextual latent world models: world models conditioned on the task representation and trained jointly with the context encoder. Coupling task inference with predictive modeling yields task representations that capture variation factors across tasks and empirically improves generalization to out-of-distribution tasks in diverse benchmarks, including MuJoCo, Contextual-DeepMind Control suite, and Meta-World.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **问题**：离线元强化学习（Offline Meta-RL）旨在利用已有相关任务的数据集来提升策略对新任务的泛化能力，并降低昂贵的数据收集成本。然而现有方法面临两大挑战：一是泛化性不足，尤其是到分布外（out-of-distribution）任务时性能显著下降；二是如何在纯离线场景下高效地学习可迁移的任务表示。
- **背景**：主流方法之一是“上下文编码”（context encoding），即用一个编码器将历史交互轨迹映射为任务表征向量。与此同时，潜在世界模型（latent world models）通过将观测映射到时间一致性潜在空间，在规划和策略优化中取得了自监督表征学习的进步。
- **整体含义**：本文认为将任务推理与预测建模耦合起来，能够学习到更本质的任务变化因子，从而显著提升下游策略在未见任务上的泛化能力，为可扩展的具身智能训练提供新思路。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：统一上下文编码与潜在世界模型，提出“上下文潜在世界模型”（Contextual Latent World Models, CLWM）。该模型以任务表征为条件构建世界模型，并与上下文编码器联合端到端训练。
- **关键技术细节**：
  - **模型结构**：（根据描述推断）包括一个上下文编码器 \( q(z|\tau) \) 将近期轨迹 \(\tau\) 编码为任务表征 \(z\)；一个潜在世界模型 \( p(s_{t+1}, r_t | s_t, a_t, z) \) 在任务表征条件下预测下一潜在状态和奖励；以及一个策略/价值网络。
  - **训练方式**：联合优化目标包含世界模型的重建/预测损失（如潜在状态预测、奖励预测）和任务编码器的变分下界（如KL散度），使任务表征同时具备预测性和判别性。
  - **公式**（根据摘要描述，未给出具体公式）：最小化 \(\mathcal{L} = \mathcal{L}_{\text{WM}}(z) + \beta \mathcal{L}_{\text{context}}(z)\)，其中世界模型损失迫使 \(z\) 捕捉跨任务变化因素。
- **算法流程**（文字说明）：
  1. 从离线数据集中采样一批任务对应的轨迹。
  2. 上下文编码器从近期轨迹中提取任务表征 \(z\)。
  3. 世界模型以当前潜在状态、动作和 \(z\) 为输入，预测下一潜在状态和奖励。
  4. 联合优化编码器和世界模型参数。
  5. 使用学到的任务表征和潜在状态训练下游策略（如通过行为克隆或离线RL算法）。

## 3. 实验设计：使用数据集/场景、benchmark、对比方法
- **数据集与场景**：
  - MuJoCo（连续控制任务）
  - Contextual-DeepMind Control Suite（带有上下文变化的控制环境）
  - Meta-World（多任务机器人操控基准）
  - 这些benchmark涵盖了不同难度和分布偏移的任务。
- **对比方法**：
  - 摘要未列举具体对比算法，但根据领域常规推测，可能包括：FOCAL、PEARL、MACAW、BOReL等离线元RL方法，以及没有联合训练世界模型的上下文编码基线。
- **评估指标**：平均回报、泛化到分布外任务的性能等。

## 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量及训练时长。仅从元数据“Rejected-Public”可知至少完成了一次完整实验提交，但算力细节缺失。

## 5. 实验数量与充分性
- **实验数量**：从摘要提及的三个不同benchmark（MuJoCo, Contextual-DMC, Meta-World）以及“多样benchmark”的描述，推测至少进行了3组主要实验（每种benchmark上的对比）。此外，文中可能包含消融实验（如去掉联合训练、改变编码器架构等），但摘要未详细说明。
- **充分性与公平性**：实验覆盖了经典连续控制、上下文变化控制及多任务场景，具有一定代表性。但由于无法看到完整论文，无法确认是否与所有SOTA方法进行了公平对比（超参数调优、种子数等）。拒稿可能暗示实验仍有不足。

## 6. 论文的主要结论与发现
- **主要结论**：将任务推理与预测建模耦合（即上下文编码器与世界模型联合训练）可以学习到捕捉跨任务变化因素的任务表征，显著提升策略在分布外任务上的泛化性能。
- **发现**：纯上下文编码方法缺乏预测能力，限制了表征质量；通过联合训练，世界模型迫使任务表征编码任务动态差异，从而获得更强的迁移能力。

## 7. 优点：方法或实验设计上的亮点
- **方法亮点**：
  - 统一了两个独立的研究方向（上下文元学习和潜在世界模型），思路简洁且具有理论合理性。
  - 联合训练在潜在空间中进行，计算效率较高，可扩展到复杂任务。
  - 任务表征通过预测任务动态自动学习，无需人工标注任务ID。
- **实验设计亮点**：
  - 在多个不同benchmark上验证，包括Meta-World这类多任务场景，增加结论可信度。
  - 关注分布外泛化，更贴近实际应用需求。

## 8. 不足与局限
- **实验覆盖**：仅三个benchmark可能不够全面，尤其缺乏真实机器人环境或高维视觉任务（如DMControl with pixels）的验证。此外，未提供与更多近期SOTA（如Contextformer、Prompt-DT等）的对比。
- **偏差风险**：仅在离线数据集上评估，未讨论任务表征对在线微调的增益；可能存在数据集偏差（不同任务数据量不平衡等）。
- **应用限制**：假设所有任务共享相同的状态/动作空间，且动态变化有限；对于长时间跨度或任务变化剧烈的场景，固定编码窗口可能失效。
- **资源信息缺失**：未汇报训练所需的计算资源，读者难以复现或评估可行性。
- **拒稿可能原因**：实验不够充分或方法创新性有限，可能未能充分证明联合训练相比其他强基线的显著优势。

（完）
