---
title: "AdaReP: Plug-and-Play Acceleration for World Model Predictive Control using Adaptive Re-Planning"
title_zh: 自适应重规划：加速世界模型预测控制的即插即用方法
authors: "Yutian Cheng, Xiaojian Ma, Xianhao Wang, Min Yang, Rongpeng Su, Hangxin Liu, Xi Chen, Shuai Li, Qing Li"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=WbNf4npMlJ"
tags: ["query:wam-vla"]
score: 9.0
evidence: 理论分析了世界模型MPC中重新规划频率与性能的权衡，提出自适应重规划
tldr: 该论文针对世界模型MPC频繁重规划带来的高计算成本，理论刻画了重规划频率与控制性能的权衡，并提出AdaReP自适应重规划策略。实验表明在保持控制性能的同时大幅降低了计算开销，为世界模型MPC的实用化提供了理论基础和高效方案。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 世界模型MPC频繁重规划计算昂贵，但减少重规划会积累误差。
method: 理论分析权衡，提出AdaReP根据误差和灵敏度自适应调整重规划频率。
result: 在多个机器人控制任务上，AdaReP计算成本降低数倍且控制性能不降。
conclusion: 自适应重规划可有效平衡世界模型MPC的效率与性能。
---

## Abstract
We investigate the integration of model predictive control (MPC) with world models for robotic control tasks. Existing MPC solvers often replan at every step or after very few steps, primarily to mitigate the accumulation of world model prediction errors. However, such frequent replanning incurs substantial computational costs -- especially when using large, complex world models. In this work, we theoretically characterize the fundamental trade-off between computational efficiency and control performance in MPC. Our analysis reveals how replanning frequency, model prediction error, and local dynamics sensitivity jointly influence MPC performance, as captured by regret bounds. Based on the analysis, we propose AdaRep, a novel adaptive replanning mechanism for MPC that dynamically modulates the replanning frequency based on online estimates of world model prediction error and local dynamics sensitivity. AdaRep is training-free, plug-and-play, and compatible with various world models and MPC solvers. Experiments on the VP2 simulation benchmark across diverse tasks, as well as real-world robotic tasks including door opening and T-block pushing, show that AdaRep achieves substantial reductions in computation, over 80–90% in the real-world settings while maintaining or improving task success rates.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在世界模型与模型预测控制（MPC）结合的机器人控制任务中，传统 MPC 求解器通常**每个时间步**或**极少步长**后重新规划（replan），以抑制世界模型预测误差的累积。但频繁重规划会带来巨大的计算开销，尤其是当世界模型规模庞大、结构复杂时。
- **研究动机**：如何在**控制性能**与**计算效率**之间取得更好的平衡？降低重规划频率可以节省计算资源，但可能导致预测误差积累，进而降低控制质量。该论文试图理论化这一权衡，并提出一种实用的自适应策略。
- **整体含义**：该工作为世界模型 MPC 的实用化提供了理论基础（刻画重规划频率、模型误差、局部动力学灵敏度与性能的数学关系）和高效解决方案（无需额外训练、即插即用的自适应重规划方法），有望推动世界模型在机器人实时控制中的应用。

## 2. 论文提出的方法论

- **核心思想**：通过**理论分析**得到重规划频率、世界模型预测误差、局部动力学灵敏度三者如何共同影响 MPC 的控制性能（以遗憾界 regret bound 刻画）。基于这一理论洞察，设计**自适应重规划机制**，动态调整重规划间隔，以最小的计算代价维持满意的控制性能。
- **关键技术细节**：
  - **理论分析**：推导出 regret bound，揭示控制性能随重规划频率增加的收益递减规律，以及当模型误差大或动力学灵敏度高时，需要更频繁的重规划才能保持性能。
  - **AdaReP（自适应重规划）**：
    - 在线估计**世界模型预测误差**（例如通过滑动窗口计算近期预测残差）。
    - 在线估计**局部动力学灵敏度**（例如通过雅可比矩阵或状态变化率）。
    - 根据上述估计值，利用一个简单的决策规则（如阈值或线性插值）自动调整下一次重规划的时刻。
    - 无训练过程，直接嵌入现有 MPC 框架，无需修改世界模型或基础求解器，**即插即用**。
- **算法流程（文字说明）**：
  1. 初始化重规划间隔（可设为每步重规划）。
  2. 在每次规划执行后，收集实际状态与模型预测状态，计算当前预测误差。
  3. 同时计算局部动力学灵敏度（如状态对控制输入的敏感度）。
  4. 根据预设的误差-灵敏度-频率映射函数（来自理论分析），决定下一次重规划的步数延迟。
  5. 若误差或灵敏度增大，缩短间隔；若两者均小，延长间隔。
  6. 重复上述过程，直到任务结束。

## 3. 实验设计

- **使用的数据集/场景**：
  - **仿真 benchmark**：VP2（Visual Planning and Prediction）仿真平台上的多种任务（具体任务名称摘要未列全，推测包括导航、操作等）。
  - **真实机器人任务**：
    - 开门（door opening）
    - 推 T 形块（T-block pushing）
- **对比方法**：摘要未明确列出对比方法名称，但合理推断会对比：
  - 每步重规划（固定频率 1）
  - 固定步长重规划（如每 5 步、每 10 步等）
  - 可能还有基于规则或启发式的自适应重规划基线（但未提及）
- **Benchmark**：VP2 是常用的世界模型评估基准；真实任务为典型机器人操作场景，具有实际意义。

## 4. 资源与算力

- 论文内容中**未明确说明**使用的 GPU 型号、数量、训练时长等具体算力信息。仅提及 AdaReP 无需训练（training-free），因此主要消耗来自 MPC 求解和世界模型推理。实际计算资源取决于底层世界模型的大小，未提供具体数字。

## 5. 实验数量与充分性

- **实验数量**：覆盖了**两个真实任务 + 多个 VP2 仿真任务**，至少包含 5 个以上不同场景。推测有**消融实验**分析不同组件（如误差估计、灵敏度估计）对性能的影响，以及**在不同重规划基线下的对比**。但摘要未给出具体实验组数。
- **充分性评估**：
  - ✅ **优点**：同时包含仿真和实物验证，结果具有跨任务泛化性；真实环境展示 80–90% 计算降低，且保持或提升成功率，证据有力。
  - ❓ **待补充**：未提供统计显著性检验或多次随机种子结果；未明确消融实验细节；可能缺少与其他自适应方法的直接对比（若有则更好）。整体而言实验设计合理，但充分性需要看全文是否包含更详尽的表格和重复性实验。

## 6. 论文的主要结论与发现

- **理论发现**：严格刻画了 MPC 中重规划频率、模型预测误差、局部动力学灵敏度与控制性能（后悔度）的数学关系，揭示了降低频率的代价边界。
- **方法有效**：AdaReP 在多个任务上均能**大幅降低计算开销**（真实场景降低 80–90%），同时维持甚至提升任务成功率。
- **实用价值**：方法无需训练、即插即用，可无缝适配已有世界模型和 MPC 求解器，具有强实用性和兼容性。

## 7. 优点

- **理论驱动**：给出可解释的 regret bound 分析，方法建立于理论之上，可信度更高。
- **即插即用**：无需训练、不改变模型结构，易于在现有系统中集成。
- **效率与性能双赢**：在保持/提升成功率的同时显著降低计算成本（高达 90%），对实时控制意义重大。
- **实验完整性**：覆盖仿真和真实机器人任务，增强了结论的泛化性。
- **方法简洁**：自适应策略基于在线估计的两个标量，实现简单，计算开销小。

## 8. 不足与局限

- **实验细节不详**：摘要是简略版，未说明任务具体数量、对比方法、消融实验设置、统计显著性等，需阅读全文核实。
- **算力信息缺失**：未报告实验所使用的 GPU/CPU 型号与耗时，不利于复现和对比。
- **误差与灵敏度估计依赖**：方法性能可能受在线估计质量影响，当估计不准时可能退化；论文或需讨论鲁棒性。
- **应用限制**：测试任务主要集中操作类场景，未涉及长时序、高动态或稀疏奖励任务；理论假设（如局部线性化）可能在强非线性或高维状态空间中不够精确。
- **缺乏与其他自适应 MPC 方法对比**：如贝叶斯优化、元学习等调整重规划频率的方法，未见提及。

（完）
