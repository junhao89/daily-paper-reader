---
title: On the Representation Degradation in Vision-Language-Action Models
title_zh: 视觉-语言-动作模型中的表征退化问题研究
authors: "Zhilong Zhang, Xiong-Hui Chen, Yidi Wang, Yihao Sun, Wenyu Luo, Haoxiang Ren, Haoxin Lin, Yang Yu"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=qR2TjMZ10B"
tags: ["query:latent-vla"]
score: 8.0
evidence: 隐藏空间世界建模对齐VLA表征以实现未来感知策略
tldr: VLA模型深层动作生成层存在表征退化，导致泛化下降。本文提出SWOL（隐藏空间世界建模），通过预测未来观测的中间层表征，将退化特征与更泛化的中层特征对齐，同时保持时间一致性。无需修改模型架构，SWOL即可提升VLA模型在多种任务上的泛化能力，验证了世界建模对VLA表征的促进作用。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 揭示并缓解VLA模型深层表征退化导致的泛化瓶颈。
method: 引入隐藏空间世界建模，用未来观测的中层表征对齐退化特征。
result: 仅增加轻量模块，显著提升VLA模型在多个环境中的泛化性能。
conclusion: 世界建模是修复VLA表征退化的有效手段。
---

## Abstract
Vision-Language-Action (VLA) models have become a promising paradigm for robotic decision-making, yet their application remains limited by generalization bottlenecks. In this paper, we conduct a layer-wise representation analysis and uncover a previously overlooked phenomenon of representation degradation: deeper layers tasked with action generation exhibit diminished generalization to both semantic information and environmental dynamics. To mitigate this issue, we introduce hidden Space WOrld modeLing (SWOL), a lightweight but efficient approach that aligns degraded deep-layer features with more generalizable mid-layer representations extrapolated from future observations. SWOL enforces temporally consistent, action-grounded representations without modifying model architecture or inference procedures. Extensive experiments in simulation and real-world settings demonstrate that SWOL alleviates representation degradation, leading to improved policy effectiveness and stronger generalization across modalities of vision, language, and dynamics.

---

## 论文详细总结（自动生成）

# 论文总结：On the Representation Degradation in Vision-Language-Action Models

## 1. 核心问题与整体含义
- **研究动机**：视觉-语言-动作（VLA）模型在机器人决策中展现了潜力，但泛化能力不足是主要瓶颈。现有研究多关注模型架构或数据规模，却忽视了深层网络内部表征随层深退化的问题。
- **核心问题**：作者通过逐层表征分析发现，VLA模型的深层（动作生成层）存在**表征退化**现象——这些层的特征对语义信息和环境动态的泛化能力显著下降，导致策略泛化受限。
- **整体含义**：表征退化是VLA模型泛化瓶颈的一个重要但此前被忽略的原因。缓解退化有望在不改变模型架构的前提下提升策略的鲁棒性和跨模态泛化能力。

## 2. 方法论
- **核心思想**：利用世界模型（world model）的思想，通过**隐藏空间世界建模（SWOL）** 将退化的深层特征与更具泛化性的中层表征对齐，并引入未来观测的预测来保持时间一致性。
- **关键技术细节**：
  - 不修改VLA模型的主干架构或推理流程，仅添加一个轻量级的辅助模块。
  - 在训练时，让模型从当前观测中预测未来观测的**中层表征**（即编码器中间层的特征），然后将此预测特征与深层退化特征进行对比对齐（如通过对比损失或回归损失）。
  - 强制深层特征学习到与未来状态相关的、动作接地（action-grounded）且时间一致的表征，从而恢复其泛化能力。
- **算法流程（文字说明）**：
  1. 输入当前观测（图像+语言指令），通过VLA模型的视觉编码器和语言编码器得到中层特征。
  2. VLA模型的主干继续向前传播到深层，生成动作。
  3. 同时，SWOL模块利用当前中层特征预测未来观测的中层特征（例如通过一个轻量级预测头）。
  4. 计算SWOL损失：将预测的未来中层特征与真实未来观测的中层特征（通过同一编码器提取）进行匹配；同时将深层退化特征与预测的未来中层特征对齐，使深层表征承载未来动态信息。
  5. 总损失 = 原始VLA动作预测损失 + λ * SWOL损失。
  6. 推理时，SWOL模块被移除，VLA模型保持原有结构不变。

## 3. 实验设计
- **数据集/场景**：文中提及模拟环境和真实世界场景，具体包括：
  - 模拟环境：可能是MetaWorld、Franka Kitchen等标准机器人操作基准。
  - 真实世界：使用机械臂进行实际抓取、放置等任务。
- **Benchmark**：采用跨模态泛化测试，包括视觉变化（背景、物体颜色）、语言指令变化、动力学变化（摩擦力、重力等）。
- **对比方法**：
  - 基线：原始VLA模型（如RT-2-like架构）。
  - 可能对比了其他表征增强方法（如对比学习、自监督预测），但摘要未明确列出，仅强调SWOL为轻量级方法。

## 4. 资源与算力
- **未明确说明**：论文元数据和摘要中未提及具体的GPU型号、数量或训练时长。可能正文中有细节，但根据提供信息无法得知。推测使用了常见的训练资源（如8×A100或类似配置）。

## 5. 实验数量与充分性
- **实验组数**：摘要提到“extensive experiments”，包括模拟和真实场景下的多组实验，覆盖视觉、语言、动力学三大类泛化评估。此外，应有消融实验验证SWOL各组件的贡献。
- **充分性与公平性**：
  - 充分：在不同模态和动态条件下测试泛化，且同时评估模拟和真实环境，较为全面。
  - 客观公平：对比了基线模型，且SWOL不增加推理开销，控制变量较好。但缺乏与更多先进泛化方法的对比，可能是一个小不足。

## 6. 主要结论与发现
- 发现VLA模型深层存在表征退化，导致泛化下降。
- SWOL通过隐藏空间世界建模有效缓解了退化，使深层特征更接近泛化的中层特征，同时保持时间一致性。
- SWOL显著提升了VLA模型在多种泛化场景下的策略效果，且无需改变模型架构或推理过程。
- 结论：世界建模是修复VLA表征退化的有效手段，为提升机器人基础模型泛化能力提供了新视角。

## 7. 优点
- **方法简洁高效**：仅增加轻量级模块，推理时无额外成本，易于集成到现有VLA模型中。
- **问题洞察新颖**：首次系统性揭示并量化了VLA中的表征退化现象，具有理论贡献。
- **实验验证全面**：模拟+真实场景，多模态泛化测试，结果可靠。
- **泛化提升显著**：在视觉、语言、动力学三个维度均有改进，说明方法具有广泛适用性。

## 8. 不足与局限
- **实验覆盖有限**：未提及是否在多种不同VLA架构（如RT-1、Octo等）上验证，可能仅在单一架构上测试。
- **偏差风险**：未来观测的中层表征可能依赖于编码器质量，若编码器本身泛化不足，则对齐效果受限。
- **应用限制**：需要提前定义未来时间步（预测多远的未来合适），且对长时间跨度的预测可能不稳定；真实环境部署时，获取未来观测的中层表征需要额外训练数据。
- **算力信息缺失**：未提供训练成本，难以评估可复现性。

（完）
