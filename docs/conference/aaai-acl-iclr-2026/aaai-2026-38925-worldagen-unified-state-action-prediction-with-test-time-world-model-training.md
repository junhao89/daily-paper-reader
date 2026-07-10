---
title: "WorldAgen: Unified State-Action Prediction with Test-Time World Model Training"
title_zh: WorldAgen：通过测试时世界模型训练实现统一的状态-动作预测
authors: "Chi Wan, Kangrui Wang, Yuan Si, Pingyue Zhang, Manling Li"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38925/42887"
tags: ["query:wam-vla"]
score: 9.0
evidence: 统一的VLA和世界模型，支持测试时适应
tldr: 本文提出WorldAgen，一个统一的框架，结合世界模型和动作预测于单个Transformer中，支持测试时训练以快速适应新环境。与现有VLA方法相比，它能够泛化到未见过的动力学变化场景。实验表明，测试时训练显著提升了在分布外环境中的性能，为VLA模型的环境适应提供了有效途径。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38925/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1839, \"height\": 1866, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38925/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1541, \"height\": 808, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38925/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 753, \"height\": 799, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38925/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 882, \"height\": 390, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38925/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1673, \"height\": 952, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38925/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 745, \"height\": 283, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38925/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 454, \"height\": 303, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38925/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 646, \"height\": 265, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38925/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 735, \"height\": 181, \"label\": \"Table\"}]"
motivation: VLA模型在环境动力学变化时泛化失败。
method: 共享Transformer骨干，双头结构（世界模型头+动作预测头），测试时训练。
result: 在多种环境变化下性能优于基线。
conclusion: 测试时世界模型训练是VLA模型适应新环境的关键。
---

## Abstract
How can vision-language-action (VLA) models adapt to new environments where world dynamics shift?
While recent research has combined world modeling and action prediction to improve VLA performance, existing methods largely rely on pretraining in static datasets, without mechanisms for active adaptation to new environments. As a result, these models often fail to generalize when deployed in unseen scenarios with novel object configurations or dynamics.

We present WorldAgen, a unified framework that jointly learns world modeling and action prediction while enabling test-time training (TTT) to adapt to new environments. WorldAgen employs a shared Transformer backbone with two heads: (1) a world-model head that predicts future states from past state-action trajectories, and (2) an agent-model head that predicts actions conditioned on task instructions. During test time, WorldAgen samples exploratory actions, collects ground-truth state transitions, and performs lightweight TTT updates to refine its world model. This adaptation improves the model's understanding to the environments and leads to more accurate action predictions.

Experiments on the CALVIN and LIBERO benchmarks demonstrate that our baseline model achieves comparable, and in some cases superior, performance to current state-of-the-art approaches. Moreover, with TTT on a small number of samples, our method surpasses existing state-of-the-art models, highlighting the effectiveness of adapting world models at inference time.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前的视觉-语言-动作（VLA）模型在预训练时使用静态数据集，一旦部署到具有新物体配置或动力学变化的新环境中，无法主动适应世界动态的分布偏移，导致泛化失败。
- **整体含义**：论文将世界模型从被动的预训练目标转变为主动的测试时适应机制，通过在线收集探索数据并微调世界模型，使VLA模型能够在推理时快速调整对环境的理解，从而提升鲁棒性和泛化能力。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：构建一个统一的Transformer框架，同时学习世界建模（预测未来状态）和动作预测（根据指令预测动作），并在测试时通过轻量级的LoRA微调更新世界模型，以在线适应新环境。
- **关键技术细节**：
  - **联合状态-动作预测架构**：共享一个Transformer骨干，配备两个头部：世界模型头（预测未来观测）和智能体模型头（预测动作）。任务指令仅输入到智能体模型头，世界模型头为任务无关。
  - **混合单向注意力掩码（Mixed Unidirectional Attention Mask）**：包含局部掩码（防止当前单元内动作块attending到占位符）和全局掩码（使世界模型头不可见任务指令），避免信息泄露并实现因果预测。
  - **轨迹分割与动作分块**：将原始轨迹划分为轨迹单元，每个单元包含观测块（长度n）、状态块（n）、动作块（长度m），通过均匀子采样减少冗余视觉信息。
  - **两步推理**：测试时先由智能体模型预测动作块，再由世界模型基于更新后的动作块预测下一个观测块，交替执行。
  - **测试时训练（TTT）**：
    - Stage 1：在新环境中随机探索，收集无标签轨迹，并用“no-lang”标记替换语言指令。
    - Stage 2：只对共享骨干应用LoRA微调，优化世界模型的观测预测损失\( \mathcal{L}_o \)，智能体模型头保持冻结。更新后，共享表示被改进，间接提升动作预测性能。

### 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集**：
  - **CALVIN**：开源模拟基准，用于长期语言条件机器人操作任务。
  - **LIBERO**：终身学习基准，强调多样化任务间的知识迁移。
- **Benchmark**：两个基准均要求模型连续完成多个子任务，评价指标为成功率（%）或平均成功长度。
- **对比方法**：
  - CALVIN上：RoboFlamingo、SuSIE、GR-1、3D Diffusor Actor、CLOVER、Seer（含Large版本）。
  - LIBERO上：MT-ACT、MVP、MPL、OpenVLA、Seer。
- **自身变体**：WorldAgen（基线）与WorldAgen-TTT（应用测试时训练）。

### 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）

- 文中明确提到：TTT阶段（随机探索+LoRA微调）可在单张RTX 4090 GPU上运行，CALVIN每个任务约8分钟，LIBERO每个场景约2分钟。
- 未明确说明预训练阶段的GPU型号、数量或时长，也未详细说明基线模型的训练资源。

### 5. 实验数量与充分性：大概做了多少组实验，这些实验是否充分、是否客观、公平

- **主要实验**：在CALVIN（表1）和LIBERO（表2）上各有一组与多个SOTA方法的全面对比。
- **消融/分析实验**：
  - 世界建模能力（表3）：对比有无图像预测，验证世界建模对策略学习的贡献。
  - LoRA秩（表4）：对比秩16/32/64/128/256的影响。
  - TTT数据量（表5）：6/90/204/340样本的效果。
  - 噪声鲁棒性（表6）：在观测上添加高斯噪声模拟真实条件。
- **充分性评价**：
  - 实验覆盖了两个主流基准、多个SOTA基线，以及关键的消融分析，较为充分。
  - 消融实验控制变量合理（固定学习率、数据量等），结论明确。
  - 但未包含真实机器人实验（仅在模拟器加噪声模拟），也未对更大模型规模或不同骨干网络进行对比。

### 6. 论文的主要结论与发现

- 联合世界建模+动作预测的共享架构能提升动作预测性能（在CALVIN上平均成功长度从2.96提升至3.87，LIBERO成功率从46.5%提升至78.0%）。
- 测试时训练（TTT）通过少量探索数据和轻量级LoRA微调，能在推理时进一步改善环境理解，超越现有SOTA（CALVIN上ASL达3.93，LIBERO上平均成功率79.0%）。
- TTT性能对LoRA秩不敏感（表4），但需要适当的数据量（表5：204样本效果最佳，过多可能过拟合）。
- 在添加噪声的模拟实验中，TTT仍优于基线，显示鲁棒性。

### 7. 优点：方法或实验设计上有哪些亮点

- **方法创新**：将测试时训练引入VLA领域，将世界模型从静态监督信号转变为在线适应机制，思路新颖。
- **架构优雅**：通过混合注意力掩码统一两个任务，避免信息泄漏，同时保持共享表示。
- **高效适应**：仅用少量探索数据（约200个样本）和轻量级LoRA微调（8分钟/任务）即可提升性能，实用价值高。
- **实验全面**：覆盖两种不同难度的基准、多种SOTA方法，并进行了多项消融分析，验证了每个组件的贡献。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **模型规模单一**：仅评估了一种模型大小和骨干（Qwen3），未探索更大模型或不同架构的泛化性。
- **TTT仅针对世界模型**：未来可尝试联合微调动作预测头，或者设计更复杂的适应策略。
- **模拟环境局限性**：仅在模拟器中进行实验，加噪声模拟真实条件但未在真实机器人上验证；真实世界的传感器噪声、延迟、非凸等问题可能带来额外挑战。
- **TTT数据收集成本**：虽然样本量小，但仍需要环境交互，可能在某些高成本或危险场景中受限。
- **过拟合风险**：当TTT数据量过大时（表5中340样本），性能略有下降，表明需要谨慎选择数据预算。

（完）
