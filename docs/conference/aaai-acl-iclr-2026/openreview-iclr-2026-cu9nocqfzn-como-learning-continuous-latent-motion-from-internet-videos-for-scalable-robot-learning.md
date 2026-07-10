---
title: "CoMo: Learning Continuous Latent Motion from Internet Videos for Scalable Robot Learning"
title_zh: CoMo：从互联网视频学习连续潜在运动以实现可扩展机器人学习
authors: "Jiange Yang, Yansong Shi, Haoyi Zhu, Mingyu Liu, Kaijing Ma, Yating Wang, Gangshan Wu, Tong He, Limin Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=Cu9NOcqfzN"
tags: ["query:latent-vla"]
score: 9.0
evidence: 从互联网视频学习连续潜在运动用于可扩展机器人学习
tldr: CoMo致力于从互联网规模视频中无监督学习连续潜在运动。它采用早期时间特征差异机制防止捷径学习，并引入信息瓶颈约束潜在维度以平衡动作相关信息和背景噪声，从而提取出精确且可泛化的机器人动作表征，可用于多种操作策略。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 从视频无监督学习运动对于构建通用机器人至关重要，但离散方法损失信息。
method: 利用早期时间特征差异和信息瓶颈约束，从互联网视频中学习连续潜在运动。
result: 学到的连续潜在运动在多个机器人操作任务上表现出更好的泛化性和精度。
conclusion: 连续潜在运动学习可成为可扩展机器人动作表征的有效途径。
---

## Abstract
Unsupervised learning of latent motion from Internet videos is crucial for building
generalist robots. However, existing discrete methods suffer from information
loss and struggle with complex and fine-grained dynamics. We propose CoMo,
which aims to learn more precise continuous latent motion from internet-scale
videos. CoMo employs a early temporal feature difference mechanism to prevent
shortcut learning and suppress static appearance noise. Furthermore, guided by the
information bottleneck principle, we constrain the latent motion dimensionality
to achieve a balance between retaining sufficient action-relevant information and
minimizing the inclusion of action-irrelevant background noise. Additionally, we
also introduce two effective metrics for more directly and affordably evaluating
and analyzing motion and guiding motion learning methods development: (i)
MSE of action prediction, and (ii) cosine similarity between past-to-current and
future-to-current motion embeddings. Critically, CoMo exhibits strong zero-shot
generalization, enabling it to generate effective pseudo actions for unseen videos.
The shared continuous distribution of robot action and video latent motion also
directly benefits the joint learning of unified policy. Extensive simulated and real-
world experiments show that policies co-trained with CoMo pseudo actions achieve
superior performance with both diffusion and autoregressive architectures.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：从互联网视频中无监督学习动作/运动表征是实现通用机器人技能的关键，但现有离散式运动学习方法（如VQ-VAE）存在信息损失，难以捕捉复杂和精细的动态变化。
- **整体含义**：本文提出CoMo（Continuous Latent Motion），旨在从互联网规模视频中无监督学习**连续**的潜在运动表征，以保留更丰富的动作相关信息，提升机器人学习的可扩展性和泛化能力，为机器人策略提供更精确的伪动作（pseudo actions），从而实现在多种操作任务中的零样本或联合训练。

## 2. 论文提出的方法论（核心思想、关键技术细节）
- **核心思想**：利用早期时间特征差异（Early Temporal Feature Difference）机制防止网络利用静态外观噪声走捷径；引入信息瓶颈（Information Bottleneck）原则约束潜在运动维度，在保留足够动作相关信息的同时最小化无关背景噪声。
- **关键技术细节**：
  - **早期时间特征差异**：在模型早期阶段计算连续帧之间的特征差，迫使模型关注运动变化而非静态外观。
  - **信息瓶颈约束**：限制潜在运动编码的维度或互信息，使表征仅保留与动作相关的信息，抑制背景干扰。
  - **连续潜在运动表示**：不同于离散编码（如VQ），采用连续向量空间，避免量化误差，增强表达能力。
  - **评估指标**：提出两个低成本、直接的运动质量指标：①动作预测的均方误差（MSE）；②过去→当前与未来→当前运动嵌入的余弦相似度（用于衡量运动的一致性和预测能力）。
- **算法流程简述**：
  1. 输入互联网视频帧序列；
  2. 提取早期时空特征，计算帧间差异；
  3. 通过编码器映射到连续潜在空间，受信息瓶颈正则化；
  4. 解码器（或下游策略头）利用潜在运动进行动作预测或重建；
  5. 通过无监督损失（如重建损失、对比损失）优化，使潜在运动能够零样本泛化到未见视频。

## 3. 实验设计
- **数据集与场景**：文中提及使用“互联网规模视频”进行预训练，并在**模拟环境和真实世界机器人操作任务**上进行评估。但具体数据集名称（如Something-Something, Epic-Kitchens等）未在摘要中明确给出。
- **Benchmark**：未明确说明使用的标准测试基准；但通过联合训练与扩散模型（Diffusion）和自回归架构（Autoregressive）的策略对比性能。
- **对比方法**：推测主要与离散运动学习方法（如VQ-VAE、VideoGPT等）以及无运动学习基线进行对比。摘要强调CoMo在零样本生成伪动作和联合训练中表现更优。

## 4. 资源与算力
- **文中未明确说明**：摘要和元数据中没有提及GPU型号、数量、训练时长等算力信息。因此无法总结具体算力开销。

## 5. 实验数量与充分性
- **实验数量**：摘要描述为“广泛的（Extensive）模拟和真实世界实验”，并涉及两种主流策略架构（Diffusion & Autoregressive）。推测包含多组环境、多种任务对比以及消融研究。
- **充分性与客观性**：
  - 涵盖仿真和实物，验证了方法的实用性和泛化性。
  - 对两种架构均进行了测试，避免了特定架构的偏见。
  - 但未提供具体图表或数值，也无法确认消融实验是否完整（如是否对比了不同信息瓶颈强度、特征差异设计等）。总体倾向于充分，但缺乏细节。

## 6. 论文的主要结论与发现
- 连续潜在运动（CoMo）相比离散方法能更精确地表征复杂动作，并减少信息损失。
- 早期时间特征差异和信息瓶颈是防止捷径学习、提取干净运动信息的关键设计。
- 学得的连续运动表征具有强零样本泛化能力，可为未见视频生成有效的伪动作。
- 机器人动作与视频潜在运动共享连续分布，可直接促进统一策略的联合学习，在扩散和自回归架构上均取得优越性能。

## 7. 优点
- **创新性**：首次将连续潜在运动学习引入互联网视频的机器人动作表征，克服离散方法的瓶颈。
- **设计简洁有效**：早期特征差异+信息瓶颈，无复杂标注要求，可扩展至海量无标签视频。
- **评估指标创新**：提出低成本的运动评价指标（MSE和余弦相似度），便于研究者直接分析与迭代方法。
- **零样本能力**：无需微调即可为陌生视频生成伪动作，对机器人策略的泛化至关重要。
- **架构兼容**：同时支持扩散和自回归策略，具有通用性。

## 8. 不足与局限
- **信息缺失**：摘要和元数据未提供详细实验设置、数据集名称、对比方法定量结果、计算资源等，影响可复现性评估。
- **潜在偏差风险**：互联网视频可能包含与机器人动作不匹配的运动（如人体非交互运动），若视频分布与机器人操作领域差距过大，伪动作质量可能下降。
- **应用限制**：信息瓶颈可能过于激进，导致丢弃部分细微的动作相关信息；早期特征差异可能对极慢或极快运动处理不佳。
- **评估指标局限性**：所提MSE和余弦相似度虽然便宜，但未必完全反映下游策略最终性能，仍需通过真实机器人实验最终验证。
- **实验覆盖**：未提及是否在多种机器人形态（如机械臂、移动机器人）或不同难度任务上验证，泛化广度有待确认。

（完）
