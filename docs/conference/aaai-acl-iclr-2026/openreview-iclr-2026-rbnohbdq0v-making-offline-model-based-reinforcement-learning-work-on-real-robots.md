---
title: Making Offline Model-Based Reinforcement Learning Work on Real Robots
title_zh: 让离线模型强化学习在真实机器人上发挥作用
authors: "Chenhao Li, Andreas Krause, Marco Hutter"
date: 2025-09-11
pdf: "https://openreview.net/pdf?id=rbNOhbdQ0v"
tags: ["query:wam-vla"]
score: 8.0
evidence: 扩展自回归世界模型与认知不确定性用于真实机器人离线MBRL
tldr: 该论文针对离线模型强化学习在真实机器人上泛化困难的问题，提出RWM-O管道，在自回归世界模型中融入认知不确定性估计。实验证明在真实机器人数据集上有效缓解了分布偏移和累积误差，为离线MBRL的实用化提供了可靠方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 离线MBRL在真实机器人数据上因误差累积和分布偏移效果差。
method: 提出RWM-O，在自回归世界模型中集成认知不确定性估计。
result: 在真实机器人任务上，RWM-O显著优于现有离线MBRL方法。
conclusion: 合理的不确定性建模是离线MBRL走向真实应用的关键。
---

## Abstract
Reinforcement Learning (RL) has achieved impressive results in robotics, yet high-performing pipelines remain highly task-specific, with little reuse of prior data. Offline Model-based RL (MBRL) offers greater data efficiency by training policies entirely from existing datasets, but suffers from compounding errors and distribution shift in long-horizon rollouts. Although existing methods have shown success in controlled simulation benchmarks, robustly applying them to the noisy, biased, and partially observed datasets typical of real-world robotics remains challenging. We present a principled pipeline for making offline MBRL effective on physical robots. Our RWM-O extends autoregressive world models with epistemic uncertainty estimation, enabling temporally consistent multi-step rollouts with uncertainty effectively propagated over long horizons. We combine RWM-O with MOPO-PPO, which adapts uncertainty-penalized policy optimization to the stable, on-policy PPO framework for real-world control. We evaluate our approach on diverse manipulation and locomotion tasks in simulation and on a real quadruped, training policies entirely from offline datasets. The resulting policies consistently outperform model-free and uncertainty-unaware model-based baselines, and fusing real-world data in model learning further yields robust policies that surpass online model-free baselines trained solely in simulation.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：离线模型强化学习（Offline Model-Based RL, MBRL）在真实机器人数据集上表现不佳，主要原因是：
  - 长时域展开中的累积误差（compounding errors）
  - 分布偏移（distribution shift）
  - 真实数据具有噪声、偏倚、部分可观测等特点，现有方法仅在仿真基准中有效。
- **整体含义**：本文旨在提出一种**实用的管道**，使离线MBRL能够可靠地应用于物理机器人，从而提升数据效率并实现跨任务的先验数据复用。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：在自回归世界模型中引入**认知不确定性估计**（epistemic uncertainty estimation），使得多步展开（rollouts）在时间上保持一致，且不确定性能够在长时域上有效传播。
- **关键技术细节**：
  - **RWM-O（Recurrent World Model with Uncertainty）**：扩展自回归世界模型，附加认知不确定性模块。
  - **MOPO-PPO**：将传统MOPO（Model-based Offline Policy Optimization）中的不确定性惩罚策略优化适配到**稳定、在策略**的PPO框架中，以适合真实世界控制。
- **算法流程（文字说明）**：
  1. 从离线数据集中学习一个带认知不确定性估计的自回归世界模型（RWM-O）。
  2. 利用该模型生成虚拟轨迹，并对每个预测引入不确定性惩罚。
  3. 使用PPO算法在虚拟轨迹上优化策略，同时利用不确定性惩罚避免分布偏移。
  4. 最终策略可直接在真实机器人上部署。

> **注意**：由于论文全文未提供，上述细节基于摘要和元数据中的简短描述推断，可能与原文有出入。

## 3. 实验设计

- **数据集/场景**：
  - 仿真环境：多种操作（manipulation）和移动（locomotion）任务。
  - 真实机器人：四足机器人（quadruped），使用完全离线数据集训练。
- **基准（Benchmark）**：
  - 对比方法：模型无关（model-free）基线、未考虑不确定性（uncertainty-unaware）的模型基线。
  - 另对比了在线模型无关基线（仅在仿真中训练）。
- **评估指标**：策略性能（奖励、成功率等），具体指标原文未详述。

## 4. 资源与算力

- **文中未明确说明**：未提及使用的GPU型号、数量、训练时长等算力信息。
- **推测**：由于涉及真实机器人实验和仿真，可能需要至少一台配备GPU的工作站，但无具体数字。

## 5. 实验数量与充分性

- **实验数量**：摘要提到“在仿真和真实四足机器人上的多种操作和移动任务中评估”。至少包含：
  - 多个仿真任务（操作+移动）
  - 一个真实机器人任务（四足机器人）
  - 与多个基线对比（model-free、uncertainty-unaware、在线model-free）
- **充分性评价**：
  - 正面：同时包含仿真和真实实验，覆盖不同场景；对比基线多样；消融实验（不确定性的影响）隐含在对比中。
  - 不足：未提供任务具体数量、随机种子次数、统计显著性检验等详细信息（因摘要篇幅限制）。**实验的充分性无法完全判断，但方向合理**。

## 6. 论文的主要结论与发现

- RWM-O在真实机器人任务上**显著优于**现有离线MBRL方法和模型无关基线。
- 将真实数据与模型学习融合后，得到的鲁棒策略甚至能够**超越**仅在仿真中训练的在线模型无关基线。
- **核心结论**：合理的不确定性建模是离线MBRL走向真实应用的关键。

## 7. 优点：方法或实验设计上的亮点

- **方法层面**：
  - 在自回归世界模型中集成认知不确定性估计，解决长时域误差累积问题。
  - 将MOPO适配到PPO框架，利用其在策略学习的稳定性，更适合真实控制。
- **实验层面**：
  - 在真实四足机器人上验证，结果具有直接现实意义。
  - 消融对比（uncertainty-aware vs. uncertainty-unaware）直接证明了不确定性的重要性。
  - 融合真实数据后性能超越纯仿真训练，说明数据复用价值。

## 8. 不足与局限

- **实验覆盖**：
  - 仅测试了四足机器人一种实物平台，未在多种真实机器人（如机械臂、灵巧手）上验证。
  - 仿真任务可能偏向特定难度。
- **偏差风险**：
  - 对比的model-free基线可能不是最优（如未使用最新算法）。
  - MOPO-PPO与RWM-O的组合可能存在超参数敏感问题。
- **应用限制**：
  - 认知不确定性估计的计算开销可能较大，对实时部署有挑战。
  - 离线数据集的质量和多样性对结果影响未知。
- **信息缺失**：由于全文未提供，无法深入分析算法细节、收敛性、泛化边界等。元数据标注为ICLR-2026-Rejected，可能未被顶级会议接收，需谨慎看待其贡献。

（完）
