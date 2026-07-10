---
title: "PointWorld: Scaling 3D World Models for In-The-Wild Robotic Manipulation"
title_zh: PointWorld：为野外机器人操作扩展3D世界模型
authors: "Wenlong Huang, Yu-Wei Chao, Arsalan Mousavian, Ming-Yu Liu, Dieter Fox, Kaichun Mo, Li Fei-Fei"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=XZ0pRezf4O"
tags: ["query:wam-vla"]
score: 7.0
evidence: 3D世界模型根据动作预测点流用于操作
tldr: 机器人操作需要预测物理世界如何响应动作。提出PointWorld，一个基础3D世界模型，在共享空间域中统一状态和动作，给定RGB-D图像和动作序列预测每点3D位移。构建大规模数据集包括真实和模拟操作，实验证明其在多种开放世界场景中有效预测动力学，为野外操作提供了可扩展的动力学预测基础。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 缺乏可扩展的3D世界模型用于复杂操作动力学预测。
method: 提出PointWorld，在3D点云空间预测动作引起的点流。
result: 在大规模数据集上训练，对多种操作场景的动力学预测准确。
conclusion: 3D世界模型为野外操作提供了可扩展的动力学预测基础。
---

## Abstract
Humans anticipate, from a glance and a contemplated action of their bodies, how the
3D world will respond. This predictive ability is equally vital for enabling robots
to manipulate and interact with the physical world. We introduce PointWorld,
a foundation 3D world model that unifies state and action in a shared spatial
domain and predicts 3D point flow over short horizons: given one or a few RGB-D
images and a sequence of robot actions, PointWorld forecasts per-point scene
displacements that responds to the actions. To train our 3D world model, we curate
a large-scale dataset for 3D dynamics learning spanning real and simulated robotic
manipulation in diverse open-world environments—enabled by recent advances
in 3D vision and diverse simulated environments—totaling about 2M trajectories
and 500 hours. Through rigorous, large-scale empirical studies of backbones,
action representations, learning objectives, data mixtures, domain transfers, and
scaling, we distill design principles for large-scale 3D world modeling. PointWorld
enables zero-shot simulation from in-the-wild RGB-D captures. It also powers
model-based planning and control on real hardware that generalizes across diverse
objects, and environments, all without task-specific demonstrations
or training.

---

## 论文详细总结（自动生成）

# PointWorld：为野外机器人操作扩展3D世界模型

## 1. 核心问题与整体含义

- **研究动机**：人类在看到场景和预想动作后，能够预测3D世界如何响应。机器人需要具备类似的动力学预测能力，才能实现灵巧的物理世界操作。然而，现有方法缺乏可扩展的3D世界模型来预测复杂操作中的动力学变化。
- **核心问题**：如何构建一个统一的、可扩展的3D世界模型，使其能够根据当前场景和机器人动作，预测未来短时间内的三维点云位移（点流），从而支持零样本的模型预测控制与规划。
- **整体含义**：PointWorld 提出了一种基础3D世界模型，将状态和动作统一到共享的3D空间域中，通过预测每点的3D位移来响应动作，为野外机器人操作提供动力学预测基础。

## 2. 方法论

- **核心思想**：在3D点云空间中，以统一的方式表示环境状态和机器人动作，并预测动作引起的点流（per-point 3D displacement）。
- **输入**：一张或几张 RGB-D 图像（提供场景的3D点云）以及一个机器人动作序列。
- **输出**：每个场景点的3D位移向量，即未来点云相对于当前点云的变化。
- **关键技术细节**：
  - 使用点云作为共享空间域，自然地融合了视觉观测和动作信息。
  - 预测短时间内的点流，而非长期轨迹，降低预测难度且保留空间细节。
  - 模型架构：通过大规模实证研究，设计适合3D世界建模的 backbone、动作表示、学习目标等。
- **公式/算法流程**（文字说明）：
  1. 接收 RGB-D 图像，重建场景点云。
  2. 将机器人动作编码为与点云对齐的空间特征（例如每个点的动作向量或全局动作条件）。
  3. 模型以点云和动作特征为输入，输出每个点的3D位移。
  4. 通过回归损失（如Chamfer距离或点位移误差）训练模型最小化预测点流与真实未来点云之间的差异。

## 3. 实验设计

- **数据集与场景**：
  - 构建大规模数据集用于3D动力学学习，涵盖真实和模拟的机器人操作。
  - 模拟数据来自多种开放世界环境（利用最新3D视觉和多样化模拟环境）。
  - 总计约 **200万条轨迹**，**500小时** 的操作数据。
  - 同时包含 real-world 数据，支持跨域迁移。
- **Benchmark**：
  - 在零样本的仿真动力学预测上进行评估（从野外的RGB-D捕获直接预测）。
  - 在真实硬件上进行基于模型的规划与控制，测试泛化到不同物体、环境的能力。
- **对比方法**：
  - 对不同类型的 backbone（如点云网络、Transformer等）、动作表示（绝对动作、相对动作、隐式条件）、学习目标（点位移 vs 场景流 vs 占用场）进行了大规模对比。
  - 未列明具体对比的 baseline 名称，但强调了系统性研究与消融。

## 4. 资源与算力

- **文中未明确说明使用的 GPU 型号、数量或训练时长**。仅提到数据集规模（200万轨迹、500小时），但未提及训练所需算力细节。

## 5. 实验数量与充分性

- **实验组数**：包含对 backbone、动作表示、学习目标、数据混合、域迁移、扩展规律（scaling）等六个主要维度的系统性实证研究。每一维度下应有多组消融或对比实验。
- **充分性与公平性**：
  - 实验覆盖了从模型设计、数据配置到零样本泛化、真实硬件部署的完整链路。
  - 强调“大规模、严格”的实证研究，为设计原则提供了依据。
  - 但未提供与其他同类3D世界模型（如Vid2Robot、UniPi等）的定量对比，公平性难以全面判断，主要贡献在于建立了一条新的路线（点云点流预测）。

## 6. 主要结论与发现

- PointWorld 能够从野外 RGB-D 图像零样本生成3D动力学预测，无需任务特定的演示或训练。
- 基于 PointWorld 的模型预测控制可以在真实机器人上泛化到多样化的物体和环境。
- 大规模数据集和系统性设计原则表明：点云空间中的点流预测是可扩展、有效的3D世界模型范式。
- 设计的模型在短时动力学预测中表现准确，为野外操作提供了可靠基础。

## 7. 优点

- **统一空间域**：将状态和动作都表示为3D点云，避免了2D到3D的投影损失，且动作自然融入场景几何。
- **零样本迁移**：仅需RGB-D输入即可对新场景进行动力学预测，无需额外微调。
- **大规模数据驱动**：构建了目前最大规模的机器人操作动力学数据集之一（200万轨迹），支撑了模型的可扩展性研究。
- **系统性的设计原则提炼**：通过多维度消融，为后续3D世界模型研究提供了具体指导。

## 8. 不足与局限

- **对输入传感的依赖**：必须要有高质量的RGB-D图像，在光线差、纹理少或传感器噪声大的场景可能受限。
- **预测时间窗口有限**：只预测短时间内的点流，长期动力学预测需要循环或外推，可能累积误差。
- **动作空间限制**：未明确说明是否支持连续、复杂接触的操作（如高精度的力控）。
- **实验对比不够全面**：缺少与2D视频预测模型（如VideoGPT、VQ-VAE）或基于隐式场的3D动力学模型的直接量化比较，难以衡量其相对优势的绝对值。
- **计算开销**：点云处理和大规模模型推理对算力需求可能较高，文中未提供实时性分析。

（完）
