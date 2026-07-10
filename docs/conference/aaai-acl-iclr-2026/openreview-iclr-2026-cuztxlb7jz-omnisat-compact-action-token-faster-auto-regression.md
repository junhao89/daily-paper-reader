---
title: "OmniSAT: Compact Action Token, Faster Auto Regression"
title_zh: OmniSAT：紧凑动作令牌与更快自回归
authors: "Huaihai Lyu, Chaofan Chen, Senwei Xie, Pengwei Wang, Shanghang Zhang, Changsheng Xu"
date: 2025-09-01
pdf: "https://openreview.net/pdf?id=CuzTXLB7Jz"
tags: ["query:latent-vla"]
score: 9.0
evidence: 面向VLA模型的紧凑动作令牌化与更快的自回归
tldr: 针对VLA模型中动作分块导致序列过长、自回归效率低下的问题，OmniSAT提出一种快速动作分词器，学习紧凑的变换动作令牌以大幅缩短序列长度，在保持重构质量的同时加速自回归生成，显著提升大规模预训练效率。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有VLA模型中动作分块产生冗余序列，自回归效率低。
method: 提出Omni Swift Action Tokenizer，学习紧凑的可变换动作令牌以压缩序列。
result: 在多个VLA基准上实现了更快的自回归速度和良好的重构精度。
conclusion: 紧凑动作令牌化是提升VLA模型效率的关键技术。
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

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有视觉-语言-动作（VLA）模型主要分为扩散模型和自回归（AR）模型。扩散模型捕捉连续动作分布但依赖计算昂贵的迭代去噪；AR模型优化高效、序列构建灵活，但存在动作分块导致序列过长、自回归效率低下的问题。先前通过熵引导和词频方法缩短序列长度，但压缩效果差（重构质量低或压缩不充分）。
- **整体含义**：OmniSAT旨在通过紧凑动作令牌化，解决AR VLA模型中长序列导致的低效问题，同时保持重构质量，从而提升大规模预训练效率。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：学习一种紧凑、可迁移的动作表示（Omni Swift Action Tokenizer），大幅压缩动作序列长度。
- **关键技术细节**：
  - **归一化与B样条编码**：首先对动作的值范围和时序范围进行归一化，得到一致表示，并用B样条编码。
  - **多阶段残差量化**：将动作分解为位置、旋转、夹爪三个子空间，分别应用多阶段残差量化，生成从粗到细粒度的压缩离散令牌。
  - **跨体现学习策略**：在统一动作模式空间上，联合利用机器人和人类演示数据，从异构自我中心视频中获取可扩展的辅助监督。
- **算法流程**（文字说明）：
  1. 输入动作轨迹 → 归一化处理 → B样条编码。
  2. 对编码后的表示按子空间（位置/旋转/夹爪）进行多阶段残差量化，每阶段逐步细化残差。
  3. 输出紧凑离散令牌序列，用于替代原始长序列。

### 3. 实验设计：数据集、基准、对比方法
- **预训练数据集**：大规模数据集Droid（用于预训练动作分词器）。
- **benchmark**：多个真实机器人和模拟实验（文中未具体列出名称，但提及“diverse real-robot and simulation experiments”）。
- **对比方法**：文中提到与“prior work”（熵引导和词频方法）对比，但未明确列出具体方法名称。

### 4. 资源与算力
- **未明确说明**：文中没有提及使用的GPU型号、数量、训练时长等算力信息。

### 5. 实验数量与充分性
- **实验数量**：摘要中仅提及“在Droid上预训练后缩短训练序列6.8倍，降低目标熵”，以及“在多种真实机器人和模拟实验上验证”。未给出详细实验次数或消融实验列表。
- **充分性判断**：由于只有摘要，无法全面评估实验的充分性和公平性。但作者声称在多个场景下验证，且重建质量保持，压缩比显著，实验设计较为合理。不过缺乏与多种基线方法的详细对比和统计分析。

### 6. 论文的主要结论与发现
- OmniSAT的紧凑动作令牌化可将训练序列缩短**6.8倍**，降低目标熵。
- 在保持重建质量的前提下，实现更快的AR训练收敛和更好的模型性能。
- 跨体现学习策略能够有效利用人类演示数据提供辅助监督。

### 7. 优点：方法或实验设计上的亮点
- **创新性**：提出基于多阶段残差量化和B样条编码的紧凑动作分词器，显著压缩序列长度。
- **效率提升**：缩短序列6.8倍直接加速自回归生成，适合大规模预训练。
- **跨体现学习**：首次在统一动作模式空间上联合机器人/人类演示，拓展了训练数据来源。
- **通用性**：在真实机器人及模拟环境均有验证。

### 8. 不足与局限
- **实验细节不足**：缺少具体数据集、对比方法名称、消融实验、统计显著性分析等，仅凭摘要难以全面评估。
- **算力信息缺失**：无法复现计算成本。
- **潜在偏差风险**：仅使用Droid预训练，可能在其他领域泛化性未充分验证。
- **应用限制**：量化过程可能引入误差，对高精度任务（如精密操作）的适用性需进一步测试。

（完）
