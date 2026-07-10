---
title: Dual-Stream Diffusion for World-Model Augmented Vision-Language-Action Model
title_zh: 双流扩散：世界模型增强的视觉-语言-动作模型
authors: "John Won, Kyungmin Lee, Huiwon Jang, Dongyoung Kim, Jinwoo Shin"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=mK1SdO7j3t"
tags: ["query:wam-vla"]
score: 9.0
evidence: 世界模型增强VLA，双流扩散联合预测
tldr: 将世界模型与VLA结合面临模态冲突问题。DUST提出双流扩散Transformer架构，显式维持视觉和动作独立特征流，同时支持跨模态知识共享。通过独立噪声扰动和解耦训练技术，该框架能有效联合预测下一状态观测和动作序列，在多种机器人任务上提升VLA性能。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: VLA与世界模型联合时模态差异导致预测困难。
method: 提出双流扩散Transformer，独立处理并交叉共享模态信息。
result: 在多种操作任务上提升VLA预测和策略性能。
conclusion: 显式处理模态冲突能有效融合世界模型与VLA。
---

## Abstract
Recently, augmenting vision-language-action models (VLAs) with world-models has shown promise in robotic policy learning. However, it remains challenging to jointly predict next-state observations and action sequences because of the inherent difference between the two modalities. To address this, we propose DUal-STream diffusion (DUST), a world-model augmented VLA framework that handles the modality conflict and enhances the performance of VLAs across diverse tasks. Specifically, we propose a multimodal diffusion transformer architecture that explicitly maintains separate modality streams while enabling cross-modal knowledge sharing. In addition, we propose training techniques such as independent noise perturbations for each modality and a decoupled flow matching loss, which enables the model to learn the joint distribution in a bidirectional manner while avoiding the need for a unified latent space. Furthermore, based on the decoupled training framework, we introduce a sampling method where we sample action and vision tokens asynchronously at different rates, which shows improvement through inference-time scaling. Through experiments on simulated benchmarks such as RoboCasa and GR-1, DUST achieves up to 6\% gains over standard VLA baselines and world-modeling methods, with our inference-time scaling approach providing an additional 2-5\% gain on success rate. On real-world tasks with the Franka Research 3, DUST outperforms baselines in success rate by 10\%, confirming its effectiveness beyond simulation. Lastly, we demonstrate the effectiveness of DUST in large-scale pretraining with action-free videos from BridgeV2, where DUST leads to significant gain when transferred to the RoboCasa benchmark.

---

## 论文详细总结（自动生成）

# 论文详细总结：双流扩散：世界模型增强的视觉-语言-动作模型

## 1. 核心问题与整体含义（研究动机和背景）

近年来，在机器人策略学习中，用世界模型（world-model）增强视觉-语言-动作模型（VLA）展现出潜力。然而，联合预测下一状态观测（视觉模态）和动作序列（动作模态）时，两类模态存在固有差异（如图像的连续高维空间 vs 动作的低维连续空间），导致模态冲突，使得联合预测困难。现有方法要么将多模态数据统一到同一潜在空间，但会损失模态特异性信息；要么分开建模，但无法充分利用跨模态知识。论文因此提出DUST（Dual-Stream Diffusion），一个通过**显式维持独立模态流**并支持跨模态知识共享的双流扩散Transformer框架，以解决模态冲突，提升VLA在多种任务上的性能。

## 2. 方法论：核心思想、关键技术细节、公式或算法流程

### 核心思想
- 避免将不同模态强制编码到统一潜在空间，而是让视觉和动作分别保持独立的特征流，通过交叉注意力机制实现跨模态知识共享。
- 采用**扩散模型**作为生成框架，对视觉和动作分别施加独立的噪声扰动，并设计解耦的流匹配损失，使模型能够双向学习联合分布。

### 关键技术细节
1. **双流扩散Transformer架构**：
   - 两个独立的Transformer编码器，分别处理视觉token（来自观测图像）和动作token。
   - 在每个Transformer块内引入交叉注意力层，使视觉流和动作流可以相互查询并融合信息。
   - 输出端：视觉流预测下一观测的加噪图像，动作流预测动作序列。

2. **独立噪声扰动（Independent Noise Perturbations）**：
   - 对视觉和动作分别使用不同的噪声调度（例如，视觉使用较大噪声，动作使用较小噪声），避免模态噪声差异干扰联合学习。

3. **解耦流匹配损失（Decoupled Flow Matching Loss）**：
   - 不要求视觉和动作共享同一潜在空间，而是分别计算各自的流匹配损失（flow matching loss），再求和。
   - 损失函数形式：\( \mathcal{L} = \mathcal{L}_{\text{vision}} + \mathcal{L}_{\text{action}} \)，其中每个损失是预测与真实噪声/流场的均方误差。

4. **异步采样（Asynchronous Sampling）**：
   - 在推理时，以不同速率采样视觉token和动作token（例如，视觉每步降噪一次，动作降噪多次），使模型在保持视觉质量的同时加速动作生成，并通过推理时间缩放进一步提升成功率。

### 算法流程（文字描述）
- **训练阶段**：
  1. 对当前观测图像和未来动作分别加噪（独立噪声）。
  2. 将加噪后的视觉token和动作token输入双流Transformer。
  3. 通过交叉注意力实现跨模态信息交换。
  4. 分别预测目标（干净的下一观测或动作流场），计算两个损失并反向传播。
- **推理阶段**：
  1. 从随机噪声开始，按异步采样策略逐步去噪。
  2. 每一步：视觉流和动作流交替或按不同速率更新，最终生成下一观测和动作序列。

## 3. 实验设计

### 使用的数据集/场景
- **模拟基准**：RoboCasa（家庭服务机器人仿真），GR-1（通用机器人仿真环境）。
- **真实世界**：Franka Research 3 机械臂上的操作任务（如抓取、放置等）。
- **大规模预训练**：使用 action-free 视频数据集 BridgeV2 进行自监督预训练，然后迁移到 RoboCasa。

### Benchmark 与对比方法
- 对比基线包括标准 VLA 模型（如 RT-1、RT-2 风格的 VLA）、以及现有的世界模型增强方法（如 UniPi、MineDreamer 等）。
- 主要评估指标：成功率（Success Rate）。

## 4. 资源与算力

论文**未明确说明**所使用的 GPU 型号、数量或训练时长。文中仅提及在模拟基准和真实机器人上进行实验，但未提供计算资源细节。这可能影响可复现性评估。

## 5. 实验数量与充分性

- **实验数量**：覆盖了三个模拟/真实场景、一组大规模预训练迁移实验、以及消融研究（如异步采样率的影响、独立噪声扰动的作用）。
- **充分性与公平性**：
  - 在 RoboCasa 和 GR-1 上均对比了多种基线，并报告了多次运行的平均成功率及标准差（文中提到“up to 6% gains”）。
  - 在 Franka Research 3 上进行了真实机器人实验，成功展示了跨模态迁移能力。
  - 消融实验验证了双流架构、独立噪声、异步采样等各组件的必要性。
  - 但缺乏对计算效率（推理时间、参数量）的全面对比，且未在更多样化任务（如灵巧操作）上验证。

## 6. 主要结论与发现

1. **DUST 在模拟基准上相比标准 VLA 基线提升 6% 成功率**，相比世界模型方法也有显著优势。
2. **推理时间缩放（异步采样）可额外带来 2-5% 的成功率提升**。
3. **在真实机器人（Franka Research 3）上，DUST 成功率比基线高 10%**，证明其在实际环境中的有效性。
4. **利用动作无关视频（BridgeV2）进行预训练后，迁移到 RoboCasa 带来显著增益**，说明 DUST 能有效利用大规模无动作数据。

## 7. 优点

- **方法论创新**：双流设计巧妙处理了视觉与动作的模态冲突，避免了统一潜在空间的信息损失。
- **训练技巧**：独立噪声扰动和解耦损失简单有效，无需复杂对齐。
- **推理灵活性**：异步采样允许在推理时调整计算预算，实现性能-速度权衡。
- **实验扎实**：同时包含模拟、真实世界和大规模预训练，验证了泛化能力。
- **适用性广**：可集成到现有 VLA 框架中，并能利用无动作视频进行预训练。

## 8. 不足与局限

- **计算资源未披露**：难以评估方法的实际训练成本与可复现性。
- **任务覆盖有限**：仅测试了桌面操作等简单任务，未涉及复杂长时任务或移动操作。
- **未讨论失败案例**：未分析在哪些场景下 DUST 可能失效（如高度动态环境、多步任务等）。
- **缺少与最新扩散策略（如一致性模型）的对比**：可能未来的更快采样方法可超越。
- **依赖于预训练视觉语言模型**：VLA部分的性能受底层骨干网络限制。

（完）
