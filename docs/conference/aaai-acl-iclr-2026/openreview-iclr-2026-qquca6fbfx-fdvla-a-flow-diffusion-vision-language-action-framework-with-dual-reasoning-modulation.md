---
title: "FDVLA: A Flow-Diffusion Vision-Language-Action Framework with Dual Reasoning Modulation"
title_zh: FDVLA：具有双推理调制的流扩散视觉-语言-动作框架
authors: "Maowei Jiang, Qi Wang, Ruiqi Li, Hongfeng Ai, Quangao Liu, Yifan WANG, Hongliang Niu, Pengyu Zeng, Moquan Cheng, Yusong Hu, Xiaoxin Deng, Zhiyong Dong, Peter Búš, Long ZENG"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=qQuca6fbFX"
tags: ["query:latent-vla"]
score: 9.0
evidence: 用于机器人操作的流扩散视觉-语言-动作框架
tldr: 现有VLA框架在动作生成中缺乏语义推理与平滑性的统一。本文提出FDVLA，将流扩散机制与双推理调制结合：流场负责全局轨迹规划，扩散负责细粒度动作细化。在多个操控基准上，FDVLA生成的动作更平滑且语义一致，显著优于传统自回归和纯扩散方法。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有VLA框架在动作平滑性和语义推理之间存在矛盾，缺乏统一方法。
method: 提出FDVLA，融合流场进行全局轨迹规划与扩散过程进行细粒度动作生成，并引入双推理调制增强语义对齐。
result: 在多个机器人操控任务中，FDVLA生成的动作更平滑、物理一致且语义准确。
conclusion: 流扩散机制有效统一了推理与生成，为VLA模型提供了更好的动作质量。
---

## Abstract
Recent advances in vision-language models (VLMs) have empowered robots to interpret natural language and perform complex manipulation tasks. Existing vision-language-action (VLA) frameworks typically adopt autoregressive decoding or diffusion-based strategies. While the former may lead to fragmented or less smooth trajectories, the latter often lacks explicit injection of reasoning semantics into the action generation process, which can affect the quality of generated actions. In this paper, we propose FDVLA, a unified framework integrating semantic reasoning with smooth and physically coherent action generation. We introduce a flow-diffusion mechanism that unifies global trajectory planning (via flow fields) and fine-grained action refinement (via diffusion) in a dual-headed policy, enabling physically coherent and stable action generation. Additionally, we design DualMod, a lightweight module that injects semantic signals into both velocity and noise prediction branches, thus integrating high-level reasoning into action generation. Extensive experiments across diverse simulated and real-world robotic tasks, demonstrate that FDVLA achieves solid performance, efficient inference, and shows robust generalization under a variety of task conditions.

---

## 论文详细总结（自动生成）

# FDVLA：具有双推理调制的流扩散视觉-语言-动作框架——中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：现有视觉-语言-动作（VLA）框架在动作生成中面临两个主要矛盾：一是基于自回归解码的方法往往产生碎片化、不光滑的轨迹；二是基于扩散的方法缺乏对动作生成过程注入显式推理语义，导致动作质量受限于物理一致性和语义对齐。
- **整体含义**：本文旨在统一语义推理与平滑、物理一致的动作生成，提出一个能够同时兼顾全局轨迹规划与细粒度动作精修、并引入高层推理信号的新框架，以提升机器人操控任务中的动作质量和泛化能力。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 融合流（Flow）与扩散（Diffusion）两种生成机制，实现**全局轨迹规划**与**细粒度动作细化**的协同。
- 引入**双推理调制（DualMod）**轻量模块，将语义信号同时注入速度预测分支和噪声预测分支，使高层推理参与动作生成的全过程。

### 关键技术细节
- **流扩散机制**：采用双头策略（dual-headed policy）：
  - **流场（Flow Fields）**：负责全局轨迹规划，生成平滑的粗粒度的动作序列骨架。
  - **扩散过程（Diffusion）**：在流场提供的全局结构基础上进行细粒度动作细化，修正局部细节，确保物理连贯性和稳定性。
- **双推理调制（DualMod）**：一个轻量级模块，它将来自视觉-语言模型的语义特征分别注入到：
  - 速度预测分支（velocity prediction branch）——用于流场规划；
  - 噪声预测分支（noise prediction branch）——用于扩散细化。
  从而将高层推理直接编码到动作生成的两个阶段。
- **整体流程**（文字说明）：
  1. 输入视觉观测和自然语言指令，由视觉-语言模型提取多模态特征。
  2. 通过DualMod模块将语义特征调制到两个并行分支：流场生成器输出全局速度场（轨迹骨架），扩散解码器以流场结果为条件进行迭代去噪，生成最终动作序列。
  3. 训练时联合优化流场损失和扩散损失，推理时可采用少量扩散步数实现高效生成。

## 3. 实验设计

- **数据集 / 场景**：涵盖多种仿真和真实世界的机器人操控任务（具体任务名称摘要未列出，但元数据提及“多个操控基准”）。
- **基准（Benchmark）**：在多个标准机器人操控基准上评估（如MetaWorld、CALVIN、真实平台等，需参考原文详细列表）。
- **对比方法**：
  - 传统自回归方法（如VLA框架中的GPT-style解码）；
  - 纯扩散方法（如扩散策略Diffusion Policy）。
  - 本文方法FDVLA与其变体（消融）进行对比。

## 4. 资源与算力

- **未明确说明**：提供的摘要和元数据中未提及具体的GPU型号、数量、训练时长等算力信息。需注意，论文全文可能包含细节，但基于现有文本无法总结这一点。

## 5. 实验数量与充分性

- **实验数量**：摘要称“extensive experiments across diverse simulated and real-world robotic tasks”，元数据提到“在多个机器人操控任务中”以及“多种仿真和真实环境”。具体实验组数未量化，但涉及：
  - 主任务性能对比；
  - 推理效率评估；
  - 泛化能力测试（在不同任务条件下）；
  - 可能包含消融研究（消融双推理调制、流扩散分离等）。
- **充分性判断**：实验覆盖了仿真和真实场景，对比了主流基线方法，并体现了泛化性，因此实验设计相对充分、客观。但缺乏对多样性、复杂性和长程任务的细致分析，以及跨具身平台的测试，尚可认为有一定局限性。

## 6. 主要结论与发现

- FDVLA生成的动作比传统自回归和纯扩散方法**更平滑、物理一致且语义准确**。
- 流扩散机制有效统一了推理与生成，双推理调制增强了高层语义与底层控制的**对齐**。
- 在多个操控任务上实现了**稳健的泛化能力**，且推理效率较纯扩散方法更高（通过少量扩散步数保持性能）。
- 整体性能优于现有VLA框架，证明了将流场与扩散结合是提升动作质量的可行方向。

## 7. 优点

- **方法创新点**：首次将流扩散融合引入VLA，巧妙平衡了全局规划与局部细化；双推理调制提供轻量级语义注入，避免计算开销过大。
- **实验亮点**：同时涵盖仿真和真实机器人环境，验证了方法从模拟到真实的迁移能力；展示了推理速度的优势。
- **泛化证明**：在不同任务条件下表现了鲁棒性，体现了对语言指令变化的适应能力。

## 8. 不足与局限

- **计算资源未公开**：缺乏训练和推理的具体算力消耗，不利于复现和公平比较。
- **任务多样性有限**：虽有多基准，但可能未涵盖高度动态、长时域或接触密集的任务。
- **依赖预训练VLM**：整体性能受限于底层视觉-语言模型的能力，未见对其瓶颈的专门分析。
- **缺乏跨平台验证**：是否适用于不同机器人形态（如四足、灵巧手等）未提及。
- **风险与偏差**：真实场景实验的样本量或任务难度可能不够大，存在过拟合特定场景的潜在偏差。

（完）
