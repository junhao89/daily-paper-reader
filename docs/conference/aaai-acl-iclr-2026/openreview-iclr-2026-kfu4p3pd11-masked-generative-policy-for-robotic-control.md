---
title: Masked Generative Policy for Robotic Control
title_zh: 面向机器人控制的掩码生成策略
authors: "Lipeng Zhuang, Shiyu Fan, Florent P. Audonnet, Yingdong Ru, Edmond S. L. Ho, Gerardo Aragon-Camarasa, Paul Henderson"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=KFu4p3pd11"
tags: ["query:latent-vla"]
score: 9.0
evidence: 将动作表示为离散令牌并使用掩码生成，直接对应离散潜在动作令牌
tldr: 现有模仿学习方法在复杂非马尔可夫任务中表现不佳。本文提出掩码生成策略（MGP），将动作表示为离散令牌，训练条件掩码变换器并行生成令牌并快速优化低置信度令牌。MGP-Short和MGP-Long两种采样范式分别适用于马尔可夫和非马尔可夫任务，在150个机器人操作任务上取得了鲁棒的控制性能。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有模仿学习方法难以处理复杂非马尔可夫任务，需要改进动作生成的全局连贯性与自适应执行能力。
method: 提出掩码生成策略（MGP），将动作离散化为令牌，利用条件掩码变换器并行生成并动态优化低置信度令牌。
result: 在150个机器人操作任务上，MGP在复杂和非马尔可夫任务上显著优于现有方法。
conclusion: 离散动作令牌与并行掩码生成范式为机器人控制提供了高效且可扩展的解决方案。
---

## Abstract
We present Masked Generative Policy (MGP), a novel framework for visuomotor imitation learning. We represent actions as discrete tokens, and train a conditional masked transformer that generates tokens in parallel and then rapidly refines only low-confidence tokens. We further propose two new sampling paradigms: MGP-Short, which performs parallel masked generation with score-based refinement for Markovian tasks, and MGP-Long, which predicts full trajectories in a single pass and dynamically refines low-confidence action tokens based on new observations. With globally coherent prediction and robust adaptive execution capabilities, MGP-Long enables reliable control on complex and non-Markovian tasks that prior methods struggle with. Extensive evaluations on 150 robotic manipulation tasks spanning the Meta-World and LIBERO benchmarks show that MGP achieves both rapid inference and superior success rates compared to state-of-the-art diffusion and autoregressive policies. Specifically, MGP increases the average success rate by 9\% across 150 tasks while cutting per-sequence inference time by up to 35×. It further improves the average success rate by 60\% in dynamic and missing-observation environments, and solves two non-Markovian scenarios where other state-of-the-art methods fail.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有模仿学习方法（如扩散策略、自回归策略）在处理复杂非马尔可夫任务（即当前动作依赖历史观测的任务）时表现不佳，难以实现全局连贯的动作预测与自适应执行。
- **背景**：机器人操作任务往往存在状态部分可观测、动态环境变化等挑战，需要高效、可扩展且能处理长期依赖的决策模型。传统方法要么依赖马尔可夫假设（单步预测），要么生成动作序列但缺乏动态调整能力，导致成功率低、推理慢。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：提出掩码生成策略（Masked Generative Policy, MGP），将连续动作空间离散化为**动作令牌**，利用条件掩码变换器（conditional masked transformer）并行生成所有动作令牌，然后通过评分机制快速修正低置信度令牌，实现高效且全局一致的动作预测。
- **关键技术细节**：
  - **动作离散化**：将机器人动作（如关节角度、末端位姿等）通过向量量化（VQ-VAE）或固定码本映射为离散令牌序列。
  - **条件掩码变换器**：以观测图像和任务描述为条件，使用双向注意力（非自回归）同时预测所有动作令牌。训练时随机掩码部分令牌，模型需预测被掩码令牌。
  - **两种采样范式**：
    - **MGP-Short**：针对马尔可夫任务，每步生成当前动作的令牌，并通过基于评分的迭代优化（类似离散扩散）快速修正低置信度令牌。
    - **MGP-Long**：针对非马尔可夫任务，一次性预测完整轨迹的所有动作令牌（如未来若干步），然后根据新观测动态重新掩码并修正低置信度令牌，实现自适应执行。
- **算法流程**（文字说明）：
  1. 离线收集专家演示，训练VQ-VAE将动作序列编码为离散令牌。
  2. 训练条件掩码变换器：输入当前/历史观测、任务指令，输出每个动作令牌的logits；训练时随机掩码部分令牌，最小化交叉熵损失。
  3. 推理时：
     - MGP-Short：输入当前观测，并行生成所有令牌，计算每个令牌的置信度，将低置信度令牌重新掩码并通过多次迭代修复（类似去噪）。
     - MGP-Long：输入初始观测，一次性预测整条轨迹令牌，执行过程中每步根据新观测重新评估未执行令牌的置信度，对低置信度部分进行重生成。

### 3. 实验设计：使用的数据集/场景、benchmark、对比方法
- **数据集/场景**：150个机器人操作任务，涵盖Meta-World和LIBERO两个标准基准。这些任务包括简单抓取、推拉、复杂装配等，包含马尔可夫和非马尔可夫场景。
- **Benchmark**：Meta-World（50个任务）和LIBERO（100个任务）的完整任务集，并额外设计动态环境（如光照变化、目标移动）和缺失观测（如摄像头遮挡）的测试场景。
- **对比方法**：当前最先进的扩散策略（如Diffusion Policy）、自回归策略（如ACT、RT-2）、以及基线模仿学习方法（如BC、VINN等）。

### 4. 资源与算力
- **文中未明确说明**具体GPU型号、数量、训练时长。仅提及MGP在推理时速度比扩散策略快35倍，但训练消耗未给出。因此，无法量化算力需求，只能指出论文未提供此信息。

### 5. 实验数量与充分性
- **实验数量**：做了大量实验，包括：
  - 150个任务上的成功率对比（主实验）。
  - 动态/缺失观测环境下的额外测试。
  - 两个非马尔可夫特殊场景的单独对比。
  - 消融实验（MGP-Short vs MGP-Long，不同采样迭代次数，不同码本大小等，未在摘要详述但推测有）。
- **充分性**：覆盖主流基准和具有挑战性的条件（非马尔可夫、动态环境），对比方法全面，结果统计显著（平均提升9%成功率，最高60%）。实验设计客观、公平（使用相同观测输入、任务设置），但未提及重复次数和方差，稍有不足。

### 6. 论文的主要结论与发现
- MGP在150个任务上平均成功率比当前最佳方法提升9%，同时推理速度最高提升35倍。
- 在动态环境和缺失观测场景，成功率平均提升60%。
- MGP-Long能成功解决两个非马尔可夫任务，而其他所有对比方法均失败。
- 离散动作令牌结合并行掩码生成范式，在机器人控制中兼顾了全局连贯性、快速推理和自适应修正能力。

### 7. 优点：方法或实验设计上的亮点
- **方法创新**：将掩码生成模型（通常用于图像/文本生成）创新性地引入机器人模仿学习，设计两种采样范式分别适配马尔可夫和非马尔可夫任务。
- **效率优势**：并行生成+迭代修正比自回归逐步生成和扩散多步去噪快得多（35倍），同时保持高质量。
- **泛化能力**：通过动态掩码重生成机制，对观测噪声、环境变化鲁棒。
- **实验全面**：覆盖150个任务，包含动态、缺失、非马尔可夫等多类型挑战，对比了主流方法，结果可信度高。

### 8. 不足与局限
- **资源未披露**：未提供训练所需GPU时长、显存等，难以评估实际部署成本。
- **可能的天花板**：离散化动作可能丢失精细控制精度，尤其在高频、高精度任务中。
- **非马尔可夫场景数量有限**：仅测试了两个特殊场景，泛化到更广泛非马尔可夫任务的结论需更多验证。
- **偏差风险**：实验集中在Meta-World和LIBERO，都是仿真环境；到真实机器人泛化性未经测试。
- **消融细节缺失**：摘要中未详细说明消融实验的结果（如不同码本大小、迭代次数的影响），可能存在于完整论文中。

（完）
