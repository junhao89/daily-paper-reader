---
title: "OmniSAT: Compact Action Token, Faster Auto Regression"
title_zh: "OmniSAT: 紧凑动作标记与快速自回归"
authors: "Huaihai Lyu, Chaofan Chen, Senwei Xie, Pengwei Wang, Shanghang Zhang, Changsheng Xu"
date: 2025-09-01
pdf: "https://openreview.net/pdf?id=CuzTXLB7Jz"
tags: ["query:latent-vla"]
score: 9.0
evidence: 用于VLA模型的离散潜在动作标记
tldr: 针对自回归VLA模型动作序列过长、压缩效率低的问题，提出Omni Swift Action Tokenizer，学习紧凑的离散潜在动作标记。该方法在缩短序列长度的同时保持高重构保真度，从而加速自回归推理并支持大规模预训练。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有VLA模型的自回归变体面临动作块导致长序列和维度扩展的问题，需要更高效的动作压缩。
method: 提出全向快速动作分词器，学习紧凑的离散潜在动作表示，实现高效压缩与重构。
result: 在保持重构质量的同时显著缩短序列长度，提升自回归效率。
conclusion: 紧凑动作标记技术为大规模VLA预训练提供了高效路径。
---

## Abstract
Existing Vision-Language-Action (VLA) models can be broadly categorized into diffusion-based and auto-regressive (AR) approaches: diffusion models capture continuous action distributions but rely on computationally heavy iterative denoising. In contrast, AR models enable efficient optimization and flexible sequence construction, making them better suited for large-scale pretraining. To further improve AR efficiency, particularly when action chunks induce extended and high-dimensional sequences, prior work applies entropy-guided and token-frequency techniques to shorten the sequence length. 
However, such compression struggled with poor reconstruction or inefficient compression.
Motivated by this, we introduce an Omni Swift Action Tokenizer, which learns a compact, transferable action representation.
Specifically, we first normalize value ranges and temporal horizons to obtain a consistent representation with B-Spline encoding.
Then, we apply multi-stage residual quantization to the position, rotation, and gripper subspaces, producing compressed discrete tokens with coarse-to-fine granularity for each part.
After pre-training on the large-scale dataset Droid, the resulting discrete tokenization shortens the training sequence by 6.8$\times$, and lowers the target entropy.
To further explore the potential of OmniSAT, we develop a cross-embodiment learning strategy that builds on the unified action-pattern space and jointly leverages robot and human demonstrations. It enables scalable auxiliary supervision from heterogeneous egocentric videos.
Across diverse real-robot and simulation experiments, OmniSAT encompasses higher compression while preserving reconstruction quality, enabling faster AR training convergence and model performance.

---

## 论文详细总结（自动生成）

# OmniSAT 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：现有视觉-语言-动作（Vision-Language-Action, VLA）模型主要分为扩散模型和自回归（AR）模型两类。扩散模型能捕获连续动作分布，但依赖计算密集的迭代去噪；自回归模型支持高效优化和灵活序列构建，更适合大规模预训练。然而，自回归模型在处理动作块（action chunk）时会产生**过长的序列**和**高维度表示**，导致训练和推理效率低下。
- **背景问题**：先前工作尝试通过熵引导和令牌频率技术来缩短序列长度，但面临**重构质量差**或**压缩不充分**的困境。因此，需要一种既能高效压缩动作序列、又能保持高重构保真度的新方法。
- **整体目标**：提出**Omni Swift Action Tokenizer（OmniSAT）**，学习紧凑、可迁移的离散动作表示，从而加速自回归VLA模型的训练与推理，并支持大规模预训练。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：将连续动作空间压缩成**紧凑的离散潜在标记**，通过多阶段残差量化实现对动作的高效编码，同时利用B样条编码保证时序一致性。
- **关键技术细节**：
  - **归一化与B样条编码**：首先对动作的值域和时间范围进行归一化，得到一致的表示，再利用B样条编码（B-Spline encoding）对连续动作轨迹进行平滑参数化。
  - **多阶段残差量化（Multi-Stage Residual Quantization）**：将动作空间分解为三个子空间：**位置（position）、旋转（rotation）、夹爪（gripper）**。对每个子空间独立应用多级残差量化，生成从粗到细粒度的离散标记。每一级学习残差以逐步逼近原始动作。
  - **跨实体学习策略（Cross-Embodiment Learning）**：基于统一的动作模式空间，同时利用机器人演示和人类演示数据，从异构的第一人称视频中获取可扩展的辅助监督信号，提升标记器的通用性。
- **伪算法流程（文字说明）**：
  1. 输入：连续动作序列（位置、旋转、夹爪）。
  2. 对每个动作维度进行归一化，并使用B样条拟合轨迹，得到平滑表示。
  3. 将轨迹分别输入三个独立的量化分支（位置、旋转、夹爪），每个分支执行多级残差量化：
     - 第一级：粗量化得到初始离散码。
     - 后续级：对残差进行更细粒度的量化，逐步逼近原始值。
  4. 合并所有子空间的离散标记，形成紧凑的动作序列。
  5. 利用预训练数据集（Droid）进行自监督预训练，优化重构损失和量化损失。
  6. 在跨实体学习中，将机器人数据和人类视频数据共同编码，训练统一的动作标记器。

## 3. 实验设计
- **使用数据集**：
  - **大规模预训练数据集**：**Droid**（大型机器人操作数据集）。
  - **跨实体学习辅助数据**：机器人演示 + 人类第一人称视频（异构egocentric视频）。
- **Benchmark**：未明确列出具体基准任务，但提到在**多样真实机器人实验**和**仿真实验**中评估，对比对象可能是传统的熵引导压缩方法、令牌频率方法或其他基线（如直方图量化、VAE等）。
- **对比方法**：摘要中提及先前的“熵引导”和“令牌频率”技术，但未列出具体对比算法名称。可推断对比了其他动作压缩或动作标记化方法。
- **评估指标**：压缩比（序列长度缩短倍数）、重构保真度（重构误差）、自回归训练收敛速度（收敛所需步数或epoch）、模型最终性能（成功率或精度）。

## 4. 资源与算力
- 论文摘要和元数据中**未明确说明**所使用的GPU型号、数量或训练时长。
- 可推断：由于在大规模数据集（Droid）上预训练，且使用了多阶段残差量化，可能消耗一定计算资源，但具体细节缺失。

## 5. 实验数量与充分性
- **实验数量**：摘要中仅概括提到“跨多样真实机器人实验和仿真实验”，未给出具体实验组数或消融实验数量。从元数据看，可能包含：
  - 预训练阶段在Droid上的压缩比验证（6.8×缩短）。
  - 跨实体学习的迁移实验。
  - 与基线方法的效率对比。
- **充分性评估**：
  - **优点**：覆盖了大规模预训练和下游微调，含跨实体泛化测试，具有一定说服力。
  - **不足**：缺乏详细的消融实验（如量化级数、子空间划分的影响）、弱基线（如直接动作分块基线）的比较、以及在不同机器人平台上的泛化性测试。实验设计看似合理，但论文全文可能提供更全面的消融，摘要信息有限导致无法充分判断其客观公平性。总体而言，实验覆盖了主要场景，但完备性一般。

## 6. 主要结论与发现
- **压缩效率**：在Droid数据集上预训练后，OmniSAT将训练序列长度**缩短了6.8倍**，并降低了目标熵。
- **重构质量**：在保持较高重构保真度的同时实现高效压缩。
- **训练加速**：紧凑的离散标记使自回归VLA模型的训练收敛速度更快，最终性能优于现有方法。
- **跨实体潜力**：通过联合利用机器人和人类演示，可扩展辅助监督，增强模型泛化性。

## 7. 优点
- **高压缩比与低重构误差兼得**：解决了先前压缩方法在保真度和效率之间的权衡问题。
- **模块化设计**：将动作分解为位置、旋转、夹爪三个子空间独立量化，便于分工和粒度控制。
- **跨实体学习能力**：支持从人类视频中学习，降低了机器人数据采集成本，提升了标记器的通用性和可迁移性。
- **兼容大规模预训练**：缩短的序列长度降低了自回归模型的自回归步数，适用于更大规模数据预训练。

## 8. 不足与局限
- **资源信息缺失**：未报告训练所需算力（GPU型号、数量、时长），使得可复现性和效率评估不完整。
- **实验细节不充分**：消融实验、基线对比、鲁棒性测试等未被提及，难以判断方法的全面优势。
- **偏差风险**：仅在Droid数据集上进行预训练，动作模式可能偏向该数据集的特征，泛化到其他领域（如灵巧操作、长时任务）的效果未知。
- **应用限制**：多阶段残差量化增加了编码复杂度，可能对实时性要求高的场景带来延迟；另外，跨实体学习依赖人类视频的可用性和对齐精度，有一定数据依赖。

（完）
