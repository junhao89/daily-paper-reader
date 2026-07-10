---
title: Latent Action Robot Foundation World Models for Cross-Embodiment Adaptation
title_zh: 潜伏动作机器人基础世界模型用于跨身体适应
authors: "Huang Huang, Sriram Yenamandra, Arjun Majumdar, Elie Aljalbout, Tushar Nagarajan, Tsung-Yen Yang, Akshara Rai, Michael Rabbat, Li Fei-Fei, Jiajun Wu, Tingfan Wu, Franziska Meier"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=vEZgPr1deb"
tags: ["query:wam-vla"]
score: 8.0
evidence: 在学到的统一潜伏动作空间中操作的基础世界模型
tldr: 针对机器人跨身体泛化中动作空间不统一的问题，提出LAC-WM，在统一潜伏动作空间中学习世界模型。通过对比显式运动标签条件模型，证明统一潜伏动作空间能更好地适应未见过的机器人形态，提升规划决策能力。实验跨多种机器人平台验证其泛化性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 不同机器人形态的动作空间多样性阻碍了世界模型泛化。
method: 提出LAC-WM，在学到的统一潜伏动作空间中训练基础世界模型。
result: 在多种未见机器人形态上展现出更强的适应性和规划性能。
conclusion: 统一潜伏动作空间是构建跨身体通用世界模型的有效策略。
---

## Abstract
Robot action-conditioned video generation models, also known as robot world models, hold great potential for enhancing robotic planning and decision-making. However, the diversity of robot embodiments and action spaces makes it challenging to build models that generalize across different embodiments. We introduce a robot foundation world model, LAC-WM, which operates within a learned unified latent action space shared across diverse embodiments. We explore how this unified action space improves the world model’s performance when adapted to previously unseen robot embodiments. Specifically, we compare LAC-WM to a baseline model EAC-WM conditioned on explicit motion labels. Our results show that conditioning on explicit labels creates disjoint action spaces across embodiments, limiting downstream task performance when adapting to new robots. We evaluate both models on a dexterous manipulation task. The latent action-conditioned model LAC-WM achieves up to a 46.7% improvement in performance over EAC-WM. Crucially, the unified latent action space allows LAC-WM’s downstream performance to scale positively with the number of embodiments used during pretraining. In contrast, the disjoint action space in EAC-WM leads to decreased performance as the number of pretraining embodiments increases. These results highlights the importance of a unified action space for efficient cross-embodiment learning, addressing a key challenge in robotics.

---

## 论文详细总结（自动生成）

根据提供的文本，这实际上是一个 OpenReview 的 CAPTCHA 验证页面，而非论文的完整内容。页面仅包含标题、URL来源和一段要求用户完成验证的提示，以及一段元数据（来自其他系统，可能用于检索）。元数据中包含了论文的标题、作者、摘要等，但摘要内容（关于 LAC-WM 的总结）是元数据的一部分，而非从页面正文提取的论文原文。因此，无法基于该页面进行详细的学术论文分析。

不过，我可以基于元数据中提供的摘要和结论，对这篇论文（标题：*Latent Action Robot Foundation World Models for Cross-Embodiment Adaptation*）做一个简洁的推断性总结，但需要说明信息来源有限。

---

### 基于可用元数据的推断总结（非完整论文分析）

**注意**：以下内容仅基于元数据中的摘要、结论和关键标签，并非来自完整论文原文，因此细节可能不准确。

#### 1. 核心问题与整体含义
- **研究动机**：不同机器人形态（embodiment）的动作空间差异很大，导致现有机器人世界模型（action-conditioned video prediction models）难以跨形态泛化。
- **整体含义**：提出一种在统一潜在动作空间中学习的基础世界模型，旨在提升跨身体（cross-embodiment）的适应性和规划能力。

#### 2. 方法论核心思想
- **核心思想**：构建 LAC-WM（Latent Action-Conditioned World Model），学习一个共享的统一潜在动作空间，不同机器人的动作都映射到这个空间中，从而训练通用世界模型。
- **关键技术细节**：对比基线模型 EAC-WM，它使用显式运动标签（explicit motion labels）作为条件，导致不同机器人的动作空间是分离的。LAC-WM 则通过自监督或对比学习等方式学习潜在动作表示，使不同形态的机器人共享同一动作空间。
- **算法流程**：（未在元数据中详述，推测为：收集多机器人交互数据 → 学习潜在动作编码器 → 在潜在空间上训练视频预测模型 → 在新机器人上微调或零样本规划）

#### 3. 实验设计
- **数据集/场景**：采用了灵巧操作任务（dexterous manipulation task）。
- **Benchmark**：与 EAC-WM（显式运动标签条件模型）对比。
- **对比方法**：只提及一个基线 EAC-WM，未提及其他方法。

#### 4. 资源与算力
- 未在元数据中提及任何 GPU 型号、数量或训练时长信息。

#### 5. 实验数量与充分性
- 元数据中仅报告了在单一灵巧操作任务上，LAC-WM 相对于 EAC-WM 的性能提升（最高 46.7%），以及预训练阶段使用的机器人形态数量增加时，LAC-WM 性能正向扩展而 EAC-WM 性能下降。
- 未提及其他数据集或消融实验。实验充分性难以评估，但仅凭一个任务和一种对比，显得不够充分。

#### 6. 主要结论与发现
- 统一潜在动作空间是实现高效跨身体学习的关键，能有效提升对新机器人形态的适应能力。
- 随着预训练形态数量增加，统一空间模型性能提升，而分离空间模型性能下降，突出了统一动作空间的重要性。

#### 7. 优点
- 方法设计简洁：通过学习潜在动作空间解决跨形态泛化问题，避免手工设计动作归一化。
- 实验展示了正相关扩展的重要性质，有实际应用价值。

#### 8. 不足与局限
- 实验仅在一个任务上（灵巧操作）验证，缺乏在多任务、多场景下的泛化测试。
- 算力需求未报告，难以评估实用性。
- 仅对比一个基线，缺乏与现有其他跨形态模型（如基于视觉-语言-动作的模型）的比较。
- 潜在动作空间的解释性和可控性未知。

---

（完）
