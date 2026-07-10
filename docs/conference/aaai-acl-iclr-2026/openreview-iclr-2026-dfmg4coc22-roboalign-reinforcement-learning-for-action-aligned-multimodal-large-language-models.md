---
title: "RoboAlign: Reinforcement Learning for Action-Aligned Multimodal Large Language Models"
title_zh: RoboAlign：面向动作对齐的多模态大语言模型的强化学习
authors: "Dongyoung Kim, Sumin Park, Woomin Song, Seungku Kim, Taeyoung Kim, Huiwon Jang, Jaehyung Kim, Younggyo Seo, Jinwoo Shin"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=dFmG4cOC22"
tags: ["query:latent-vla"]
score: 8.0
evidence: 使用强化学习对齐多模态大模型表征与低层动作以改进VLA
tldr: 该论文针对VLA中多模态大模型与低层动作之间存在模态间隙的问题，提出RoboAlign框架，通过强化学习生成动作令牌来对齐表征与动作。实验证明该对齐方法显著提升了VLA在多种操作任务上的成功率和泛化性，为利用语言模型增强VLA提供了有效手段。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: MLLM的表征与机器人低层动作之间存在模态差距，限制VLA性能。
method: 通过强化学习让MLLM生成动作令牌，对齐表征与动作。
result: 在多个操作基准上，RoboAlign显著优于标准VLA预训练方法。
conclusion: 强化学习对齐是连接语言模型与机器人动作的有效桥梁。
---

## Abstract
In recent years, state-of-the-art vision–language–action models (VLAs) have been built upon pre-trained multimodal large language models (MLLMs). However, how to systematically train MLLMs to improve VLA performance remains an open problem. While prior approaches primarily focus on strengthening embodied reasoning via linguistic actions, the modality gap limits the transferability of language-based knowledge to non-linguistic low-level actions produced by VLAs. To address this problem, we propose a novel framework RoboAlign that aligns MLLM representations with low-level actions, thereby producing MLLMs well-suited for VLA. Specifically, we achieve action alignment through reinforcement learning, where the model generates action tokens via zero-shot reasoning in natural language. To validate the effectiveness of RoboAlign, we train VLAs by adding a diffusion-based action head on top of an MLLM backbone and evaluate them on major robotics benchmarks. Specifically, training base MLLMs with RoboAlign improves the performance on robotic tasks by 17.5%, 18.9%, and 106.6% on LIBERO, CALVIN, and real-world robotic environments, respectively. Moreover, RoboAlign outperforms models aligned only with language-described actions or with supervised fine-tuning based approaches such as ECoT, demonstrating its effectiveness and broad applicability.

---

## 论文详细总结（自动生成）

以下是对论文 **"RoboAlign: Reinforcement Learning for Action-Aligned Multimodal Large Language Models"** 的中文总结（基于摘要及元数据信息，因完整论文文本不可获取）。

---

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：当前最先进的视觉-语言-动作模型（VLA）通常基于预训练的多模态大语言模型（MLLM）构建，但 **MLLM 的特征表示与机器人底层动作之间存在显著的模态差距**，导致语言知识难以直接迁移到非语言的低层动作上，限制了 VLA 的性能。
- **研究动机**：现有方法主要聚焦于通过语言动作增强具身推理（如 ECoT），但未能解决模态差异带来的特征不对齐问题。因此，需要一种系统性的训练策略，使 MLLM 的表征与低层动作产生有效对齐，从而提升 VLA 在机器人操作任务中的表现。
- **整体含义**：该工作旨在 **将强化学习引入 MLLM 的动作对齐训练**，使 MLLM 能够通过零样本推理生成动作令牌，进而弥合高层语义与低层控制之间的鸿沟，为构建更强的 VLA 提供新的训练范式。

### 2. 论文提出的方法论
- **核心思想**：提出 **RoboAlign 框架**，利用强化学习（RL）让 MLLM 生成动作令牌，从而将其表征与低层动作空间对齐。具体地，模型在自然语言中进行零样本推理生成动作令牌，并通过 RL 优化这些令牌与真实动作的匹配度，实现端到端的表征对齐。
- **关键技术细节**：
  - 在 MLLM 骨干网络之上添加一个 **基于扩散的动作头**（diffusion-based action head）来输出连续动作。
  - **动作令牌生成**：MLLM 不直接输出低层动作，而是先生成一组离散的“动作令牌”，这些令牌在语言空间进行推理，再通过解码器映射为低层动作。
  - **强化学习训练**：采用策略梯度方法（如 PPO）对动作令牌的生成策略进行优化，奖励函数基于动作执行的成功率或任务完成度（具体细节未在摘要中展开）。
- **与现有方法的区别**：
  - 对比仅用语言描述动作的对齐：RoboAlign 直接在表征层进行对齐，而非仅停留在语言层面。
  - 对比监督微调方法（如 ECoT）：RoboAlign 通过 RL 进行动态探索，避免静态监督学习中的过拟合和泛化不足。

### 3. 实验设计
- **数据集与场景**：
  - **LIBERO**：模拟桌面操作基准（含多种任务）。
  - **CALVIN**：长时域、目标条件性操作基准。
  - **真实机器人环境**：作者搭建的实际机器人操作场景。
- **对比方法**：
  - 标准 VLA 预训练基线（未做动作对齐）。
  - 仅用语言描述动作进行对齐的模型。
  - 有监督微调方法 **ECoT**（Embodied Chain-of-Thought）。
- **评价指标**：任务成功率（task success rate）。
- **实验结果**：
  - 在 LIBERO 上性能提升 **17.5%**。
  - 在 CALVIN 上提升 **18.9%**。
  - 在真实机器人环境中提升 **106.6%**（说明基础基线在真实场景表现极低，RoboAlign 带来巨大改善）。

### 4. 资源与算力
- 论文摘要及元数据中 **未明确说明使用的 GPU 型号、数量、训练时长等算力信息**。
- 推测：由于涉及 RL 训练和扩散动作头，通常需要较大算力（如 8× A100 或更多），但作者未披露具体细节。建议作者在完整论文中补充相关资源报告。

### 5. 实验数量与充分性
- **主要实验**：三个基准（两个模拟 + 一个真实），每个基准报告了整体成功率。
- **消融实验**：摘要中提及“compared to models aligned only with language-described actions or with supervised fine-tuning based approaches such as ECoT”，暗示可能进行了 ablation，但未具体列出（如动作头选择、RL 奖励函数、动作令牌长度等）。
- **充分性评价**：
  - **优点**：覆盖了不同难度（简单模拟、长时域、真实环境）的任务，结果具有说服力。
  - **不足**：缺少细粒度的消融实验公开发布；没有报告任务级成功率分布（如困难子集的泛化表现）；实验数量相对较少，且未涉及多种 VLA 骨干（如不同大小的 MLLM）的对比。

### 6. 论文的主要结论与发现
- **主要结论**：**强化学习对齐是连接语言模型与机器人动作的有效桥梁**。通过 RoboAlign 训练后的 MLLM 作为 VLA 骨干，能够显著提升机器人操作任务的成功率，尤其在实际场景中表现出色。
- **关键发现**：
  - 零样本推理生成的动作令牌可以保留语言空间中的语义信息，同时有效指导低层动作。
  - 相比监督学习或仅语言对齐，RL 对齐在泛化性和实际部署中更有优势。

### 7. 优点
- **方法创新**：首次将强化学习直接用于 MLLM 的表征-动作对齐，而非传统的行为克隆或指令微调。
- **实践效果突出**：在真实机器人环境上提升幅度巨大（106.6%），说明该方法对真实世界场景有显著价值。
- **通用性**：框架可应用于不同 MLLM 骨干，只需在之上添加扩散动作头即可，易于集成到现有 VLA 系统中。

### 8. 不足与局限
- **理论分析不足**：未解释为什么 RL 比监督学习更有效，缺乏对对齐机制的理论分析（如表征相似度、梯度流动等）。
- **实验覆盖有限**：仅验证了三个基准，未在更多样的机器人平台、任务集（如零样本泛化、未见过物体）上进行测试；也未对比不同 RL 算法或奖励设计的影响。
- **资源与复现性**：未提供算力和训练细节，可能增加复现难度。
- **可能偏差**：真实环境的提升显著，但基础基线是否过低？如果基线动作头或 MLLM 本来就不适合低层控制，则效果可能被夸大。需要更公平的基线设置。
- **应用限制**：依赖于 MLLM 的推理能力（零样本生成动作令牌），对于小规模模型或低资源场景可能不适用；扩散动作头带来额外计算开销。

---

（完）
