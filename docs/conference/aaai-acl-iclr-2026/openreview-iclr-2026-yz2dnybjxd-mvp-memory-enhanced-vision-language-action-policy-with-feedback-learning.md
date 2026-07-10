---
title: "MVP: Memory-enhanced Vision-Language-Action Policy with Feedback Learning"
title_zh: "MVP: 记忆增强的视觉-语言-动作策略与反馈学习"
authors: "Chubin Zhang, Yansong Tang, Wenkai Guo, Guanxing Lu, Yi Su, Haoji Zhang, Xiuwei Xu, Linqing Zhao, Ziwei Wang, Jiwen Lu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=Yz2DnYBJXd"
tags: ["query:latent-vla"]
score: 7.0
evidence: 用于操作任务的记忆增强VLA模型
tldr: 针对现有VLA模型马尔可夫假设导致的时序任务局限性，提出MVP非马尔可夫VLA模型。通过紧凑记忆表示存储历史动作和视觉观测，克服高维历史存储开销，并引入反馈学习机制，提升在长期操作任务中的性能。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有VLA模型假设马尔可夫性，无法处理长期任务和利用反馈。
method: 引入情景记忆机制，利用视频理解技术构建紧凑记忆表示，并融合历史信息进行决策。
result: 在长期操作任务上表现优于基线模型，支持反馈学习。
conclusion: 非马尔可夫设计使VLA模型更能应对真实世界的时序依赖。
---

## Abstract
Recent advances in Vision-Language-Action (VLA) models have enabled robots to perform a wide range of manipulation tasks conditioned on language instructions, offering strong generalization across tasks, objects, and environments. However, most existing VLAs operate under a Markov assumption, limiting their ability to handle temporally extended tasks and learn from feedback. To address these limitations, we propose MVP, a non-Markovian VLA model that leverages episodic memory composed of historical actions and visual observations. To mitigate the computational cost of storing high-dimensional histories, we introduce a compact memory representation inspired by video understanding techniques. Additionally, to prevent the model from disregarding historical inputs during training, we design a novel feedback learning strategy based on SO(3) trajectory perturbation. This approach encourages the model to associate actions with their environmental consequences through observation-action-observation sequences. Experimental results on both simulated and real-world benchmarks demonstrate that MVP outperforms existing models, particularly on tasks that require temporal reasoning and history-dependent decision-making. Our findings highlight the importance of memory and feedback in advancing the capabilities of general-purpose robotic manipulation systems.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有视觉-语言-动作（VLA）模型大多基于马尔可夫假设，即当前决策只依赖当前观测，无法处理长期任务（如需要记忆历史步骤或利用反馈）中的时序依赖关系。
- **研究动机**：真实机器人操作任务往往需要回溯之前的动作和观察结果（例如错误纠正、多步推理），马尔可夫模型会忽略历史信息，导致性能受限。同时，存储完整历史的高维观测会带来巨大的计算开销，且训练中模型容易忽略历史输入。
- **整体含义**：通过引入非马尔可夫记忆机制，构建记忆增强的VLA策略，使机器人能够更有效地应对需要时序推理和反馈学习的操作任务，推动通用机器人操作系统的能力提升。

## 2. 论文提出的方法论：核心思想、关键技术细节、算法流程

- **核心思想**：提出MVP（Memory-enhanced Vision-Language-Action Policy with Feedback Learning），一种非马尔可夫的VLA模型，利用情景记忆（episodic memory）存储历史动作和视觉观测，并设计反馈学习策略迫使模型关注历史信息。
- **关键技术细节**：
  - **紧凑记忆表示**：借鉴视频理解技术（如视频压缩或特征提取方法），将高维历史观测序列压缩成低维紧凑表示，缓解存储和计算开销。
  - **反馈学习策略**：基于SO(3)轨迹扰动（trajectory perturbation），在训练时对动作轨迹施加旋转扰动，迫使模型通过“观测-动作-观测”序列关联动作与环境结果，从而主动使用历史信息进行决策。
- **算法流程（文字说明）**：
  1. 输入当前语言指令和视觉观测，同时从紧凑记忆缓冲区读取历史动作和观测的压缩表示。
  2. 模型融合当前信息与历史记忆，输出动作决策。
  3. 在训练阶段，对真实动作轨迹施加SO(3)扰动生成负样本，要求模型从扰动后的轨迹中分辨并纠正，从而学习动作的长期后果。
  4. 通过联合训练语言理解、视觉感知、记忆回放和扰动反馈，优化整个策略网络。

## 3. 实验设计：数据集/场景、Benchmark、对比方法

- **数据集/场景**：
  - 模拟环境：未具体说明环境名称，但提到“simulated benchmarks”。
  - 真实世界：真实机器人操作任务。
- **Benchmark**：未明确列出标准benchmark名称，但任务类型涉及“需要时序推理和历史依赖决策”的长期操作任务。
- **对比方法**：与“现有的VLA模型”进行比较，具体方法未在摘要中列出（原文可能包含具体基线如RT-2、PaLM-E等，但用户提供信息有限）。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等算力信息。摘要和元数据均未提及训练资源配置，因此无法总结。

## 5. 实验数量与充分性

- **实验数量**：从摘要看，至少包含模拟和真实世界两类实验，但未提及具体实验组数或消融实验数量。元数据中“result”提到“在长期操作任务上表现优于基线模型，支持反馈学习”，说明有对比实验。
- **充分性判断**：由于缺乏详细实验描述，难以判断充分性。但提到了反馈学习策略的消融（通过扰动机制），表明进行了模块分析。总体上，公开信息不足以评估实验的全面性，但方法设计有针对性验证。

## 6. 论文的主要结论与发现

- MVP在需要时序推理和历史依赖决策的任务上优于现有VLA模型。
- 非马尔可夫设计使VLA模型更能应对真实世界中的时序依赖。
- 记忆增强和反馈学习是提升通用机器人操作能力的关键因素。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：引入情景记忆打破马尔可夫假设，并使用紧凑表示解决高维历史存储问题。
- **反馈学习机制**：基于SO(3)扰动迫使模型主动关联历史，防止记忆被忽略，这是训练技巧上的亮点。
- **兼顾模拟和真实**：在两类场景进行验证，增强了结论的可信度。

## 8. 不足与局限

- **实验覆盖不全**：未提供具体数据集名称、任务数量、统计显著性等细节，可能存在选择性报告风险。
- **算力与复现性**：未说明训练资源配置，不利于复现。
- **应用限制**：紧凑记忆表示可能丢失部分细粒度信息，且SO(3)扰动策略可能不适用于非旋转动作（如抓取力控制）。此外，仅针对操作任务，对其他机器人领域（如导航）的泛化性未知。
- **偏差风险**：如果基线模型未做公平的马尔可夫版本适配，对比可能不充分。

（完）
