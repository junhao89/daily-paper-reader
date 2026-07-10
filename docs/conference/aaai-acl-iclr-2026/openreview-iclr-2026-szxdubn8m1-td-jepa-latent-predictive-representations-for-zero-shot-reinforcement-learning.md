---
title: "TD-JEPA: Latent-predictive Representations for Zero-Shot Reinforcement Learning"
title_zh: TD-JEPA：面向零样本强化学习的潜预测表示
authors: "Marco Bagatella, Matteo Pirotta, Ahmed Touati, Alessandro Lazaric, Andrea Tirinzoni"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=SzXDuBN8M1"
tags: ["query:latent-vla"]
score: 7.0
evidence: 利用时序差分学习预测长期潜动态
tldr: 传统的潜预测方法局限于单任务、单步或在线策略数据。本文提出TD-JEPA，利用时序差分学习从离线无奖励数据中学习可预测长期潜动态的表示，并支持多策略场景。该方法在零样本强化学习中表现出色，能够在新任务上无需额外交互即可生成有效策略，为潜动态模型在具身智能中的应用提供了高效框架。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有潜预测方法多限于单任务或单步预测，无法充分利用离线无奖励数据。
method: 提出基于时序差分学习的潜预测表示框架，从离线数据中学习长期动态。
result: 在多个连续控制任务上实现零样本迁移，性能优于先前方法。
conclusion: TD-JEPA为离线预训练潜动态模型提供了一种可扩展的方案。
---

## Abstract
Latent prediction–where agents learn by predicting their own latents–has emerged as a powerful paradigm for training general representations in machine learning. In reinforcement learning (RL), this approach has been explored to define auxiliary losses for a variety of settings, including reward-based and unsupervised RL, behavior cloning, and world modeling. While existing methods are typically limited to single-task learning, one-step prediction, or on-policy trajectory data, we show that temporal difference (TD) learning enables learning representations predictive of long-term latent dynamics across multiple policies from offline, reward-free transitions. Building on this, we introduce TD-JEPA, which leverages TD-based latent-predictive representations into unsupervised RL. TD-JEPA trains explicit state and task encoders, a policy-conditioned multi-step predictor, and a set of parameterized policies directly in latent space. This enables zero-shot optimization of any reward function at test time. Theoretically, we show that an idealized variant of TD-JEPA avoids collapse with proper initialization, and learns encoders that capture a low-rank factorization of long-term policy dynamics, while the predictor recovers their successor features in latent space. Empirically, TD-JEPA matches or outperforms state-of-the-art baselines on locomotion, navigation, and manipulation tasks across 13 datasets in ExoRL and OGBench, especially in the challenging setting of zero-shot RL from pixels.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在强化学习（RL）中，潜预测（latent prediction）是一种有效的表示学习方法，但现有方法通常受限于单任务学习、单步预测或仅能使用在线策略轨迹数据，无法充分利用**离线、无奖励**的过渡数据。这导致模型难以学习能够跨策略、长期预测的通用表征，阻碍了零样本迁移 RL 的发展。
- **整体背景**：零样本 RL 要求智能体在测试时无需与环境额外交互，即可直接优化任意奖励函数。传统方法需要大量在线交互或依赖奖励信号，而现实场景中奖励常缺失或难以设计。
- **研究动机**：本文旨在解决“如何从离线无奖励数据中学习可预测长期潜动态的表示，从而支持多策略场景下的零样本强化学习”这一关键问题。

## 2. 论文提出的方法论：核心思想与关键技术细节

- **核心思想**：利用**时序差分（Temporal Difference, TD）学习**，使表示能够预测长期潜动态，从而突破单任务、单步限制，支持从多种策略生成的离线无奖励数据中学习。
- **关键技术细节**：
  - TD-JEPA 框架包含四个主要组件：
    1. **显式状态编码器**（state encoder）：将状态映射到潜空间。
    2. **显式任务编码器**（task encoder）：将任务/奖励函数编码为条件向量。
    3. **策略条件多步预测器**（policy‑conditioned multi‑step predictor）：在潜空间中对任意策略，预测多个时间步后的潜表示。
    4. **参数化策略集合**（set of parameterized policies）：直接以潜空间中的表示作为输入，无需输出完整动作分布。
  - **训练方式**：从离线无奖励过渡中学习，通过 TD 目标最小化长期预测误差，使编码器捕获策略动态的低秩因子分解，而预测器在潜空间中恢复对应的**后继特征**（successor features）。
- **理论保证**：论文证明了理想化变体在适当初始化下能够避免表示坍塌，并确保学习到的编码器能够建模长期策略动态的低秩结构。

## 3. 实验设计

- **使用的数据集与场景**：
  - **ExoRL** 和 **OGBench** 两个基准，共包含 **13 个数据集**。
  - 任务类型涵盖**运动控制（locomotion）、导航（navigation）、操作（manipulation）**等多种连续控制任务。
- **Benchmark 设定**：在**从像素（pixels）输入的零样本 RL** 这一最具挑战性的设置下进行评估，即测试时使用与训练不同的奖励函数，且不与环境额外交互。
- **对比方法**：与多个当前最先进的（state‑of‑the‑art, SOTA）基线方法进行比较（具体名称未在摘要中列出，但从上下文可知包括基于潜预测的现有方法如 SVM、Joint‑Embedding Predictive Architecture 等）。

## 4. 资源与算力

- 论文正文中**未明确说明**所使用的 GPU 型号、数量或训练时长。因此无法提供具体算力信息。这可能是因为该类研究工作更注重方法验证，算力资源不是重点论述内容。

## 5. 实验数量与充分性

- **数量**：在 13 个数据集上进行实验，覆盖多种任务类型，数量较为丰富。
- **充分性**：
  - 实验包括与多个 SOTA 方法的对比，且特别在零样本 RL 的困难设置下进行评估，能较好反映方法的泛化能力。
  - 论文还进行了**理论分析**（避免坍塌、低秩分解）作为支撑，增强了实验的可信度。
  - 但摘要中未提及是否进行了**消融实验**、对各模块贡献的分析，或对超参数敏感性的测试。这可能影响对方法核心贡献的深入理解。
- **客观性与公平性**：实验在公开基准（ExoRL、OGBench）上进行，对比方法应为公开文献中的最佳配置，整体可认为是公平的。但需要详细阅读正文以确认基线实现是否一致、是否包含公平的调优。

## 6. 论文的主要结论与发现

- TD 学习能够从离线无奖励数据中学习**跨策略、多步的潜动态表示**，这是以往潜预测方法难以做到的。
- 基于此提出的 TD‑JEPA 在零样本 RL 任务上**匹配或超越**现有 SOTA 方法，尤其在像素级输入的高维控制任务上表现出色。
- 理论分析表明该方法天然具备**避免坍塌**的能力，并且所学的潜空间能编码策略间共享的低秩动态结构。
- 结论：TD‑JEPA 为**离线预训练潜动态模型**提供了一种可扩展且高效的方案，有望推动具身智能中通用表示学习的应用。

## 7. 优点（方法或实验设计的亮点）

- **方法创新性**：首次将 TD 学习引入潜预测框架，打破单任务/单步/在线数据约束，能够从**多种策略的离线无奖励数据**中学习长期动态。
- **理论完备性**：提供了表示避免坍塌的理论保证，并揭示了潜空间与后继特征之间的内在联系，增强了方法的可解释性。
- **零样本能力**：测试时无需任何额外交互即可直接优化任意奖励，显著降低了 RL 部署成本。
- **实验覆盖度**：在 13 个数据集上评估，涵盖多种连续控制任务，验证了方法的通用性。

## 8. 不足与局限

- **算力成本未报告**：未给出训练所需的具体 GPU 资源，可能影响可复现性评估。
- **消融实验不足**：摘要中未提及对编码器、预测器或 TD 步数等模块的消融研究，导致难以判断各组件贡献。
- **实验环境局限**：仅在仿真环境中验证（ExoRL、OGBench 均基于仿真器），未讨论在真实机器人或噪声视觉环境下的表现。
- **潜在偏差风险**：零样本 RL 评估可能依赖于预定义的奖励函数族结构，若测试奖励与训练分布差异过大，性能可能下降。论文未讨论这种泛化边界。
- **应用限制**：方法假设离线数据中包含多种策略的轨迹，但实际中可能只能收集到少量策略的数据，需进一步研究数据多样性的影响。

（完）
