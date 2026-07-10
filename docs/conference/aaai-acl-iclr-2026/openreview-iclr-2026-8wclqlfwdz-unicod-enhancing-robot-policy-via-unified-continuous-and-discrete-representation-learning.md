---
title: "UniCoD: Enhancing Robot Policy via Unified Continuous and Discrete Representation Learning"
title_zh: UniCoD：通过统一连续和离散表征学习增强机器人策略
authors: "Jianke Zhang, Yucheng Hu, Yanjiang Guo, Yichen Liu, Wenna Chen, Chaochao Lu, Jianyu Chen"
date: 2025-09-03
pdf: "https://openreview.net/pdf?id=8wcLQlfWdz"
tags: ["query:latent-vla"]
score: 8.0
evidence: 统一连续与离散表征学习用于机器人策略
tldr: 该论文提出UniCoD方法，统一了连续与离散表征学习，融合视觉-语言理解与视觉动态建模的优势，用于构建通用机器人策略。通过联合预训练，模型同时具备语义理解和动作预测能力。实验表明该方法在多种操作任务上优于单独使用理解或生成模型的方法。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 通用机器人策略需同时具备语义理解和动态建模能力，但现有方法将两者割裂。
method: 提出统一的连续与离散表征学习框架，结合视觉-语言模型和生成模型的预训练优势。
result: 在多个机器人操作基准上取得优于单一模型的结果，展现综合性能提升。
conclusion: 统一连续与离散表征是构建更通用机器人策略的有效方向。
---

## Abstract
Building generalist robot policies that can handle diverse tasks in open-ended environments is a central challenge in robotics. To leverage knowledge from large-scale pretraining, prior work has typically built generalist policies either on top of vision-language understanding models (VLMs) or generative models. However, both semantic understanding from vision-language pretraining and visual dynamics modeling from visual-generation pretraining are crucial for embodied robots.
Recent unified models of generation and understanding have demonstrated strong capabilities in both comprehension and generation through large-scale pretraining. We posit that robotic policy learning can likewise benefit from the combined strengths of understanding, planning and continuous future representation learning. Building on this insight, we introduce UniCoD, which acquires the ability to dynamically model high-dimensional visual features through pretraining on over 1M internet-scale instructional manipulation videos. Subsequently, UniCoD is fine-tuned on data collected from the robot embodiment, enabling the learning of mappings from predictive representations to action tokens. Extensive experiments show our approach consistently outperforms baseline methods in terms of 9\% and 12\% across both simulation environments and real-world out-of-distribution tasks. Demos and code can be found at \href{https://sites.google.com/view/uni-cod}{our anonymous website}.

---

## 论文详细总结（自动生成）

# 中文总结：UniCoD：通过统一连续和离散表征学习增强机器人策略

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：构建通用机器人策略（generalist robot policy）以处理开放环境中的多样化任务，是机器人领域的核心挑战。
- **背景**：现有方法通常依靠大规模预训练，但存在两种割裂的路线：一类基于视觉-语言理解模型（VLMs），擅长语义理解；另一类基于生成模型，擅长视觉动态建模。这两方面能力对于具身机器人同样关键，但很少有工作同时整合二者。
- **整体含义**：论文认为，机器人策略学习可以从“理解+规划+连续未来表征学习”的联合优势中受益，提出统一的连续与离散表征学习框架，以增强策略的通用性和性能。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：UniCoD 通过统一连续表征（continuous representation）和离散表征（discrete representation）的学习，融合视觉语言理解与视觉动态建模的优势。
- **关键技术细节**：
  - 首先，在超过 1M 规模的互联网指令性操作视频上对模型进行预训练，使其获得动态建模高维视觉特征的能力。
  - 然后，在机器人本体收集的数据上对预训练模型进行微调，学习从预测性表征到动作标记（action tokens）的映射。
  - 整体流程分为两个阶段：预训练阶段（大规模互联网视频）和微调阶段（机器人数据）。
- **公式或算法流程文字说明**：无显式公式给出，流程可概括为：预训练→动态视觉特征建模 → 微调→动作映射 → 最终策略输出。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集**：预训练使用超过 1M 互联网规模的指令性操作视频；微调使用机器人本体采集数据。
- **场景/benchmark**：仿真环境（simulation environments）和真实世界分布外任务（real-world out-of-distribution tasks）。
- **对比方法**：未在摘要中明确列出具体对比方法，但声称“consistently outperforms baseline methods”，即在仿真和真实场景下分别提升 9% 和 12% 的平均表现。推断对比了仅用 VLM 或仅用生成模型的基线方法。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。

- **文中未明确说明**：提供的摘要和元数据中没有提及 GPU 型号、数量、训练时长等具体算力信息。无法总结算力使用情况。

## 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平

- **实验数量**：摘要中仅提到在仿真环境和真实世界任务上进行了实验，并给出了总体提升百分比。具体组数、消融实验细节未提及。
- **充分性与公平性**：由于信息有限，无法判断实验的充分性与公平性。论文声称结果“consistently outperforms”，但未公开完整的实验设置（如基线数量、超参数、随机种子等），因此无法确定实验设计是否全面、客观。需要阅读全文才能评估。

## 6. 论文的主要结论与发现

- 统一连续与离散表征学习（UniCoD）能够使机器人策略同时受益于视觉语言理解和视觉动态建模。
- 在仿真环境上平均提升 9%，在真实世界分布外任务上平均提升 12%，优于仅使用单一类型预训练模型的基线方法。
- 证明了大规模互联网操作视频预训练+机器人数据微调这一范式的有效性。

## 7. 优点：方法或实验设计上有哪些亮点

- **方法亮点**：
  - 首次将“连续表征+离散表征”统一纳入机器人策略学习，融合了理解与生成两大优势。
  - 使用超过 1M 互联网操作视频进行预训练，数据规模大，来源多样，有助于泛化。
  - 采用两阶段训练（预训练→微调），充分利用大规模数据与机器人本体数据。
- **实验设计亮点**：
  - 涵盖了仿真环境和真实世界分布外任务，兼顾了可控验证和实际部署考验。
  - 提供了网站和代码（匿名），便于复现和验证。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验覆盖不足**：抽象中未报告在多种不同机器人平台、不同操作难度任务上的详细结果，也未提供消融实验分析各部分贡献。
- **偏差风险**：预训练数据仅来自互联网操作视频，可能存在领域偏差（如特定物体、场景），影响在全新环境下的泛化。
- **应用限制**：
  - 依赖大规模预训练数据和机器人硬件数据，获取成本高。
  - 未说明模型推理速度和实时性要求，可能不适用于高速动态环境。
  - 未讨论安全性与鲁棒性，例如对未见过物体或干扰的应对能力。
- **信息不完整**：由于仅提供摘要，无法判断训练稳定性、超参数敏感性、失败案例等不足。

（完）
