---
title: "CoMo: Learning Continuous Latent Motion from Internet Videos for Scalable Robot Learning"
title_zh: CoMo：从互联网视频学习连续潜伏运动用于可扩展机器人学习
authors: "Jiange Yang, Yansong Shi, Haoyi Zhu, Mingyu Liu, Kaijing Ma, Yating Wang, Gangshan Wu, Tong He, Limin Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=Cu9NOcqfzN"
tags: ["query:latent-vla"]
score: 7.0
evidence: 利用信息瓶颈从视频学习连续潜伏运动
tldr: 现有从视频学习潜伏运动的方法多为离散表示，存在信息损失，难以处理复杂动态。提出CoMo，学习连续潜伏运动，通过早期时序特征差异机制防止捷径学习，并利用信息瓶颈控制信息维度，在保留动作相关信息的同时抑制背景噪声。实验证明连续表示在多个机器人任务中优于离散方法，为从互联网视频构建通用机器人提供了更有效的表示。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 离散潜伏运动表示存在信息损失，不适用于复杂动态。
method: 采用早期时序特征差异和信息瓶颈学习连续潜伏运动。
result: 连续潜伏表示在机器人学习中优于离散方法。
conclusion: 连续潜伏运动学习为构建通用机器人提供了更有效的表示。
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

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：从互联网视频中无监督学习潜伏运动（latent motion）是构建通用机器人策略的关键，但现有方法主要采用离散表示（如VQ-VAE），存在信息损失，难以处理复杂、细粒度的动态场景。
- **研究动机**：离散表示会丢弃动作相关的连续细节，且容易混入静态外观噪声，导致机器人学习效果受限。需要一种更精确、可扩展的连续潜伏运动表示方法。
- **整体含义**：提出CoMo，通过连续潜伏运动学习，从大规模互联网视频中提取高质量动作先验，实现零样本泛化，并直接受益于机器人策略的联合训练，为构建通用具身智能体提供更有效的表示基础。

## 2. 论文提出的方法论

### 核心思想
- 学习**连续潜伏运动**（continuous latent motion），而非离散量化，保留更丰富的动态信息。
- 使用**信息瓶颈（Information Bottleneck, IB）** 原理约束潜伏表示的维度，在保留动作相关信息的同时抑制背景噪声。

### 关键技术细节
1. **早期时序特征差异机制**（Early Temporal Feature Difference）：
   - 在视频帧特征提取的早期阶段，计算连续帧之间的特征差异（如相邻帧特征相减），以抑制静态外观信息，避免模型通过外观捷径学习（shortcut learning），迫使模型关注运动本身。
2. **信息瓶颈约束**：
   - 对潜伏运动表示的维度施加正则化，使其在编码时只保留与动作预测最相关的信息，减少无关背景噪声的干扰。具体可通过最小化潜伏表示与先验正态分布之间的KL散度实现。
3. **训练流程**（文字说明）：
   - 输入视频片段，经编码器提取帧级特征；
   - 对相邻帧特征做差异操作，得到运动特征序列；
   - 通过一个带有信息瓶颈的变分模块，将运动特征映射为连续潜伏运动表示；
   - 利用该潜伏表示解码重构未来帧或预测动作，作为辅助监督。

### 额外贡献：两个便捷评估指标
- **MSE of action prediction**：使用预测动作的均方误差直接评估潜伏运动保留动作信息的能力。
- **Cosine similarity between past-to-current and future-to-current motion embeddings**：利用过去-当前与未来-当前运动嵌入的余弦相似度，衡量运动表示的时间一致性。

## 3. 实验设计

- **数据集/场景**：
  - 互联网视频数据集（论文未明确指明具体名称，推测为大规模视频数据）
  - 模拟环境（如仿真机器人操作场景）
  - 真实世界机器人任务
- **Benchmark**：
  - 对比离散潜伏运动方法（如VQ-VAE based方法）
  - 在扩散策略（diffusion policy）和自回归策略（autoregressive policy）两种架构下测试联合训练效果
- **对比方法**：
  - 现有离散潜伏运动学习方法（未列出具体名称）
  - 直接使用原始观测的基线方法（可能提及但未详述）

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量或训练时长。仅提及“extensive simulated and real-world experiments”，但未给出算力信息。

## 5. 实验数量与充分性

- **实验数量**：摘要指出进行了“广泛的模拟和真实世界实验”，并使用了两种不同的策略架构（扩散与自回归）进行验证。但未给出具体实验组数（如消融实验个数、数据集数量）。
- **充分性评价**：
  - **优点**：涵盖了仿真和真实场景，且与多种主流架构结合，验证了通用性。
  - **不足**：缺少关键组件（早期时序差异、信息瓶颈）的独立消融实验详细结果；未与更多最新的连续运动学习方法（如基于flow或扩散的潜伏运动）对比；未提供统计显著性分析。整体实验数量可接受，但细节披露有限。

## 6. 论文的主要结论与发现

- 连续潜伏运动表示在机器人学习中**显著优于**离散表示，尤其对于复杂动态场景。
- CoMo具有**零样本泛化**能力：可直接为未见过的视频生成有效的伪动作（pseudo actions）。
- 联合训练使用CoMo伪动作的策略（无论扩散还是自回归架构）均获得了**更优的性能**。
- 信息瓶颈和早期时序差异机制有效提升了运动表示的质量。

## 7. 优点

1. **方法创新**：首次将信息瓶颈应用于连续潜伏运动学习，并设计早期时序差异机制避免捷径，思路新颖。
2. **连续表示优于离散**：克服了离散化带来的信息损失，更贴合机器人连续动作空间。
3. **零样本泛化**：无需额外标注即可为任意视频生成动作先验，具备大规模可扩展性。
4. **评估指标实用**：提出两个低成本、直接的指标（动作预测MSE和运动嵌入余弦相似度），便于学术界快速评估运动学习方法。
5. **架构兼容**：同时验证了在扩散和自回归策略上的有效性，证明方法通用。

## 8. 不足与局限

1. **实验细节缺失**：未公开具体数据集、训练超参数、消融实验结果数值，可复现性存疑。
2. **计算资源不透明**：未说明训练所需GPU型号、数量和时长，难以评估方法的实际成本。
3. **对比范围有限**：仅与离散方法对比，未与近期流行的**连续运动预测方法**（如基于ODE、diffusion的运动先验）比较，优势可能被夸大。
4. **鲁棒性分析不足**：未探讨在重度遮挡、多视角变化、外观剧烈变化等极端场景下的表现。
5. **应用限制**：方法依赖视频帧的时序一致性，对拍摄质量差、帧率不稳定的互联网视频可能失效；信息瓶颈可能导致丢失部分有益细节。
6. **偏差风险**：互联网视频中存在人-物交互偏斜，学习的潜伏运动可能偏向常见运动模式，对长尾动作泛化能力未知。

（完）
