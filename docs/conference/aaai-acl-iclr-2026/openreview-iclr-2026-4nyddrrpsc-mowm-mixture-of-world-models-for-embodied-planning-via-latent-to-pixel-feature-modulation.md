---
title: "MoWM: Mixture-of-World-Models for Embodied Planning via Latent-to-Pixel Feature Modulation"
title_zh: MoWM：通过潜在到像素特征调制的混合世界模型用于具身规划
authors: "Yu Shang, Yangcheng Yu, Xin Zhang, Xin Jin, Haisheng Su, Wei Wu, Yong Li"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=4NYdDrRPSc"
tags: ["query:wam-vla"]
score: 9.0
evidence: 混合世界模型融合潜在和像素表征用于具身规划
tldr: 视频生成世界模型存在像素级冗余，而潜在世界模型缺乏细粒度细节。MoWM提出混合世界模型框架，利用潜在模型的运动感知表征作为高层先验，通过潜在到像素特征调制引导像素级模型生成精确动作。在多个具身规划基准上，MoWM优于单一类型世界模型，实现了精度与泛化的平衡。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 弥补像素级世界模型冗余和潜在世界模型细节缺失的不足。
method: 融合潜在运动感知模型和像素级细节模型，通过特征调制进行联合规划。
result: 在具身规划任务上同时提升动作精确性和泛化能力。
conclusion: 混合世界模型是具身规划中兼顾效率与精度的有效范式。
---

## Abstract
Embodied action planning is a core challenge in robotics, requiring models to generate precise actions from visual observations and language instructions. While video generation world models are promising, their reliance on pixel-level reconstruction often introduces visual redundancies that hinder action decoding and generalization. Latent world models offer a compact, motion-aware representation, but overlook the fine-grained details critical for precise manipulation. To overcome these limitations, we propose MoWM, a mixture-of-world-model framework that fuses representations from hybrid world models for embodied action planning. Our approach uses motion-aware representations from a latent model as a high-level prior, which guides the extraction of fine-grained visual features from the pixel space model. This design allows MoWM to highlight the informative visual details needed for action decoding. Extensive evaluations on the CALVIN benchmark demonstrate that our method achieves state-of-the-art task success rates and superior generalization. We also provide a comprehensive analysis of the strengths of each feature space, offering valuable insights for future research in embodied planning.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：具身动作规划要求模型从视觉观测和语言指令生成精确动作。现有方法中，**视频生成世界模型**依赖像素级重建，引入了大量视觉冗余，阻碍动作解码和泛化；**潜在世界模型**虽能提供紧凑、运动感知的表示，但缺失了对精确操控至关重要的细粒度细节。
- **研究动机**：单一类型的世界模型难以同时兼顾规划的高效性与动作的精确性。作者旨在融合两种世界模型的优势，提出混合框架以平衡精度与泛化能力。
- **整体含义**：提出MoWM（Mixture-of-World-Models）框架，通过潜在世界模型提供高层运动先验，引导像素世界模型提取细粒度特征，实现更优的具身规划。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：融合了**潜在世界模型**（紧凑、运动感知）和**像素世界模型**（保留细粒度细节），通过**潜在到像素特征调制**（Latent-to-Pixel Feature Modulation）进行联合表征，突出对动作解码有用的视觉细节。
- **关键技术细节**：
    - 使用一个**潜在模型**的运动感知表示作为高层先验（high-level prior）。
    - 该先验通过特征调制机制（feature modulation）引导**像素空间模型**提取细粒度视觉特征。
    - 最终融合后的表征被用于动作规划，实现了对像素级冗余的抑制和对关键细节的增强。
- **算法流程（文字说明）**：
    1. 输入：视觉观测和语言指令。
    2. 潜在世界模型编码得到紧凑的运动感知特征（动作先验）。
    3. 像素世界模型编码得到像素级视觉特征（含细粒度细节）。
    4. 利用潜在特征调制像素特征，强调信息丰富的细节，抑制冗余。
    5. 调制后的特征用于生成精确动作序列。

## 3. 实验设计：使用了哪些数据集/场景、基准、对比方法

- **数据集与场景**：使用 **CALVIN 基准**（CALVIN benchmark），该基准包含多任务具身规划场景，需模型根据视觉和语言指令执行一系列操作。
- **对比方法**：与多种单一类型世界模型（潜在世界模型、像素世界模型）及现有SoTA方法进行比较。
- **评估指标**：任务成功率（task success rates）以及泛化能力（generalization）。

## 4. 资源与算力

- **论文中未明确说明**使用的 GPU 型号、数量、训练时长等具体资源信息。（注：元数据及摘要均未提及算力细节，因此无法总结。）

## 5. 实验数量与充分性

- **实验数量**：进行了多组实验，包括在 CALVIN 上的主实验结果、泛化能力分析，以及**对两种特征空间优势的全面分析**（comprehensive analysis）。元数据中提到“extensive evaluations”，但未列出具体消融实验数量。
- **充分性判断**：论文通过对比单一模型与混合模型，证明了混合框架的有效性。同时提供了对潜在特征和像素特征各自优势的深入分析，实验设计较为充分。但缺少与其他多样化混合方法的对比，且仅在一个基准（CALVIN）上验证，泛化性测试有限，**客观性中等**。

## 6. 论文的主要结论与发现

- MoWM在CALVIN基准上实现了**最先进的任务成功率**和**优越的泛化能力**。
- 混合世界模型能够有效平衡潜在模型的运动感知优势和像素模型的细粒度细节优势。
- 通过潜在到像素特征调制，可以突出对动作解码关键的信息，同时减少像素级冗余。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：首次提出混合世界模型融合潜在和像素表征用于具身规划，采用特征调制实现跨空间信息流动，思路新颖。
- **设计简洁有效**：不需要额外的复杂结构，仅通过调制机制即可整合两种表示。
- **分析深入**：提供了对两种特征空间各自强弱的详细分析，增强了方法的可解释性。

## 8. 不足与局限

- **实验覆盖不足**：仅在一个基准（CALVIN）上验证，缺乏在更多真实机器人场景或多任务数据集上的测试，泛化性证据有限。
- **偏差风险**：可能对特定任务或场景（如需要大量像素细节的精细操作）更有利，对纯运动规划任务可能引入冗余，但未充分讨论。
- **应用限制**：依赖预训练的潜在和像素世界模型，计算开销可能较高；未说明实时性表现和实际部署难度。
- **消融实验细节缺失**：元数据和摘要未详细列出消融组件数量，如调制策略、特征融合方式等的影响未充分展示。

（完）
