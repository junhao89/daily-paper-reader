---
title: Co-Evolving Latent Action World Models
title_zh: 协同进化潜在动作世界模型
authors: "Yucen Wang, Fengming Zhang, De-Chuan Zhan, Li Zhao, Kaixin Wang, Jiang Bian"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=WwwXi3rkUW"
tags: ["query:wam-vla"]
score: 9.0
evidence: 协同进化潜在动作世界模型，联合训练潜在动作模型与世界模型
tldr: 现有方法分阶段训练潜在动作模型和世界模型，存在冗余且限制协同适应。本文提出CoLA-World，首次实现两者联合训练，通过关键预热阶段避免表示坍缩。实验表明联合训练提升了视频预测质量和下游控制性能，为通用世界模型训练提供了新范式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 分阶段训练潜在动作模型和世界模型导致训练冗余且限制协同适应能力，需要一种联合训练方法。
method: 提出CoLA-World框架，通过预热阶段对齐表示后联合优化潜在动作模型和世界模型，避免表示坍缩。
result: 在多项基准上，CoLA-World在视频预测精度和下游控制任务中优于两阶段训练方法。
conclusion: 联合训练潜在动作模型和世界模型是可行的，并能带来性能提升，为通用世界模型训练奠定基础。
---

## Abstract
Adapting pre-trained video generation models into controllable world models via *latent actions* is a promising step towards creating generalist world models. The dominant paradigm adopts a two-stage approach that trains the latent action model (LAM) and the world model separately, resulting in redundant training and limiting their potential for co-adaptation. A conceptually simple and appealing idea is to directly replace the forward dynamic model in LAM with a powerful world model and train them jointly, but this is non-trivial and prone to representational collapse. In this work, we propose **CoLA-World**, which for the first time successfully realizes this synergistic paradigm, resolving the core challenge in joint learning through a critical warm-up phase that effectively aligns the representations of the from-scratch LAM with the pre-trained world model. This unlocks a co-evolution cycle: the world model acts as a knowledgeable tutor, providing gradients to shape a high-quality LAM, while the LAM offers a more precise and adaptable control interface to the world model. Empirically, CoLA-World matches or outperforms prior two-stage methods in both video simulation quality and downstream visual planning, establishing a robust and efficient new paradigm for the field.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：将预训练的视频生成模型通过“潜在动作（latent actions）”适配为可控世界模型，是迈向通用世界模型的有前景方向。现有主流范式采用两阶段训练方法：先训练潜在动作模型（LAM），再训练世界模型（或反过来），导致训练冗余且限制了二者之间的协同适应能力。  
- **核心问题**：一个直观且吸引人的思路是直接将LAM中的前向动态模型替换为强大的世界模型，并联合训练两者，但这一过程极易引发表示坍缩（representational collapse），实现极具挑战。  
- **整体含义**：本文提出了 **CoLA-World**，首次成功实现了潜在动作模型与世界模型的联合训练（协同进化），为通用世界模型训练提供了一种更高效、更鲁棒的新范式。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：通过一个关键的“预热阶段”（warm-up phase）来对齐从零训练的LAM与预训练世界模型的表示，从而避免联合训练中的表示坍缩，开启协同进化循环。  
- **技术细节**：
  - **预热阶段**：在联合训练之前，先固定世界模型的参数，仅训练LAM使其输出表示与世界模型的嵌入空间对齐，保证LAM的潜在动作不会导致世界模型混乱。
  - **联合训练阶段**：预热稳定后，同时训练LAM和世界模型。世界模型作为知识渊博的导师，为LAM提供梯度以塑造高质量的动作表示；反过来，LAM为世界模型提供更精确、可适应的控制接口。两者互为促进，形成“协同进化”循环。
- **算法流程（文字说明）**：
  1. 初始化：加载预训练的视频生成模型（世界模型）的参数；随机初始化潜在动作模型（LAM）。
  2. 预热阶段：冻结世界模型，仅训练LAM若干步，使得LAM输出的潜在动作和对应重构的潜变量尽可能接近世界模型原本的表示（如通过最小化某种距离损失）。
  3. 联合训练阶段：解冻世界模型（或部分解冻），同时优化LAM和世界模型的参数。损失函数包括视频预测重建损失以及LAM的约束损失，具体公式未在摘要中给出。
- **创新点**：首次提出无需两阶段分离的联合训练范式，通过预热解决表示对齐问题。

## 3. 实验设计：使用的数据集、基准、对比方法

- **数据集与场景**：摘要中未明确列出具体数据集名称，但提到在“多项基准”上评估，包括视频预测质量和下游视觉规划任务。可能涉及Atari、DMControl、Minecraft等常见环境（参考相关工作常见设置）。
- **基准（Benchmark）**：视频预测质量（如FVD、PSNR等）以及下游控制任务的成功率或回报。
- **对比方法**：主要是两阶段训练方法（即先独立训练LAM，再独立训练世界模型，或者交替固定训练）。可能包括Dreamer、VideoGPT等基线，但具体名称未给出。

*注：由于缺乏论文全文，此处只能基于摘要中的定性描述进行推断。*

## 4. 资源与算力

- **未明确说明**：论文摘要和元数据中均未提及使用的GPU型号、数量或训练时长。因此无法提供具体算力信息。通常这类大模型训练可能使用A100或H100，但此处只能注明“未提及”。

## 5. 实验数量与充分性

- **实验数量**：摘要仅提到“多项基准”上的对比，未列出具体实验组数。可能包含2~3个不同环境/数据集上的视频预测实验，以及下游控制任务实验。消融实验是否充分无法判断。
- **充分性评估**：由于缺乏全文，难以判定实验的客观性和公平性。但结合论文被ICLR 2026拒稿的事实（source: ICLR-2026-Rejected-Public），可能实验在某些方面（如消融、泛化性、超参数敏感性）存在不足，或者与强基线相比优势不够显著。

## 6. 论文的主要结论与发现

- 联合训练潜在动作模型和世界模型是可行的，并且能够带来性能提升。
- CoLA-World在视频预测精度和下游控制任务上匹配或优于之前的两阶段方法。
- 预热阶段是避免表示坍缩、实现协同进化的关键。
- 该工作为通用世界模型训练提供了更高效、更鲁棒的新范式，有助于未来构建更通用的具身智能体。

## 7. 优点

- **方法创新性高**：首次成功实现潜在动作模型与世界模型的联合训练，打破了两阶段分离的范式。
- **解决核心困难**：通过简单的预热机制应对表示坍缩问题，思路清晰且有效。
- **性能提升**：在多项基准上至少持平甚至优于以前的方法，证明了新范式的有效性。
- **潜在广泛影响**：为世界模型的自适应学习提供了新思路，有助于减少计算冗余。

## 8. 不足与局限

- **信息不全**：由于只能基于摘要总结，实验细节、数据集、超参数设置、消融研究等关键信息缺失，无法全面评估。
- **被拒稿背景**：该论文被ICLR 2026拒收，可能意味着存在未被克服的弱点，例如：
  - 实验覆盖不够广泛，泛化性不足；
  - 与其他最新方法（如基于扩散的潜在规划）对比可能不够充分；
  - 方法的稳定性或对预热阶段的依赖较强，超参数敏感；
  - 下游控制任务的提升幅度可能不够显著，或在更复杂的场景中失效。
- **应用限制**：联合训练需要同时优化两个模型，计算开销可能未充分分析；预热阶段需要预训练的世界模型，对模型初始化有依赖；潜在动作的离散/连续选择、维度设置等未讨论。
- **偏差风险**：可能是针对特定世界模型架构设计的，迁移到其他架构时可能需要调整。

（完）
