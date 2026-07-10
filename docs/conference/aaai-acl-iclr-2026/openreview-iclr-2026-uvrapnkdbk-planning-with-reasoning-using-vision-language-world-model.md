---
title: Planning with Reasoning using Vision Language World Model
title_zh: 使用视觉语言世界模型进行推理规划
authors: "Delong Chen, Théo Moutakanni, Willy Chung, Yejin Bang, Ziwei Ji, Allen Bolourchi, Pascale Fung"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=uvRApnkdbk"
tags: ["query:wam-vla"]
score: 8.0
evidence: 视觉语言世界模型，联合学习动作策略和动态模型用于规划
tldr: VLWM（视觉语言世界模型）在自然视频上训练，给定观测后先推断目标再预测由动作和状态变化交织的轨迹。它同时学习动作策略和动态模型，支持反应式系统-1计划和反思式系统-2规划，为具身智能提供高效的世界理解与规划能力。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有世界模型缺乏高层动作的语义和时间抽象推理能力。
method: 训练一个联合视觉语言世界模型，从视频中学习预测目标、动作和状态变化的轨迹。
result: 在规划任务上相比基线方法取得更好性能，并支持双系统规划。
conclusion: 视觉语言世界模型能有效桥接世界模型与语言条件策略。
---

## Abstract
Effective planning in the physical world requires strong world models, but models that can reason about high-level actions with semantic and temporal abstraction remain underdeveloped. We introduce the Vision Language World Model (VLWM), a foundation model trained for language-based world modeling on natural videos. Given visual observations, VLWM first infers the overall goal to be achieved and then predicts a trajectory composed of interleaved actions and world state changes. These targets are extracted by iterative LLM self-refinement conditioned on compressed future observations represented by a Tree of Captions. VLWM learns both an action policy and a dynamics model, enabling reactive system-1 plan decoding and reflective system-2 planning via cost minimization. The cost evaluates the semantic distance between hypothetical future states predicted by VLWM and the expected goal state, and is measured by a critic model trained in a self-supervised manner. VLWM achieves state-of-the-art performance on the Visual Planning for Assistance benchmark and our proposed PlannerArena human evaluations, where system-2 improves Elo score by 27% over system-1. It also outperforms strong VLM baselines on RoboVQA and WorldPrediction benchmarks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
现有世界模型在处理高层级动作时，缺乏语义抽象和时间抽象推理能力。物理世界中的有效规划需要强大的世界模型，但当前模型难以从自然视频中学习具有高层语义和时序抽象的动作规划。本文提出视觉语言世界模型（VLWM），旨在桥接世界模型与语言条件策略，使具身智能体能够理解世界并基于推理进行规划。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：训练一个联合视觉语言世界模型，从自然视频中同时学习动作策略和动态模型，支持双系统规划（反应式System-1和反思式System-2）。
- **关键技术细节**：
  - 给定视觉观测，VLWM首先推断整体目标（goal），然后预测由动作和世界状态变化交织的轨迹。
  - 目标提取采用迭代的LLM自我精炼（self-refinement），基于压缩的未来观测表示——标题树（Tree of Captions）。
  - 模型同时学习动作策略（直接输出动作）和动态模型（预测未来状态）。
  - System-1：反应式计划解码，直接输出动作序列。
  - System-2：反思式规划，通过成本最小化（cost minimization）进行搜索。成本由评论模型（critic model）以自监督方式训练得到，衡量VLWM预测的假设未来状态与期望目标状态之间的语义距离。

## 3. 实验设计：数据集、基准与对比方法
- **数据集与基准**：
  - **Visual Planning for Assistance (VPA)** 基准（SOTA表现）。
  - **PlannerArena** 人类评估（本文提出的新基准）。
  - **RoboVQA** 和 **WorldPrediction** 基准（在VLM基线上取得优势）。
- **对比方法**：与强VLM基线进行比较，在RoboVQA和WorldPrediction上表现更优。
- **评估指标**：包括Elo分数（人类评估）、规划成功率等。

## 4. 资源与算力
论文中未明确说明使用的GPU型号、数量或训练时长等算力资源信息。因此无法总结具体训练配置。

## 5. 实验数量与充分性
- 在四个基准（VPA、PlannerArena、RoboVQA、WorldPrediction）上进行了评估，覆盖了辅助规划、人类偏好、机器人问答和世界预测等任务。
- 包含System-1与System-2的对比消融（System-2提升Elo分数27%），验证了双系统规划的有效性。
- 未提及对模型组件（如标题树、评论模型等）的详细消融实验，但整体实验设计较为充分，结果客观。

## 6. 论文的主要结论与发现
- VLWM在Visual Planning for Assistance基准上达到SOTA。
- 在PlannerArena人类评估中，System-2反思式规划相比System-1反应式规划，Elo分数提升27%。
- 在RoboVQA和WorldPrediction上超越强VLM基线。
- 视觉语言世界模型能有效桥接世界模型与语言条件策略，为具身智能提供高效的世界理解与规划能力。

## 7. 优点：方法或实验设计上的亮点
- **双系统规划**：结合快速反应（System-1）和慢速反思（System-2），灵活适应不同精度需求。
- **自监督评论模型**：无需人工标注，自动学习评估未来状态与目标之间的语义距离。
- **标题树表示**：通过迭代LLM精炼压缩未来观测，提高轨迹预测的语义抽象能力。
- **统一框架**：同时在自然视频上学习动作策略和动态模型，无需专门的动作标注。

## 8. 不足与局限
- **算力与训练细节缺失**：未报告训练成本，难以评估方法在实际部署中的可复现性。
- **实验覆盖**：仅评估了机器人辅助规划、VQA和世界预测，未涵盖真实机器人控制、复杂多步任务或动态环境。
- **偏差风险**：依赖LLM自我精炼和标题树，可能引入语言模型的固有偏见；评论模型的自监督训练可能对分布外场景泛化不足。
- **应用限制**：目前针对自然视频训练，对仿真或真实机器人环境的直接迁移未验证。

（完）
