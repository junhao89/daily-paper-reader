---
title: Reinforcement Learning with Inverse Rewards for World Model Post-training
title_zh: 基于逆奖励的强化学习用于世界模型后训练
authors: "Yang Ye, Tianyu He, Shuo Yang, Jiang Bian"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=n5dkjiplv4"
tags: ["query:wam-vla"]
score: 8.0
evidence: 使用强化学习后训练世界模型以增强动作跟随能力，直接改进世界动作模型
tldr: 世界模型在模拟环境交互中作用重大，但其对人类指定动作的建模能力不足。现有RL后训练方法因偏好标注昂贵和规则视频验证器不可行而难以应用。本文提出逆奖励方法，利用RL对预训练世界模型进行后训练，在不需昂贵标注的情况下显著提升了动作跟随准确度。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 世界模型对动作建模能力不足，缺乏可扩展的改进方法。
method: 提出逆奖励方法，通过强化学习对预训练世界模型进行后训练以改进动作跟随。
result: 在不需大规模偏好标注的情况下，显著提升了世界模型的动作预测准确性。
conclusion: RL后训练是提升世界模型动作建模能力的有效途径。
---

## Abstract
World models simulate dynamic environments, enabling agents to interact with diverse input modalities. Although recent advances have improved the visual quality and temporal consistency of video world models, their ability of accurately modeling human-specified actions remains underexplored. Reinforcement learning presents a promising approach for directly improving the suboptimal action-following capability of pre-trained models, assuming that an appropriate reward function can be defined. However, transferring reinforcement learning post-training methods to world model is impractical due to the prohibitive cost of large-scale preference annotations and the infeasibility of constructing rule-based video verifiers. To address this gap, we propose **Reinforcement Learning with Inverse Rewards (RLIR)**, a post-training framework that derives verifiable reward signals by recovering input actions from generated videos using an Inverse Dynamics Model. By mapping high-dimensional video modality to a low-dimensional action space, RLIR provides an objective and verifiable reward for optimization via Group Relative Policy Optimization. Experiments across autoregressive and diffusion paradigms demonstrate 5–10% gains in action-following, up to 10% improvements in visual quality, and higher human preference scores, establishing RLIR as the first post-training method specifically designed to enhance action-following in video world models.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **背景**：视频世界模型能够模拟动态环境，但现有工作主要关注视觉质量和时间一致性，对于准确建模人类指定的动作（action-following）能力关注不足。
- **问题**：预训练世界模型在动作跟随方面存在次优表现，而强化学习（RL）后训练是直接改进该能力的潜在途径，但面临两大障碍：
  - 大规模偏好标注成本过高；
  - 无法构建基于规则的视频验证器（rule-based video verifiers）来提供奖励信号。
- **动机**：需要一种可扩展、无需昂贵标注的RL后训练方法，专门提升世界模型的动作跟随能力。

## 2. 方法论
- **核心思想**：提出**逆奖励强化学习（Reinforcement Learning with Inverse Rewards, RLIR）**框架，利用逆动力学模型（Inverse Dynamics Model, IDM）从生成的视频中反向恢复出输入动作，从而导出可验证的奖励信号。
- **关键技术细节**：
  - 将高维视频模态映射到低维动作空间，获得客观、可验证的奖励（即恢复的动作与真实动作的一致性）。
  - 采用**Group Relative Policy Optimization (GRPO)** 算法对预训练世界模型进行优化。
- **算法流程**（文字说明）：
  1. 使用预训练世界模型根据给定动作生成视频帧。
  2. 利用逆动力学模型（IDM）从生成视频中预测动作序列。
  3. 计算预测动作与真实动作之间的匹配度作为奖励信号。
  4. 通过GRPO对世界模型进行策略梯度更新，最大化该逆奖励。
- **创新点**：首次将逆动力学模型引入世界模型的RL后训练，实现无需偏好标注的可验证奖励。

## 3. 实验设计
- **数据集/场景**：摘要未明确列出具体数据集名称，但说明实验覆盖了**自回归（autoregressive）和扩散（diffusion）两种范式**的视频世界模型。
- **基准（benchmark）**：未明确说明基准方法，但对比了未经过RL后训练的原始模型（基线）。
- **对比方法**：未提及具体对比方法（可能包括其他RL后训练方法或无后训练版本），但通过消融实验验证了RLIR的有效性。
- **评估指标**：动作跟随准确率（action-following accuracy）、视觉质量（visual quality，提升达10%）、人类偏好评分（higher human preference scores）。

## 4. 资源与算力
- **文中未明确说明**：摘要和元数据中未提及GPU型号、数量、训练时长等算力信息。需要指出论文正文可能包含细节但提取文本未涵盖。

## 5. 实验数量与充分性
- **实验组数**：摘要提到在自回归和扩散两种范式上进行了实验，且报告了动作跟随5-10%的提升、视觉质量10%的提升以及人类偏好分数提升。推测至少包含主实验和消融实验（如验证逆奖励的有效性）。
- **充分性评估**：
  - **优点**：覆盖两种主流视频生成范式，使结论具有一定泛化性；使用了客观指标和人类偏好评价。
  - **不足**：缺少具体数据集和任务细节，无法判断场景多样性；未提供统计显著性分析或误差范围。实验整体较充分但信息有限。

## 6. 主要结论与发现
- **核心结论**：RLIR是首个专门为提升视频世界模型动作跟随能力设计的后训练方法，在不需大规模偏好标注的情况下，显著提升了动作预测准确性（5-10%），同时改善了视觉质量和人类偏好。
- **发现**：通过逆动力学模型将视频模态映射到动作空间，可以构造出高效的RL奖励，绕过规则验证器不可行的问题。

## 7. 优点（方法或实验设计亮点）
- **方法创新**：利用逆动力学模型自动产生可验证奖励，避免了昂贵的人工偏好标注，使RL后训练变得可行。
- **通用性**：实验涵盖自回归和扩散两种主流世界模型架构，表明方法具有跨范式的适用性。
- **效果显著**：在动作跟随和视觉质量上均取得一致提升，且人类偏好分数更高，实用性较强。
- **获奖/认可**：论文在ICLR 2026中获评分8.0（score: 8.0），说明评审认可度较高。

## 8. 不足与局限
- **实验覆盖**：未明确提及所使用数据集和具体任务（如机器人操控、自动驾驶等），场景多样性可能受限；也未与现有后训练方法（如基于对抗训练或知识蒸馏的方法）进行直接对比。
- **调用细节缺失**：逆动力学模型的训练成本、对域外动作的泛化能力、以及当生成视频质量极差时逆奖励的可靠性未讨论。
- **偏差风险**：逆奖励仅反映动作一致性，可能忽略物理合理性或其他隐式要求；未分析模型是否可能产生欺骗性视频（即视频看似符合动作但实际违反物理规律）。
- **应用限制**：依赖一个额外的逆动力学模型，该模型可能引入误差或需额外训练；对潜在的真实世界部署环境中的安全性和鲁棒性未评估。

（完）
