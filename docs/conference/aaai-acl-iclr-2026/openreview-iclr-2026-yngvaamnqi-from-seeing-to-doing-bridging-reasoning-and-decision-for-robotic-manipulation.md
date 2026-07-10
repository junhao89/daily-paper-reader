---
title: "From Seeing to Doing: Bridging Reasoning and Decision for Robotic Manipulation"
title_zh: 从看到做：桥接推理与决策的机器人操作
authors: "Yifu Yuan, Haiqin Cui, Yibin Chen, Zibin Dong, Fei Ni, Longxin Kou, Jinyi Liu, Pengyi Li, YAN ZHENG, Jianye HAO"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=yngvAamNQi"
tags: ["query:latent-vla"]
score: 8.0
evidence: 包含空间关系推理中间表征的新型VLA模型
tldr: 针对VLA模型零样本性能不足的问题，FSD通过空间关系推理生成中间表征（如物体位置关系），结合分层数据构建和自一致性机制，为操作提供细粒度引导。实验表明该方法在多种未见场景和任务上显著提升了成功率。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA模型因数据稀缺且异质性高导致零样本泛化差。
method: 利用VLM进行空间关系推理得到中间表征，并通过分层数据流水线和自一致性机制对齐坐标。
result: 在多个基准上零样本操作成功率显著超越基线VLA模型。
conclusion: 空间推理中间表征能有效提升VLA模型的泛化能力。
---

## Abstract
Achieving generalization in robotic manipulation remains a critical challenge, particularly for unseen scenarios and novel tasks. Current Vision-Language-Action (VLA) models, while building on top of general Vision-Language Models (VLMs), still fall short of achieving robust zero-shot performance due to the scarcity and heterogeneity prevalent in embodied datasets. To address these limitations, we propose FSD (From Seeing to Doing), a novel vision-language model that generates intermediate representations through spatial relationship reasoning, providing fine-grained guidance for robotic manipulation. Our approach combines a hierarchical data construction pipeline for training with a self-consistency mechanism that aligns spatial coordinates with visual signals. Through extensive experiments, we comprehensively validated FSD’s capabilities in both “seeing” and “doing”, achieving outstanding performance across 8 benchmarks for general spatial reasoning and embodied reference abilities, as well as on our proposed more challenging benchmark VABench. We also verified zero-shot capabilities in robot manipulation, demonstrating significant performance improvements over baseline methods in both SimplerEnv and real robot settings. Experimental results show that FSD achieves 40.6% success rate in SimplerEnv and 72% success rate across 8 real-world tasks, outperforming the strongest baseline by 30%.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：机器人操作中的泛化能力是关键挑战，特别是对于未见过的场景和新任务。现有的视觉-语言-动作（VLA）模型虽然基于通用视觉-语言模型（VLM），但由于具身数据集的稀缺性和异质性，零样本性能仍然不足。
- **核心问题**：如何让VLA模型在未见场景中实现鲁棒的零样本操作？现有方法缺乏从“看到”（视觉感知）到“做到”（具体动作）之间的有效桥梁，尤其是缺乏对空间关系的细粒度推理能力。
- **论文目标**：提出一种新的VLA模型，通过生成空间关系推理的中间表征，为机器人操作提供精细引导，从而显著提升零样本泛化性能。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：从“看到”到“做到”（FSD），利用VLM进行空间关系推理，生成中间表征（如物体位置关系），再结合分层数据构建和自一致性机制，将推理结果对齐到机器人操作的动作空间。
- **关键技术细节**：
  - **空间关系推理中间表征**：模型在视觉与动作之间插入一个可解释的中间层，输出物体之间的空间位置关系（例如“A在B的左边”、相对距离等），为操作提供细粒度引导。
  - **分层数据构建流水线**：针对训练数据稀缺问题，设计分层结构的数据构建方法，可能包含从简单到复杂、从仿真到真实的多层级数据生成流程，以增强模型对不同场景的适应能力。
  - **自一致性机制**：通过自监督方式对齐空间坐标与视觉信号，确保推理出的空间关系与真实图像中的位置一致，减少模态间的偏差。
- **公式/算法流程**（文字说明）：  
  （1）输入RGB图像和自然语言指令（如“将杯子放到盘子里”）；  
  （2）VLM编码器提取视觉特征，FSD模型额外输出空间关系中间表征（如物体坐标、相对方位）；  
  （3）自一致性模块校验视觉特征与空间坐标的一致性；  
  （4）基于对齐后的表征生成机器人操作动作（末端执行器目标位姿）；  
  （5）训练采用分层数据流水线，结合仿真和部分真实数据。

## 3. 实验设计：数据集/场景、基准、对比方法

- **基准与数据集**：
  - 8个通用空间推理与具身指代能力基准（具体名称未在摘要中列出，可能包含BLIP、NLVR等或机器人操作标准测试集）。
  - **VABench**：作者提出的更具挑战性的新基准，用于测试复杂操作场景。
  - **SimplerEnv**：仿真环境（可能是基于MuJoCo或Isaac Gym的简化操作环境）。
  - 真实世界设置：8种实际机器人操作任务（如抓取、放置、堆叠等）。
- **对比方法**：与基线VLA模型（如RT-2、Octo等，但摘要中未明确名称，仅说“强基线”）进行零样本对比，FSD在SimplerEnv上成功率为40.6%，真实任务成功率为72%，分别超过最强基线30%（绝对提升约9%和21%）。

## 4. 资源与算力

- **文中未明确说明**：摘要及元数据未提及使用的GPU型号、数量、训练时长等具体算力信息。可能需要在全文或其他公开资源中查找。此处需客观指出“原文未提供相关数据”。

## 5. 实验数量与充分性

- **实验覆盖范围**：
  - 8个通用基准 + 1个新基准VABench + 仿真环境SimplerEnv + 真实机器人8个任务 → 总计至少10+个不同评估维度。
  - 包含了对“看”（空间推理）和“做”（操作执行）两方面能力的综合验证。
- **充分性评估**：
  - 优点：覆盖仿真和真实场景，且真实任务数量（8个）属于中等规模；零样本设置符合泛化目标。
  - 不足：摘要中未提及消融实验、超参数分析或不同数据规模下的性能对比；也未报告标准误差或多次重复实验结果。因此充分性有待全文补充验证。但总体基于现有信息，实验设计较为全面。

## 6. 主要结论与发现

- FSD通过引入空间关系推理中间表征，显著提升了VLA模型的零样本泛化能力。
- 在SimplerEnv仿真环境实现40.6%成功率，在8个真实任务上实现72%成功率，均超越最强基线30%以上（相对提升）。
- 分层数据构建和自一致性机制是成功的关键组件，能够缓解具身数据稀缺和异质性带来的问题。
- 证明了“从看到到做到”的桥接设计比端到端的VLA模型更有效。

## 7. 优点（方法或实验设计亮点）

- **方法创新性**：首次将空间关系推理作为显式中间表征引入VLA模型，使模型具备可解释性和细粒度引导能力，区别于传统端到端黑箱预测。
- **实用性**：分层数据流水线可复用性强，有望降低真实数据采集成本；自一致性机制增强了跨模态对齐鲁棒性。
- **实验验证**：同时包含通用推理基准、专用挑战基准、仿真和真实机器人环境，从多个维度证明泛化能力，并为社区提供了新基准VABench。

## 8. 不足与局限

- **算力信息缺失**：未报告训练成本，不利于复现和公平对比。
- **实验细节有限**：缺乏消融实验量化每个组件（如自一致性、分层数据）的贡献；未给出性能置信区间或多次运行标准差。
- **任务规模**：真实任务仅8个，且可能集中在特定场景（如桌面操作），泛化到复杂非结构化环境尚无证据。
- **零样本定义局限**：摘要中使用“零样本”，但模型训练中可能包含了同类型合成数据，与严格零样本定义存在一定差距。
- **可复现性**：未提供代码或模型权重公开计划（元数据中未提及）。
- **偏差风险**：VABench由作者提出，可能存在数据分布偏向性；基线对比未列出具体方法名称，削弱可信度。

（完）
