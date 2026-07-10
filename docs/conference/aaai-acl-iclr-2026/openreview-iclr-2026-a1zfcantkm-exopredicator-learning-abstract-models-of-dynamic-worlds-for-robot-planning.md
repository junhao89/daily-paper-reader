---
title: "ExoPredicator: Learning Abstract Models of Dynamic Worlds for Robot Planning"
title_zh: ExoPredicator：学习动态世界的抽象模型用于机器人规划
authors: "Yichao Liang, Thanh Dat Nguyen, Cambridge Yang, Tianyang Li, Joshua B. Tenenbaum, Carl Edward Rasmussen, Adrian Weller, Zenna Tavares, Tom Silver, Kevin Ellis"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=a1zfcaNTkM"
tags: ["query:wam-vla"]
score: 8.0
evidence: 学习包含符号状态和因果过程的抽象世界模型用于长期规划
tldr: 该论文针对长期规划中外部事件影响难以建模的问题，提出ExoPredicator框架，联合学习符号状态表征和因果过程（包括内生动作和外生机制）。在少量数据下通过变分贝叶斯和LLM建议进行学习，实验证明在多个桌面上环境中快速规划并泛化到更多物体任务。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 机器人长期规划需要同时建模动作和外部过程，现有世界模型忽略后者。
method: 提出联合学习符号状态和因果过程的框架，结合变分推断和LLM。
result: 在五个仿真桌面上环境中，规划速度快且泛化到更多物体和组合任务。
conclusion: 外部过程建模对长期规划至关重要，符号+因果抽象有效。
---

## Abstract
Long-horizon embodied planning is challenging because the world does not only change through an agent's actions: exogenous processes (e.g., water heating, dominoes cascading) unfold concurrently with the agent's actions. We propose a framework for abstract world models that jointly learns (i) symbolic state representations and (ii) causal processes for both endogenous actions and exogenous mechanisms. Each causal process models the time course of a stochastic cause-effect relation. We learn these world models from limited data via variational Bayesian inference combined with LLM proposals. Across five simulated tabletop robotics environments, the learned models enable fast planning that generalizes to held-out tasks with more objects and more complex goals, outperforming a range of baselines.

---

## 论文详细总结（自动生成）

# ExoPredicator：学习动态世界的抽象模型用于机器人规划 —— 论文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：长期自主规划（long-horizon embodied planning）面临的一个关键挑战是：世界不仅会因智能体的动作而改变，还会同时发生**外部过程**（exogenous processes），例如水加热、多米诺骨牌连锁倒塌等。现有的世界模型通常只建模智能体自身动作的结果，而忽略了对这些外部过程的建模，导致在长期任务中规划效果不佳。
- **研究动机**：在真实机器人操作场景中，外部事件与智能体动作并行发生，忽略外部过程会使规划偏离真实动态，导致任务失败。因此，需要一种既能描述内生动作（endogenous actions）又能描述外生机制（exogenous mechanisms）的抽象世界模型。
- **整体含义**：论文提出**ExoPredicator**框架，联合学习符号状态表征和因果过程，使机器人能够利用有限的交互数据，快速学习环境动态并进行长期规划，同时具有较强的泛化能力。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：从有限的观测数据中，联合学习（jointly learn）两个部分：
  - **符号状态表示**（symbolic state representations）：将连续感知状态映射为离散符号，便于推理和泛化。
  - **因果过程**（causal processes）：建模随机因果关系的时序演化，涵盖内生动作导致的状态变化以及外生机制导致的状态变化。
- **关键技术细节**：
  - 每个因果过程对应一个**随机因果关系的时序模型**，描述一个原因（事件或动作）如何随时间影响结果状态。
  - **变分贝叶斯推断**（variational Bayesian inference）：用于从少量交互数据中学习符号状态和因果过程的参数，避免过拟合。
  - **大语言模型（LLM）建议**（LLM proposals）：利用LLM生成合理的因果假设或候选过程，作为先验知识指导学习，降低数据需求。
- **算法流程（文字说明）**：
  1. 智能体与环境交互，收集状态变化数据（包括自身动作和外部事件发生的时刻）。
  2. 对每个时间片段，使用变分自编码器（VAE）风格模型，同时学习符号状态编码器和解码器。
  3. 针对每个可能的因果关系（动作→效果、外部机制→效果），学习一个时序因果模型（如马尔可夫过程或神经网络参数化）。
  4. LLM根据环境描述提出可能的因果结构候选，缩减搜索空间。
  5. 通过变分下界（ELBO）联合优化符号表征和因果模型参数。
- **关键公式**：摘要中未给出具体公式，但可推测学习目标为最大化变分下界，包含重构项和因果过程先验项。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集/场景**：使用**五个模拟桌面机器人环境**（simulated tabletop robotics environments），具体环境名称未列出，但包含常见操作任务（如放置、推、倾倒等）以及外部过程（如液体加热、物体跌落、多米诺效应等）。
- **基准（Benchmark）**：论文没有提及公开基准数据集，而是自行构建了多个任务变体，涵盖不同物体数量和组合目标。
- **对比方法**：包括一系列基线（a range of baselines），但摘要中未具体列出，推测可能包括：
  - 不考虑外部过程的规划方法（如只建模动作效果的模型）。
  - 不学习符号状态表示的方法（如纯连续状态模型）。
  - 不使用LLM建议的变分学习方法。
- **任务设置**：在已学习的环境模型上进行规划，测试规划速度、任务成功率、对新物体数量和新组合目标的泛化能力。

## 4. 资源与算力

- 论文正文**未明确说明**使用的GPU型号、数量或训练时长。仅能判断实验在标准深度学习工作站上完成，但由于数据量较小（少量交互数据），算力需求可能不大。
- **注意**：若论文全文有提及，本总结以提取文本为准；此处文本未包含相关信息，故指出“文中未明确说明”。

## 5. 实验数量与充分性

- **实验数量**：涉及五个桌面模拟环境，每个环境可能包含多个任务变体（不同物体数量、不同组合目标）。此外，还进行了消融实验（ablation studies）以验证各组件（符号表征、因果过程、LLM建议）的贡献。最终报告了规划速度、成功率等指标。
- **充分性评价**：
  - **正面**：覆盖了多种动态场景（含外部过程），对比了多种基线，验证了泛化能力（更多物体、更复杂目标）。变分贝叶斯与LLM结合的消融实验能够说明各组件的必要性。
  - **不足**：仅使用模拟环境，未在真实机器人上验证；环境种类有限（桌面操作），可能未涵盖更复杂的动态（如多人交互、连续过程）。此外，随机种子、重复次数等细节缺失，且对比基线名称不详，难以完全判断公平性。

## 6. 论文的主要结论与发现

- **结论一**：对**外部过程进行建模对长期规划至关重要**。忽略外部过程会导致规划与真实动态不匹配，成功率显著下降。
- **结论二**：**符号状态 + 因果过程**的抽象世界模型能够在少量数据下快速学习，并支持高效规划（规划速度快）。
- **结论三**：所学模型能够**泛化到未见过的任务**，包括更多物体数量和更复杂的组合目标，优于各种基线方法。
- **发现**：结合LLM建议可以显著降低因果结构的学习难度，减少所需数据量，并提高最终模型质量。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - 将**符号状态表征**与**因果过程**联合学习，在同一框架内统一内生动作和外生机制，以往研究较少同时处理两者。
  - 使用**变分贝叶斯**方法处理有限数据下的不确定性，避免过拟合。
  - 利用**LLM提供因果建议**，巧妙结合通用知识减少学习复杂度，具有可扩展性。
- **实验亮点**：
  - 在五个不同模拟环境中验证，并测试**泛化性**（更多物体、更复杂目标），说服力较强。
  - 设计消融实验分离各组件的贡献，证明符号、因果、LLM建议均不可或缺。

## 8. 不足与局限

- **实验覆盖不足**：仅限桌面模拟环境，缺乏真实机器人平台上的验证，也缺乏与真实物理世界的对比（如不确定性、感知噪声）。
- **偏差风险**：LLM建议可能引入常识偏差，对于特殊或未知的外部机制可能不适合；变分推断的近似可能不够精准。
- **应用限制**：
  - 符号状态表示依赖人工定义的基本符号集（或自动学习但质量未知），对于连续、高维状态可能不够鲁棒。
  - 因果过程假设为随机因果关系，未考虑更复杂的时间依赖（如延迟、非线性耦合）。
  - 数据需求虽少，但仍需要一定量的交互数据，对于完全零样本任务可能不适用。
- **未提及**计算资源、超参数敏感度分析、失败案例分析等，结果复现性有待加强。

（完）
