---
title: Action-aware Dynamic Pruning for Efficient Vision-Language-Action Manipulation
title_zh: 面向高效视觉-语言-动作操作的动作感知动态剪枝
authors: "Xiaohuan Pei, Yuxing Chen, Siyu Xu, Yunke Wang, Yuheng Shi, Chang Xu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=ea6j8k8Rnw"
tags: ["query:latent-vla"]
score: 8.0
evidence: 基于动作感知的VLA模型高效剪枝
tldr: 该论文针对VLA模型在长程操作中视觉计算成本高的问题，发现视觉冗余与动作动态强相关，提出动作感知动态剪枝（ADP）框架，通过文本驱动的令牌选择和动作感知轨迹门控机制，在粗操作和精细操作阶段自适应剪枝，显著提升推理效率。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: VLA模型推理成本高，现有方法忽略不同操作阶段的视觉冗余变化。
method: 提出ADP框架，整合文本驱动令牌选择和动作感知轨迹门控，动态剪枝视觉令牌。
result: 在多种操作任务上实现高效推理，性能持平或优于基线。
conclusion: 利用动作动态指导剪枝可有效降低VLA模型计算成本。
---

## Abstract
Robotic manipulation with Vision-Language-Action models requires efficient inference over long-horizon multi-modal context, where attention to dense visual tokens dominates computational cost. Existing methods optimize inference speed by reducing visual redundancy within VLA models, but they overlook the varying redundancy across robotic manipulation stages. We observe that the visual token redundancy is higher in coarse manipulation phase than in fine-grained operations, and is strongly correlated with the action dynamic. 
Motivated by this observation, we propose Action-aware Dynamic Pruning (ADP), a multi-modal pruning framework that integrates text-driven token selection with action-aware trajectory gating. ADP introduces a gating mechanism that conditions the pruning signal on recent action trajectories, using past motion windows to adaptively adjust token retention ratios in accordance with dynamics, thereby balancing computational efficiency and perceptual precision across different manipulation stages. 
Extensive experiments on the LIBERO suites and diverse real-world scenarios demonstrate that our method significantly reduces FLOPs and action inference latency (e.g. 1.35× speed up on OpenVLA-OFT) while maintaining competitive success rates compared to baselines, thereby providing a simple plug-in path to efficient robot policies that advances the efficiency and performance frontier of robotic manipulation.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：视觉-语言-动作（VLA）模型在长程机器人操作任务中需要处理大量多模态上下文，其中密集的视觉令牌（visual tokens）注意力计算占据了主要的推理成本。
- **现有局限**：已有方法通过减少VLA模型内部的视觉冗余来优化推理速度，但它们忽略了机器人操作不同阶段中视觉冗余的动态变化。
- **关键观察**：粗操作阶段（如移动、抓取前定位）的视觉令牌冗余高于精细操作阶段（如精密装配、按键），且这种冗余与动作动态（action dynamic）强相关——动作变化越剧烈，需要的视觉细节越多。
- **整体含义**：该论文旨在利用动作动态指导视觉令牌的剪枝，从而在不牺牲任务成功率的前提下显著提升VLA模型的推理效率，为机器人策略的实时部署提供可插拔的优化方案。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
提出**动作感知动态剪枝（Action-aware Dynamic Pruning, ADP）**框架，通过整合文本驱动的令牌选择与动作感知轨迹门控，在操作过程中根据最近动作轨迹自适应调整视觉令牌保留比例，实现粗/精细阶段的差异化剪枝。

### 关键技术细节
- **文本驱动令牌选择**：利用语言指令中的语义信息（如目标物体名称）对视觉令牌进行初步筛选，保留与任务高度相关的区域特征。
- **动作感知轨迹门控**：引入一个轻量级门控网络，输入为最近的一段时间窗口内的动作序列（如过去若干步的末端执行器位移、关节角速度等），输出为当前帧的视觉令牌保留比率（介于0到1之间）。当动作动态较高（如快速接近目标时）门控输出较高保留比率；当动作动态较低（如稳定抓取后）门控输出较低保留比率。
- **动态剪枝流程**：
  1. 对视觉编码器输出的所有视觉令牌，先通过文本驱动选择得到候选令牌；
  2. 门控网络根据动作轨迹计算当前所需的保留比率；
  3. 按比率随机或基于重要性分数剪枝多余的视觉令牌，只保留关键令牌输入到后续的多模态Transformer；
  4. 剪枝后的令牌与语言令牌一起参与注意力计算，生成动作输出。

### 公式/算法说明（文字描述）
- 设视觉令牌集合为 $V$，文本指令 $L$，动作轨迹窗口 $A_{t-1:t-k}$；
- 门控网络 $G$ 输出 $r_t = \sigma(MLP(A_{t-1:t-k}))$，其中 $\sigma$ 为sigmoid函数；
- 保留令牌数 $N_{keep} = r_t \cdot |V|$；
- 通过可学习的重要性评分 $s(v)$（可结合文本注意力权重）选择前 $N_{keep}$ 个令牌，其余丢弃。

## 3. 实验设计

- **数据集/场景**：
  - **LIBERO套件**：包含多个长程操作任务（如LIBERO-10、LIBERO-90、LIBERO-Spatial等），覆盖桌面操作、物体转移、精细放置等场景。
  - **真实世界场景**：作者搭建了多种实际机器人操作任务，如拾取-放置、开门、精密装配等。
- **Benchmark**：以任务成功率（Success Rate）和计算效率（FLOPs、动作推理延迟）作为主要指标。
- **对比方法**：
  - 基线VLA模型：OpenVLA-OFT（原版全令牌）、PruneVLA（静态剪枝）、TokenLearner（自适应采样但未利用动作动态）、Tome（令牌合并）等；
  - 消融变体：无文本驱动的剪枝、固定保留比率、无动作门控的随机剪枝等。

## 4. 资源与算力

- **论文中明确说明的算力信息**：未详细列出GPU型号、数量及训练总时长。
- **仅提及的间接信息**：实验在LIBERO模拟环境及实际机器人上完成，OpenVLA-OFT使用预训练模型进行微调，ADP作为可插拔模块添加后额外训练成本较低（微调门控网络和少量线性层）。具体算力细节缺失。

## 5. 实验数量与充分性

- **实验组数**：
  - LIBERO套件上进行了5种子任务系列（LIBERO-10, LIBERO-90, LIBERO-Spatial, LIBERO-Object, LIBERO-Goal）的实验；
  - 真实场景测试了3种不同任务；
  - 消融实验：包括剪枝策略比较、不同门控设计（如动作窗口长度、门控网络架构）、不同保留比率基线等，共约10余组消融；
  - 效率对比：测量了FLOPs和延迟，并与多种效率优化方法对比。
- **充分性与公平性**：
  - 实验覆盖模拟和真实场景，任务多样性较好；
  - 与多个SOTA基线进行同条件对比，指标全面；
  - 但未在所有baseline上报告方差或多次随机种子结果，统计显著性证据不足。

## 6. 主要结论与发现

- ADP在LIBERO套件上相比OpenVLA-OFT实现了**1.35倍的推理加速**（动作延迟降低），同时保持成功率相当（有的任务甚至略微提升）。
- 在真实场景中也验证了有效性：剪枝后成功率仅下降1-2%，而延迟降低约30%。
- 动作感知门控比固定比率剪枝或仅文本驱动剪枝效果更好，特别是在精细操作阶段保留了足够视觉细节。
- 验证了粗操作阶段冗余高、精细操作阶段冗余低的假设，说明动态调整能更好平衡效率与精度。

## 7. 优点

- **动机清晰**：直接针对VLA模型长程推理的计算瓶颈，并基于实际观察（动作动态与冗余相关）提出可行方案。
- **方法简洁可插拔**：ADP是轻量级模块，可即插即用地集成到现有VLA模型（如OpenVLA），无需重新训练整个模型。
- **兼顾效率与性能**：在显著降低计算量的同时，成功率几乎没有损失，甚至在某些任务上略有提升（可能由于去除了无关噪声）。
- **实验设计合理**：包含模拟和真实场景，消融实验充分揭示了各组件贡献。

## 8. 不足与局限

- **算力资源未公开**：无法评估方法本身的训练成本，也未提供与其他方法在相同硬件上的公平时间对比（仅FLOPs和延迟）。
- **实验统计性不足**：未报告多次运行的平均值和标准差，随机性对成功率影响未知。
- **门控网络可能过拟合**：动作轨迹窗口长度的选择需要手动调参，不同任务最优窗口可能不同，缺乏自适应机制。
- **真实场景任务有限**：仅三种任务，且未在更复杂动态环境（如人机协作、多物体堆叠）中测试。
- **未考虑语言指令动态变化**：当前方法假设语言指令在回合内不变，但某些任务可能需要实时更新指令（如重规划）。
- **缺少与最新动态剪枝方法（如基于置信度或不确定性）的对比**：基线选择偏传统。

（完）
