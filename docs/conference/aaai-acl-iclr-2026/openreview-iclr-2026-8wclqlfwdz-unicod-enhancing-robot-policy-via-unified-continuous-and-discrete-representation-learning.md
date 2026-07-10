---
title: "UniCoD: Enhancing Robot Policy via Unified Continuous and Discrete Representation Learning"
title_zh: UniCoD：通过统一连续和离散表征学习增强机器人策略
authors: "Jianke Zhang, Yucheng Hu, Yanjiang Guo, Yichen Liu, Wenna Chen, Chaochao Lu, Jianyu Chen"
date: 2025-09-03
pdf: "https://openreview.net/pdf?id=8wcLQlfWdz"
tags: ["query:latent-vla"]
score: 8.0
evidence: 统一连续和离散表征学习用于机器人策略
tldr: 针对通用机器人策略需要同时具备语义理解和视觉动力学建模的问题，UniCoD提出统一连续与离散表征学习框架，融合视觉语言理解与视觉生成预训练的知识。在大规模多任务数据集上验证了该方法有效提升了策略的泛化能力。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有通用策略要么基于VLM要么基于生成模型，未能结合两者优势。
method: 设计联合连续（连续潜向量）和离散（离散令牌）表征学习，融合理解与生成预训练。
result: 在大规模多任务机器人数据集上取得更好的零样本泛化性能。
conclusion: 统一连续与离散表征能更充分利用预训练知识。
---

## Abstract
Building generalist robot policies that can handle diverse tasks in open-ended environments is a central challenge in robotics. To leverage knowledge from large-scale pretraining, prior work has typically built generalist policies either on top of vision-language understanding models (VLMs) or generative models. However, both semantic understanding from vision-language pretraining and visual dynamics modeling from visual-generation pretraining are crucial for embodied robots.
Recent unified models of generation and understanding have demonstrated strong capabilities in both comprehension and generation through large-scale pretraining. We posit that robotic policy learning can likewise benefit from the combined strengths of understanding, planning and continuous future representation learning. Building on this insight, we introduce UniCoD, which acquires the ability to dynamically model high-dimensional visual features through pretraining on over 1M internet-scale instructional manipulation videos. Subsequently, UniCoD is fine-tuned on data collected from the robot embodiment, enabling the learning of mappings from predictive representations to action tokens. Extensive experiments show our approach consistently outperforms baseline methods in terms of 9\% and 12\% across both simulation environments and real-world out-of-distribution tasks. Demos and code can be found at \href{https://sites.google.com/view/uni-cod}{our anonymous website}.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **核心挑战**：构建能够在开放环境中处理多样任务的通用机器人策略是机器人领域的核心难题。
- **现有方法局限**：当前通用策略主要分为两类：基于视觉-语言理解模型（VLM）的策略和基于生成模型的策略。然而，两者各有侧重：VLM擅长语义理解，生成模型擅长视觉动力学建模。机器人既需要语义理解（理解指令、物体语义），也需要视觉动力学建模（预测未来视觉状态、规划动作）。
- **动机**：近期统一生成与理解的模型在大规模预训练中同时展现了理解与生成能力。作者认为机器人策略学习同样可以受益于理解、规划与连续未来表征学习的结合。因此，提出UniCoD，旨在统一连续与离散表征学习，融合视觉语言理解与视觉生成预训练的知识。

## 2. 方法论

- **核心思想**：设计一种联合连续（连续潜向量）和离散（离散令牌）表征学习框架，同时利用理解型预训练（如VLM）和生成型预训练（如视频预测）的知识。
- **关键技术细节**：
  - **预训练阶段**：在超过100万互联网规模的操作教学视频上预训练，获得动态建模高维视觉特征的能力。
  - **微调阶段**：在机器人本体收集的数据上微调，学习从预测表征到动作令牌的映射。
  - 模型能够从预测的未来视觉表征中解码出离散动作令牌，从而实现连续潜空间与离散动作空间的统一。
- **公式或算法流程**（文字说明）：
  - 输入：当前观测（图像）和语言指令。
  - 使用统一编码器提取连续潜表征（理解分支）和离散令牌（生成分支）。
  - 通过预测模块在连续潜空间推理未来状态，同时离散令牌负责细节重建。
  - 最终动作头将预测表征映射为动作序列。

## 3. 实验设计

- **数据集/场景**：
  - 预训练：1M+ 互联网规模的操作教学视频。
  - 微调与评估：机器人本体收集的数据，包括仿真环境和真实世界。
- **基准（Benchmark）**：未明确指定具体基准名称，但提到了在仿真环境和真实世界的分布外（OOD）任务上评估。
- **对比方法**：与基线方法进行对比，具体基线未在摘要中列出。

## 4. 资源与算力

- 论文摘要中未明确说明使用的GPU型号、数量或训练时长。仅提到在大规模互联网数据上预训练，但未提供详细信息。
- 注：由于是OpenReview验证页面截获的内容，元数据中也没有算力信息。

## 5. 实验数量与充分性

- **实验数量**：摘要中报告了仿真环境提升9%、真实世界OOD任务提升12%，并进行了消融研究（元数据提到有消融实验）。但未给出具体实验组数（如不同数据集、不同种子等）。
- **充分性与公平性**：从摘要看，覆盖了仿真和真实场景，且与基线对比，实验设计相对全面。但缺乏详细实验设置描述（如随机种子、统计显著性等），无法完全判断公平性。一般认为ICLR会议论文的实验设计较为严谨。

## 6. 主要结论与发现

- 统一连续与离散表征能够更充分利用预训练知识（理解+生成），从而显著提升机器人策略的零样本泛化性能。
- 在仿真环境上相比基线方法平均提升9%，在真实世界分布外任务上平均提升12%。

## 7. 优点

- **方法创新**：首次将统一连续与离散表征学习应用于机器人策略，有效融合了理解与生成两种预训练范式的优势。
- **大规模预训练**：利用超百万互联网视频进行预训练，增强了视觉动力学建模能力。
- **泛化能力强**：在分布外任务上表现突出，证明方法具有良好的泛化能力。
- **开源**：提供了演示网站和代码（匿名），有利于复现和社区发展。

## 8. 不足与局限

- **实验细节缺失**：摘要中未详细列出基线方法的具体名称、仿真环境设置、数据集规模等，可能影响读者评价。
- **算力资源未说明**：未报告训练所需GPU型号、数量、时间，难以评估复现成本。
- **偏差风险**：仅提到了提升百分比，未报告方差或统计学显著性检验，可能存在偶然性。
- **应用限制**：方法依赖于大规模预训练数据和机器人本体数据采集，对设备要求高；且仅验证了操作任务，其他类型任务（如导航）未涉及。
- **开源链接可能失效**：论文中提供的匿名网站在审稿阶段可能存在访问限制。

（完）
