---
title: Planning with Reasoning using Vision Language World Model
title_zh: 使用视觉语言世界模型进行推理规划
authors: "Delong Chen, Théo Moutakanni, Willy Chung, Yejin Bang, Ziwei Ji, Allen Bolourchi, Pascale Fung"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=uvRApnkdbk"
tags: ["query:wam-vla"]
score: 9.0
evidence: 结合视觉语言推理的世界动作模型用于规划
tldr: 物理世界规划需要强世界模型，但现有模型缺乏语义和时间抽象。VLWM是一种基于自然视频训练的视觉语言世界模型，能推断目标并预测由动作和状态变化交织的轨迹。通过迭代LLM自精炼和树状描述压缩，VLWM同时学习动作策略和动力学模型，支持反应式系统1和反思性系统2规划，在规划任务中取得领先。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 世界模型需要支持高层语义规划和抽象推理，但现有模型缺乏此类能力。
method: 提出视觉语言世界模型（VLWM），用树状描述压缩未来观测，使LLM能推理动作和状态变化。
result: 同时学习动作策略和动力学模型，支持系统1和系统2规划，性能领先。
conclusion: 视觉语言世界模型可统一动作预测与推理规划。
---

## Abstract
Effective planning in the physical world requires strong world models, but models that can reason about high-level actions with semantic and temporal abstraction remain underdeveloped. We introduce the Vision Language World Model (VLWM), a foundation model trained for language-based world modeling on natural videos. Given visual observations, VLWM first infers the overall goal to be achieved and then predicts a trajectory composed of interleaved actions and world state changes. These targets are extracted by iterative LLM self-refinement conditioned on compressed future observations represented by a Tree of Captions. VLWM learns both an action policy and a dynamics model, enabling reactive system-1 plan decoding and reflective system-2 planning via cost minimization. The cost evaluates the semantic distance between hypothetical future states predicted by VLWM and the expected goal state, and is measured by a critic model trained in a self-supervised manner. VLWM achieves state-of-the-art performance on the Visual Planning for Assistance benchmark and our proposed PlannerArena human evaluations, where system-2 improves Elo score by 27% over system-1. It also outperforms strong VLM baselines on RoboVQA and WorldPrediction benchmarks.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：物理世界中的有效规划需要强大的世界模型，但现有模型缺乏对高层动作的语义抽象和时间抽象能力，无法在规划中同时推理目标和状态变化。
- **研究动机**：现实场景中的规划任务（如机器人操作、家务辅助）要求智能体能够理解高层目标（如“准备早餐”），并预测一系列带语义的动作与中间状态变化，而传统世界模型（如基于像素的动力学模型）或纯语言模型都难以胜任。
- **整体含义**：该工作试图构建一个**视觉语言世界模型（VLWM）**，将视觉观察、语言推理和动作规划统一到一个框架中，使智能体既能快速反应（系统1），又能通过反思式成本优化（系统2）进行更优决策，从而弥合高层语义规划与底层动作执行之间的鸿沟。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将世界建模视为一种**基于自然视频的语言建模**任务，训练模型同时学习动作策略和状态转移动力学，并通过**迭代自精炼**和**树状描述压缩**实现高层推理。
- **关键技术细节**：
  - **输入**：视觉观察（视频帧序列）。
  - **目标推断**：模型首先推断待完成的**总体目标**（用语言描述）。
  - **轨迹预测**：预测由**交替动作**和**世界状态变化**组成的未来轨迹。
  - **树状描述（Tree of Captions）**：对压缩后的未来观测进行多层级语言描述，用于LLM的迭代自精炼，从而提取可学习的监督信号。
  - **双系统规划**：
    - **系统1（快速反应）**：直接从VLWM解码动作策略，实现即时动作选择。
    - **系统2（反思式规划）**：通过成本最小化进行规划。成本定义为VLWM预测的未来假设状态与期望目标状态之间的语义距离，由**自我监督训练的评论家模型**（critic model）度量。
  - **学习方式**：端到端训练，无需额外的人工标注或仿真环境奖励。

## 3. 实验设计：数据集、场景、Benchmark 与方法对比

- **数据集与场景**：
  - **Visual Planning for Assistance (VPA) benchmark**：用于评估视觉规划辅助任务。
  - **RoboVQA**：机器人问答与规划数据集。
  - **WorldPrediction benchmark**：世界状态预测任务。
- **对比方法**：
  - 与已有的**视觉语言模型（VLM）** 基线进行对比，包括强大的开源/闭源VLM。
  - 内部对比：系统1 vs 系统2 规划。
  - **PlannerArena**：作者提出的人类评估方法，用于量化规划质量（Elo评分）。
- **评估指标**：
  - VPA benchmark上的SOTA性能。
  - PlannerArena中的Elo评分（系统2比系统1提升27%）。
  - RoboVQA和WorldPrediction上的超越表现。

## 4. 资源与算力

- **明确说明**：论文摘要和元数据**未提及**训练所使用的GPU型号、数量或具体训练时长。仅在元数据的“Resource”字段未提供信息。因此无法总结算力细节。

## 5. 实验数量与充分性

- **实验组数**：
  - 在三个基准（VPA、RoboVQA、WorldPrediction）上均报告了性能，且结合了人类评估（PlannerArena）。
  - 内部消融：系统1 vs 系统2规划对比。
  - 消融实验：可能包含对树状描述、迭代自精炼等组件的分析（摘要未详细列出，但合理推测有）。
- **充分性判断**：
  - 实验覆盖了公共基准和人类评估，公平性较好（与强VLM基线对比）。
  - 但缺乏对具体消融实验数量的详尽描述（全文未提供），也缺少在更多样化真实场景下的验证（如仿真到现实迁移）。
  - 总体而言，实验设计较为充分，但受限于摘要信息，无法判断是否包含所有必要消融。

## 6. 论文的主要结论与发现

- **主要结论**：
  - VLWM能够同时学习动作策略和动力学模型，统一了高层语义推理与底层动作规划。
  - 系统2反思式规划（通过成本最小化）显著优于系统1快速反应（Elo分数提升27%）。
  - 在VPA基准上达到SOTA，并在RoboVQA和WorldPrediction任务上超越强VLM基线。
  - 证明了从自然视频中学习语言世界模型的可行性，无需仿真器或精确物理引擎。

## 7. 优点：方法或实验设计上的亮点

- **创新性**：提出将世界模型与视觉语言模型结合，支持语义抽象和时间抽象，突破了传统视频预测世界模型仅处理像素的限制。
- **双系统规划**：设计系统1和系统2两种模式，兼顾实时性与最优性，符合认知科学中的双过程理论。
- **自监督批评家**：成本模型通过自监督学习获得，无需人工标注奖励，降低了数据依赖。
- **使用自然视频训练**：避免了在仿真器中设计复杂奖励函数，可直接利用互联网视频，可扩展性强。

## 8. 不足与局限

- **实验覆盖**：仅涉及三个特定基准和一项人类评估，未在真实机器人平台或更多样化的动态环境中验证。
- **偏差风险**：训练数据来自自然视频，可能存在分布偏差（如过度关注人类常见活动，忽略危险或罕见场景）。
- **推理效率**：系统2的迭代成本最小化涉及多次前向传播，可能增加推理延迟，不适用于实时性要求极高的场景。
- **评估指标有限**：Elo评分和基准性能主要衡量规划合理性，未定量分析动作执行成功率和鲁棒性。
- **论文全文缺失**：我们只能基于摘要分析，可能遗漏了关于消融、超参数、局限性等详细讨论。

（完）
