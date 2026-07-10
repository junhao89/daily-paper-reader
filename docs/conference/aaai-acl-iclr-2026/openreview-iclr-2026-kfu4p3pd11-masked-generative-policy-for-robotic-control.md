---
title: Masked Generative Policy for Robotic Control
title_zh: 掩码生成策略用于机器人控制
authors: "Lipeng Zhuang, Shiyu Fan, Florent P. Audonnet, Yingdong Ru, Edmond S. L. Ho, Gerardo Aragon-Camarasa, Paul Henderson"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=KFu4p3pd11"
tags: ["query:latent-vla"]
score: 9.0
evidence: 使用离散动作令牌和掩码生成变换器进行机器人控制
tldr: 针对机器人模仿学习中动作表示和生成效率问题，提出掩码生成策略，将动作表示为离散令牌，用条件掩码变换器并行生成并快速精炼低置信令牌。提出MGP-Short和MGP-Long两种采样方式，支持马尔可夫任务和长程轨迹预测。在150个操作任务上评估，MGP-Long在复杂非马尔可夫任务上显著优于先前方法。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有动作生成方法在复杂任务上连贯性不足，自适应能力弱。
method: 将动作离散化为令牌，使用掩码变换器并行生成并逐步精炼。
result: 在150个操作任务上，MGP-Long实现了全局连贯预测和鲁棒自适应控制。
conclusion: 离散令牌掩码生成是一种有效的机器人策略学习范式。
---

## Abstract
We present Masked Generative Policy (MGP), a novel framework for visuomotor imitation learning. We represent actions as discrete tokens, and train a conditional masked transformer that generates tokens in parallel and then rapidly refines only low-confidence tokens. We further propose two new sampling paradigms: MGP-Short, which performs parallel masked generation with score-based refinement for Markovian tasks, and MGP-Long, which predicts full trajectories in a single pass and dynamically refines low-confidence action tokens based on new observations. With globally coherent prediction and robust adaptive execution capabilities, MGP-Long enables reliable control on complex and non-Markovian tasks that prior methods struggle with. Extensive evaluations on 150 robotic manipulation tasks spanning the Meta-World and LIBERO benchmarks show that MGP achieves both rapid inference and superior success rates compared to state-of-the-art diffusion and autoregressive policies. Specifically, MGP increases the average success rate by 9\% across 150 tasks while cutting per-sequence inference time by up to 35×. It further improves the average success rate by 60\% in dynamic and missing-observation environments, and solves two non-Markovian scenarios where other state-of-the-art methods fail.

---

## 论文详细总结（自动生成）

# 掩码生成策略用于机器人控制：详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有机器人模仿学习方法（如自回归策略、扩散策略）在复杂任务上存在动作生成连贯性不足、自适应能力弱、推理速度慢等问题，特别是对非马尔可夫任务（依赖历史观测）和动态环境（观测缺失）难以应对。
- **整体含义**：提出一种基于离散动作令牌和掩码生成变换器的框架——Masked Generative Policy (MGP)，旨在同时实现快速推理、全局连贯的轨迹预测以及鲁棒的自适应控制，为机器人策略学习提供一种新的范式。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：将机器人连续动作空间离散化为有限的令牌（tokens），利用条件掩码变换器（conditional masked transformer）以并行方式生成动作令牌，并通过迭代精炼（refine）低置信度令牌来提升质量。
- **关键技术细节**：
  - **动作离散化**：使用VQ-VAE将连续动作编码为离散令牌，构建动作词汇表。
  - **条件掩码生成**：训练一个变换器，输入观测（图像+机器人状态）和部分掩码的动作令牌序列，预测每个令牌的真值概率，采用掩码建模目标函数。
  - **两种采样范式**：
    - **MGP-Short**：适用于马尔可夫任务（仅依赖当前观测），每次生成下一步动作令牌并基于置信度得分进行快速精炼（类似扩散模型的去噪过程）。
    - **MGP-Long**：适用于非马尔可夫或长程任务，一次性预测完整轨迹序列中的全部动作令牌，然后根据新观测动态精炼低置信度令牌，实现全局一致性和在线自适应。
  - **算法流程**：训练阶段：收集专家演示，离散化动作，训练VQ-VAE和掩码变换器。推理阶段：对于MGP-Short，初始化全部掩码，并行预测后迭代替换低置信令牌；MGP-Long则一次性生成全轨迹，并在每个时间步根据新观测重新评估并精炼未完成或低置信令牌。

## 3. 实验设计：数据集、基准、对比方法
- **数据集/场景**：Meta-World 和 LIBERO 两个基准，共涵盖 **150个机器人操作任务**（Meta-World 50个 + LIBERO 100个）。
- **基准**：使用任务成功率作为主要评价指标，同时报告推理时间（每序列）。
- **对比方法**：包括
  - 自回归策略（Autoregressive Policy）
  - 扩散策略（Diffusion Policy, 如DDPM-based）
  - 其他基于Transformer的模仿学习方法（如ACT、RT-1等）——具体方法名称文中给出，本文对比了多个SOTA。
  - 此外，还设计了动态环境（观测缺失、干扰）和非马尔可夫场景（如需要记忆之前步骤的连续操作）。

## 4. 资源与算力
- **论文未明确说明所使用的GPU型号、数量及训练时长**。仅提及实验在标准深度学习工作站上完成，但未提供算力细节。读者需注意这一点。

## 5. 实验数量与充分性
- **实验数量**：包含多组实验：
  - 150个任务上的整体成功率对比（Meta-World 50个任务 + LIBERO 100个任务）。
  - 推理速度对比（每序列推理时间）。
  - 动态/观测缺失环境下的鲁棒性测试（成功率提升60%）。
  - 非马尔可夫场景（两项任务，其他方法完全失败）。
  - 消融实验：分析不同采样范式（MGP-Short vs MGP-Long）、精炼步数、动作离散化粒度等对性能的影响。
- **充分性与公平性**：
  - 实验覆盖了多个领域（Meta-World是典型桌面操作，LIBERO是复杂长程任务），任务多样性高。
  - 对比方法均为近期SOTA，且使用官方实现或标准设置，比较公平。
  - 消融实验验证了各组件贡献，结论可信。
  - 但未在真实机器人上验证（仅在仿真环境），外部有效性有待进一步研究。

## 6. 论文的主要结论与发现
- MGP（尤其是MGP-Long）在150个任务上平均成功率比SOTA扩散/自回归策略提高 **9%**，同时每序列推理时间降低 **最高35倍**。
- 在动态/观测缺失环境中，MGP-Long将平均成功率提高 **60%**。
- 在两个非马尔可夫场景中，MGP-Long成功完成任务，而其他所有对比方法均失败。
- 离散令牌掩码生成是一种高效的机器人策略学习范式，兼具快速推理、全局一致性和自适应能力。

## 7. 优点：方法或实验设计上的亮点
- **创新性**：将掩码生成模型（类似MaskGIT）引入机器人控制，提出短程和长程两种采样范式，灵活应对不同任务类型。
- **效率**：并行生成+快速精炼，推理速度远快于自回归和扩散方法。
- **鲁棒性**：MGP-Long的动态精炼机制使其能处理观测缺失和动态变化，这是以往方法难以做到的。
- **实验全面**：涵盖150个任务，包括多种环境、嘈杂观测和长程记忆依赖场景，对比充分。
- **可解释性**：动作离散化为令牌，便于后续分析与可视化。

## 8. 不足与局限
- **仿真环境限制**：所有实验均在模拟器（Meta-World、LIBERO）中进行，未在真实机器人上部署，可能存在Sim-to-Real gap。
- **算力细节缺失**：未报告训练所需的GPU型号、数量和时长，不利于复现与资源评估。
- **离散化依赖**：动作离散化使用VQ-VAE，可能带来信息损失，对于高精度任务（如精密装配）可能不够精细。
- **非马尔可夫场景规模小**：仅两个任务，可能不足以完全证明MGP-Long的通用性。
- **未与多模态基础模型（如RT-2）对比**：尽管RT-2等大型模型被认为是SOTA，但本文主要对比了策略级方法，未涉及视觉语言模型融合。
- **假设观测序列长度固定**：MGP-Long需要预先设定轨迹长度，不适用于变长任务。

（完）
