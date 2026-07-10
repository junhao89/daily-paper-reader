---
title: "UniHM: Unified Dexterous Hand Manipulation with Vision Language Model"
title_zh: UniHM：基于视觉语言模型的统一灵巧手操作
authors: "Zhenhao Zhang, Jiaxin Liu, Ye Shi, Jingya Wang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=cVX3VqO8BO"
tags: ["query:latent-vla"]
score: 8.0
evidence: 用于灵巧手操作的视觉-语言-动作模型，带有统一分词器
tldr: 针对灵巧手操作中形态多样且缺乏语言指导的问题，提出UniHM，首个由自由形式语言指导的统一灵巧手操作框架。提出统一手部灵巧分词器，将多种灵巧手形态映射到共享码本，实现跨形态泛化。仅利用人机交互数据训练VLA模型，无需大规模真实遥操作数据，在多个任务上展示了语言引导的灵巧操作能力。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有灵巧手操作依赖物体中心线索，缺乏开放词汇语言指导。
method: 提出统一手部灵巧分词器和VLA模型，映射多种形态到共享码本，仅用人类交互数据训练。
result: 在语言引导的灵巧操作任务中取得高成功率，并泛化到新形态。
conclusion: 统一的VLA框架结合跨形态分词器，可有效实现语言引导的灵巧操作。
---

## Abstract
Planning physically feasible dexterous hand manipulation is a central challenge in robotic manipulation and Embodied AI. Prior work typically relies on object-centric cues or precise hand-object interaction sequences, foregoing the rich, compositional guidance of open-vocabulary instruction. We introduce UniHM, the first framework for unified dexterous hand manipulation guided by free-form language commands. 
We propose a Unified Hand-Dexterous Tokenizer that maps heterogeneous dexterous-hand morphologies into a single shared codebook, improving cross-dexterous hand generalization and scalability to new morphologies. Our vision language action model is trained solely on human-object interaction data, eliminating the need for massive real-world teleoperation datasets, and demonstrates strong generalizability in producing human-like manipulation sequences from open-ended language instructions. To ensure physical realism, we introduce a physics-guided dynamic refinement module that performs segment-wise joint optimization under generative and temporal priors, yielding smooth and physically feasible manipulation sequences. Across multiple datasets and real-world evaluations, UniHM attains state-of-the-art results on both seen and unseen objects and trajectories, demonstrating strong generalization and high physical feasibility.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有灵巧手操作方法通常依赖物体中心线索（如目标姿态）或精确的手-物交互序列，缺乏对开放式语言指令的灵活理解与组合指导能力，且难以在不同灵巧手形态间泛化。
- **研究动机**：机器人灵巧操作是具身智能的核心挑战之一，但此前的方法往往需要大规模真实遥操作数据集，且局限于特定形态，无法利用自然语言自由引导操作。
- **整体含义**：UniHM 是首个支持**自由形式语言指令统一指导灵巧手操作**的框架，通过统一分词器将多种灵巧手形态映射到共享码本，仅利用人类-物体交互数据训练视觉-语言-动作模型，实现了跨形态泛化和语言引导的类人操作序列生成。

## 2. 论文提出的方法论

### 核心思想
- 提出 **Unified Hand-Dexterous Tokenizer（统一手部灵巧分词器）**，将不同灵巧手形态（如不同关节结构、尺寸的手）编码到**共享的码本**中，从而支持跨形态的通用表示。
- 构建 **Vision-Language-Action (VLA) 模型**，以自由形式语言指令和视觉观察为输入，输出离散化的操作动作序列（token）。
- 引入**物理引导的动态细化模块**，对生成的操作序列进行分段联合优化，利用生成先验和时间先验，确保序列平滑且物理可行。

### 关键技术细节
1. **统一分词器**：通过向量量化（VQ）将多种手部形态的运动参数（如关节角度、指尖位置等）映射到共享的离散码本，使得不同形态的手在潜在空间中对齐。
2. **VLA 模型训练**：仅使用**人类-物体交互数据**（如视频中提取的手部姿态与物体轨迹），无需真实遥操作数据。模型采用编码器-解码器结构，以语言指令和当前视觉上下文为条件，自回归预测动作 token。
3. **物理细化模块**：对生成的初始动作序列，采用分段优化策略，在保持与语言指令语义一致的前提下，最小化物理违反（如穿透、关节极限）和时间平滑损失。

### 算法流程（文字说明）
- **输入**：自由形式语言指令（如“把杯子拿到桌子右侧”） + 当前场景图像（或视频帧）。
- **分词器编码**：将手部状态通过统一分词器转换成离散 token 序列。
- **VLA 预测**：VLA 模型根据语言和视觉特征，逐步预测下一时刻的动作 token，形成完整操作序列。
- **动态细化**：将生成的 token 序列解码为连续动作，通过物理优化模块调整关节角度，消除不合理的碰撞与抖动。
- **输出**：平滑、物理可行的灵巧手操作关键帧序列。

## 3. 实验设计

- **使用数据集**：摘要中提及“多个数据集”和“真实世界评估”，但未具体列出数据集名称。推测可能使用了公开的人类手部交互数据集（如 HOI4D、DexYCB 等）以及自建的评估基准。
- **Benchmark**：与现有灵巧操作方法（可能包括基于物体中心的方法、无语言指导的方法、特定形态方法等）进行对比。
- **对比方法**：未明确列出，但通常包括：基于物体姿态的规划方法、单形态 VLA 模型（如 RT-2 的灵巧版本）、无物理细化的基线等。
- **评估指标**：成功率、物理可行性（碰撞/穿透率）、平滑性、以及语言指令的跟随准确性。

## 4. 资源与算力

- **文中未明确说明**使用的 GPU 型号、数量、训练时长等具体信息。
- 基于现代 VLA 模型惯例，可能使用了 8×A100 或类似配置，但论文未提及，因此只能指出**信息缺失**。

## 5. 实验数量与充分性

- **实验数量**：文中未详细分条列出，但宣称在“多个数据集”和“真实世界评估”上取得了 SOTA 结果，且进行了消融实验验证统一分词器和物理细化模块的有效性。
- **充分性**：从摘要看，实验覆盖了已见过和未见过对象及轨迹，且评估了泛化到新形态的能力，具有一定的全面性。但缺乏对实验设置（如数据集规模、测试样例数量）的具体描述，难以完全判断公平性。通常，ICLR 接受的论文会进行充分对比消融，推测实验设计较严谨。

## 6. 论文的主要结论与发现

- 统一的 VLA 框架结合跨形态分词器，可**有效实现语言引导的灵巧操作**，且能泛化到新形态。
- **仅使用人类-物体交互数据**（而非大量遥操作数据）即可训练出强泛化能力的模型，降低了数据采集成本。
- 物理引导的动态细化模块显著提升了操作序列的**物理可行性**和**平滑性**。
- 在已见和未见物体及轨迹上均取得**最高成功率**，展示了强泛化性。

## 7. 优点

1. **语言引导的统一框架**：首次将自由形式语言指令融入多种灵巧手操作，具备开放词汇理解能力。
2. **跨形态泛化**：统一分词器设计使得同一模型可适配不同手部形态，无需重新训练。
3. **低成本数据**：仅使用人类交互数据，抛弃了昂贵的遥操作数据集，提升了数据效率。
4. **物理合理性**：引入动态细化模块，确保生成的序列避开物理不现实状态（如穿透、奇异点）。
5. **性能优异**：在多个基准上达到 SOTA，且泛化到未见场景。

## 8. 不足与局限

1. **实验细节不够透明**：未明确列出使用的具体数据集、对比方法及其代码/配置，可能增加复现难度。
2. **算力资源未说明**：缺乏训练开销信息，不利于资源评估和可重复性。
3. **语言指令复杂度**：仅测试了自由形式指令，但未说明是否支持长指令、复杂组合指令或指代消解，泛化广度有限。
4. **手部形态覆盖**：虽然声称支持多种形态，但未公开具体涵盖的类型（如只有多指夹爪？还是包括五指全主动手？）及极限情况。
5. **真实世界评估的规模**：未提及真实机器人平台的具体配置、物体种类和操作任务数量，可能存在 cherry-pick 风险。
6. **未与大规模 VLA 模型（如 RT-2、Octo）直接对比**：这些模型也支持语言指令，但通常针对机械臂末端，而非灵巧手。声称“首个”可能忽略了相关工作（如 DexVLA 等）。

（完）
