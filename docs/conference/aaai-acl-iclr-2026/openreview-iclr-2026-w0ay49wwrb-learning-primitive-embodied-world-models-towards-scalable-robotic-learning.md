---
title: "Learning Primitive Embodied World Models: Towards Scalable Robotic Learning"
title_zh: 学习原始具身世界模型：迈向可扩展的机器人学习
authors: "Qiao Sun, Liujia Yang, Wei Tang, Wei Huang, Kaixin Xu, Yongchao Chen, Mingyu Liu, Haoyi Zhu, Jiange Yang, Yating Wang, Yilun Chen, Xili Dai, Nanyang Ye, Qinying Gu"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=W0AY49wWrb"
tags: ["query:wam-vla"]
score: 7.0
evidence: 用于可扩展潜在世界建模的原始具身世界模型
tldr: 针对具身数据稀缺和高维性导致的视频生成世界模型训练瓶颈，提出原始具身世界模型（PEWM）。通过将视频生成限制在固定的短原始运动片段，大幅降低生成复杂性，使得在有限交互数据上训练可扩展的世界模型成为可能。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 视频生成世界模型依赖大规模具身交互数据，但数据稀缺且高维，难以缩放。
method: 将世界模型拆解为原始运动片段的生成，降低数据需求和生成难度。
result: 在少数据条件下实现了可扩展的世界建模。
conclusion: 原始运动先验为具身领域达到GPT时刻提供了可行路径。
---

## Abstract
While video-generation-based embodied world models have gained increasing
attention, their reliance on large-scale embodied interaction data remains a key
bottleneck. The scarcity, difficulty of collection, and high dimensionality of em-
bodied data fundamentally limit the alignment granularity between language and
actions and exacerbate the challenge of long-horizon video generation—hindering
generative models from achieving a "GPT moment" in the embodied domain. There
is a naive observation: the diversity of embodied data far exceeds the relatively
small space of possible primitive motions. Based on this insight, we propose a novel
paradigm for world modeling–Primitive Embodied World Models (PEWM). By
restricting video generation to fixed short horizons, our approach 1) enables fine-
grained alignment between linguistic concepts and visual representations of robotic
actions, 2) reduces learning complexity, 3) improves data efficiency in embodied
data collection, and 4) decreases inference latency. By equipping with a modular
Vision-Language Model (VLM) planner and a Start-Goal heatmap Guidance mech-
anism (SGG), PEWM further enables flexible closed-loop control and supports
compositional generalization of primitive-level policies over extended, complex
tasks. Our framework leverages the spatiotemporal vision priors in video mod-
els and the semantic awareness of VLMs to bridge the gap between fine-grained
physical interaction and high-level reasoning, paving the way toward scalable,
interpretable, and general-purpose embodied intelligence.

---

## 论文详细总结（自动生成）

好的，以下是对该论文的详细中文总结。

---

### 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：基于视频生成的世界模型在具身智能领域日益受到关注，但其发展受限于大规模具身交互数据的稀缺性、高维性和收集难度。这导致语言指令与机器人动作之间的对齐粒度不足，并且长程视频生成任务难以求解，阻碍了生成式模型在具身领域实现类似大语言模型（LLM）的“GPT时刻”。
- **关键洞察**：论文观察到具身数据的多样性虽然看似庞大，但实际可用的基本原始运动（primitive motions）空间相对较小。基于此，提出将世界模型的视频生成任务限制在固定的短时间原始运动片段上，从而大幅降低学习复杂度和数据需求。

### 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出**原始具身世界模型（Primitive Embodied World Models, PEWM）**。通过将视频生成限定在固定的短视界（short horizon）上，将复杂的全局运动生成拆解为对有限原始运动片段的预测。
- **关键技术细节**：
  - **视频生成约束**：模型不直接生成整个任务的长视频，而是生成固定时长的短片段，每个片段对应一个原始运动（如“向前走”、“抓取”等）。这显著降低了生成难度和计算开销。
  - **对齐粒度提升**：固定短视界使得语言概念（如“向左转”）与机器人动作的视觉表示之间能够实现更细粒度的对齐。
  - **模块化规划器**：引入模块化的视觉-语言模型（VLM）作为高层规划器，负责将复杂任务分解为一系列原始运动序列。
  - **起点-终点热图引导机制（Start-Goal heatmap Guidance, SGG）**：该机制用于引导世界模型生成符合高层规划意图的原始运动片段，实现灵活的闭环控制，并支持原始级别策略在长程复杂任务上的组合泛化。

### 3. 实验设计：数据集、基准、对比方法

- **数据集 / 场景**：论文原文摘要及元数据中**未明确提及**具体使用的数据集或仿真/真实场景名称。
- **基准（Benchmark）**：未说明。
- **对比方法**：未在摘要中列举具体对比方法。

> **注**：由于提供的文本仅包含摘要和元数据，无法获取完整的实验设置信息。需要查阅全文才能获知具体测试环境、比较基线等。

### 4. 资源与算力

- **未提及**：摘要和元数据中**没有提及**任何关于GPU型号、数量、训练时长等算力资源的信息。

### 5. 实验数量与充分性

- **实验数量**：从摘要可推测作者可能进行了**多组实验**（例如不同任务、消融研究等）来验证PEWM在数据效率、对齐粒度、推理延迟等方面的优势，但具体实验数量无法从现有材料中确定。
- **充分性判断**：基于摘要声称的“在少数据条件下实现了可扩展的世界建模”，该方法在数据效率上应有充分证据。但由于缺乏具体实验细节（如数据集规模、基线方法对比结果、统计显著性等），无法从当前文本客观评估其实验的充分性和公平性。需要完整论文才能判断。

### 6. 主要结论与发现

- **主要结论**：原始运动先验为具身领域实现类似LLM的“GPT时刻”提供了一条可行路径。PEWM通过将世界模型限制在原始运动片段上，在有限交互数据下实现了可扩展的世界建模，并同时提升了语言-动作对齐粒度、数据效率与推理速度。
- **关键发现**：借助VLM规划器和SGG机制，PEWM能够在长程复杂任务中实现原始级策略的组合泛化，表明该方法可以桥接细粒度物理交互与高层语义推理。

### 7. 优点

- **创意性强**：提出将世界模型生成目标从复杂全局视频简化为原始运动片段，巧妙利用了“原始运动空间小”这一朴素观察，降低了模型对海量数据的依赖。
- **综合架构设计**：将视频生成世界模型、VLM规划器、热图引导机制有机整合，形成完整的闭环控制与泛化框架。
- **潜在实用性**：提高了数据效率和推理速度，更易于在实际机器人系统中部署。
- **可解释性**：通过原始运动片段分解，模型行为更易被理解和干预。

### 8. 不足与局限

- **实验细节缺失**：根据摘要和元数据，无法得知具体测试环境、对比基线、超参数设置等，难以全面评估方法有效性。
- **原始运动定义模糊**：如何定义和提取“原始运动”边界？是否对所有任务都适用？文中未展开说明。
- **长程任务泛化性**：虽然声称支持组合泛化，但实际复杂任务中运动组合的爆炸性增长仍可能带来挑战。
- **数据收集成本**：虽然降低了视频生成数据需求，但原始运动片段的标注或预定义本身仍需专家知识。
- **真实场景验证未知**：没有提及是否在真实机器人上进行了物理验证，仅限于仿真或离线评估的风险未排除。

（完）
