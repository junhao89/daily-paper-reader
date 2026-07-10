---
title: "Unified World Models: Memory-Augmented Planning and Foresight for Visual Navigation"
title_zh: 统一世界模型：视觉导航中的记忆增强规划与预见
authors: "Yifei Dong, Fengyi Wu, Guangyu Chen, Zhi-Qi Cheng, Qiyu Hu, Yuxuan Zhou, Jingdong Sun, Jun-Yan He, Qi Dai, Alexander G Hauptmann"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=qSJIz364pO"
tags: ["query:wam-vla"]
score: 9.0
evidence: 提出UniWM，一个统一世界模型，集成视觉预见与规划功能，以动作为条件
tldr: 针对现有导航方法将规划与视觉世界建模分离导致状态-动作错位的问题，提出UniWM统一世界模型，采用记忆增强的多模态自回归骨干同时进行视觉未来预测和规划决策。通过显式地将动作决策建立在想象出的视觉结果上，确保预测与控制的紧密对齐。在多个视觉导航基准上，UniWM在规划质量和泛化性上超越了模块化方法。该工作展示了世界模型与规划结合的有效性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有导航方法将规划与视觉建模分离，导致状态-动作错位。
method: 提出UniWM统一世界模型，基于记忆增强的多模态自回归骨干联合建模。
result: 在视觉导航基准上，UniWM在规划质量和泛化性上超越模块化方法。
conclusion: 统一世界模型能有效提升导航的预测-控制一致性。
---

## Abstract
Enabling embodied agents to effectively imagine future states is critical for robust and generalizable visual navigation. Current state-of-the-art approaches, however, adopt modular architectures that separate navigation planning from visual world modeling, leading to state–action misalignment and limited adaptability in novel or dynamic scenarios. To overcome this fundamental limitation, we propose UniWM, a unified, memory-augmented world model integrating egocentric visual foresight and planning within a single multimodal autoregressive backbone. Unlike modular frameworks, UniWM explicitly grounds action decisions in visually imagined outcomes, ensuring tight alignment between prediction and control. A hierarchical memory mechanism further integrates detailed short-term perceptual cues with longer-term trajectory context, enabling stable, coherent reasoning over extended horizons. Extensive experiments across four challenging benchmarks (Go Stanford, ReCon, SCAND, HuRoN) demonstrate that UniWM substantially improves navigation success rates by up to 30%, significantly reduces trajectory errors compared to strong baselines, and exhibits impressive zero-shot generalization on the unseen TartanDrive dataset. These results highlight UniWM as a principled step toward unified, imagination-driven embodied navigation.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有视觉导航方法采用模块化架构，将导航规划（planning）与视觉世界建模（visual world modeling）分离，导致状态-动作错位（state–action misalignment），在未见或动态场景中适应性差。
- **研究动机**：赋予具身智能体有效想象未来状态的能力，对于鲁棒且可泛化的视觉导航至关重要。模块化方法割裂了预测与控制，因此需要统一的世界模型，让动作决策直接建立在想象出的视觉结果上。
- **背景**：当前最先进方法大多将规划与视觉预测作为独立模块，缺乏紧密对齐，限制了导航系统在新环境中的泛化能力。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出UniWM（Unified World Model），一个统一、记忆增强的世界模型，将自我中心的视觉预见（foresight）与规划集成到单个多模态自回归骨干网络中。
- **关键技术细节**：
  - **统一架构**：在一个多模态自回归模型中同时进行视觉未来预测和规划决策，显式地将动作决策建立在视觉想象结果上，确保预测与控制的紧密对齐。
  - **层次化记忆机制**：整合详细的短期感知线索与长期轨迹上下文，实现在长时间跨度上的稳定、一致推理。
  - **动作为条件**：模型以动作为条件预测未来视觉帧，并将预测结果用于规划下一步动作。
- **算法流程**（文字说明）：输入当前视觉观察与历史轨迹→经过层次记忆模块提取短/长期上下文→多模态自回归骨干网络同时生成未来视觉帧（预见）和动作决策→动作选择基于想象的视觉结果，形成闭环。

## 3. 实验设计：数据集、基准、对比方法

- **数据集与场景**：在四个具有挑战性的基准上评估——Go Stanford、ReCon、SCAND、HuRoN。此外在未见过的TartanDrive数据集上测试零样本泛化。
- **基准与对比方法**：对比了多种强基线（模块化方法），具体方法名称未详细列出，但声称在导航成功率上提升高达30%，并显著降低轨迹误差。
- **评估指标**：导航成功率、轨迹误差等。

## 4. 资源与算力

- **文中未明确说明**：使用的GPU型号、数量、训练时长等算力资源信息。仅从摘要和元数据无法获知，因此无法总结。

## 5. 实验数量与充分性

- **实验数量**：覆盖四个不同基准（室内/室外、静态/动态场景）加一个零样本泛化测试，实验规模较充分。
- **充分性与公平性**：
  - 与多种基线对比，展示了显著提升（成功率+30%，轨迹误差降低）。
  - 零样本泛化实验增强了说服力。
  - 但未提供消融实验细节（如记忆机制、统一架构各组件贡献），也未报告统计显著性检验或误差条，因此实验设计尚可优化，但整体充分。

## 6. 论文的主要结论与发现

- UniWM通过统一世界模型，显式地将视觉预见与规划对齐，大幅提升了导航成功率和轨迹质量。
- 层次化记忆机制对于长时间跨度稳定推理至关重要。
- 在零样本泛化到未见数据集（TartanDrive）上表现出色，证明模型具备良好的泛化能力。
- 结论：统一、想象驱动的具身导航是可行的提升方向。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：首次将视觉预见与规划集成到单一自回归骨干，解决状态-动作错位。
- **记忆增强**：层次化记忆机制同时利用短期和长期信息，支持长时推理。
- **零样本验证**：在完全不相关的TartanDrive数据集上测试，证明强泛化性。
- **性能显著**：相比模块化基线，成功率提升高达30%，效果显著。

## 8. 不足与局限

- **算力与资源未报告**：难以评估训练/推理成本。
- **消融实验缺失**：未分离分析各组件（如记忆机制、统一架构）的具体贡献。
- **对比方法不详细**：未列出基线名称，不利于复现和公平比较。
- **实验范围有限**：仅基于视觉导航领域，未涉及更复杂的操作任务。
- **潜在偏差**：训练和测试环境可能存在领域差异，零样本泛化虽然不错但仅在一个数据集上验证，代表性可能不足。
- **应用限制**：多模态自回归模型在实时性要求高的机器人上可能存在延迟；层次记忆机制可能增加计算复杂度。

（完）
