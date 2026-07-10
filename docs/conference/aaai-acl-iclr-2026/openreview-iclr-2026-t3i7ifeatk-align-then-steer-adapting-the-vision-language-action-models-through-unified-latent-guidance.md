---
title: "Align-Then-stEer: Adapting the Vision-Language Action Models through Unified Latent Guidance"
title_zh: 对齐再引导：通过统一潜在引导适应视觉-语言-动作模型
authors: "Yang Zhang, Chenwei Wang, ouyang lu, Yuan Zhao, Yunfei Ge, Zhenglong Sun, Xiu Li, Chi Zhang, Chenjia Bai, Xuelong Li"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=T3i7Ifeatk"
tags: ["query:latent-vla"]
score: 8.0
evidence: 通过潜在空间动作对齐实现VLA模型适应
tldr: 该论文提出Align-Then-stEer（ATE）框架，用于数据高效地适应预训练VLA模型到新下游任务。通过构建统一潜在空间，利用反向KL散度约束的变分自编码器对齐不同动作分布，再通过潜在引导进行任务微调，大幅减少了所需数据量和计算资源，在多种机器人操控任务上实现了高效的域迁移。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: VLA模型在预训练和下游任务之间存在动作分布不匹配，导致微调需要大量数据和计算。
method: ATE先构建统一潜在空间对齐动作分布，再使用潜在引导进行任务自适应，采用变分自编码器和反向KL散度约束。
result: 在多个操控任务上，ATE以极少的微调数据实现了与全量微调相当甚至更优的性能。
conclusion: 潜在空间对齐是解决VLA模型动作分布不匹配的高效途径，可显著降低迁移成本。
---

## Abstract
Vision-Language-Action (VLA) models pre-trained on large, diverse datasets show remarkable potential for general-purpose robotic manipulation. However, a primary bottleneck remains in adapting these models to downstream tasks, especially when the robot's embodiment or the task itself differs from the pre-training data. This discrepancy leads to a significant mismatch in action distributions, demanding extensive data and compute for effective fine-tuning. To address this challenge, we introduce Align-Then-stEer (ATE), a novel, data-efficient, and plug-and-play adaptation framework. ATE first aligns disparate action spaces by constructing a unified latent space, where a variational autoencoder constrained by reverse KL divergence embeds adaptation actions into modes of the pre-training action latent distribution. Subsequently, it steers the diffusion- or flow-based VLA's generation process during fine-tuning via a guidance mechanism that pushes the model's output distribution towards the target domain. We conduct extensive experiments on cross-embodiment and cross-task manipulation in both simulation and real world. Compared to direct fine-tuning of representative VLAs, our method improves the average multi-task success rate by up to 9.8% in simulation and achieves a striking 32% success rate gain in a real-world cross-embodiment setting. Our work presents a general and lightweight solution that greatly enhances the practicality of deploying  VLA models to new robotic platforms and tasks. Our code is released at \url{https://github.com/TeleHuman/Align-Then-Steer}.

---

## 论文详细总结（自动生成）

# 论文《Align-Then-stEer: Adapting the Vision-Language Action Models through Unified Latent Guidance》详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大规模预训练的视觉-语言-动作模型（VLA）在通用机器人操控中表现突出，但将其适配到新的下游任务时，存在严重的动作分布不匹配问题——即预训练数据与目标任务/机器人本体之间的动作空间差异巨大，导致微调需要大量数据和计算资源。
- **研究动机**：现有的直接微调（full fine-tuning）虽然有效，但数据效率和计算效率低下，限制了VLA模型在新机器人平台和任务上的实际部署。
- **整体含义**：本文提出一种数据高效、即插即用的适配框架，通过先对齐动作分布、再引导生成过程，大幅降低迁移成本，增强VLA模型的实际可用性。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将适配过程分为两个阶段：① 对齐（Align）：构建统一潜在空间，使下游动作嵌入到预训练动作分布的“模式”中；② 引导（steer）：在微调时通过潜在引导机制，推动VLA模型的生成输出向目标域偏移。
- **关键技术细节**：
  - **统一潜在空间构建**：使用变分自编码器（VAE），并通过**反向KL散度约束**（reverse KL divergence）迫使适配动作的潜在表示嵌入到预训练动作潜在分布的模态（modes）中。这样新动作与预训练动作在潜在空间中对齐。
  - **引导机制**：用于扩散模型或流模型（flow-based）的VLA。在微调过程中，通过一个额外的引导信号（guidance）调整模型的生成过程，使输出分布向目标域靠近。
  - **整体流程**：先训练一个受反向KL约束的VAE（对齐阶段），然后利用该VAE的潜在表示作为引导，对原有VLA模型进行轻量微调（引导阶段）。
- **公式/算法**：论文未在摘要中提供具体公式，但描述了反向KL散度约束和VAE的联合训练，以及引导机制的具体实现（可参考论文正文）。

## 3. 实验设计：数据集/场景、基准方法、对比方法

- **数据集/场景**：
  - **模拟环境**：交叉本体（cross-embodiment）和交叉任务（cross-task）操控任务，具体任务类型未在摘要中详述。
  - **真实世界**：真实机器人交叉本体操控任务。
- **基准方法**：代表性的VLA模型（如RT-2等）的直接微调（full fine-tuning）。
- **对比方法**：ATE与直接微调对比，评估多任务成功率。

## 4. 资源与算力

- **明确说明**：摘要和元数据中**未提及**具体使用的GPU型号、数量、训练时长等信息。仅有“轻量级（lightweight）”的描述，但未给出具体算力数值。
- **推测**：由于强调数据高效和计算高效，可能使用了少量GPU（如1-4张A100），但无确认信息。

## 5. 实验数量与充分性

- **实验数量**：
  - 模拟环境：交叉本体和交叉任务多个场景。
  - 真实世界：一项交叉本体任务。
  - 此外，元数据中暗示了消融实验（如对齐+引导 vs 仅微调等），但摘要未明确列出。
- **充分性与客观性**：
  - **优点**：覆盖了模拟和真实世界，并涉及不同本体和任务，具有一定代表性。
  - **不足**：实验环境/任务细节未充分描述（如具体任务类型、数据量大小），且仅与直接微调做了对比，缺乏与其他适配方法（如LoRA、adapter tuning等）的比较。消融实验未在摘要中展开，公平性无法完全评估。

## 6. 论文的主要结论与发现

- **核心发现**：通过潜在空间对齐（反向KL约束VAE）再配合引导微调，ATE方法在**极少数据**下即可达到**全量微调相当甚至更优的性能**。
- **量化结果**：
  - 模拟环境中，平均多任务成功率提升最高达**9.8%**。
  - 真实世界交叉本体设置中，成功率提升高达**32%**。
- **结论**：潜在空间对齐是解决VLA模型动作分布不匹配的高效途径，可显著降低迁移成本，提升VLA模型的实用性。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - **即插即用**：无需修改VLA模型主干，仅需额外训练一个对齐VAE并添加引导机制。
  - **数据高效**：大幅减少下游任务所需的微调数据量。
  - **反向KL散度约束**创新地引导下游动作嵌入到预训练分布的模态中，避免破坏预训练知识。
- **实验亮点**：
  - 同时涵盖模拟和真实世界，验证方法在真实机器人上的有效性。
  - 交叉本体和交叉任务设置，评估了泛化能力。
  - 与直接微调对比，提升显著。

## 8. 不足与局限

- **实验覆盖不足**：未公开具体任务细节、数据量大小，也未与其他高效适配方法（如LoRA、prompt tuning）对比，基准单一。
- **偏差风险**：仅基于特定VLA模型（不确定是哪一种）评估，可能对模型结构敏感。
- **应用限制**：
  - 需要预训练VLA模型提供潜在空间，且对齐阶段仍需要少量目标域数据。
  - 引导机制依赖于扩散/流模型，不适用于其他类型（如自回归）的VLA。
- **算力与资源未公开**：无法判断方法的实际计算成本。
- **消融实验信息不足**：摘要中未详细展示各组件贡献，无法确认对齐和引导的各自实际影响力。

（完）
