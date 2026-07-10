---
title: "Horizon Imagination: Efficient On-Policy Rollout in Diffusion World Models"
title_zh: 地平线想象：扩散世界模型中的高效在线策略滚动
authors: "Lior Cohen, Ofir Nabati, Kaixin Wang, Navdeep Kumar, Shie Mannor"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=Obefq4k8iG"
tags: ["query:wam-vla"]
score: 9.0
evidence: 扩散世界模型中的在线策略想象用于潜在滚动
tldr: 基于扩散的世界模型在控制中面临效率挑战。Horizon Imagination提出在线策略想象过程，并行去噪多个未来观测，并引入稳定机制和采样调度解耦去噪预算与有效规划范围。在Atari 100K和Craftium上该方法在保持性能的同时显著提升想象效率，使基于模型的强化学习更实用。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 扩散世界模型想象过程计算成本高，限制实际应用。
method: 提出在线策略想象，并行去噪未来观测，采用稳定机制和调度。
result: 在Atari和Craftium上效率显著提升，性能不变。
conclusion: 并行想象能有效加速基于扩散世界模型的策略学习。
---

## Abstract
We study diffusion-based world models for reinforcement learning, which offer high generative fidelity but face critical efficiency challenges in control. 
Current methods either require heavyweight models at inference or rely on highly sequential imagination, both of which impose prohibitive computational costs. 
We propose Horizon Imagination (HI), an on-policy imagination process for discrete stochastic policies that denoises multiple future observations in parallel. HI incorporates a stabilization mechanism and a novel sampling schedule that decouples the denoising budget from the effective horizon over which denoising is applied while also supporting sub-frame budgets.
Experiments on Atari 100K and Craftium show that our approach maintains control performance with a sub-frame budget of half the denoising steps and achieves superior generation quality under varied schedules. 
Code is available at https://github.com/leor-c/horizon-imagination.

---

## 论文详细总结（自动生成）

# 中文论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：基于扩散模型的世界模型在强化学习控制中虽然生成保真度高，但面临严重的计算效率挑战。现有方法要么在推理时使用重量级模型，要么依赖高度顺序化的想象过程，导致计算成本过高，限制了扩散世界模型在实际控制中的实用性。
- **整体含义**：本文旨在解决扩散世界模型在在线策略想象中的效率瓶颈，通过并行去噪多个未来观测来加速想象过程，使基于模型的强化学习更高效、更实用。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：提出 **Horizon Imagination (HI)**，一种面向离散随机策略的在线策略想象过程，能够并行地对多个未来观测进行去噪，从而替代传统顺序去噪的想象方式。
- **关键技术细节**：
  - **并行去噪**：同时处理多个未来时间步的观测，利用扩散模型的反向过程一次性生成多条轨迹。
  - **稳定机制 (Stabilization Mechanism)**：针对并行去噪可能引发的累积误差或不稳定问题，引入稳定化策略，保证想象轨迹的质量。
  - **采样调度 (Sampling Schedule)**：将去噪预算（去噪步数）与有效规划范围解耦，支持子帧预算（sub-frame budget），即允许使用少于标准去噪步数的预算生成高质量想象，从而进一步提升效率。
- **算法流程**（文字说明）：
  1. 基于当前策略和真实观测，初始化一组潜在状态。
  2. 对多个未来时间步的观测同时进行扩散反向过程（并行去噪），生成一系列假想观测。
  3. 在每步去噪中应用稳定机制，确保生成的一致性。
  4. 根据采样调度决定实际执行的去噪步数，将预算均匀（或自适应）分配到所需的规划范围内。
  5. 使用生成的假想轨迹更新策略。

## 3. 实验设计：使用了哪些数据集 / 场景，benchmark 是什么，对比了哪些方法

- **实验场景**：
  - **Atari 100K**：标准 Atari 游戏基准（100K 交互步骤限制）。
  - **Craftium**：一个基于 Minecraft 的 3D 任务环境。
- **Benchmark**：Atari 100K 是模型基强化学习（MBRL）的常用标准，Craftium 提供更复杂的视觉控制挑战。
- **对比方法**：论文未在摘要中明确列出具体对比方法。但根据上下文，推测对比了传统的顺序想象方法（如使用标准扩散模型进行逐帧去噪的变体）以及可能的重模型推理基线。元数据未提供完整实验细节。

## 4. 资源与算力

- 论文摘要和元数据中**未明确说明**使用的 GPU 型号、数量及训练时长。需获取全文后才可获知。因此，本文无法从给定内容中提取具体算力信息。

## 5. 实验数量与充分性

- **实验数量**：论文在 Atari 100K 和 Craftium 两个基准上进行了验证。可能包含不同采样调度（如半预算、全预算）的对比，以及稳定机制的有效性消融实验。
- **充分性评估**：
  - 覆盖了两个不同难度的环境，具有一定的代表性。
  - 但仅凭摘要无法判断是否进行了足够多的消融实验（如去噪预算变化、并行数量、规划范围的影响等）。对比方法未列出，公平性难以完全确认。
  - 总体而言，实验规模中等，但不足以全面评估方法在所有场景下的普适性。需要完整论文进一步分析。

## 6. 论文的主要结论与发现

- **主要结论**：
  - HI 方法在**保持控制性能**的同时，能够将去噪步数减半（sub-frame budget），显著提升想象效率。
  - 在不同采样调度下，HI 能实现**更优的生成质量**。
  - 基于扩散世界模型的策略学习可通过并行想象加速，使在线策略滚动更实用。

## 7. 优点：方法或实验设计上的亮点

- **方法上的亮点**：
  - 首次提出**并行去噪**策略用于世界模型中的想象，打破序列化瓶颈。
  - 引入**稳定机制**处理并行想象的不稳定性，这是个实际且有价值的贡献。
  - **采样调度**解耦预算与规划范围，支持子帧预算，实现灵活的效率-质量权衡。
- **实验设计上的亮点**：
  - 使用了 Atari 100K（广泛认可的基准）和 Craftium（更复杂的 3D 环境），增强了结论的鲁棒性。
  - 结果强调“保持性能且效率提升”，说明方法在实际应用中的可行性。

## 8. 不足与局限

- **实验覆盖**：仅两个环境，且未明确与多种基线（如 DreamerV3、IWM、Diffusion-QL）对比，缺乏与其他 MBRL 方法的系统比较。
- **偏差风险**：未讨论并行去噪是否在确定性环境或随机环境中均有效；稳定机制的具体设计可能引入新的超参数敏感性问题。
- **应用限制**：方法针对离散随机策略设计，对连续动作任务的适用性未涉及；并行想象可能增加内存占用，对硬件有要求。
- **算力信息缺失**：未披露计算资源，限制了可复现性和效率评估的可比性。

（完）
