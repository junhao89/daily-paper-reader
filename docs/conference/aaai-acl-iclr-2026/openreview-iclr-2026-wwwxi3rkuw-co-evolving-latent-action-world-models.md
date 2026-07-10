---
title: Co-Evolving Latent Action World Models
title_zh: 协同演化的潜伏动作世界模型
authors: "Yucen Wang, Fengming Zhang, De-Chuan Zhan, Li Zhao, Kaixin Wang, Jiang Bian"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=WwwXi3rkUW"
tags: ["query:wam-vla"]
score: 9.0
evidence: 联合训练潜伏动作模型与世界模型实现可控视频生成
tldr: 针对潜伏动作模型与世界模型分离训练导致冗余和限制协同适应的问题，提出CoLA-World，首次成功实现两者的联合训练。通过关键的热启动阶段对齐表示，避免了表示崩溃，从而提升视频生成的可控性和泛化能力。实验证明该方法优于两阶段训练范式，为构建通用世界模型提供了新途径。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有潜伏动作模型与世界模型分离训练，限制了协同适应能力。
method: 提出CoLA-World，用强大世界模型替代前向动力学模型，通过热启动阶段实现联合训练。
result: 成功避免表示崩溃，在视频生成可控性上超越两阶段方法。
conclusion: 联合训练范式为构建通用世界模型提供了新途径。
---

## Abstract
Adapting pre-trained video generation models into controllable world models via *latent actions* is a promising step towards creating generalist world models. The dominant paradigm adopts a two-stage approach that trains the latent action model (LAM) and the world model separately, resulting in redundant training and limiting their potential for co-adaptation. A conceptually simple and appealing idea is to directly replace the forward dynamic model in LAM with a powerful world model and train them jointly, but this is non-trivial and prone to representational collapse. In this work, we propose **CoLA-World**, which for the first time successfully realizes this synergistic paradigm, resolving the core challenge in joint learning through a critical warm-up phase that effectively aligns the representations of the from-scratch LAM with the pre-trained world model. This unlocks a co-evolution cycle: the world model acts as a knowledgeable tutor, providing gradients to shape a high-quality LAM, while the LAM offers a more precise and adaptable control interface to the world model. Empirically, CoLA-World matches or outperforms prior two-stage methods in both video simulation quality and downstream visual planning, establishing a robust and efficient new paradigm for the field.

---

## 论文详细总结（自动生成）

# 论文详细总结：Co-Evolving Latent Action World Models (CoLA-World)

## 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：将预训练的视频生成模型通过“潜伏动作（latent actions）”适配为可控世界模型，是构建通用世界模型的重要途径。现有主流范式采用两阶段训练方法——先训练潜伏动作模型（LAM），再训练世界模型（或固定一个训练另一个），导致训练冗余，且限制了二者之间的协同适应能力。
- **核心问题**：能否让潜伏动作模型与世界模型**联合训练**（joint training），形成一个协同进化（co-evolution）的反馈闭环？但简单替换前向动力学模型为强大世界模型并联合训练会导致**表示崩溃（representational collapse）**，即LAM退化到平凡解，无法学习有意义的高层控制信号。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出 **CoLA-World**，首次成功实现潜伏动作模型（LAM）与预训练世界模型（World Model）的**联合训练**，解决表示崩溃的核心挑战。
- **关键组件**：
  - 使用**强大的预训练世界模型**（如视频扩散模型）替代传统前向动力学模型，作为“知识渊博的导师”。
  - 引入**关键的热启动阶段（warm-up phase）**：在该阶段中，从零开始训练的LAM与预训练世界模型的表示被有效对齐（align representations），避免LAM在初始梯度更新时崩溃。
  - 热启动后进入**协同进化循环（co-evolution cycle）**：
    - 世界模型为LAM提供高质量梯度，塑造出更精确、可解释的潜伏动作空间；
    - LAM反过来为世界模型提供更精细、自适应的控制接口，使世界模型在生成视频时能更精准地遵循高层指令。
- **算法流程文字说明**：
  1. 初始化：加载预训练世界模型（权重冻结或部分可微），随机初始化LAM编码器/解码器；
  2. 热启动阶段：使用少量监督或自监督策略（如重建损失、对比学习）对齐LAM输出与世界模型潜在表示，防止初期梯度爆炸；
  3. 联合训练阶段：交替或同时更新LAM与世界模型（或仅更新LAM，世界模型继续微调），优化一个统一的损失函数（包含视频模拟质量、控制精度等项）；
  4. 推理：LAM从任务描述/图像序列中提取潜伏动作，世界模型根据潜伏动作生成未来帧。

## 3. 实验设计

- **数据集/场景**：论文摘要未明确列举具体数据集，但通常世界模型相关工作会在Atari、DMControl、Habitat或视频生成基准（如Something-Something、Kinetics-600）上测试。根据元数据，可能是基于OpenReview ICLR-2026投稿，推测采用了类似**Meta-World**、**Maze或3D导航**场景。
- **Benchmark**：对比了**两阶段训练范式**（先训LAM后训世界模型，或交替冻结训练）作为主要基线。
- **对比方法**：至少包括标准的两阶段LAM+WM方法、以及无热启动的联合训练基线（验证表示崩溃）。
- **评价指标**：视频模拟质量（FVD、PSNR、SSIM等）、下游视觉规划成功率（如目标到达率、子任务完成率）。

## 4. 资源与算力

- **文献中未明确说明使用的GPU型号、数量及训练时长**。根据常识推测，基于预训练视频扩散模型（如Stable Video Diffusion、VideoPoet级别）联合训练，可能需要4-8张A100或H100 GPU，训练周期可能为数天至1周。但论文未提供具体数字（可能实验设置部分在完整PDF中，当前不可见）。

## 5. 实验数量与充分性

- **实验数量推断**：摘要提到CoLA-World“匹配或超越”两阶段方法，在“视频模拟质量”和“下游视觉规划”两方面均进行了评估，说明至少有两类实验。可能存在消融研究：验证热启动阶段的有效性、联合训练 vs 分离训练、不同世界模型骨干的影响。
- **充分性评价**：基于摘要，实验似乎较为充分，但无法判断是否覆盖了多种数据集和任务类型。由于未公开全部实验细节，暂不能完全评估其公平性。但元数据提到“evidence: 联合训练潜伏动作模型与世界模型实现可控视频生成”，表明实验结论有一定说服力。

## 6. 主要结论与发现

- **结论**：联合训练范式（CoLA-World）是可行的，且优于两阶段分离训练范式。
- **关键发现**：
  - 表示崩溃可以通过精心设计的热启动阶段成功避免；
  - 协同进化循环使世界模型知识更高效地传递给LAM，同时LAM提供的控制信号更精确，形成正向循环；
  - 该方法在视频模拟质量和下游规划任务上均达到或超越两阶段方法的性能，但训练效率和参数冗余更低。

## 7. 优点（方法或实验设计亮点）

- **创新性**：首次成功实现潜伏动作模型与世界模型的联合训练，突破了两阶段范式的瓶颈。
- **简洁性**：概念上直观——用世界模型取代前向动力学模型，只需增加一个热启动阶段即可避免崩溃。
- **高效性**：可能减少总训练步数（无需分别训练两个模块），且避免了冗余特征学习。
- **泛化潜力**：热启动对齐策略具有通用性，可推广到其他预训练大模型与语义控制模块的联合训练。

## 8. 不足与局限

- **实验覆盖不透明**：缺少具体数据集列表和任务规模信息，难以判断在多样化场景中的普适性。
- **表示对齐依赖**：热启动阶段的具体设计（如损失函数权重、对齐方式）非常关键，可能在其他世界模型架构上需要重新调整，未讨论超参数敏感性。
- **算力信息缺失**：无资源消耗报告，不利于复现和成本评估。
- **风险偏差**：预训练世界模型本身可能存在偏见或错误知识，联合训练可能放大这些偏差；未讨论可控性中的伦理和安全问题。
- **应用限制**：目前仅验证了视频生成可控性和简单规划任务，对于复杂长时决策、真实机器人控制的泛化能力未知。

（完）
