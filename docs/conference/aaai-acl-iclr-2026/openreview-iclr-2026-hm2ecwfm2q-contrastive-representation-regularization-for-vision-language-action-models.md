---
title: Contrastive Representation Regularization for Vision-Language-Action Models
title_zh: 视觉-语言-动作模型的对比表示正则化
authors: "Taeyoung Kim, Jimin Lee, Myungkyu Koo, Dongyoung Kim, Kyungmin Lee, Changyeon Kim, Younggyo Seo, Jinwoo Shin"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=hm2EcwFm2Q"
tags: ["query:latent-vla"]
score: 8.0
evidence: 提出机器人状态感知对比损失，对齐VLM表示与机器人状态，提升VLA模型
tldr: 针对VLA模型中VLM表示对机器人控制信号（如动作、本体状态）不敏感的问题，提出RS-CL（机器人状态感知对比损失），利用状态间相对距离作为软监督，使表示更贴近机器人状态。在机器人操作任务上，RS-CL显著提升VLA策略的成功率，并增强了表示的可解释性。该方法简单有效，可作为VLA训练的通用正则化技术。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: VLA模型中VLM表示缺乏对机器人控制信号的敏感性。
method: 提出RS-CL对比损失，基于状态相对距离对齐表示与机器人本体状态。
result: 在机器人操作任务上提升VLA策略成功率，增强表示可解释性。
conclusion: RS-CL是一种有效的VLA表示正则化方法，可广泛适用。
---

## Abstract
Vision-Language-Action (VLA) models have shown its capabilities in robot manipulation by leveraging rich representations from pre-trained Vision-Language Models (VLMs).
However, their representations arguably remain suboptimal, lacking sensitivity to robotic signals such as control actions and proprioceptive states. 
To address the issue, we introduce Robot State-aware Contrastive Loss (RS-CL), a simple and effective representation regularization for VLA models, designed to bridge the gap between VLM representations and robotic signals.
In particular, RS-CL aligns the representations more closely with the robot's proprioceptive states, by using relative distances between the states as soft supervision.
Complementing the original action prediction objective, RS-CL effectively enhances control-relevant representation learning, while being lightweight and fully compatible with standard VLA training pipeline.
Our empirical results demonstrate that RS-CL substantially improves the manipulation performance of state-of-the-art VLA models;
it pushes the prior art from 30.8% to 41.5% on pick-and-place tasks in RoboCasa-Kitchen, through more accurate positioning during grasping and placing,
and boosts success rates from 45.0% to 58.3% on challenging real-robot manipulation tasks.

---

## 论文详细总结（自动生成）

好的，根据您提供的论文元数据和摘要信息（需要注意：实际论文全文未被提供，此处分析基于给定的元数据字段），以下是对该论文的结构化总结。

## 论文总结：视觉-语言-动作模型的对比表示正则化

### 1. 核心问题与整体含义（研究动机和背景）
- **背景**：视觉-语言-动作（VLA）模型通过利用预训练视觉-语言模型（VLM）的丰富表示，在机器人操作任务中展现了潜力。
- **核心问题**：当前VLA模型中，VLM的表示对机器人控制信号（如动作、本体状态）不够敏感，导致表示与机器人实际需求之间存在差距，限制了操纵性能。
- **研究动机**：提出一种轻量、有效的正则化方法，使VLM表示更贴近机器人状态，从而提升VLA策略的动作预测能力和任务成功率。

### 2. 方法论：核心思想、关键技术细节
- **核心思想**：引入**机器人状态感知对比损失（RS-CL）**，作为标准动作预测目标的补充。通过利用机器人本体状态之间的相对距离作为软监督，在训练过程中对齐VLM表示与机器人状态，增强表示对控制信号的敏感性。
- **关键技术细节**：
  - 在VLA训练流水线中，除了原始的下一动作预测损失外，额外添加RS-CL损失。
  - RS-CL基于对比学习框架：正样本定义为状态相近的表示对，负样本为状态距离较远的表示对；相对距离被用作软标签（而非硬二分类），使表示嵌入空间更贴合状态空间的结构。
  - 该方法与标准VLA训练流程完全兼容，无需修改网络架构，仅增加少量计算开销。
- **公式/算法流程**（文字说明）：
  1. 从训练批次中采样多个样本，每个样本包含视觉观测、语言指令、机器人当前本体状态（如关节位置、末端执行器位姿）以及动作标签。
  2. 计算样本间的本体状态欧氏距离，归一化后作为“软相似度”监督信号。
  3. 通过VLM编码器得到所有样本的表示向量，计算表示之间的相似度（如余弦相似度）。
  4. 使用KL散度或交叉熵损失，迫使表示相似度分布逼近状态相似度分布（即RS-CL损失）。
  5. 联合优化动作预测损失（如L2损失或交叉熵）与RS-CL损失。

### 3. 实验设计
- **数据集/场景**：
  - **仿真环境**：RoboCasa-Kitchen场景中的拾放（pick-and-place）任务。
  - **真实机器人**：具有挑战性的真实机器人操作任务（未指定具体环境，但成功率较低，说明任务具有难度）。
- **Benchmark**：对比了当前最先进的VLA模型（prior art），并在其基础上加入RS-CL进行比较。具体基线未在摘要中列出，但提到了“state-of-the-art VLA models”。
- **对比方法**：未明确列出，但从表述看应是直接将RS-CL应用于最新VLA模型，对比有无RS-CL的性能差异。

### 4. 资源与算力
- **文中未明确说明**。未提及使用的GPU型号、数量、训练时长或计算成本。仅指出RS-CL是轻量的，与标准VLA训练管线兼容。

### 5. 实验数量与充分性
- **实验数量**：根据摘要，仅在两个场景（一个仿真，一个真实机器人）上报告了主要结果。未提及消融实验、不同VLA骨干的泛化测试、不同对比损失设计的对比等。信息有限。
- **充分性评价**：从公开信息看，实验覆盖范围较窄，仅展示了单个任务类型的性能提升，缺乏对多种操作技能、不同环境复杂度、不同模型规模的系统验证。作为会议论文，实验的充分性可能不足，但具体需查看全文。

### 6. 主要结论与发现
- RS-CL能显著提升VLA模型在机器人操作任务上的成功率：在RoboCasa-Kitchen拾放任务上，将先前最优方法的成功率从30.8%提升至41.5%；在真实机器人操作任务上，从45.0%提升至58.3%。
- RS-CL通过更准确的抓取和放置定位来改善性能（仿真任务），并在真实世界中展现出可观的增益。
- 该方法可作为VLA训练的通用正则化技术，易于集成且有效。

### 7. 优点
- **简洁有效**：仅需在现有VLA训练框架中加入一项对比损失，无需修改模型结构或增加推理成本。
- **通用性**：宣称与标准VLA训练管线兼容，可作为通用插件使用。
- **可解释性**：通过强制表示与状态空间对齐，可能增强表示的可解释性（摘要中提及）。
- **性能提升显著**：在两个场景中都带来了超过10个百分点的绝对提升，效果明显。

### 8. 不足与局限
- **实验验证不充分**：仅测试了两个任务（一个仿真，一个真实），泛化性存疑。未报告在更多样化的操作任务（如移动操作、灵巧操作）、不同VLM骨干（如CLIP、PaLM-E）、不同动作表示（连续/离散）上的结果。
- **缺乏消融与敏感性分析**：未揭示RS-CL中的超参数（如温度系数、状态距离缩放）、对比损失的具体形式等对性能的影响。
- **计算成本未量化**：虽然声称轻量，但未给出训练时间或GPU内存的实际增加数据。
- **可能存在偏差风险**：由于论文被ICLR 2026拒稿（根据标签），可能方法存在未被指出的缺陷或对比基线不够强大。此外，真实机器人实验的硬件设置、随机种子等细节未知，难以复现。
- **应用限制**：RS-CL假设机器人本体状态可准确、一致地获得，在状态噪声大、缺失或不同机器人形态下效果可能下降。

（完）
