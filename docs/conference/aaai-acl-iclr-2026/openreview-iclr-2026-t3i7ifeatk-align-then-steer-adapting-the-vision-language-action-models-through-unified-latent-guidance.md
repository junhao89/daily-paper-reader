---
title: "Align-Then-stEer: Adapting the Vision-Language Action Models through Unified Latent Guidance"
title_zh: "Align-Then-stEer: 通过统一潜在引导适应视觉-语言-动作模型"
authors: "Yang Zhang, Chenwei Wang, ouyang lu, Yuan Zhao, Yunfei Ge, Zhenglong Sun, Xiu Li, Chi Zhang, Chenjia Bai, Xuelong Li"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=T3i7Ifeatk"
tags: ["query:latent-vla"]
score: 9.0
evidence: 通过VAE构建统一潜空间进行VLA模型适应
tldr: 针对VLA模型在下游任务中动作分布失配的问题，提出ATE框架。首先通过逆KL散度约束的变分自编码器将不同动作空间对齐到统一潜在空间，然后在该潜空间中引导策略动作生成。该方法即插即用，数据效率高，在多种异构操作任务上显著降低微调数据需求。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: VLA模型预训练后面对不同载体或任务时动作分布失配，微调需要大量数据。
method: 使用VAE构建统一潜空间对齐动作空间，并在潜空间中进行引导。
result: 在多个下游操作任务上显著降低微调数据量，提升适应效果。
conclusion: 统一潜在空间引导是一种高效、通用的VLA适应策略。
---

## Abstract
Vision-Language-Action (VLA) models pre-trained on large, diverse datasets show remarkable potential for general-purpose robotic manipulation. However, a primary bottleneck remains in adapting these models to downstream tasks, especially when the robot's embodiment or the task itself differs from the pre-training data. This discrepancy leads to a significant mismatch in action distributions, demanding extensive data and compute for effective fine-tuning. To address this challenge, we introduce Align-Then-stEer (ATE), a novel, data-efficient, and plug-and-play adaptation framework. ATE first aligns disparate action spaces by constructing a unified latent space, where a variational autoencoder constrained by reverse KL divergence embeds adaptation actions into modes of the pre-training action latent distribution. Subsequently, it steers the diffusion- or flow-based VLA's generation process during fine-tuning via a guidance mechanism that pushes the model's output distribution towards the target domain. We conduct extensive experiments on cross-embodiment and cross-task manipulation in both simulation and real world. Compared to direct fine-tuning of representative VLAs, our method improves the average multi-task success rate by up to 9.8% in simulation and achieves a striking 32% success rate gain in a real-world cross-embodiment setting. Our work presents a general and lightweight solution that greatly enhances the practicality of deploying  VLA models to new robotic platforms and tasks. Our code is released at \url{https://github.com/TeleHuman/Align-Then-Steer}.

---

## 论文详细总结（自动生成）

### 论文总结：Align-Then-stEer: 通过统一潜在引导适应视觉-语言-动作模型

#### 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：视觉-语言-动作（VLA）模型在大规模多样数据集上预训练后展现出强大的通用机器人操作潜力，但将其适配到下游任务时面临主要瓶颈——机器人的实体形态或任务本身与预训练数据存在差异，导致动作分布显著失配。直接微调需要大量数据和计算资源，效率低下。
- **问题本质**：如何实现数据高效、即插即用的VLA模型适配，以应对跨实体（cross-embodiment）和跨任务（cross-task）场景下的动作分布偏移。
- **整体含义**：提出一种通用、轻量级的适配策略，提升VLA模型部署到新机器人平台和任务中的实用性。

#### 2. 方法论
- **核心思想**：通过“对齐-引导”（Align-Then-Steer）两阶段框架，先在统一潜在空间中对齐不同动作空间，再在该潜空间中引导策略生成。
- **关键技术细节**：
  - **阶段一：对齐（Align）**：使用变分自编码器（VAE）构建统一潜在空间。VAE通过**逆KL散度**约束，将适配动作（来自下游任务）嵌入到预训练动作潜在分布的**模式（modes）**中，从而对齐不同动作空间。
  - **阶段二：引导（Steer）**：在微调过程中，通过一个**引导机制**（guidance mechanism）作用于基于扩散或流的VLA生成过程，推动模型输出分布向目标域移动。
- **算法流程（文字描述）**：
  1. 收集预训练动作数据，训练一个VAE学习其潜在分布；
  2. 对下游任务的少量数据，利用逆KL散度最小化，将下游动作映射到预训练潜空间的模式；
  3. 在微调VLA模型时，将VAE的潜在引导信号注入到扩散/流模型的去噪或采样过程中，使生成的动作接近目标域分布；
  4. 无需修改预训练模型架构，即插即用。

#### 3. 实验设计
- **数据集/场景**：
  - 仿真环境：跨实体和跨任务操作任务（具体类型未在摘要中详述，但提及“simulation”）。
  - 真实世界：跨实体操作设置（具体机器人平台未详述）。
- **Benchmark**：对比的是**直接微调（direct fine-tuning）**代表性VLA模型（如基于扩散或流的模型）。
- **对比方法**：主要对比直接微调（未提及其他SOTA方法名称），但验证了ATE相对于基线方法的提升。

#### 4. 资源与算力
- **文中未明确说明**：未提及GPU型号、数量、训练时长等具体算力信息。仅指出该方法“数据高效”，未涉及计算资源量化。

#### 5. 实验数量与充分性
- **实验组数**：包括仿真环境的多任务成功率对比、真实世界的成功率对比，以及可能存在的消融实验（文中提到“extensive experiments”，但摘要未列出具体数量）。从结果看，至少覆盖仿真和实物两大场景。
- **充分性与公平性**：实验设计覆盖了跨实体和跨任务两种关键场景，对比基线为直接微调（最直接的对比对象）。但未提供与更多近期方法的比较（如LoRA、Prompt tuning等轻量适配方法），也未展示在更多不同机器人平台上的结果，因此充分性中等，但结论可信。

#### 6. 主要结论与发现
- 在仿真环境中，ATE相比直接微调将**平均多任务成功率提升高达9.8%**。
- 在真实世界跨实体设置中，成功率**提升高达32%**。
- 验证了统一潜在空间引导是一种高效、通用的VLA适应策略，显著降低微调数据需求量。

#### 7. 优点
- **数据高效**：仅需少量下游数据即可完成适配。
- **即插即用**：无需更改预训练VLA模型架构，易于集成。
- **理论创新**：利用逆KL散度对齐动作分布到潜空间的模式，避免了直接拟合可能带来的模式坍塌。
- **跨场景验证**：仿真和真实世界双重验证，且跨实体和跨任务均有显著提升。

#### 8. 不足与局限
- **实验覆盖有限**：未与更多轻量适配方法（如LoRA、Adapter）对比，也未在多种不同类型的VLA模型（如不同骨干网络）上验证。
- **依赖VAE训练**：需要预训练动作数据来训练VAE，若预训练数据分布与下游差距过大，对齐效果可能下降。
- **引导机制细节不明确**：摘要未提及如何具体将潜在引导注入扩散/流模型，可能存在实现复杂性。
- **算力消耗未提及**：无法评估训练VAE和微调阶段的实际计算成本。
- **真实世界场景单一**：仅提及一个跨实体设置，未说明机器人平台类型和任务多样性，泛化性证据不足。

（完）
