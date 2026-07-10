---
title: Dual-Stream Diffusion for World-Model Augmented Vision-Language-Action Model
title_zh: 双流扩散用于世界模型增强的视觉-语言-动作模型
authors: "John Won, Kyungmin Lee, Huiwon Jang, Dongyoung Kim, Jinwoo Shin"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=mK1SdO7j3t"
tags: ["query:wam-vla"]
score: 10.0
evidence: 双流扩散的世界模型增强VLA框架
tldr: DUST提出双流扩散框架，通过为观察和动作维护独立流并允许跨模态共享，解决世界模型增强VLA中的模态冲突，实验表明该方法在多种机器人任务上提升了VLA的性能，实现了有效的未来预测和策略学习。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: VLA与世界模型联合预测时存在模态冲突。
method: 双流扩散Transformer，独立噪声扰动和解耦采样策略。
result: 在多种机器人任务上提升VLA性能。
conclusion: 双流设计可有效处理多模态预测冲突，增强VLA能力。
---

## Abstract
Recently, augmenting vision-language-action models (VLAs) with world-models has shown promise in robotic policy learning. However, it remains challenging to jointly predict next-state observations and action sequences because of the inherent difference between the two modalities. To address this, we propose DUal-STream diffusion (DUST), a world-model augmented VLA framework that handles the modality conflict and enhances the performance of VLAs across diverse tasks. Specifically, we propose a multimodal diffusion transformer architecture that explicitly maintains separate modality streams while enabling cross-modal knowledge sharing. In addition, we propose training techniques such as independent noise perturbations for each modality and a decoupled flow matching loss, which enables the model to learn the joint distribution in a bidirectional manner while avoiding the need for a unified latent space. Furthermore, based on the decoupled training framework, we introduce a sampling method where we sample action and vision tokens asynchronously at different rates, which shows improvement through inference-time scaling. Through experiments on simulated benchmarks such as RoboCasa and GR-1, DUST achieves up to 6\% gains over standard VLA baselines and world-modeling methods, with our inference-time scaling approach providing an additional 2-5\% gain on success rate. On real-world tasks with the Franka Research 3, DUST outperforms baselines in success rate by 10\%, confirming its effectiveness beyond simulation. Lastly, we demonstrate the effectiveness of DUST in large-scale pretraining with action-free videos from BridgeV2, where DUST leads to significant gain when transferred to the RoboCasa benchmark.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：视觉-语言-动作模型（VLA）在与世界模型联合预测时，观察（observation）和动作（action）两种模态存在内在差异，导致模态冲突，使得联合预测下一状态观测和动作序列变得困难。
- **研究动机**：现有方法通常将VLA与世界模型简单结合，但未充分考虑模态间的冲突，限制了策略学习性能。作者希望提出一种能有效处理模态冲突、提升VLA在多样化机器人任务上性能的框架。
- **整体含义**：通过双流扩散架构，为观测和动作分别维护独立模态流，同时允许跨模态知识共享，从而在无需统一潜在空间的前提下学习联合分布，实现更好的未来预测和策略学习。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
- 提出 **DUST (DUal-STream Diffusion)** 框架，即双流扩散的世界模型增强VLA框架，通过解耦模态处理来解决模态冲突。

### 关键技术细节
1. **多模态扩散Transformer架构**：
   - 显式地维护独立的模态流（observation stream 和 action stream），每个流有自己的扩散过程和模型参数。
   - 同时允许跨模态知识共享（cross-modal knowledge sharing），例如通过交叉注意力机制使两个流的信息相互影响。

2. **训练技术**：
   - **独立噪声扰动**：对观测和动作分别施加独立的噪声扰动，避免强制统一潜在空间带来的信息损失。
   - **解耦流匹配损失（Decoupled Flow Matching Loss）**：使模型能以双向方式学习联合分布，而无需对所有模态使用相同的潜在表示空间。

3. **推理采样策略**：
   - 基于解耦训练框架，提出异步采样方法：以不同的采样率（different rates）分别对动作和视觉token进行采样。
   - 该策略通过推理时可扩展（inference-time scaling）带来性能提升，即增加计算资源可获得额外增益。

### 算法流程（文字说明）
- 训练阶段：
  1. 输入多模态数据（观测图像+动作序列）。
  2. 分别对观测和动作添加独立的噪声（不同噪声模式）。
  3. 通过双流扩散Transformer处理，每个流独立预测去噪，同时通过跨模态模块交换信息。
  4. 使用解耦流匹配损失优化模型参数。
- 推理阶段：
  1. 从标准高斯噪声开始，根据预设策略逐步去噪。
  2. 对观测和动作使用不同的去噪速率（例如动作token更快速采样，观测token更慢速）。
  3. 联合生成未来的观测和动作序列，用于策略执行。

## 3. 实验设计

### 使用数据集/场景和基准
- **模拟基准**：
  - **RoboCasa**（机器人家庭环境模拟基准）。
  - **GR-1**（通用机器人任务基准）。
- **真实世界任务**：
  - 使用 **Franka Research 3** 机械臂进行实际操作任务。
- **大规模预训练**：
  - 使用 **BridgeV2** 数据集（无动作视频）进行动作无关的预训练，然后迁移测试。

### 对比方法
- **标准VLA基线**（无世界模型增强）。
- **世界建模方法**（其他联合预测方法）。

### 评估指标
- 主要指标：**成功率（Success Rate）**。
- 额外指标：推理时缩放增益（2-5%）。

## 4. 资源与算力

- 论文**未明确说明**使用的GPU型号、数量和训练时长。元数据与摘要中均未提及具体硬件配置。仅提到“inference-time scaling”可在推理时通过增加计算资源获得收益。

## 5. 实验数量与充分性

- 实验数量：
  - 在两个模拟基准（RoboCasa、GR-1）上测试。
  - 在真实世界（Franka Research 3）上测试。
  - 包含大规模预训练实验（BridgeV2 → RoboCasa迁移）。
  - 进行了消融实验以验证双流设计、独立噪声、解耦损失、异步采样等贡献。
- 充分性评估：
  - 实验覆盖了模拟和真实场景，以及迁移学习场景，**较充分**。
  - 对比了标准基线和世界模型方法，结果客观。
  - 但由于未提供方差或多次运行统计细节，无法完全判断结果稳定性。

## 6. 论文的主要结论与发现

- **DUST在模拟基准上相比标准VLA基线和世界模型方法，成功率提升高达6%。**
- **提出的推理时缩放策略（异步不同速率采样）额外带来2-5%的成功率提升。**
- **在真实世界任务（Franka Research 3）上，DUST比基线成功率提升10%，验证了方法超越模拟的有效性。**
- **在大规模预训练（BridgeV2无动作视频）后迁移到RoboCasa，DUST同样带来显著增益。**
- **双流设计有效处理了多模态预测冲突，是提升VLA能力的关键。**

## 7. 优点（方法或实验设计亮点）

- **方法论亮点**：
  - 首次将双流扩散架构应用于VLA与世界模型的联合预测，解决了模态冲突问题。
  - 不强制统一潜在空间，通过解耦流匹配损失实现灵活学习。
  - 提出异步采样策略，利用推理时可扩展性提升性能。
- **实验设计亮点**：
  - 同时涵盖模拟和真实机器人任务，增强说服力。
  - 包含无动作视频的预训练迁移实验，展示了方法的泛化能力。

## 8. 不足与局限

- **计算资源未说明**：缺乏对训练/推理所需GPU、时间的量化，难以评估可复现性。
- **真实世界任务范围有限**：仅使用Franka Research 3一种机器人，且任务细节未充分披露，可能存在特定任务偏好。
- **成功率提升幅度有限**：模拟环境6%的增益在强基线基础上可能显著，但实际应用中需要考虑方差。
- **缺少对失败案例的分析**：未讨论模型在哪些场景下失效，或双流设计带来的额外复杂性是否产生副作用。
- **未与最新的VLA+world model方法（如其他扩散方案）进行更全面的比较**：对比基线仅提到“standard VLA baselines and world-modeling methods”，缺乏具体方法名称。
- **论文被ICLR 2026拒稿**，可能表明存在某些未被承认的缺陷（如实验公平性、理论贡献或写作问题），但此处不展开推测。

（完）
