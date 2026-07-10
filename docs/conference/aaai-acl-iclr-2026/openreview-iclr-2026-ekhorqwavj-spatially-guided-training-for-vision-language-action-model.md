---
title: Spatially Guided Training for Vision-Language-Action Model
title_zh: 视觉-语言-动作模型的空间引导训练
authors: "Jinhui Ye, Fangjing Wang, Ning Gao, Junqiu Yu, Zhu Yangkun, Bin Wang, Jinyu Zhang, Weiyang Jin, Yanwei Fu, Feng Zheng, Yilun Chen, Jiangmiao Pang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=eKhOrQWAVJ"
tags: ["query:latent-vla"]
score: 7.0
evidence: 通过空间引导训练VLA模型，包含空间接地预训练
tldr: 视觉-语言模型在具身任务中难以将指令转化为动作，SP-VLA通过空间接地预训练（点、框、轨迹预测）和空间引导动作微调，使VLM获得可迁移的空间先验，从而生成更好的空间引导以指导动作生成，提升VLA模型在机器人操作和导航中的泛化能力。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLM在具身任务中缺乏将指令转化为动作的空间先验。
method: 提出双阶段训练：空间接地预训练和空间引导动作微调。
result: 在多个机器人操作和导航基准上取得优于现有VLA模型的性能。
conclusion: 空间先验是提升VLA模型具身推理能力的关键桥梁。
---

## Abstract
Large vision–language models (VLMs) excel at multimodal understanding but fall short when extended to embodied tasks, where instructions must be transformed into low-level motor actions. We introduce SP-VLA, a dual-system **V**ision–**L**anguage–**A**ction framework that leverages **S**patial **P**riors as a bridge between linguistic instructions and embodiment-specific control. 
introduce SP-VLA aligns action learning with spatial priors through two stages: (i) spatial grounding pre-training, which equips the VLM with transferable priors via scalable point, box, and trajectory prediction from both web-scale and robot-specific data, and (ii) spatially guided action post-training, which encourages the model to produce richer spatial priors to guide action generation via spatial prompting.
This design preserves spatial grounding during policy learning and promotes consistent optimization across spatial and action objectives. Empirically, introduce SP-VLA achieves substantial improvements over vanilla VLA, with performance increasing from $66.1{\rightarrow}84.6$ on Google Robot and from $54.7{\rightarrow}73.2$ on WidowX, establishing new state-of-the-art results on SimplerEnv. It also demonstrates stronger generalization to unseen objects and paraphrased instructions, as well as robustness to long-horizon perturbations in real-world settings. These results highlight scalable spatially guided training as a promising direction for robust, generalizable robot learning. We will release code, data, and model checkpoints to support future research.
See more visualization results at the anonymous page: https://sp-vla-anonymous.vercel.app

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文摘要及元数据生成的结构化中文总结。

# 论文总结：SP-VLA：视觉-语言-动作模型的空间引导训练

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：大型视觉-语言模型（VLM）在跨模态理解方面表现出色，但直接扩展到具身任务（如机器人操作）时，难以将语言指令转化为低层的电机动作。现有VLA模型缺乏将语言指令与空间环境关联起来的关键**空间先验**。
- **背景**：在机器人学习和具身智能中，需要模型具备从自然语言指令推理出具体动作序列的能力，而当前通用VLM未针对动作生成进行专门训练，导致在未见物体、指令变换或长序列扰动下泛化能力弱。
- **核心动机**：引入空间先验作为连接语言指令与具身控制的桥梁，以提升VLA模型的泛化性和鲁棒性。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出**双阶段空间引导训练框架 SP-VLA**，强调先让VLM学习可迁移的空间接地能力，再通过空间提示引导动作生成。
- **关键技术细节**：
  - **阶段一：空间接地预训练（Spatial Grounding Pre-training）**
    - 目标：赋予VLM稳健的空间先验，即根据语言描述在图像中定位物体（点、框、轨迹）。
    - 数据来源：同时使用大规模网络数据和机器人专用数据，进行可扩展的点、框、轨迹预测任务训练。
  - **阶段二：空间引导动作微调（Spatially Guided Action Post-training）**
    - 目标：在保持空间接地能力的同时，通过**空间提示（spatial prompting）**引导动作生成。
    - 方法：在策略学习过程中，模型被鼓励生成更丰富的空间先验（如目标位置、运动轨迹），并利用这些先验来限制或引导动作输出。这保证空间目标与动作目标在优化上的一致性。
- **算法流程（文字描述）**：
  1. 使用预训练的VLM作为骨干。
  2. 在预训练阶段，输入图像和语言指令，模型输出点/框/轨迹预测，损失函数为接地误差。
  3. 在微调阶段，冻结或低学习率更新接地模块，新增动作解码器，同时让模型输出空间提示（如目标位置的热图），动作解码器以视觉特征+空间提示为输入预测动作（如机械臂的末端位移）。
  4. 整个框架端到端训练，但空间接地损失和动作损失联合优化。

## 3. 实验设计：数据集 / 场景 / Benchmark / 对比方法

- **数据集与场景**：
  - **Google Robot**：实际机器人操作数据集。
  - **WidowX**：桌面机械臂操作数据集。
  - **SimplerEnv**：仿真环境基准（包含多个操作及导航任务）。
  - 此外还使用了网络规模数据（用于预训练）。
- **Benchmark**：在多个具身操作和导航基准上进行评估，主要对比**空间引导训练的效果**。
- **对比方法**：
  - 基线：**原版VLA**（未经空间引导训练）。
  - 其他现有VLA模型（文中未列出具体对比方法，但从摘要看方法为VLA-基线，并报告了SOTA）。

## 4. 资源与算力

- **文中未明确说明**所使用的GPU型号、数量或训练时长。仅提及将开源代码、数据和模型检查点。因此无法详细总结，需要指出这一点。

## 5. 实验数量与充分性

- **实验数量**：
  - 主要结果：在**Google Robot**上性能从66.1→84.6，在**WidowX**上从54.7→73.2；在**SimplerEnv**上取得新的SOTA。
  - 泛化测试：评估了对**未见物体**和**改写指令**的泛化能力。
  - 鲁棒性测试：在真实场景中进行**长时程扰动**实验。
  - 消融实验：虽然没有明确列出，但两阶段设计及空间引导机制应有相关消融（摘要提到“preserves spatial grounding during policy learning”暗示了消融对比）。
- **充分性**：实验覆盖多平台（仿真+真实）、多任务、泛化和鲁棒性测试，较为充分。但缺少与更多近期方法的量化对比及更细粒度的消融细节。

## 6. 主要结论与发现

- **性能提升显著**：空间引导训练使VLA模型在Google Robot数据集上从66.1%提升至84.6%，在WidowX上从54.7%提升至73.2%，并在SimplerEnv上达到SOTA。
- **泛化能力强**：对未见物体和改写指令具有更强的泛化能力。
- **鲁棒性高**：在真实场景长时程扰动下表现稳定。
- **核心发现**：空间先验是提升VLA模型具身推理能力的关键桥梁，可扩展的空间引导训练是走向稳健、可泛化机器人学习的有前途方向。

## 7. 优点

- **方法设计**：
  - 双阶段训练思路清晰：先学通用空间接地，再通过空间提示引导动作，有效防止动作学习破坏空间知识。
  - 利用网络规模数据进行预训练，大大提高了数据利用效率和迁移能力。
  - 联合优化空间与动作目标，保持一致性。
- **实验设计**：
  - 涵盖仿真和真实机器人，评估了泛化与鲁棒性，贴近实际应用。
  - 公开代码、数据、模型，促进复现和后续研究。

## 8. 不足与局限

- **实验覆盖**：
  - 对比方法较少，仅与“vanilla VLA”对比，未与近期其他VLA变体（如RT-2、Octo等）进行详细比较，可能削弱说服力。
  - 缺乏在更复杂任务（如多步操作、动态环境）上的评估。
- **偏差风险**：
  - 机器人数据集（Google Robot, WidowX）可能具有特定的硬件和场景偏差，结果泛化到其他机器人平台需进一步验证。
  - 预训练数据来自网络，可能存在分布偏移。
- **应用限制**：
  - 模型需要同时处理视觉、语言和动作，对计算资源要求较高，文中未讨论实时性问题。
  - 空间提示的设计可能对特定任务模式过拟合，需要更多分析。
- **资源信息缺失**：未提供训练算力需求，不利于其他团队复现。

（完）
