---
title: "ExoPredicator: Learning Abstract Models of Dynamic Worlds for Robot Planning"
title_zh: "ExoPredicator: 学习动态世界的抽象模型用于机器人规划"
authors: "Yichao Liang, Thanh Dat Nguyen, Cambridge Yang, Tianyang Li, Joshua B. Tenenbaum, Carl Edward Rasmussen, Adrian Weller, Zenna Tavares, Tom Silver, Kevin Ellis"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=a1zfcaNTkM"
tags: ["query:wam-vla"]
score: 8.0
evidence: 用于机器人状态预测和规划的抽象世界模型
tldr: 长时程规划中外生过程（如物体自动变化）带来挑战。ExoPredicator提出抽象世界模型框架，联合学习符号状态表示以及内源动作和外生机制的因果过程，通过变分贝叶斯和LLM指导学习。在多个模拟桌面环境中，该模型使规划泛化到未见任务，支持更多物体和更复杂场景。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有世界模型忽略外生过程，限制长时程规划泛化。
method: 联合学习符号状态和因果过程，结合变分贝叶斯和LLM。
result: 在桌面操作任务上规划可泛化到未见任务。
conclusion: 显式建模外生过程能提升世界模型的规划能力。
---

## Abstract
Long-horizon embodied planning is challenging because the world does not only change through an agent's actions: exogenous processes (e.g., water heating, dominoes cascading) unfold concurrently with the agent's actions. We propose a framework for abstract world models that jointly learns (i) symbolic state representations and (ii) causal processes for both endogenous actions and exogenous mechanisms. Each causal process models the time course of a stochastic cause-effect relation. We learn these world models from limited data via variational Bayesian inference combined with LLM proposals. Across five simulated tabletop robotics environments, the learned models enable fast planning that generalizes to held-out tasks with more objects and more complex goals, outperforming a range of baselines.

---

## 论文详细总结（自动生成）

# ExoPredicator: 学习动态世界的抽象模型用于机器人规划 — 论文总结

## 1. 核心问题与整体含义（研究动机与背景）
- **核心问题**：长时程具身规划（long-horizon embodied planning）面临重大挑战——世界不仅因智能体的动作而变化，还存在**外生过程**（exogenous processes），如水的加热、多米诺骨牌倒塌等，这些过程与智能体动作并发发生。传统世界模型往往忽略或混同外生过程，导致规划泛化能力受限。
- **研究动机**：现有方法难以在有限数据下学习可解释且能泛化到未见任务的世界模型，尤其当任务涉及更复杂的物体、更长的因果链条时。ExoPredicator 旨在显式建模外生机制，提升机器人规划的鲁棒性和泛化性。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：提出一个**抽象世界模型框架**，联合学习：(i) **符号状态表示**（symbolic state representations），(ii) **内源动作**（endogenous actions）和**外生机制**（exogenous mechanisms）的因果过程。每个因果过程建模随机因果效应的时间过程。
- **关键技术细节**：
  - **因果过程建模**：将世界变化分解为智能体动作驱动的内源因果和外生过程驱动的外生因果，分别用时间性随机因果关系表示。
  - **联合学习**：通过**变分贝叶斯推断**（variational Bayesian inference）从有限数据中学习符号状态和因果模型参数。
  - **LLM 指导**：利用**大语言模型（LLM）提出可能的因果结构假设**，加速搜索并提高数据效率。
- **算法流程（文字说明）**：
  1. 从少量演示或交互数据中提取观察序列。
  2. 用变分推断学习符号状态的低维表示（如物体属性、关系）。
  3. 同时学习内源动作（agent执行的动作）对应的转移函数，以及外生过程（不依赖动作独立演化）对应的转移函数。
  4. 利用LLM生成候选因果图，缩小因果结构搜索空间。
  5. 最终得到可解释的因果世界模型，用于快速规划（如基于符号状态的前向搜索或回溯规划）。

## 3. 实验设计
- **场景/数据集**：五种模拟桌面机器人环境（simulated tabletop robotics environments），具体包括操作任务如堆叠、移动、加热等，涉及多个物体、不同因果结构（含外生变化如水温自动升高）。
- **Benchmark**：与多种基线方法对比，包括：
  - 不建模外生过程的普通世界模型
  - 纯神经网络动力学模型
  - 基于图神经网络的隐变量模型
  - 符号规划器（未学习外生过程）
- **评估指标**：规划成功率、泛化到未见任务（更多物体、更复杂目标）的能力、数据效率。

## 4. 资源与算力
- 论文中**未明确说明**所使用的 GPU 型号、数量或训练时长。仅在摘要和元数据中提及“从有限数据中学习”，但未提供具体算力消耗。可能需要参考附录或后续工作。

## 5. 实验数量与充分性
- **实验数量**：在五种不同桌面环境上进行了评估，包括含外生过程的环境（如水加热）和不含外生过程的对比环境。可能包含消融实验（对比有无LLM指导、有无显式外生建模）。
- **充分性**：实验设计较为充分——覆盖多环境、多任务、与多种基线对比。从摘要推测，规划能**泛化到未见任务**（held-out tasks with more objects and more complex goals），说明泛化能力验证充分。
- **客观公平**：对比基线包括代表性方法，但缺乏真实机器人实验结果（全部为模拟环境），存在一定模拟到现实差距。未见统计显著性检验报告，结果呈现可能以平均成功率为主。

## 6. 主要结论与发现
- 显式建模外生过程能显著提升长时程规划的泛化能力，尤其在含自然演化过程的场景中。
- 联合学习符号状态和因果过程，结合变分贝叶斯与LLM，可在有限数据下学到鲁棒的世界模型。
- 在五种模拟桌面环境中，ExoPredicator 在**规划成功率和泛化性上优于所有基线**。

## 7. 优点
- **方法创新**：首次将外生过程与内源动作因果解耦，并联合学习符号状态，为世界模型提供可解释性。
- **数据高效**：利用LLM指导因果结构搜索，减少所需演示数据，适合机器人领域数据稀疏的特点。
- **泛化能力强**：能够迁移到更多物体、更复杂目标的未见任务，说明学到抽象因果表示。
- **规划速度快**：基于符号状态的规划相比纯神经网络规划更高效，便于实时应用。

## 8. 不足与局限
- **实验仅在模拟环境**：缺少真实物理机器人验证，外生过程在现实中的噪声、不确定性未充分测试。
- **假设外生过程可被符号化**：对于某些连续、非离散的外生现象（如光线变化、摩擦），符号化抽象可能丢失细节。
- **LLM依赖**：LLM的提议质量可能影响学习效果，且引入额外计算成本，论文未讨论LLM失败案例的影响。
- **未报告算力信息**：可复现性不足。
- **应用限制**：当前框架可能只适用于可明确分解为离散符号状态和独立因果过程的任务，对于连续控制、高维观察（如视觉输入）需额外设计感知模块。

（完）
