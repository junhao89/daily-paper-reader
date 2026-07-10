---
title: Towards Efficient and Robust Manipulation via Multi-Frame Vision-Language-Action Modeling
title_zh: 高效鲁棒操作的多帧视觉-语言-动作建模
authors: "Hao Li, Shuai Yang, Yilun Chen, Xinyi Chen, Xiaoda Yang, Yang Tian, Hanqing Wang, Tai Wang, Dahua Lin, Feng Zhao, Jiangmiao Pang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38903/42865"
tags: ["query:latent-vla"]
score: 8.0
evidence: 多帧视觉-语言-动作建模实现高效操作
tldr: 现有VLA模型限于单帧图像，无法充分利用多帧时间信息。CronusVLA提出两阶段框架：首先在大规模数据集上单帧预训练并自回归预测动作令牌，然后通过高效令牌融合扩展为多帧模型。该方法在保持计算效率的同时有效利用历史帧，在机器人操作基准上取得更高成功率和鲁棒性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38903/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 879, \"height\": 591, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38903/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1803, \"height\": 598, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38903/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 836, \"height\": 634, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38903/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1836, \"height\": 927, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38903/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 841, \"height\": 513, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38903/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 704, \"height\": 431, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38903/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1636, \"height\": 706, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38903/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 876, \"height\": 687, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38903/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 869, \"height\": 242, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38903/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 780, \"height\": 300, \"label\": \"Table\"}]"
motivation: 单帧VLA模型忽略时间信息，直接多帧输入计算开销大。
method: 提出两阶段框架，先单帧预训练再通过令牌融合扩展为多帧。
result: 在多项操作任务上取得更高成功率和鲁棒性。
conclusion: 多帧时间信息对VLA模型性能提升至关重要。
---

## Abstract
Recent vision-language-action (VLA) models built on pretrained vision-language models (VLMs) have demonstrated strong performance in robotic manipulation. However, these models remain constrained by the single-frame image paradigm and fail to fully leverage the temporal information offered by multi-frame histories, as directly feeding multiple frames into VLM backbones incurs substantial computational overhead and inference latency. We propose CronusVLA, a unified framework that extends single-frame VLA models to the multi-frame paradigm. CronusVLA follows a two-stage process: (1) Single-frame pretraining on large-scale embodied datasets with autoregressive prediction of action tokens, establishing an effective embodied vision-language foundation; (2) Multi-frame post-training, which adapts the prediction of the vision-language backbone from discrete tokens to learnable features, and aggregates historical information via feature chunking. CronusVLA effectively addresses the existing challenges of multi-frame modeling while enhancing performance. To evaluate the robustness under temporal and spatial disturbances, we introduce SimplerEnv-OR, a novel benchmark featuring 24 types of observational disturbances and 120 severity levels. Experiments across three embodiments in simulated and real-world environments demonstrate that CronusVLA achieves leading performance and superior robustness, with a 70.9% success rate on SimplerEnv, a 26.8% improvement over OpenVLA on LIBERO, and the highest robustness score on SimplerEnv-OR, showing the promise of efficient multi-frame adaptation for real-world VLA deployment.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

当前视觉-语言-动作（VLA）模型通常基于预训练视觉语言模型（VLM），但主要采用单帧图像范式，无法充分利用多帧历史观测中的时间信息。直接向VLM骨干网络输入多帧图像会导致自注意力计算量随帧数平方增长，带来巨大的计算开销和推理延迟，限制了实际部署。现有低层策略（如扩散策略）已证明多帧历史可以提升性能和鲁棒性，但VLA大模型缺乏高效的多帧扩展方法。本文提出CronusVLA，旨在将单帧VLA模型高效扩展为多帧范式，同时保持快速推理、高性能和强鲁棒性。

## 2. 方法论：核心思想与关键技术细节

CronusVLA 采用两阶段训练策略：

- **第一阶段：单帧预训练（Single-frame Pretraining）**  
  在大型异构操作数据集（OXE）上，使用自回归预测离散动作令牌的方式训练基础单帧VLA模型。输入当前帧图像和语言指令，模型经VLM骨干处理后预测离散动作令牌，再解令牌获得连续动作。这一步建立了有效的具身视觉语言基础，并保持了对单帧视觉感知的保留。

- **第二阶段：多帧后训练（Multi-frame Post-training）**  
  核心思想是将离散动作令牌替换为连续可学习特征，并通过特征分块（feature chunking）聚合历史信息。具体包括：
  - **从离散令牌到特征分块**：在VLM骨干的隐藏层引入可学习特征 \( f_t \in \mathbb{R}^d \)，作为连续表示。多帧特征分块 \( F_t^M = \{f_{t-M+1}, \dots, f_t\} \) 在特征层面表示多帧图像。训练时，在批级别重排 \( M \) 步图像输入，VLM骨干独立处理 \( B \times M \) 个单帧输入。推理时，使用先进先出队列缓存历史特征，避免重复计算，显著提升速度。
  - **跨帧解码器（Cross-frame Decoder）**：基于DiT（扩散Transformer）构建的解码器，包含自注意力网络和MLP层，以特征分块为条件进行扩散去噪，预测动作块 \( a_{t:t+K-1} \)。引入了特征调制器（feature modulator），通过通道拆分和MLP动态调制当前特征与历史特征的贡献，避免历史信息主导当前关键信息。采用交叉注意力机制，让含噪动作作为查询，调制后的特征作为键值，有效交互。
  - **多帧正则化（Multi-frame Regularization）**：对历史特征施加停止梯度操作，使其梯度不更新VLM骨干，只作为解码器的辅助输入，从而保持VLM骨干的单帧感知能力，减少计算和内存开销，促进快速收敛。扩散损失为：
    \[
    \mathcal{L} = \mathbb{E}_{\epsilon \sim \mathcal{N}(0,I), i} \left[ \| \hat{\epsilon}_i - \epsilon_\theta(t, \hat{f}_{t-M+1:t-1}, f_t) \|^2 \right]
    \]
    其中 \( \hat{f}_{t-M+1:t-1} = \text{sg}(VL(I_{t-k}, l)) \)。

## 3. 实验设计

- **数据集与场景**：
  - 预训练使用 Open X-Embodiment (OXE) 数据集。
  - 多帧后训练使用 Bridge-v2 和 Fractal 数据集（约148k episodes，5M多帧剪辑）。
  - 评估在 SimplerEnv（WidowX Robot 和 Google Robot 环境）、LIBERO 基准上进行。
  - 额外提出 SimplerEnv-OR 基准，包含24种观测扰动类型和120级严重性，涵盖时空维度（全局、局部、离散扰动及不同频率）。
  - 真实世界实验在 Franka Research 3 机器人上进行，包含简单任务、长程任务、泛化与观测鲁棒性任务。

- **对比方法**：
  - 在 SimplerEnv 上对比 RT-1-X, RT-2-X, Octo-Base, RoboVLMs, SpatialVLA, π0, π0-FAST, GR00T-N1.5, OpenVLA, CogACT, TraceVLA, Magma 等。
  - 在 LIBERO 上对比 Dita, OpenVLA, TraceVLA, SpatialVLA, π0, π0-FAST, GR00T-N1, π0.5 等。
  - 在 SimplerEnv-OR 上对比 π0, TraceVLA, RoboVLMs, SpatialVLA, CogACT。
  - 真实世界对比 DP3 和 OpenVLA。

- **评估指标**：成功率（SR）和鲁棒性分数（R-Score）。

## 4. 资源与算力

论文未明确说明训练所使用的GPU型号、数量及训练时长。仅在致谢中提到获得国家重点项目和上海人工智能实验室的支持，以及使用GPU集群。没有具体的算力细节。

## 5. 实验数量与充分性

- 共进行大量实验：在 SimplerEnv 上评估了12个任务（Google Robot 两个设置，WidowX Robot 一个设置）；在 LIBERO 上评估4个任务套件；在 SimplerEnv-OR 上评估时空两个维度共24种扰动类型、120级严重性（2300个试验）；真实世界3个任务套件各50个episode。
- 消融实验包括：后训练策略消融（4种变体对比）、跨帧解码器设计消融（无交叉注意力、无调制器、MLP解码器、SiT解码器）、帧数影响分析（CronusVLA 7B/0.5B与基线对比不同帧数）。
- **充分性评价**：实验覆盖了模拟和真实环境，多种机器人和任务，对比了多种基线（包括同样是多帧的方法），消融实验设计合理，验证了各组件有效性。但缺少算力效率的量化比较（如训练速度、内存占用等），以及更大规模真实世界实验的泛化能力测试。

## 6. 主要结论与发现

- CronusVLA 在 SimplerEnv 上达到70.9%平均成功率，优于此前所有方法（如 CogACT 63.8%）。
- 在 LIBERO 上达到97.0%平均成功率，比 OpenVLA 提升26.8%。
- 在 SimplerEnv-OR 上，CronusVLA 在时空扰动下均取得最高鲁棒性分数和成功率，如常时扰动（1:0）下仍保持61.2 R-Score，稀疏扰动（1:3）下达96.2 R-Score。
- 多帧建模显著提升了长程任务和鲁棒性，而特征分块和队列缓存机制使推理速度保持在较高水平（8.73 Hz vs 基线直接输入多帧的3.09 Hz）。
- 帧数并非越多越好，CronusVLA 7B 以7帧最佳，0.5B以4帧最佳。

## 7. 优点

- **方法创新**：提出两阶段训练（单帧预训练+多帧后训练），巧妙利用单帧预训练模型的视觉语言基础，通过特征分块和停止梯度正则化实现高效多帧扩展，避免直接多帧输入的计算爆炸。
- **推理高效**：特征分块与队列缓存使得推理时不重复计算历史帧的VLM编码，速度远快于直接拼接多帧的方案。
- **鲁棒性突出**：设计了 SimplerEnv-OR 基准，系统评估观测扰动下的鲁棒性，实验证明多帧信息能有效抵御单帧被污染的情况。
- **性能领先**：在多个模拟基准和真实场景中取得SOTA，且在较小模型（0.5B）上也能超越许多大模型。

## 8. 不足与局限

- **实验覆盖**：真实世界实验仅使用 Franka 机器人，且任务种类有限（未涉及更复杂操作如装配、移动操控等），泛化能力有待进一步验证。
- **算力信息缺失**：未提供训练所需的GPU型号、数量和时间，使得可重复性和成本评估困难。
- **基线比较的公平性**：部分基线（如π0, π0-FAST）使用了额外腕部图像和机器人状态输入，而CronusVLA在主要实验中使用第三视角图像和文本指令，可能处于不同条件。虽然在 LIBERO 上添加了腕部图像，但未明确说明对最终性能的影响。
- **消融实验范围**：缺少对特征分块大小（帧数）与动作块长度的联合搜索，以及对不同数据集（如包含随机噪声的视频）的迁移实验。
- **局限性讨论**：论文未讨论在缺乏高质量多帧数据情况下的适用性，以及模型在分布外场景（如全新机械臂构型）下的表现。

（完）
