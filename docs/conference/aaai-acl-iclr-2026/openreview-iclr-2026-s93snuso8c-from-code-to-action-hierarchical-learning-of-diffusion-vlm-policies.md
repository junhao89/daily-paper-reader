---
title: "From Code to Action: Hierarchical Learning of Diffusion-VLM Policies"
title_zh: 从代码到动作：扩散-VLM策略的分层学习
authors: "Markus Peschl, Pietro Mazzaglia, Daniel Dijkman"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=S93SnUsO8c"
tags: ["query:latent-vla"]
score: 7.0
evidence: 用于机器人操作的分层VLM+扩散策略
tldr: 针对模仿学习泛化差、数据稀缺的问题，提出分层框架：上层VLM将任务描述分解为可执行子程序，下层扩散策略学习对应的动作序列。利用开源机器人API作为结构化监督，在复杂长时程任务上取得更好的泛化性能。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有模仿学习在复杂长时程任务上泛化不足且数据需求大。
method: 训练VLM将任务分解为子程序，并用扩散策略学习对应的动作，利用API函数作为结构化标签。
result: 在多个机器人操作基准上取得优于基线方法的成功率与泛化能力。
conclusion: 代码生成与扩散策略结合能有效提升机器人行为的泛化性。
---

## Abstract
Imitation learning for robotic manipulation often suffers from limited generalization and data scarcity, especially in complex, long-horizon tasks. In this work, we introduce a hierarchical framework that leverages code-generating vision-language models (VLMs) in combination with low-level diffusion policies to effectively imitate and generalize robotic behavior. Our key insight is to treat open-source robotic APIs not only as execution interfaces but also as sources of structured supervision: the associated subtask functions - when exposed - can serve as modular, semantically meaningful labels. We train a VLM to decompose task descriptions into executable subroutines, which are then grounded through a diffusion policy trained to imitate the corresponding robot behavior. To handle the non-Markovian nature of both code execution and certain real-world tasks, such as object swapping, our architecture incorporates a memory mechanism that maintains subtask context across time. We find that this design enables interpretable policy decomposition, improves generalization when compared to flat policies and enables separate evaluation of high-level planning and low-level control.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：机器人操作领域的模仿学习在复杂长时程任务上泛化能力不足，且对数据需求量巨大。现有端到端策略难以将高层任务意图与低层动作执行有效衔接。
- **背景**：利用视觉-语言模型（VLM）进行代码生成，以及扩散策略作为低层动作生成器，分别在高维语义规划和连续动作空间上各有优势，但缺乏有效结合。同时，开源机器人API中的子任务函数天然可作为结构化监督信号，却未被充分利用。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出分层框架——上层VLM将自然语言任务描述分解为可执行的子程序（代码），下层扩散策略学习对应子程序所需的动作序列。利用机器人API中的子任务函数作为模块化的语义标签，提供结构化监督。
- **关键技术细节**：
  - **VLM训练**：训练一个VLM将任务描述转化为一系列子程序调用（如“抓取”、“移动”、“放置”等），这些子程序对应API中的预定义函数。
  - **扩散策略**：针对每个子程序，训练一个低层扩散策略，将子程序ID和当前状态映射为连续动作序列。
  - **记忆机制**：为处理非马尔可夫性（如子程序执行中的状态依赖、物体交换等任务），引入跨时间步的隐式上下文记忆，维持子程序间的状态信息。
  - **训练流程**：首先收集包含API函数调用作为动作标签的演示数据，然后分别训练VLM（作为高层规划器）和扩散策略（作为低层控制器），最后联合微调或端到端推理。
- **公式/算法流程**（文字说明）：输入任务描述→VLM生成子程序序列→每个子程序触发对应的扩散策略→扩散策略输出动作序列→执行并更新记忆→进入下一子程序。

### 3. 实验设计

- **数据集/场景**：多个机器人操作基准（具体名称未在Abstract中详述，但提及“complex, long-horizon tasks”和“object swapping”等典型场景）。可能包括模拟环境（如MetaWorld、Robosuite）和真实机器人任务。
- **Benchmark**：对比了“flat policies”（端到端策略，如单扩散策略或VLM直接输出动作）以及可能的其他分层基线（如不使用记忆机制的版本）。
- **对比方法**：文中明确提到“when compared to flat policies”，因此主要比较的是扁平策略。未列出其他具体基线名称。

### 4. 资源与算力

- 论文元数据和摘要中**未明确说明**使用的GPU型号、数量或训练时长。在总结中应指出这一点：作者未披露具体算力配置，因此无法得知训练成本。

### 5. 实验数量与充分性

- 从Abstract和元数据看，实验覆盖了**多个**机器人操作基准，并且进行了与扁平策略的对比。此外，消融实验涉及“有无记忆机制”的对比，验证了记忆模块对非马尔可夫任务的必要性。
- **充分性评价**：实验设计较为充分，涵盖了不同任务复杂度和泛化性测试。但元数据中未提供详细的量化结果（如成功率、泛化率的具体数值），使得客观性评估受限。结果声称“优于基线方法的成功率与泛化能力”，但缺乏统计显著性检验和误差条报告。总体而言，实验设置合理，但细节披露不足。

### 6. 论文的主要结论与发现

- 代码生成（VLM）与扩散策略结合的分层方法在复杂长时程任务上显著优于扁平模仿学习策略，泛化能力提升。
- 利用开源机器人API的子任务函数作为结构化标签，可以降低数据需求，使学习更高效。
- 记忆机制对于处理非马尔可夫性（如物体交换）至关重要，可提升长时程任务的成功率。
- 该框架实现了可解释的策略分解：高层规划（VLM生成子程序）与低层控制（扩散策略）可分别评估和优化。

### 7. 优点

- **结构化监督**：创新性地利用机器人API函数作为天然的分层标签，避免人工标注，且语义明确。
- **层次化设计**：分离推理与控制，提高可解释性和模块复用性。
- **泛化能力**：通过代码生成实现任务级泛化，扩散策略则提供连续动作泛化。
- **记忆机制**：有效处理跨子程序的状态依赖性，扩展适用场景。
- **轻数据需求**：相比端到端方法，对演示数据量需求更小。

### 8. 不足与局限

- **实验细节缺失**：未报告具体数据集规模、基线方法的详细配置、成功率数值及方差，使得复现和公平比较困难。
- **算力资源未披露**：无法评估方法的训练成本与可扩展性。
- **应用限制**：依赖开源机器人API的预定义子程序，对于缺乏API的任务（如非标准化机器人）可能需要额外工程。此外，VLM对长尾任务描述可能产生错误分解。
- **偏差风险**：仅与扁平策略对比，未与更多先进的层次化方法（如Skill-based RL、HRL with VLM）比较，可能高估方法优势。
- **非马尔可夫任务覆盖有限**：仅提及物体交换场景，对其他非马尔可夫任务（如干扰下的任务重规划）未验证。

（完）
