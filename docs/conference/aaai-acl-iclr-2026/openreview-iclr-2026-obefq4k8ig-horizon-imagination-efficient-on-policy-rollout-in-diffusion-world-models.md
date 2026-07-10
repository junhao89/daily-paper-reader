---
title: "Horizon Imagination: Efficient On-Policy Rollout in Diffusion World Models"
title_zh: Horizon Imagination：扩散世界模型中的高效on-policy展开
authors: "Lior Cohen, Ofir Nabati, Kaixin Wang, Navdeep Kumar, Shie Mannor"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=Obefq4k8iG"
tags: ["query:wam-vla"]
score: 9.0
evidence: 提出扩散世界模型中的高效on-policy展开用于基于模型的机器人策略
tldr: 该论文针对扩散世界模型在控制中想象成本高的问题，提出Horizon Imagination，通过并行去噪未来观测和创新的采样调度，将去噪预算与有效想象解耦。在Atari和Craftium上，方法在维持性能的同时大幅降低计算开销，使扩散世界模型可用于实时策略学习。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 扩散世界模型想象过程计算昂贵，难以用于实时on-policy控制。
method: 提出并行去噪和去噪预算与想象解耦的采样调度。
result: 在基准上计算效率提升数倍，控制性能与顺序方法相当。
conclusion: 并行想象策略可实用化扩散世界模型在机器人控制中的应用。
---

## Abstract
We study diffusion-based world models for reinforcement learning, which offer high generative fidelity but face critical efficiency challenges in control. 
Current methods either require heavyweight models at inference or rely on highly sequential imagination, both of which impose prohibitive computational costs. 
We propose Horizon Imagination (HI), an on-policy imagination process for discrete stochastic policies that denoises multiple future observations in parallel. HI incorporates a stabilization mechanism and a novel sampling schedule that decouples the denoising budget from the effective horizon over which denoising is applied while also supporting sub-frame budgets.
Experiments on Atari 100K and Craftium show that our approach maintains control performance with a sub-frame budget of half the denoising steps and achieves superior generation quality under varied schedules. 
Code is available at https://github.com/leor-c/horizon-imagination.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：基于扩散模型的世界模型（diffusion world model）在强化学习中能够生成高保真度的未来观测，但其推理过程（即“想象”过程）计算开销极大。现有方法要么使用重型模型进行推理，要么依赖高度顺序化的想象（sequential imagination），这两种方式都导致计算成本过高，无法满足实时 on-policy 控制的需求。
- **整体含义**：该论文旨在提出一种高效、并行的想象策略，使得扩散世界模型能够被实际应用于需要实时决策的机器人控制或强化学习场景，在不显著牺牲控制性能的前提下大幅降低计算开销。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：Horizon Imagination（HI）是一种用于离散随机策略的 on-policy 想象过程，它通过并行去噪多个未来观测来加速想象。
- **关键技术细节**：
  - **并行去噪**：同时去噪多个时间步的未来观测，而非传统方法那样顺序地逐个生成。
  - **稳定机制**：提出一种稳定化机制，保证并行去噪过程生成的观测序列的连贯性和合理性。
  - **新颖采样调度**：将去噪预算（denoising budget）与有效想象长度（effective horizon）解耦，允许使用较少的去噪步数生成较长的未来轨迹；同时支持“子帧预算”（sub-frame budget），即每个观测可以使用少于完整去噪步数的预算。
- **算法流程（文字说明）**：
  1. 输入当前观测，通过扩散模型同时初始化多个未来时间步的噪声观测。
  2. 使用自定义调度，在所有时间步上并行执行去噪步骤，每个时间步可能分配不同的去噪步数（子帧预算）。
  3. 应用稳定机制调整中间结果，确保想象的轨迹真实可靠。
  4. 将去噪后的未来观测序列作为想象轨迹，用于策略优化（如模型预测控制或策略梯度）。

## 3. 实验设计：使用的数据集/场景、benchmark、对比方法

- **数据集/场景**：
  - Atari 100K（Atari 游戏 benchmark，仅使用 100K 环境交互步数）。
  - Craftium（一个类似 Minecraft 的 3D 环境，用于评估复杂任务）。
- **Benchmark**：Atari 100K 是强化学习领域广泛使用的数据高效学习 benchmark；Craftium 用于评估在更复杂环境下的生成与控制性能。
- **对比方法**：论文主要对比了“顺序想象方法”（即传统逐时间步扩散生成的方法）以及可能的不同调度策略。摘要指出 HI 在维持控制性能的同时，计算效率提升数倍，且生成质量优于不同调度下的顺序方法。

## 4. 资源与算力

- **文中未明确说明**：在提供的摘要和元数据中，没有提及所使用的 GPU 型号、数量、训练时长等算力信息。
- **备注**：若要完整评估方法效率，建议查看完整论文的“实验设置”或“计算资源”部分。此处无法推断。

## 5. 实验数量与充分性

- **实验数量**：主要在两个环境（Atari 100K 和 Craftium）上进行实验，并进行了不同去噪预算和调度下的对比。摘要中提及“在多种调度下”验证生成质量。
- **充分性与客观性**：
  - **优点**：Atari 100K 是公认的标准化 benchmark，Craftium 提供了高维度视觉环境的额外评估，覆盖了从简单到复杂的环境。
  - **不足**：仅报告了整体性能（控制分数、生成质量），缺乏对其他环境（如连续控制任务、真实机器人）的泛化实验；消融实验（如稳定机制的影响、不同预算的影响）的具体数量未知；对比方法可能不够全面（缺少与非扩散世界模型的比较）。总体而言，实验设计合理但覆盖范围有限，可视为初步验证。

## 6. 论文的主要结论与发现

- HI 方法能够在 Atari 100K 和 Craftium 上使用一半的去噪步数（子帧预算）维持与顺序想象方法相当的控制性能，同时计算开销大幅降低。
- 在多种调度条件下，HI 生成的想象轨迹质量优于顺序方法。
- 该工作证明了并行化想象过程可以使扩散世界模型在实时 on-policy 控制中实用化。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - 首次将并行去噪引入扩散世界模型的 on-policy 想象，从根本上解决顺序生成的计算瓶颈。
  - 提出去噪预算与想象长度的解耦机制，允许灵活分配计算资源，支持子帧预算，实现“用更少步骤想象更长未来”。
  - 稳定机制确保了并行生成的时空一致性。
  - 代码开源，易于复现和扩展。
- **实验亮点**：
  - 使用标准且具有挑战性的 benchmark（Atari 100K）和一个较新的 3D 环境（Craftium），结果具有说服力。
  - 直接比较了相同去噪预算下并行与顺序方法的性能，公平性较好。

## 8. 不足与局限

- **实验覆盖不足**：仅在离散控制环境（Atari、Craftium）上评估，未在连续控制任务（如 MuJoCo、DM Control）或真实机器人环境中验证，泛化性存疑。
- **对比方法有限**：仅对比了顺序扩散方法，未与更高效的世界模型（如基于 VAE、RNN 的方法）比较，也未说明与最新扩散策略（如扩散策略直接生成动作）的差异。
- **计算细节缺失**：缺少详细的算力消耗报告，无法准确评估方法的实际效率增益与硬件要求。
- **潜在偏差**：并行去噪可能对长时依赖或复杂动态（如物理接触、遮挡）的建模能力不足，论文未分析失败案例或边界情况。
- **应用限制**：当前方法假设离散随机策略，对连续动作空间策略的适用性未探讨；子帧预算的调度规则可能需针对具体环境手动调整。

（完）
