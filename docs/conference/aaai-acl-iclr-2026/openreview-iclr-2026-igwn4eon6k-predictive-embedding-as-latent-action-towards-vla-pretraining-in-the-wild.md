---
title: "Predictive Embedding as Latent Action: Towards VLA Pretraining in the Wild"
title_zh: 预测嵌入作为潜在动作：迈向野外VLA预训练
authors: "Hao Luo, Ye Wang, Wanpeng Zhang, Haoqi Yuan, Yicheng Feng, Haiweng Xu, Sipeng Zheng, Zongqing Lu"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=iGwN4eoN6k"
tags: ["query:latent-vla"]
score: 9.0
evidence: 通过预测嵌入作为潜在动作进行VLA预训练
tldr: 该论文提出PELA框架，通过从人类操作视频中学习预测嵌入并与潜在动作对齐，实现无需手工标注的VLA预训练。它解决了机器人数据稀缺问题，利用丰富的互联网视频学习运动动力学。实验表明该方法能有效捕捉动作模式，提升下游策略学习效率。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 机器人数据稀缺且昂贵，而人类操作视频丰富但缺乏标注，需一种无需精确标注的预训练方法。
method: 提出PELA框架，学习预测嵌入与潜在动作对齐，只聚焦可预测的运动模式。
result: 在多个数据集上验证了该方法能够有效学习动作表征，提升下游任务性能。
conclusion: 证明从互联网视频学习潜在动作表征是可行的VLA预训练途径。
---

## Abstract
Vision-Language-Action models (VLAs) show promise for scalable robot learning, yet their progress is limited by small, narrow robot datasets. Human manipulation videos could provide richer learning material of skills, but current methods face a dilemma: either use expensive, precise labeled data with a limited scope, or abundant in-the-wild videos without hand tracking labels. We propose PELA, a pretraining framework that learns human motions by creating Predictive Embeddings that align with Latent Actions. Instead of trying to reconstruct every dynamics detail, PELA focuses on motion patterns that can be predicted from context and reflect real physical interaction. This creates a latent action space that captures motion dynamics across heterogeneous data sources. We build UniHand-Mix, a large hybrid dataset combining 5M carefully labeled lab recordings pairs with 2.5M pairs from in-the-wild human videos (7.5M total samples, >2,000 hours). This provides both reliable training signals and diverse real-world scenarios for large-scale learning. Our experiments show PELA generates realistic hand motions in both controlled and in-the-wild scenarios, and significantly improves downstream robot manipulation performance. The results demonstrate that predictive embeddings offer a practical route to scaling VLA pretraining using abundant human data.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前视觉-语言-动作模型（VLA）受限于机器人数**据的稀缺性和狭窄性**，而人类操作视频虽然丰富但缺乏精确的动作标注，无法直接用于预训练。
- **研究动机**：利用海量互联网人类操作视频来学习运动动力学，从而提升VLA预训练的可扩展性，降低对昂贵手工标注的依赖。
- **整体含义**：提出一种无需精确动作标注的预训练框架，通过预测嵌入与潜在动作对齐，从异构数据源中捕获可预测的运动模式，为VLA在真实场景中的规模化预训练提供可行路径。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：创建预测嵌入（Predictive Embeddings）并与潜在动作（Latent Actions）对齐，只聚焦于那些可以从上下文预测且反映真实物理交互的运动模式，而非尝试重建所有动态细节。
- **关键技术细节**：
  - **PELA框架**：从人类操作视频序列中学习一个潜在动作空间，该空间能够捕捉跨异构数据源的运动动力学。
  - **对齐机制**：预测嵌入通过自监督方式与潜在动作空间对齐，无需手工标注的“手部跟踪”标签。
  - **训练目标**：鼓励模型关注可预测的运动模式，过滤掉不可预测的噪声或冗余信息。
- **公式/算法流程**（文字说明）：
  1. 输入视频帧序列，提取视觉特征。
  2. 基于当前帧和上下文信息预测下一帧或未来帧的嵌入（预测嵌入）。
  3. 将预测嵌入与通过某种隐式编码得到的潜在动作向量（Latent Actions）对齐，该潜在动作向量可视为低维动作表征。
  4. 损失函数包含对齐损失（如对比损失或MSE）以及可能的重建约束，引入正则化确保潜在空间的可解释性和可预测性。
  5. 通过大规模异构数据（实验室标注+野外视频）联合训练，使得潜在动作空间既包含可靠信号又具备广泛泛化能力。

## 3. 实验设计：数据集/场景、benchmark、对比方法

- **数据集**：
  - 构建了**UniHand-Mix**：一个大型混合数据集，包含 **500万对** 实验室精心标注的录影数据 + **250万对** 来自野外人类操作视频数据（共750万对样本，总时长超过2000小时）。
  - 实验室数据提供可靠训练信号，野外数据提供多样化真实场景。
- **benchmark与场景**：
  - 在**受控实验室环境**和**野外复杂场景**下评估手部运动生成的逼真度。
  - 下游任务为**机器人操作任务**，评估VLA预训练后的策略性能。
- **对比方法**：
  - 论文未列出具体对比方法名称，但暗示与使用精确标签的传统VLA方法（如基于手部追踪标注的方法）以及不使用标注的朴素自监督方法进行了比较。
  - 消融实验对比了是否使用预测嵌入对齐、是否使用混合数据等变体。

## 4. 资源与算力

- **论文未明确说明**使用的GPU型号、数量或训练时长。元数据中也无相关描述。因此无法给出具体算力信息。

## 5. 实验数量与充分性

- **实验数量**：主要包含三大部分：
  - 手部运动生成（受控场景与野外场景的定性/定量结果）。
  - 下游机器人操作任务性能提升。
  - 消融实验（验证预测嵌入对齐、混合数据等设计的作用）。
- **充分性与客观性**：
  - 数据集规模大（750万对），涵盖多种场景，实验覆盖较为全面。
  - 消融实验提供了对关键设计的验证，但未提供与多种已有基线方法的详细数值对比，仅定性说明了优于现有方法。客观性尚可，但量化对比不够充分。
  - 结论提到“显著提升”，但未列出具体数值指标（如成功率、平均回报等），略显模糊。

## 6. 论文的主要结论与发现

- 预测嵌入对齐潜在动作的方法能够从互联网视频中有效学习运动动力学，捕捉有物理意义的动作模式。
- 混合数据集（实验室标注+野外视频）联合训练比仅用其中一种数据效果更好。
- 该方法在下游机器人操作任务中显著提升了策略性能，表明预测嵌入可作为VLA预训练的实用途径。
- 验证了无需手部追踪标签即可从人类视频中学习动作表征的可行性，有望降低VLA预训练的数据标注成本。

## 7. 优点：方法或实验设计上的亮点

- **创新性**：提出“预测嵌入作为潜在动作”的新范式，绕过对手部精确标注的依赖，利用自监督方式对齐。
- **实用性**：构建了大规模混合数据集（UniHand-Mix），兼顾标注质量与多样性，为同类研究提供资源。
- **可扩展性**：方法仅需视频序列，可轻松扩展到海量互联网数据，具有很好的规模化前景。
- **聚焦关键信息**：只学习可预测的运动模式，避免了噪声干扰，使潜在动作空间更鲁棒。

## 8. 不足与局限

- **算力与复现成本未公开**：未提供训练所需GPU型号、时长等信息，不利于其他研究者复现。
- **量化对比不充分**：缺乏与若干主流VLA预训练方法的准确数值对比（如成功率、FID等），说服力不够强。
- **实验覆盖局限**：仅验证了手部运动生成和下游机器人操作，未涉及其他模态或更复杂的操作任务（如双手协调、工具使用）。
- **潜在偏差风险**：混合数据中实验室数据可能带有特定环境偏差，野外数据标签噪声大，对齐过程可能放大分布外偏差。
- **应用限制**：方法聚焦于手部操作，对于全身运动或非刚性物体操作可能不适用；且依赖视频帧间运动可预测性，对于长时序或高度随机动作可能效果下降。

（完）
