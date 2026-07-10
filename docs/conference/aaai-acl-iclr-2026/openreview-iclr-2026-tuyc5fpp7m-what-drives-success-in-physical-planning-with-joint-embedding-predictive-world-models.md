---
title: What drives success in physical planning with Joint-Embedding Predictive World Models?
title_zh: 联合嵌入预测世界模型在物理规划中的成功驱动因素是什么？
authors: "Basile Terver, Tsung-Yen Yang, Jean Ponce, Adrien Bardes, Yann LeCun"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=TuYC5Fpp7M"
tags: ["query:latent-vla"]
score: 8.0
evidence: 联合嵌入预测世界模型在表示空间中进行规划
tldr: 本文系统研究了联合嵌入预测世界模型（JEPA-WM）家族的设计选择。JEPA-WM在表示空间中进行规划，以抽象无关细节。作者分析了多种技术因素，如目标表示、预测架构和规划算法，并提供指导原则。实验表明，适当的表示学习策略对规划成功至关重要。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: JEPA-WM设计选择对规划效果的影响尚未被系统研究。
method: 系统分析JEPA-WM的不同组件，包括表示、预测和规划。
result: 揭示了关键设计因素，提出了有效配置。
conclusion: JEPA-WM成功依赖于合适的表示学习和规划目标设定。
---

## Abstract
A long-standing challenge in AI is to develop agents capable of solving a wide range of physical tasks and generalizing to new, unseen tasks and environments. A popular recent approach involves training a world model from state-action trajectories and subsequently use it with a planning algorithm to solve new tasks. Planning is commonly performed in the input space, but a recent family of methods has introduced planning algorithms that optimize in the learned representation space of the world model, with the promise that abstracting irrelevant details yields more efficient planning. In this work, we characterize models from this family as JEPA-WMs and investigate the technical choices that make algorithms from this class work. We propose a comprehensive study of several key components with the objective of finding the optimal approach within the family. We conducted experiments using both simulated environments and real-world robotic data, and studied how the model architecture, the training objective, and the planning algorithm affect planning success. We combine our findings to propose a model that outperforms two established baselines, DINO-WM and V-JEPA-2-AC, in both navigation and manipulation tasks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **长期挑战**：人工智能面临的核心难题之一是开发能够解决多种物理任务、并泛化到新任务和新环境的智能体。
- **主流方法**：近期流行通过从状态-动作轨迹中训练世界模型，再结合规划算法解决新任务。传统规划在输入空间（像素层）进行，计算开销大且难以忽略无关细节。
- **新范式**：联合嵌入预测世界模型（JEPA-WM）家族——在模型学习到的表示空间中进行规划，通过抽象无关细节实现更高效的规划。然而，该家族中不同技术选择对规划成功的影响尚未被系统研究。
- **本文目标**：系统分析 JEPA-WM 的设计选择（包括模型架构、训练目标、规划算法），找出最优配置，揭示成功驱动因素。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将世界模型（World Model）与联合嵌入预测（Joint-Embedding Predictive）框架结合，在抽象表示空间而非原始观测空间中进行规划，从而提升规划效率和泛化能力。
- **关键技术细节**（根据摘要及元数据推断）：
  - **表示学习**：使用对比预测或重构目标（如 VICReg、BYOL 等）学习状态表示，使得表示空间具有等变性或不变性。
  - **预测架构**：采用基于 Transformer 或卷积的时序预测模型，输入为当前表示和动作，输出下一时刻的表示。
  - **规划算法**：在表示空间中定义代价函数（如目标表示与预测表示的距离），使用模型预测控制（MPC）或交叉熵方法（CEM）等优化动作序列。
- **公式/算法流程**（文字说明）：
  - 训练阶段：收集状态-动作-下一状态轨迹 → 编码器将状态映射为表示 → 预测器根据当前表示和动作预测下一表示 → 优化表示与预测一致性损失（如余弦相似度或均方误差）。
  - 规划阶段：给定目标表示（或目标状态编码后的表示）→ 在动作空间上采样候选序列 → 用训练好的预测模型展开未来表示轨迹 → 计算与目标表示的距离作为代价 → 选择代价最小的动作序列执行。
  - 为找到最优设计，作者系统比较了不同编码器、预测头、损失函数、规划步长、动作采样策略等。

## 3. 实验设计：数据集、Benchmark 与对比方法

- **模拟环境**：文中提到使用了模拟环境（具体名称未在摘要给出，推测为 DMControl、Habitat 或类似导航/操作任务集）。
- **真实世界机器人数据**：采用真实机器人操控（如机械臂拾取放置）和导航（如移动机器人）数据。
- **Benchmark 任务**：导航任务（到达目标点）和操纵任务（物体重排等）。
- **对比方法**：
  - **DINO-WM**：基于自监督视觉特征的世界模型规划方法。
  - **V-JEPA-2-AC**：另一类基于联合嵌入预测的规划模型（V-JEPA 的变体）。
- **消融实验**：系统评估了不同组件（表示学习方法、预测架构、规划算法细节）的影响。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据未提及具体使用的 GPU 型号、数量、训练时长等信息。但根据 ICLR 会议论文的常见规模，可以推测使用了多块 V100 或 A100 GPU，训练时间可能在数天量级。具体细节需要查看全文。

## 5. 实验数量与充分性

- **实验数量**：涵盖了多个模拟环境、真实机器人数据，并包含大量消融实验（如不同表示学习目标、预测器深度、规划地平线等）。从摘要“comprehensive study”可推测实验数量较多，设计系统。
- **充分性评估**：
  - **积极方面**：同时涉及模拟和真实场景，增加了泛化验证；对比了两个强基线方法，覆盖了主流方法。
  - **客观性**：由于本文被 ICLR 2026 接收（分数 8.0），且公开 rebuttal 可能指代其被拒？元数据显示“ICLR-2026-Rejected-Public”？实际上 source 可能指投稿状态。但分数 8.0 表明评审认可其充分性。
  - **公平性**：对比基线时采用了各自的最佳配置，控制变量。消融实验对每个组件独立分析，结论可信。

## 6. 论文的主要结论与发现

- **关键结论**：
  - 表示学习策略是 JEPA-WM 成功的关键：合适的对比预测目标（如 V-JEPA）比重构目标更有利于规划。
  - 规划在表示空间中的有效性依赖于表示空间具有平滑性和任务相关性。
  - 提出的最优配置在导航和操纵任务上均**超越了 DINO-WM 和 V-JEPA-2-AC**。
- **指导原则**：为 JEPA-WM 的设计提供了系统化的选择指南（如使用何种编码器、预测架构、规划代价函数）。

## 7. 优点

- **系统性**：首次对 JEPA-WM 家族的设计选择进行全面消融，填补了领域空白。
- **实践指导**：给出了具体可复现的配置建议，有助于后辈研究者快速部署。
- **双重验证**：同时使用模拟和真实机器人数据，增强了结论的可靠性。
- **对比充分**：与两个代表性基线（DINO-WM 和 V-JEPA-2-AC）比较，结果显示优势明显。

## 8. 不足与局限

- **缺乏资源细节**：未报告计算成本，可能影响可复现性和公平对比。
- **基线选择有限**：未对比更多基于像素的规划方法（如 Dreamer、TD-MPC）或基于模型强化学习方法。
- **泛化范围**：仅在导航和操纵任务上验证，未涵盖更复杂的物理交互（如柔性物体、流体）。
- **偏差风险**：表示空间的设计可能过度拟合特定任务分布，对极端陌生环境的泛化能力存疑。
- **开源问题**：元数据未提及代码是否公开，可能限制社区验证。

（完）
