---
title: Variational Agent Discovery
title_zh: 变分智能体发现
authors: "Aneesh Muppidi, Samuel J. Gershman, Wilka Carvalho"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=NikZZLTzjq"
tags: ["query:latent-vla"]
score: 9.0
evidence: 从像素无监督联合学习潜在动作和动力学
tldr: 针对智能体表示学习需要大量标签的问题，提出变分智能体发现方法，从像素中无监督地学习智能体为中心的表示。通过槽注意力与变分目标，联合学习逆动力学（从转换推断动作）、前向动力学（从动作预测状态）和智能体策略（动作分布）。学得的表示能够泛化到未见过的智能体和目标，支持下游动作预测和推理。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有智能体表示学习依赖监督，缺乏从像素中无监督发现潜在动作的能力。
method: 利用槽注意力和变分目标联合学习逆动力学、前向动力学和策略。
result: 无监督学习到的表示泛化到未知智能体和目标，准确预测动作。
conclusion: 无监督变分方法能从像素中有效发现潜在动作和动力学。
---

## Abstract
We introduce Variational Agent Discovery (VAD), an unsupervised agent representation learning algorithm that discovers agent-centric representations directly from pixels. We frame agent representation learning as a prediction problem where we aim to predict what latent actions describe transitions of latent variables used to model a scene. VAD leverages slot-based attention with a variational objective that jointly learns inverse dynamics (inferring actions from transitions), forward dynamics (predicting states from actions), and agent policies (distributions over actions). Without any supervision, VAD develops representations that generalize to novel agents and goals with minimal performance degradation. Our learned representations enable downstream tasks like action prediction and goal inference. Notably, VAD exhibits shared action representations across multiple observed agents—feature dimensions that consistently activate for the same action regardless of which agent performs it—and demonstrates teleological reasoning capabilities similar to 12-month-old infants, suggesting that these cognitive phenomena can emerge from our unsupervised agent representation learning objective.

---

## 论文详细总结（自动生成）

# 变分智能体发现（Variational Agent Discovery）论文总结

## 1. 核心问题与整体含义（研究动机和背景）
现有智能体表示学习严重依赖监督学习，需要大量人工标注的动作标签或环境状态，缺乏直接从像素序列中无监督地发现潜在动作和动力学的能力。受人类幼儿（如12个月婴儿）能通过观察他人行为推断意图和动作的启发，本文提出一种无监督的智能体表示学习算法——**变分智能体发现（VAD）**，旨在直接从像素中学习以智能体为中心的表示，而无需任何动作或目标标签。

## 2. 方法论
### 核心思想
将智能体表示学习建模为一个预测问题：预测哪些潜在动作描述了用于建模场景的潜在变量之间的转换。通过联合学习三个关键模块实现无监督发现：
- **逆动力学**：从连续两个状态（转换）推断潜在动作。
- **前向动力学**：从当前状态和潜在动作预测下一状态。
- **智能体策略**：学习潜在动作的分布（即动作先验）。

### 关键技术细节
- 基于**槽注意力（slot-based attention）**机制，将场景分解为多个“槽”，每个槽对应一个潜在的智能体或物体表示。
- 使用**变分目标**（variational objective）联合优化上述三个模块，形成一个自监督的循环：通过逆动力学推断潜在动作，再通过前向动力学验证预测，同时策略网络提供动作的先验分布。
- 整个框架无需真实动作标签，仅通过像素序列进行端到端训练。

## 3. 实验设计
**注意：论文摘要中仅提到泛化到未见过的智能体和目标、下游任务（动作预测和目标推理），以及展示了共享动作表示和类似婴儿的因果推理能力，但未提及具体数据集、benchmark和对比方法。** 根据摘要推断，实验可能包括：
- 在具有多个智能体的合成或简单物理场景（如网格世界、机器人操控）中进行训练和测试。
- 对比方法可能包括：有监督的智能体表示学习、其他无监督方法（如VAE、SLAC等）。
- 评估指标可能包括：动作预测准确率、目标推理正确率、表示泛化能力（迁移到新智能体/新目标时的性能下降程度）。
- 未提供具体benchmark名称，推测为自定义环境。

## 4. 资源与算力
文中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。

## 5. 实验数量与充分性
由于仅提供摘要，无法得知具体实验数量。摘要提到“generalize to novel agents and goals with minimal performance degradation”以及下游任务，但**缺少消融实验、不同任务设置、统计显著性检验等细节**。因此，现有信息不足以判断实验的充分性和公平性，需要阅读完整论文才能评估。

## 6. 主要结论与发现
- VAD能够**无监督地学习到以智能体为中心的表示**，且表示具有跨智能体的**共享动作特征维度**（同一动作在不同智能体下激活相同维度），表明出现了潜在的动作不变性表示。
- 学到的表示支持**下游动作预测和目标推理**，且泛化到未见过的智能体和目标时性能下降很小。
- 模型展现了**类似12个月婴儿的目的论推理（teleological reasoning）能力**，即能从观察中推断智能体的意图，暗示这种认知现象可从无监督学习目标中涌现。

## 7. 优点
- **无监督学习**：摆脱了对动作标签的依赖，更符合真实世界学习场景。
- **变分框架联合学习**：巧妙结合逆动力学、前向动力学和策略，形成自洽的循环，避免模式崩溃。
- **基于槽注意力**：自然地建模多智能体场景，每个槽对应一个独立实体，增强可解释性。
- **认知启发性**：所得表示表现出与婴儿类似的因果推理能力，为AI与认知科学的交叉提供新视角。

## 8. 不足与局限
- **实验信息不完整**：摘要未提供具体实验环境、数据集、对比方法和消融实验，无法评估方法的通用性和鲁棒性。
- **应用场景有限**：仅提及从像素中学习，但未涉及复杂真实世界图像（如视频中的行人），可能高估了泛化能力。
- **算力和资源未说明**：难以判断方法的训练成本可接受性。
- **理论基础依赖假设**：变分目标可能对初始化和超参数敏感，且潜在动作空间的定义可能不唯一，导致不同随机种子下表示不一致。
- **缺乏与更强基准的对比**：未提到与当前最好的无监督表示学习方法（如ACT、Dreamer等）的比较。

（完）
