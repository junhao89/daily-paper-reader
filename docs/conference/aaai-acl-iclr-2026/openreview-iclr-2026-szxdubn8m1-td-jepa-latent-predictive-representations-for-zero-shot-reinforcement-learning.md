---
title: "TD-JEPA: Latent-predictive Representations for Zero-Shot Reinforcement Learning"
title_zh: TD-JEPA：面向零样本强化学习的潜在预测表示
authors: "Marco Bagatella, Matteo Pirotta, Ahmed Touati, Alessandro Lazaric, Andrea Tirinzoni"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=SzXDuBN8M1"
tags: ["query:latent-vla"]
score: 7.0
evidence: 基于TD学习的长期潜在动力学预测表示
tldr: 现有潜在预测方法通常局限于单任务、单步或在线轨迹数据，TD-JEPA引入时序差分学习，从离线无奖励数据中学习跨策略的长期潜在动力学预测表示，有效支撑零样本强化学习、行为克隆和世界建模等任务，拓展了潜在动力学模型在具身控制中的应用。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有潜在预测方法无法从离线无奖励数据中学习跨策略的长期动力学。
method: 利用时序差分学习构建跨策略的长期潜在动力学预测表示。
result: 在零样本强化学习等任务中展现出优越的表示能力和泛化性能。
conclusion: TD-JEPA为具身智能体提供了通用的潜在动力学表示学习框架。
---

## Abstract
Latent prediction–where agents learn by predicting their own latents–has emerged as a powerful paradigm for training general representations in machine learning. In reinforcement learning (RL), this approach has been explored to define auxiliary losses for a variety of settings, including reward-based and unsupervised RL, behavior cloning, and world modeling. While existing methods are typically limited to single-task learning, one-step prediction, or on-policy trajectory data, we show that temporal difference (TD) learning enables learning representations predictive of long-term latent dynamics across multiple policies from offline, reward-free transitions. Building on this, we introduce TD-JEPA, which leverages TD-based latent-predictive representations into unsupervised RL. TD-JEPA trains explicit state and task encoders, a policy-conditioned multi-step predictor, and a set of parameterized policies directly in latent space. This enables zero-shot optimization of any reward function at test time. Theoretically, we show that an idealized variant of TD-JEPA avoids collapse with proper initialization, and learns encoders that capture a low-rank factorization of long-term policy dynamics, while the predictor recovers their successor features in latent space. Empirically, TD-JEPA matches or outperforms state-of-the-art baselines on locomotion, navigation, and manipulation tasks across 13 datasets in ExoRL and OGBench, especially in the challenging setting of zero-shot RL from pixels.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有基于潜在预测的表示学习方法（如JEPA系列）在强化学习中表现出潜力，但普遍存在三个限制：① 仅支持单任务学习；② 仅进行单步预测；③ 需要在线策略轨迹数据。这些缺陷阻碍了从大规模离线、无奖励数据中学习通用的跨策略潜在动力学表示。
- **核心问题**：如何从离线、无奖励的过渡数据中学习能够预测长期潜在动力学、并支持跨策略泛化的表示，以实现零样本强化学习（Zero-Shot RL）。
- **整体含义**：TD-JEPA将时序差分学习（TD）引入潜在预测框架，使得学习到的表示能够捕获长期策略依赖的动力学结构，为具身智能体提供一种通用的潜在动力学表示学习范式，从而在无奖励数据上预训练后直接优化任意奖励函数。

## 2. 论文提出的方法论

- **核心思想**：利用时序差分（TD）学习，从离线、无奖励的过渡数据中训练一个策略条件的多步潜在预测器，同时学习显式的状态编码器和任务编码器，并在潜在空间中直接参数化一组策略。
- **关键技术细节**：
  - 训练目标：通过TD误差最小化长期潜在预测与目标潜变量之间的差异，使编码器捕获低秩的长期策略动力学分解，预测器恢复潜在空间中的后继特征（successor features）。
  - 结构组件：① 状态编码器（state encoder）；② 任务编码器（task encoder）；③ 策略条件多步预测器（policy-conditioned multi-step predictor）；④ 一组潜在空间中的参数化策略。
  - 理论保证：理想化变体在适当初始化下可避免表示坍缩，且编码器捕获的因子分解与策略动力学低秩结构一致。
- **算法流程（文字说明）**：
  1. 从离线数据集中采样无奖励过渡片段；
  2. 使用状态编码器将当前状态映射到潜在空间；
  3. 给定候选策略嵌入，预测器输出多步后的潜变量预测；
  4. 通过TD更新使预测逼近未来实际编码；
  5. 测试时，对于任意奖励函数，任务编码器将其映射为潜在空间中的线性目标，利用预训练的策略集直接优化获取最优策略。

## 3. 实验设计

- **数据集/场景**：
  - **ExoRL** 和 **OGBench** 两个基准，共13个数据集。
  - 任务类型涵盖：运动（locomotion）、导航（navigation）、操作（manipulation）。
  - 特别关注从像素输入进行零样本RL的挑战性设置。
- **基准对比**：与当前最先进的（state-of-the-art）基线方法进行比较，包括基于奖励的RL、无监督RL、行为克隆、世界建模等类别的代表性方法（具体方法名称未在摘要中列出，但声称匹配或超越它们）。
- **对比方法**：文中未列出具体方法名称，仅提到“state-of-the-art baselines”。

## 4. 资源与算力

- 论文提供的摘要/元数据中**未明确说明**所使用的算力资源（如GPU型号、数量、训练时长等）。
- 仅可推断是常规的深度强化学习训练配置，但无法给出具体数字。

## 5. 实验数量与充分性

- **实验数量**：在13个不同的数据集上进行了评估，覆盖三个不同类型的任务域（运动、导航、操作），并且包含从像素输入和状态输入两种条件。
- **充分性评估**：
  - 优点：数据集规模较大，任务类型多样，且包含零样本设置，验证了泛化能力。
  - 不足：摘要中未提及消融实验、超参数敏感性分析、表示维度影响等；也没有说明是否在多个随机种子下重复；未报告基线方法的比较细节和统计显著性。因此实验充分性属于中等水平，需阅读全文才能判断。

## 6. 论文的主要结论与发现

- TD-JEPA能够从离线无奖励数据中学习到跨策略的长期潜在动力学预测表示，避免表示坍缩。
- 理论上证明了编码器可捕获低秩的策略动力学分解，预测器恢复后继特征。
- 在运动、导航、操作等13个数据集的零样本RL任务中，匹配或超越现有最先进方法，尤其是在像素级零样本RL场景中表现突出。

## 7. 优点

- **方法创新性**：首次将时序差分学习与潜在预测框架结合，克服了单任务、单步、在线数据的限制，实现了跨策略、多步、离线的表示学习。
- **理论支撑**：给出了避免坍缩的理论条件以及表示结构（低秩因子分解、后继特征恢复），增强了方法的可解释性。
- **实用性**：支持零样本优化任意奖励函数，无需在测试时重新训练，适用于具身控制场景中的快速适应。
- **实验覆盖**：在多个基准和任务类型上验证，尤其包含像素级输入这一实际挑战。

## 8. 不足与局限

- **实验细节缺失**：未报告具体基线方法名称、超参数设置、计算资源、重复次数、统计显著性，消融实验也未提及，使得重现和比较存在困难。
- **应用限制**：方法假设离线数据集包含足够的策略多样性（跨策略），若数据仅来自单一策略，学习可能退化；此外，任务编码器需依赖奖励函数的特定表示形式，可能对复杂奖励函数的设计有要求。
- **偏差风险**：仅基于ExoRL和OGBench，可能无法涵盖所有具身控制场景（如稀疏奖励、长 horizon 任务）；像素级实验的图片分辨率、环境视觉复杂度未说明。
- **理论假设**：理想化变体要求适当初始化，实践中是否容易满足尚需验证。

（完）
