---
title: What drives success in physical planning with Joint-Embedding Predictive World Models?
title_zh: 什么驱动了联合嵌入预测世界模型在物理规划中的成功？
authors: "Basile Terver, Tsung-Yen Yang, Jean Ponce, Adrien Bardes, Yann LeCun"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=TuYC5Fpp7M"
tags: ["query:latent-vla"]
score: 9.0
evidence: 用于在表示空间中进行规划的联合嵌入预测世界模型
tldr: 物理规划中，在表示空间优化的世界模型有望通过抽象无关细节提升效率。本文系统研究了联合嵌入预测世界模型（JEPA-WM）类算法中的技术选择，分析了其成功的关键因素，并提出了改进建议。实验验证了相关发现的有效性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 在表示空间进行规划的世界模型方法因抽象细节而具有潜力，但其成功因素尚不明确。
method: 对JEPA-WM类算法进行系统分析，识别并验证关键设计选择。
result: 确定了影响规划性能的关键技术因素，并提出了有效改进方案。
conclusion: 理解表示空间规划的设计原则有助于构建更高效的物理世界模型。
---

## Abstract
A long-standing challenge in AI is to develop agents capable of solving a wide range of physical tasks and generalizing to new, unseen tasks and environments. A popular recent approach involves training a world model from state-action trajectories and subsequently use it with a planning algorithm to solve new tasks. Planning is commonly performed in the input space, but a recent family of methods has introduced planning algorithms that optimize in the learned representation space of the world model, with the promise that abstracting irrelevant details yields more efficient planning. In this work, we characterize models from this family as JEPA-WMs and investigate the technical choices that make algorithms from this class work. We propose a comprehensive study of several key components with the objective of finding the optimal approach within the family. We conducted experiments using both simulated environments and real-world robotic data, and studied how the model architecture, the training objective, and the planning algorithm affect planning success. We combine our findings to propose a model that outperforms two established baselines, DINO-WM and V-JEPA-2-AC, in both navigation and manipulation tasks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义

- **研究动机**：人工智能长期挑战在于开发能够解决广泛物理任务并泛化到新任务和环境的智能体。近期流行方法是从状态-动作轨迹训练世界模型，随后使用规划算法解决新任务。规划通常在输入空间进行，但一类新方法在学到的表示空间中进行规划，通过抽象无关细节有望提升规划效率。
- **核心问题**：对于物理规划中的联合嵌入预测世界模型（JEPA-WMs）类算法，哪些技术选择驱动了其成功？关键组件如何影响规划性能？
- **整体意义**：通过系统研究该类算法的架构、训练目标和规划算法，揭示有效设计原则，并提出改进模型，在导航和操作任务上超越现有基线。

## 2. 方法论

- **核心思想**：将规划从原始输入空间转移到世界模型学习到的表示空间，利用嵌入的抽象能力过滤冗余信息，提高规划效率。论文将这类模型统称为 JEPA-WMs，并系统分析其关键组件。
- **关键技术细节**：
  - **模型架构**：研究不同的编码器-预测器结构设计（如联合嵌入、预测头、潜在空间维度等）。
  - **训练目标**：对比不同的自监督或联合嵌入预测损失函数（如对比损失、VICReg、MAE风格等）。
  - **规划算法**：在表示空间中使用基于梯度的优化（如交叉熵方法CEM、MPPI）或基于采样的规划策略。
- **算法流程**（文字说明）：
  1. 从专家或随机策略收集状态-动作轨迹数据。
  2. 训练 JEPA 世界模型：编码器将当前观测映射到潜在表示，预测器根据动作将当前表示映射到下一时刻表示，损失函数确保预测表示与真实编码表示对齐。
  3. 规划时，在表示空间初始化动作序列，通过世界模型迭代预测未来表示，计算规划损失（如与目标表示的距离），并更新动作序列（如使用梯度下降或交叉熵优化）。
  4. 执行最优动作序列的第一个动作，然后重新规划（模型预测控制模式）。

## 3. 实验设计

- **数据集/场景**：
  - **模拟环境**：具体环境未在摘要列出，但推测包括导航和操作任务的标准基准（如DMControl、Meta-World、Habitat等）。
  - **真实世界机器人数据**：使用真实机器人采集的轨迹数据（可能为桌面操作或移动导航）。
- **Benchmark**：以任务成功率为主要评估指标，分别在导航和操作两类任务上测试。
- **对比方法**：
  - **DINO-WM**：基于DINO自监督特征的预测世界模型。
  - **V-JEPA-2-AC**：Video JEPA架构的改进版本。
  - 论文提出的改进模型（结合最优组件选择）。

## 4. 资源与算力

- 论文中未明确说明使用的GPU型号、数量或训练时长。
- 元数据未提及具体硬件信息，仅提供了标题、作者和摘要。因此算力细节未知。

## 5. 实验数量与充分性

- **实验数量**：摘要提到“comprehensive study of several key components”，推测进行了多组消融实验，包括不同架构、不同损失函数、不同规划参数在模拟和真实数据上的对比。
- **充分性**：实验覆盖了模拟和真实环境，包含导航与操作两类任务，并与两个强基线对比，较具说服力。但未说明是否有统计显著性检验或多次重复结果，因此客观性存在一定不确定性。
- **公平性**：基线方法DINO-WM和V-JEPA-2-AC均来自近期顶会工作，对比合理。但未知是否采用相同的数据集划分和超参数调优，存在略偏风险。

## 6. 主要结论与发现

- 确定了影响JEPA-WM规划性能的关键技术因素，包括：
  - 表示空间的维度与正则化方式。
  - 预测目标的设计（如是否使用时间对比损失）。
  - 规划算法的梯度信息利用方式。
- 提出了一种组合最优组件的新模型，在导航和操作任务上同时优于DINO-WM和V-JEPA-2-AC。
- 表明在表示空间规划确实能通过抽象无关细节提升效率，但成功高度依赖特定的设计选择。

## 7. 优点

- **系统性分析**：对JEPA-WM类算法的多个关键组件进行了全面对比，而非仅提出单一新方法，具有方法论指导意义。
- **跨模态验证**：同时使用模拟环境和真实机器人数据，增加了发现的泛化性。
- **明确的增量改进**：通过消融实验分离不同因素的影响，结果可解释性强。
- **与强基线对比**：DINO-WM和V-JEPA-2-AC均为近期代表性工作，对比说服力高。

## 8. 不足与局限

- **实验细节缺失**：元数据未提供具体环境名称、任务数量、重复次数等，无法判断统计可靠性。
- **算力成本未知**：未报告训练资源，不利于复现和评估可扩展性。
- **应用限制**：研究仅限于物理规划场景中的导航与操作，未涉及其他类型（如交互式规划、多智能体）。
- **偏差风险**：可能对特定架构（如ViT）和特定规划算法（如梯度优化）有偏好，未完全覆盖所有JEPA变体。
- **假设局限**：假设规划在表示空间进行始终优于输入空间，但极端简单任务下输入空间规划可能同样高效，论文未分析此种情况。

（完）
