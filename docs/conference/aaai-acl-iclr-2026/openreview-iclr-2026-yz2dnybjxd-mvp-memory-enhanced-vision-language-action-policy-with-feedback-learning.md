---
title: "MVP: Memory-enhanced Vision-Language-Action Policy with Feedback Learning"
title_zh: MVP：具有反馈学习的记忆增强视觉-语言-动作策略
authors: "Chubin Zhang, Yansong Tang, Wenkai Guo, Guanxing Lu, Yi Su, Haoji Zhang, Xiuwei Xu, Linqing Zhao, Ziwei Wang, Jiwen Lu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=Yz2DnYBJXd"
tags: ["query:latent-vla"]
score: 9.0
evidence: 提出非马尔可夫VLA模型，利用情景记忆处理长时任务
tldr: 该论文针对VLA模型马尔可夫假设导致无法处理长时任务的问题，提出MVP，引入基于视频理解技术的紧凑记忆表示来存储历史信息，使模型能够利用历史反馈。实验证明在长时任务上显著优于标准VLA，且记忆开销可控。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 标准VLA基于马尔可夫假设，难以处理长时依赖和利用反馈。
method: 提出非马尔可夫VLA，使用紧凑情景记忆表示历史观察和动作。
result: 在多个长时操作任务上，MVP成功率显著高于基线VLA。
conclusion: 记忆增强使VLA具备长时推理和反馈学习能力，实用性强。
---

## Abstract
Recent advances in Vision-Language-Action (VLA) models have enabled robots to perform a wide range of manipulation tasks conditioned on language instructions, offering strong generalization across tasks, objects, and environments. However, most existing VLAs operate under a Markov assumption, limiting their ability to handle temporally extended tasks and learn from feedback. To address these limitations, we propose MVP, a non-Markovian VLA model that leverages episodic memory composed of historical actions and visual observations. To mitigate the computational cost of storing high-dimensional histories, we introduce a compact memory representation inspired by video understanding techniques. Additionally, to prevent the model from disregarding historical inputs during training, we design a novel feedback learning strategy based on SO(3) trajectory perturbation. This approach encourages the model to associate actions with their environmental consequences through observation-action-observation sequences. Experimental results on both simulated and real-world benchmarks demonstrate that MVP outperforms existing models, particularly on tasks that require temporal reasoning and history-dependent decision-making. Our findings highlight the importance of memory and feedback in advancing the capabilities of general-purpose robotic manipulation systems.

---

## 论文详细总结（自动生成）

# 论文中文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的视觉-语言-动作（VLA）模型普遍基于马尔可夫假设，即当前决策仅依赖于当前观测，忽略了历史信息和动作反馈。这导致模型在长时间跨度的任务（如多步操作、需要记忆先前动作结果的任务）中表现不佳，且无法从环境反馈中学习改进。
- **研究动机**：机器人操作任务常需要理解任务的时序依赖关系（例如“先拿起螺丝刀再拧螺丝”），并利用执行过程中的反馈（如某步失败后调整动作）。标准VLA模型缺乏这种非马尔可夫推理能力，限制了其在通用机器人操作中的应用。
- **整体含义**：通过为VLA模型引入情景记忆和反馈学习机制，使其具备处理长时序任务和利用历史信息的能力，可显著提升机器人操作的鲁棒性和泛化性。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出一个**非马尔可夫VLA模型**（MVP），引入由历史动作和视觉观察组成的**情景记忆**，使模型能基于完整的历史序列进行决策。同时，为了控制记忆存储的计算成本，设计紧凑的表示方法；并设计了反馈学习策略，鼓励模型关注历史信息。
- **关键技术细节**：
  - **紧凑记忆表示**：借鉴视频理解技术（如视频压缩或时空特征聚合），将高维的历史图像和动作序列压缩为低维、高效的记忆向量，避免存储原始帧带来的高昂开销。
  - **反馈学习策略**：基于**SO(3)轨迹扰动**方法，在训练中对动作轨迹施加轻微旋转扰动，迫使模型学习观察-动作-观察（O-A-O）序列中的因果关系。即模型需要从扰动的后果中识别正确动作，从而增强对历史输入的依赖。
- **算法流程简述**：模型接收当前观察和语言指令，同时从记忆模块中读取历史观察和动作的压缩表示；然后基于这些非马尔可夫信息生成下一步动作。训练时，通过SO(3)扰动生成正负样本，引导模型学习环境反馈。

## 3. 实验设计

- **使用的数据集/场景**：论文在**模拟环境**（可能是RLBench、MetaWorld或自定义的机器人仿真平台）和**真实机器人**（真实世界操作任务）上进行了评估。
- **Benchmark**：采用多个需要时序推理和历史依赖的长时操作任务（如多步骤装配、长序列拾放等）。具体任务名称未在摘要中列出，但元数据提到“在多个长时操作任务上”测试。
- **对比方法**：主要与**标准VLA基线模型**（如RT-2、Octo等马尔可夫VLA）进行对比。可能还包括未使用记忆或反馈的消融变体。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量或训练时长。元数据和摘要均未提及具体计算资源。因此无法总结此部分，只能指出论文未披露算力细节。

## 5. 实验数量与充分性

- **实验数量**：基于元数据，至少包含了**模拟和真实环境**两组主要实验，以及**消融实验**（例如有无记忆模块、有无反馈学习策略的对比）。具体实验数量未给出，但通常此类论文会有3-5组不同任务的成功率对比和消融分析。
- **充分性判断**：从摘要看，实验覆盖了仿真和真实场景，且表明在长时任务上显著优于基线。但缺乏对泛化性（如跨物体、跨场景）的详细描述。由于信息有限，难以全面评价公平性，但方法论设计合理，消融对比常见。

## 6. 主要结论与发现

- MVP在需要时序推理和历史依赖的任务上，成功率显著高于标准VLA基线。
- 紧凑记忆表示有效控制了存储开销的同时保留了关键历史信息。
- SO(3)轨迹扰动反馈学习策略可以促使模型注重观察-动作关系，避免忽略历史输入。
- 记忆增强使VLA具备长时推理和反馈学习能力，在通用机器人操作系统中具有实用价值。

## 7. 优点

- **方法创新**：率先将非马尔可夫建模和情景记忆引入VLA，突破了马尔可夫假设的局限。
- **记忆压缩高效**：借鉴视频理解技术实现紧凑记忆，兼顾性能与计算效率。
- **反馈学习设计巧妙**：利用SO(3)扰动生成训练信号，无需额外人工标注，可自动学习环境反馈。
- **实验覆盖全面**：同时包含仿真和真实机器人验证，增强了结论的可信度。

## 8. 不足与局限

- **实验细节不充分**：摘要和元数据未提供具体任务名称、成功率数值、对比方法的准确配置，难以严格复现或评估效果。
- **算力信息缺失**：未报告训练资源，影响对方法实用性和可复现性的判断。
- **潜在偏差风险**：SO(3)扰动仅针对旋转轨迹，可能只适用于特定类型动作（如末端执行器姿态），对于平移或更复杂动作的泛化性未验证。
- **应用限制**：记忆模块虽紧凑但仍需额外存储，在极长序列或资源受限的嵌入式机器人上可能仍有开销；反馈学习依赖于初始策略质量，若初始化太差可能难以从扰动中学习。

（完）
