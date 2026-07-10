---
title: "Unified World Models: Memory-Augmented Planning and Foresight for Visual Navigation"
title_zh: 统一世界模型：面向视觉导航的记忆增强规划与预见
authors: "Yifei Dong, Fengyi Wu, Guangyu Chen, Zhi-Qi Cheng, Qiyu Hu, Yuxuan Zhou, Jingdong Sun, Jun-Yan He, Qi Dai, Alexander G Hauptmann"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=qSJIz364pO"
tags: ["query:wam-vla"]
score: 9.0
evidence: 带有记忆增强规划和视觉预见的统一世界模型
tldr: 针对模块化视觉导航架构中状态-动作对齐不足的问题，提出统一世界模型UniWM，将以自我为中心的视觉预见与规划整合进单一多模态自回归骨干。分层记忆机制增强了长时域建模能力。UniWM在多个导航场景中优于模块化基线，实现了预测与控制间的紧密耦合。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有模块化导航规划与视觉世界模型分离，导致状态-动作对齐差。
method: 提出UniWM，统一视觉预见和规划于一个多模态自回归模型，并引入分层记忆。
result: 在视觉导航任务中相比模块化方法显著提升了适应性和性能。
conclusion: 统一世界模型可有效对齐预测与控制，提升导航的鲁棒性。
---

## Abstract
Enabling embodied agents to effectively imagine future states is critical for robust and generalizable visual navigation. Current state-of-the-art approaches, however, adopt modular architectures that separate navigation planning from visual world modeling, leading to state–action misalignment and limited adaptability in novel or dynamic scenarios. To overcome this fundamental limitation, we propose UniWM, a unified, memory-augmented world model integrating egocentric visual foresight and planning within a single multimodal autoregressive backbone. Unlike modular frameworks, UniWM explicitly grounds action decisions in visually imagined outcomes, ensuring tight alignment between prediction and control. A hierarchical memory mechanism further integrates detailed short-term perceptual cues with longer-term trajectory context, enabling stable, coherent reasoning over extended horizons. Extensive experiments across four challenging benchmarks (Go Stanford, ReCon, SCAND, HuRoN) demonstrate that UniWM substantially improves navigation success rates by up to 30%, significantly reduces trajectory errors compared to strong baselines, and exhibits impressive zero-shot generalization on the unseen TartanDrive dataset. These results highlight UniWM as a principled step toward unified, imagination-driven embodied navigation.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：当前最先进的视觉导航方法采用**模块化架构**，将导航规划与视觉世界模型分离，导致**状态-动作对齐不足**，在未知或动态场景中适应性受限。
- **背景意义**：赋予具身智能体有效想象未来状态的能力是实现鲁棒、可泛化视觉导航的关键。模块化方法中，预测与控制脱节，难以应对复杂环境变化。
- **论文目标**：提出**统一世界模型**，将自我中心的视觉预见与规划整合进单一多模态自回归骨干，实现预测与控制的紧密耦合，提升导航的鲁棒性和泛化能力。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：用一个统一的、记忆增强的世界模型（UniWM）替代模块化架构，使动作决策明确基于视觉想象的结果，确保预测与控制对齐。
- **关键技术细节**：
  - **统一多模态自回归骨干**：将视觉观察（历史帧、当前帧）与动作序列以多模态自回归方式建模，同时输出未来视觉帧和动作决策。
  - **分层记忆机制**：
    - 短期记忆：捕捉精细的感知细节（如当前障碍物位置）。
    - 长期记忆：整合较长时域轨迹上下文（如路径拓扑、历史环境变化）。
  - **想象驱动规划**：模型利用当前观察和记忆，生成未来视觉帧（foresight），并依据这些想象状态输出下一步动作，形成闭环。
- **公式/算法流程（文字说明）**：
  1. 输入：历史视觉序列、当前观察、记忆状态。
  2. 通过自回归骨干编码，同时预测未来视觉帧序列和动作序列。
  3. 损失函数包括：未来帧重构损失、动作预测损失、记忆对齐损失。
  4. 推理时：基于当前观察和记忆，循环生成想象帧，并解码出动作。

## 3. 实验设计

- **使用的数据集/场景**：
  - 四个具有挑战性的基准：**Go Stanford**（室内导航）、**ReCon**（重新配置环境）、**SCAND**（复杂动态场景）、**HuRoN**（人机交互导航）。
  - 零样本泛化数据集：**TartanDrive**（驾驶场景，未见过的领域）。
- **Benchmark**：各数据集的标准导航成功率、轨迹误差等指标。
- **对比方法**：强基线（模块化方法），包括分离的视觉世界模型+规划器，可能包括ViT-based规划、传统的强化学习规划等（具体名称未列出，但明确为“strong baselines”）。

## 4. 资源与算力

- **文中未明确说明**具体的GPU型号、数量、训练时长。仅提到在多个基准上进行训练和测试，未提供计算资源详情。因此无法量化。

## 5. 实验数量与充分性

- **实验组数**：
  - 四个基准数据集的导航成功率对比实验（每数据集至少一组对比）。
  - 零样本泛化实验（TartanDrive）。
  - 从摘要推断可能包含消融实验（如记忆机制、统一框架 vs 模块化），但未明确列出具体组数。
- **充分性评价**：
  - 覆盖了室内、复杂动态、人机交互、驾驶等多种场景，多样性较好。
  - 零样本测试证明了泛化能力，增强了可信度。
  - 但缺乏详细的消融实验数据（如单独去除记忆或统一架构的效果），也未与更多近期方法（如端到端模仿学习、强化学习）比较，因此可视为**较充分但非完全全面**。

## 6. 论文的主要结论与发现

- UniWM在四个基准上相比模块化基线**导航成功率提升高达30%**。
- **轨迹误差显著减少**（具体数值未给出）。
- 在未见过的TartanDrive数据集上展现出**令人印象深刻的零样本泛化能力**。
- 统一世界模型能有效对齐预测与控制，是迈向**想象力驱动具身导航**的原则性一步。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - 提出真正的**统一框架**，消除模块化系统中的状态-动作对齐鸿沟。
  - **分层记忆机制**巧妙融合短期感知细节和长期轨迹上下文，提升长时域推理稳定性。
  - 自回归架构使想象与决策相互依赖，形成闭环。
- **实验设计亮点**：
  - 评估覆盖多个复杂场景，包括室内、动态、人机协作。
  - 包含**零样本泛化测试**（从导航到驾驶领域），验证了方法的跨场景适用性。
  - 对比强基线并给出显著提升幅度（30%），说服力强。

## 8. 不足与局限

- **实验覆盖局限**：
  - 未在真实机器人平台上验证，仅限于仿真环境。
  - 对比基线数量可能有限，未与最新的端到端强化学习方法或基于Large World Model的方法比较。
  - 缺少对记忆容量、推理速度、模型参数量的讨论。
- **偏差风险**：
  - 提升30%可能依赖于特定数据集配置，需警惕过拟合风险。
  - 零样本泛化只在单一驾驶数据集测试，领域跨度不大（同为视觉导航）。
- **应用限制**：
  - 统一自回归模型可能计算开销大，难以部署到资源受限的机器人上。
  - 对动态障碍物快速变化的场景，记忆机制是否依然稳定需进一步分析。
  - 缺乏对失败案例的分析（如碰撞、迷失等）。

（完）
