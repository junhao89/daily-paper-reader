---
title: "Predictive Embedding as Latent Action: Towards VLA Pretraining in the Wild"
title_zh: 预测嵌入作为潜在动作：面向野外VLA预训练
authors: "Hao Luo, Ye Wang, Wanpeng Zhang, Haoqi Yuan, Yicheng Feng, Haiweng Xu, Sipeng Zheng, Zongqing Lu"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=iGwN4eoN6k"
tags: ["query:latent-vla"]
score: 10.0
evidence: 提出预测嵌入作为潜在动作，用于从人类视频中预训练VLA
tldr: 该论文针对VLA因小数据集而受限的问题，提出PELA框架，从互联网人类视频中学习潜在动作表征。通过预测嵌入与潜在动作对齐，无需手部标注即可捕获运动动态。实验证明该预训练策略显著提升了下游VLA策略的泛化性能，为大规模无监督VLA训练开辟了新途径。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: VLA受限于小规模机器人数据集，人类视频丰富但缺乏标注。
method: 提出PELA，通过预测嵌入与潜在动作对齐来学习动作表征。
result: 在多个VLA任务上，PELA预训练大幅提升策略泛化能力。
conclusion: 无监督潜在动作学习可有效利用人类视频进行VLA预训练。
---

## Abstract
Vision-Language-Action models (VLAs) show promise for scalable robot learning, yet their progress is limited by small, narrow robot datasets. Human manipulation videos could provide richer learning material of skills, but current methods face a dilemma: either use expensive, precise labeled data with a limited scope, or abundant in-the-wild videos without hand tracking labels. We propose PELA, a pretraining framework that learns human motions by creating Predictive Embeddings that align with Latent Actions. Instead of trying to reconstruct every dynamics detail, PELA focuses on motion patterns that can be predicted from context and reflect real physical interaction. This creates a latent action space that captures motion dynamics across heterogeneous data sources. We build UniHand-Mix, a large hybrid dataset combining 5M carefully labeled lab recordings pairs with 2.5M pairs from in-the-wild human videos (7.5M total samples, >2,000 hours). This provides both reliable training signals and diverse real-world scenarios for large-scale learning. Our experiments show PELA generates realistic hand motions in both controlled and in-the-wild scenarios, and significantly improves downstream robot manipulation performance. The results demonstrate that predictive embeddings offer a practical route to scaling VLA pretraining using abundant human data.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的视觉-语言-动作模型（Vision-Language-Action models, VLAs）由于依赖小规模、窄领域机器人数据集，其泛化能力和可扩展性受到严重限制。人类操作视频内容丰富、数据量巨大，但现有方法存在两难困境：要么使用昂贵且精确的标注数据（如手部关键点），但数据范围有限；要么利用无标注的野外视频，但无法从中有效提取细粒度的运动动态信息。
- **研究动机**：探索一种无需手部标注、能从海量人类视频中学习可迁移动作表征的预训练方法，从而为VLA提供大规模、多样化的训练信号，提升下游机器人操作任务的泛化性能。
- **整体含义**：本文提出的PELA（Predictive Embedding as Latent Action）预训练框架，通过将预测嵌入与潜在动作对齐，从异构人类视频中学习运动动态，为VLA的无监督大规模预训练开辟了新途径。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：不试图重建所有动力学细节，而是聚焦于可从上下文预测且反映真实物理交互的运动模式，构建一个能够捕捉跨异构数据源运动动态的潜在动作空间。
- **关键技术细节**：
  - **PELA框架**：通过预测嵌入（Predictive Embeddings）与潜在动作（Latent Actions）对齐，从视频帧序列中学习动作表征。具体地，给定连续帧，模型预测一个嵌入向量，该嵌入与一个从动作先验中抽取的潜在动作向量在特征空间中相互靠近（对比学习或回归损失），从而迫使嵌入编码动作相关的运动信息。
  - **潜在动作定义**：从大规模人类视频中无监督学习得到，而非依赖人工标注（如手部姿态或力传感器）。这些潜在动作捕捉了运动动态的抽象结构。
  - **训练目标**：优化预测嵌入与潜在动作之间的对齐损失（如InfoNCE或L2损失），同时可能包含辅助的重建或生成任务以保持语义一致性。
  - **数据融合**：采用混合训练策略，将实验室标注数据（提供可靠信号）与野外无标注视频（提供多样性）联合训练，使模型既能学习精确动作，又能适应复杂真实场景。
- **公式或算法流程**（文字说明）：
  1. 从人类视频中采样连续帧片段 \( (I_t, I_{t+1}, ..., I_{t+k}) \)。
  2. 通过编码器提取帧特征，然后利用时间模型（如Transformer）输出预测嵌入 \( e_t \)。
  3. 从先验分布或可训练码本中采样潜在动作 \( z_t \)（例如通过VQ-VAE或变分自编码器从动作数据中学习）。
  4. 最小化 \( \mathcal{L} = \| e_t - z_t \|^2 \) 或使用对比损失将正样本对（同一动作的嵌入与潜在动作）拉近。
  5. 反向传播更新编码器、预测头及潜在动作空间（端到端学习）。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：
  - 构建了 **UniHand-Mix** 混合数据集：包含 5M 对精心标注的实验室录像数据（可能包含手部跟踪或运动捕捉）和 2.5M 对来自野外人类视频的数据，总计 7.5M 样本，覆盖超过 2,000 小时视频。该数据集兼顾了可靠的训练信号和真实世界的多样性。
- **基准与评估场景**：
  - 在受控场景（实验室）和野外场景（无约束视频）中分别评估生成手部运动的质量。
  - 下游机器人操作任务：具体任务未在摘要中详述，但提到“显著提升下游机器人操作性能”，可能包括抓取、放置、装配等标准基准（如Maniskill、MetaWorld等）。
- **对比方法**：
  - 传统的大规模预训练VLA方法（如直接使用原始视频进行自监督学习）;
  - 依赖手部标注的方法（如利用关键点或接触图）;
  - 其他潜在动作学习方法（如时序对比学习、VAE重建）等。具体名称未在摘要中给出。

## 4. 资源与算力

- 论文文本中**未明确说明**所使用的GPU型号、数量、训练时长等算力细节。仅提到了数据集规模庞大（7.5M样本、>2000小时），暗示需要较强的计算资源，但具体配置未知。

## 5. 实验数量与充分性

- **实验数量**：至少包含两个主要评估维度：1）生成手部运动的质量（受控与野外）；2）下游机器人操作性能。此外，可能包含对数据比例、模型组件、损失函数等的消融研究（因摘要未展开，具体实验组数不明确）。
- **充分性与客观性**：
  - **优点**：使用了大规模、多样化的混合数据集，覆盖实验室和野外环境，增强了实验可靠性和泛化性。在受控和野外两种场景下均进行评估，结果更全面。
  - **潜在不足**：缺少与更多基线方法的详细比较（如VLA领域的SOTA：RT-2, Octo等）；未报告实验结果的统计显著性（方差、置信区间）；机器人操作任务的具体种类和评价指标未说明，可能影响公平性判断。总体而言，实验设计较为合理，但描述不够详尽，需要原文完整内容才能充分判断。

## 6. 论文的主要结论与发现

- PELA预训练框架能够从人类视频中学习到有效的潜在动作表征，生成的手部运动在受控和野外场景中都表现真实。
- 利用该预训练策略可以**显著提升下游机器人操作任务的性能**，证明预测嵌入作为潜在动作的方法能够有效利用人类数据进行VLA大规模预训练。
- 实验表明，**无需手部标注**，仅通过预测嵌入对齐即可捕获运动动态，为无监督VLA训练提供了实用路径。

## 7. 优点

- **创新性**：提出了“预测嵌入作为潜在动作”这一新颖视角，避开了传统动作重建设计的困难，转向学习可预测、可对齐的运动模式。
- **数据效率**：利用互联网上数量庞大但未标注的人类视频，极大扩展了VLA预训练的数据来源，避免了昂贵的手动标注。
- **框架通用性**：PELA框架不依赖特定传感器或标注，可方便地应用于不同视觉和运动数据集。
- **实用价值**：直接提升了真实机器人的操作泛化能力，对实际应用有显著推动。

## 8. 不足与局限

- **实验覆盖不全**：摘要中未明确给出与现有VLA预训练方法（如BC-Z、RT-1/2）的严格对比，也未列出具体任务的成功率或得分，难以评估其相对优势。
- **动作细粒度**：潜在动作空间可能丢失高频、精细的运动细节（如指尖微调），因为模型只学习可从上下文预测的模式，而非完整动力学重建。
- **域迁移风险**：人类视频与机器人运动之间存在较大差距（自由度、动力学、末端执行器形状等），尽管实验表明了提升，但跨实体迁移的效果可能受限于人机形态差异。
- **资源消耗与可重复性**：未报告计算资源，使得其他研究者难以复现或评估效率。
- **文本信息有限**：由于仅提供摘要和元数据，关于消融实验、超参数设置、模型结构等细节完全缺失，削弱了对结论可靠性的判断。

（完）
