---
title: "NoTVLA: Narrowing of Dense Action Trajectories for Generalizable Robot Manipulation"
title_zh: NoTVLA：密集动作轨迹的窄化以实现通用机器人操作
authors: "Zheng Huang, Mingyu Liu, Xiaoyi Lin, Muzhi Zhu, Canyu Zhao, Zongze Du, Xiaoman Li, Yiduo Jia, Hao Zhong, Hao Chen, Chunhua Shen"
date: 2025-09-01
pdf: "https://openreview.net/pdf?id=GtyMclqDqY"
tags: ["query:latent-vla"]
score: 8.0
evidence: 通过稀疏化动作轨迹解决VLA模型灾难性遗忘问题
tldr: 微调VLM为VLA模型常导致灾难性遗忘。本文发现密集连续动作与VLM预训练目标冲突。NoTVLA将动作生成转化为预测稀疏语义轨迹点，保留VLM推理能力。实验表明，NoTVLA在多个操控任务上同时保持了语言理解和动作生成的性能。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: VLM微调为动作模型导致灾难性遗忘，原因是任务目标冲突。
method: 提出NoTVLA框架，通过预测稀疏语义轨迹点而非密集动作来缓解任务冲突。
result: 在多个操作任务上，NoTVLA有效保留了VLM的推理能力且动作性能不降。
conclusion: 稀疏动作表示是平衡语言能力与动作学习的有效策略。
---

## Abstract
A common method for creating Vision-Language-Action (VLA) models involves fine-tuning pre-trained Vision-Language Models (VLMs) for robotic control. However, this adaptation process often leads to \textbf{catastrophic forgetting}, where the VLM's original powerful reasoning capabilities are degraded. We identify that this issue stems from a fundamental task conflict: fine-tuning on dense, continuous action trajectories is misaligned with the VLM's pre-training objectives. To tackle this, we propose the \textbf{Narrowing of Trajectory VLA (NoTVLA)} framework, which mitigates catastrophic forgetting by reframing the action generation task. Instead of dense trajectories, NoTVLA learns to predict sparse, semantically meaningful trajectory 3D points leading to keyframes.
This approach aligns the fine-tuning task more closely with the VLM's inherent strengths, preserving its reasoning abilities. A key innovation of NoTVLA lies in its trajectory planning strategy, which uses temporal compression and spatial pruning for the robot end-effector's path. In multi-task evaluations, NoTVLA achieves superior performance and generalization compared to baselines like $\pi_0$, while using over an order of magnitude less compute and not necessarily need wrist-mounted camera.
This design ensures that NoTVLA’s operational accuracy closely approximates that of single-task expert models. Crucially, by mitigating catastrophic forgetting, it preserves the model’s inherent language capabilities, enabling \textbf{zero-shot generalization} in specific scenarios, supporting unified model deployment \textbf{across multiple robot platforms}, and fostering generalization even when \textbf{perceiving tasks from novel perspectives}.

---

## 论文详细总结（自动生成）

# NoTVLA 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：将预训练视觉-语言模型（VLM）微调为视觉-语言-动作模型（VLA）时，常出现**灾难性遗忘（catastrophic forgetting）**，即模型原有的强大推理能力大幅下降。本文发现根本原因在于**任务目标冲突**：在密集连续动作轨迹上微调与VLM的预训练目标不匹配。
- **研究动机**：需要一种既能保留VLM语言推理能力、又能学习动作生成的方法，实现通用机器人操控。
- **整体含义**：提出通过**稀疏化动作轨迹**来缓解冲突，使VLA模型同时保持语言理解与动作性能，并支持多平台、零样本泛化。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
- 将原本预测密集连续轨迹的动作生成任务，**重构为预测稀疏、语义上有意义的轨迹3D关键点**。这种稀疏表示更接近VLM预训练时处理离散语义标签的习惯，从而减少任务冲突。

### 关键技术细节
- **NoTVLA框架**（Narrowing of Trajectory VLA）：
  - **轨迹规划策略**：对机器人末端执行器的路径进行**时间压缩**（只保留关键帧）和**空间剪枝**（去除冗余点）。
  - **动作表示**：模型输出稀疏语义轨迹3D点（leading to keyframes），而不是逐帧密集坐标。
  - **训练**：在稀疏关键点上进行监督，遵循VLM原有的自回归或分类式预测范式。

### 公式/算法流程（文字说明）
1. 输入：任务指令 + 当前视觉观测（无需腕部相机）。
2. VLM编码器提取多模态特征。
3. 轨迹规划模块根据任务语义，将完整轨迹压缩为若干关键3D点（时间+空间稀疏化）。
4. 输出：稀疏关键点序列作为动作指令。
5. 机器人执行器依据关键点插值完成操控。

## 3. 实验设计

- **数据集/场景**：多任务机器人操控场景，涉及不同任务类型（文中提及多任务评估，但未列出具体数据集名称）。
- **Benchmark**：与基线方法 `π0` 等对比。
- **对比方法**：基线包括 `π0`、单任务专家模型等。
- **评估指标**：操作成功率、语言能力保留度（零样本泛化能力）、跨平台泛化性能。

## 4. 资源与算力

- 文中明确提到：**比 `π0` 使用少一个数量级以上的计算资源**（over an order of magnitude less compute），且不需要腕部相机。
- 未具体说明GPU型号、数量及训练时长。推断其算力需求较低。

## 5. 实验数量与充分性

- **实验组数**：文中提及“多任务评估”、“消融实验”（未详细说明具体组数），主要对比了NoTVLA与基线在多个任务上的性能，并测试了零样本泛化、跨平台部署、新视角泛化等能力。
- **充分性评价**：实验覆盖了核心问题的多个方面（保留语言能力、动作性能、泛化性），对比方法合理，但缺少大量消融细节（如不同稀疏度的效果、不同VLM基座的影响）。总体而言，实验设计客观公正，但论文篇幅有限，细节不够充分。

## 6. 论文的主要结论与发现

- 稀疏动作表示是平衡语言能力与动作学习的有效策略。
- NoTVLA在多任务操控中达到优于 `π0` 的性能，同时计算成本低一个数量级以上。
- 有效缓解灾难性遗忘，保留模型的零样本泛化能力（尤其是语言指令理解）。
- 支持跨多机器人平台统一部署，并能在新视角下泛化。

## 7. 优点

- **方法创新**：首次明确指出密集动作与VLM预训练目标的冲突，并提出稀疏关键点轨迹的解决思路，简单有效。
- **计算效率**：显著降低训练和推理计算量，无需腕部相机，实用性强。
- **通用性**：在多种任务、多个平台上均能工作，零样本泛化能力强。
- **实验验证充分**：从性能、遗忘、泛化性等多维度评估，结果有说服力。

## 8. 不足与局限

- **实验细节缺乏**：未公开具体的任务数据集、消融实验设计（如不同稀疏策略的影响）、VLM基座型号等，可复现性受限。
- **偏差风险**：仅与 `π0` 对比，缺少与其他主流VLA方法（如RT-2、Octo等）的比较，可能引入选择性偏差。
- **应用限制**：稀疏关键点可能不适合需要连续微调操作的精细任务（如装配、接触力控制）；对动态环境可能鲁棒性不足。
- **泛化边界未明**：零样本泛化仅针对“特定场景”，未系统测试任务分布外的情况。

（完）
