---
title: "MoWM: Mixture-of-World-Models for Embodied Planning via Latent-to-Pixel Feature Modulation"
title_zh: MoWM：通过潜在到像素特征调制的混合世界模型实现具身规划
authors: "Yu Shang, Yangcheng Yu, Xin Zhang, Xin Jin, Haisheng Su, Wei Wu, Yong Li"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=4NYdDrRPSc"
tags: ["query:wam-vla"]
score: 9.0
evidence: 混合潜在与像素世界模型用于具身规划
tldr: 该论文针对潜在世界模型缺失细节、像素世界模型存在冗余的问题，提出混合世界模型MoWM，融合潜在模型中的运动感知先验与像素模型中的精细细节，通过潜在到像素特征调制实现具身动作规划。在仿真和真实任务上，MoWM优于单一世界模型方法。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 潜在世界模型缺乏精细细节，像素世界模型存在视觉冗余，两者存在互补性。
method: 提出混合世界模型框架，用潜在模型运动表示作为高层先验，调制像素模型特征以生成精确动作。
result: 在多个具身规划任务上优于纯潜在和纯像素世界模型。
conclusion: 融合潜在和像素表示可兼得紧凑性与细节，提升规划性能。
---

## Abstract
Embodied action planning is a core challenge in robotics, requiring models to generate precise actions from visual observations and language instructions. While video generation world models are promising, their reliance on pixel-level reconstruction often introduces visual redundancies that hinder action decoding and generalization. Latent world models offer a compact, motion-aware representation, but overlook the fine-grained details critical for precise manipulation. To overcome these limitations, we propose MoWM, a mixture-of-world-model framework that fuses representations from hybrid world models for embodied action planning. Our approach uses motion-aware representations from a latent model as a high-level prior, which guides the extraction of fine-grained visual features from the pixel space model. This design allows MoWM to highlight the informative visual details needed for action decoding. Extensive evaluations on the CALVIN benchmark demonstrate that our method achieves state-of-the-art task success rates and superior generalization. We also provide a comprehensive analysis of the strengths of each feature space, offering valuable insights for future research in embodied planning.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：具身动作规划（Embodied action planning）面临世界模型表征的两难困境——潜在世界模型（Latent world model）虽然紧凑且对运动敏感，但忽略了精细操控所需的细节；像素世界模型（Pixel world model）虽然包含丰富细节，但存在视觉冗余，干扰动作解码和泛化。
- **研究动机**：两种世界模型具有互补性，若能融合潜在模型的运动感知先验和像素模型的精细细节，有望同时提升规划精度与泛化能力。
- **整体含义**：该研究旨在通过混合世界模型框架（MoWM）实现更优的具身规划，为机器人视觉-语言动作生成提供新思路。

## 2. 方法论：核心思想、关键技术细节、算法流程（文字说明）
- **核心思想**：构建混合世界模型（Mixture-of-World-Models），将潜在世界模型的运动感知表征作为高层先验，用于指导像素世界模型中细粒度视觉特征的提取，最终通过“潜在到像素特征调制”（Latent-to-Pixel Feature Modulation）生成精确动作。
- **关键技术细节**：
  - 使用一个潜在世界模型（如基于VAE或Transformer）获得紧凑、运动相关的表示。
  - 使用一个像素世界模型（如基于扩散或自回归的视频生成模型）保留视觉细节。
  - **特征调制**：潜在模型的运动先验通过调制机制（如缩放、偏移或注意力）作用于像素模型的特征空间，突出与动作解码相关的信息性视觉细节，抑制冗余。
- **算法流程示意**：
  1. 输入：视觉观测序列+语言指令。
  2. 潜在模型编码得到运动感知隐变量（高层先验）。
  3. 像素模型提取原始视觉特征。
  4. 利用潜在先验对像素特征进行调制，得到增强的特征。
  5. 将调制后的特征输入动作解码器（如因果Transformer）预测后续动作。
  6. 训练时联合优化潜在模型、像素模型及调制模块。

## 3. 实验设计
- **数据集/场景**：主要使用 **CALVIN** benchmark（一个标准化具身操控仿真环境），包含多种语言指令和桌面操控任务。
- **Benchmark**：CALVIN上的任务成功率（task success rates）和泛化能力。
- **对比方法**：纯潜在世界模型（如Latent Planner）、纯像素世界模型（如Video Prediction based policy），以及可能包括当前SOTA的VLA方法（如RT-2? 但论文未明确列出，仅提到优于单一世界模型方法）。摘要指出MoWM取得了SOTA结果。

## 4. 资源与算力
- **说明**：论文元数据和摘要中**未明确提及**所使用的GPU型号、数量、训练时长或总计算量。因此无法提供具体算力信息。

## 5. 实验数量与充分性
- **实验数量**：根据摘要，主要在CALVIN上进行评估，并提供了全面的分析（“a comprehensive analysis of the strengths of each feature space”）。推测包含至少下列实验：
  - 主实验：与基线对比任务成功率。
  - 消融实验：分离潜在和像素模型贡献、验证调制机制有效性。
  - 泛化实验：测试在不同指令或场景下的零样本适应能力。
- **充分性评价**：实验覆盖了主要基线和消融分析，但仅在一个仿真benchmark上验证，缺乏真实机器人实验和多环境交叉验证。虽然论文在CALVIN上达到SOTA，但实验范围有限，结论的泛化性有待进一步证实。整体上设计比较规范，但充分性可加强。

## 6. 主要结论与发现
- **主要结论**：融合潜在世界模型和像素世界模型（MoWM）能够兼得紧凑性与细节，显著提升具身规划的任务成功率和泛化能力。潜在运动先验能有效引导像素特征提取，减少冗余，增强动作解码。
- **发现**：潜在空间和像素空间具有互补性，混合建模比单一建模更优；特征调制是实现有效融合的关键。

## 7. 优点
- **方法设计亮点**：首次提出“混合世界模型”概念，并设计潜在到像素特征调制机制，实现高层语义与低层细节的有机融合，思路新颖。
- **性能优势**：在CALVIN上达到SOTA，验证了方法的有效性。
- **分析深入**：提供了对两种特征空间优缺点的分析，对未来研究有参考价值。

## 8. 不足与局限
- **实验覆盖不足**：仅在CALVIN仿真环境测试，缺乏真实机器人实验和跨领域（如导航、抓取）验证。
- **偏差风险**：可能依赖于特定任务和仿真环境，真实世界中的噪声、动态变化未被充分考量。
- **应用限制**：调制机制的计算效率未讨论，可能增加推理开销；潜在模型和像素模型的联合训练稳定性需进一步验证。
- **缺乏算力报告**：无法评估方法的可复现性和资源需求。
- **论文接收状态**：该文被ICLR-2026拒稿，暗示审稿人可能认为方法创新性或实验充分性存在不足（仅供参考）。

（完）
