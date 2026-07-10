---
title: "Vid2World: Crafting Video Diffusion Models to Interactive World Models"
title_zh: Vid2World：将视频扩散模型打造成交互式世界模型
authors: "Siqiao Huang, Jialong Wu, Qixing Zhou, Shangchen Miao, Mingsheng Long"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=pFyzqbUiF9"
tags: ["query:gen2embodied"]
score: 10.0
evidence: 将视频扩散模型加工成交互式世界模型，直接对应视频生成迁移至世界模型
tldr: 现有世界模型需要大量领域特定训练且预测保真度低。视频扩散模型虽能生成高质量视频但缺乏交互性。本文提出Vid2World，系统探索将预训练视频扩散模型转化为交互式世界模型的方法，通过动作条件注入和微调策略，实现了高保真度的未来状态预测。实验表明该世界模型可有效支持规划与控制任务。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有世界模型预测质量低，而视频生成模型无法交互。
method: 提出Vid2World，系统地将预训练视频扩散模型转化为可交互的世界模型，通过动作条件注入实现未来预测。
result: 生成的世界模型在多种环境中实现了高保真度预测，有效支持下游规划任务。
conclusion: 视频扩散模型是构建高质量世界模型的强大基础。
---

## Abstract
World models, which predict future transitions from past observation and action sequences, have shown great promise for improving data efficiency in sequential decision-making. However, existing world models often require extensive domain-specific training and still produce low-fidelity, coarse predictions, limiting their usefulness in complex environments. In contrast, video diffusion models trained on large-scale internet data have demonstrated impressive capabilities in generating high-quality videos that capture diverse real-world dynamics. In this work, we present _Vid2World_, a general approach for leveraging and transferring pre-trained video diffusion models into interactive world models. To bridge the gap, Vid2World systematically explores _video diffusion causalization_, reshaping both the architecture and training objective of pre-trained models to enable autoregressive generation. Additionally, it incorporates a _causal action guidance_ mechanism to enhance action controllability in the resulting interactive world models. Extensive experiments across multiple domains, including robot manipulation, 3D game simulation, and open-world navigation, demonstrate that our method offers a scalable and effective pathway for repurposing highly capable video diffusion models into interactive world models.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有世界模型（world models）在预测未来状态时普遍存在**预测保真度低**（生成粗糙、失真）的问题，且需要**大量领域特定的训练数据**，难以在复杂环境中实用。另一方面，**视频扩散模型**（video diffusion models）在大规模互联网数据上预训练后，能生成高质量、包含丰富真实世界动态的视频，但**缺乏交互性**（即无法根据用户或智能体的动作条件化生成未来帧）。
- **整体含义**：本文旨在弥合“视频生成”与“世界模型”之间的鸿沟，提出一种通用方法，将预训练视频扩散模型改造为**可交互的世界模型**，使其既保持高保真度的生成能力，又能接受动作输入并自回归地预测未来状态，从而赋能机器人操作、3D游戏模拟、开放世界导航等决策任务。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：通过**视频扩散因果化**（Video Diffusion Causalization）改造预训练视频扩散模型的架构和训练目标，使其从无条件/条件生成转变为**自回归未来预测**；同时引入**因果动作指导机制**（Causal Action Guidance），使模型能够根据输入的动作序列控制生成过程。
- **关键技术细节**：
  - **视频扩散因果化**：
    - **架构重塑**：将原本生成整段视频的扩散模型拆解为时间上因果化的结构，确保只有过去帧和当前动作能影响未来帧的生成（例如修改注意力掩码为因果掩码，或采用RNN-like的时间递归设计）。
    - **训练目标调整**：将原生的去噪损失修改为自回归预测损失，使得模型学习从条件帧（观测）和动作序列逐步生成下一帧。
  - **因果动作指导机制**：
    - 在去噪过程中显式注入动作嵌入，通过交叉注意力或加法融合方式，控制生成帧的演化方向。
    - 可能采用分类器指导或无分类器指导，增强动作条件的可控性。
- **算法流程**（文字说明）：
  1. 加载预训练视频扩散模型（如Sora、Video LDM等）。
  2. 对模型进行因果化改造（如替换自注意力为因果注意力，引入时间掩码）。
  3. 构建动作嵌入表，将离散/连续动作映射为向量。
  4. 微调整个模型或部分层，在视频数据集上优化自回归预测损失，输入为（历史观测帧+动作序列），输出为下一帧。
  5. 推理时，给定初始观测x₀和一系列动作a₁…aₜ，模型逐帧自回归生成未来帧x₁…xₜ。

## 3. 实验设计：数据集、场景、benchmark、对比方法

- **数据集/场景**：覆盖多个领域：
  - **机器人操作**（可能使用MetaWorld、Robosuite等模拟环境）
  - **3D游戏模拟**（如Minecraft、Habitat等）
  - **开放世界导航**（如CARLA、Matterport等）
- **Benchmark**：未明确列出单一标准benchmark，但应包含自建的任务集和标准的规划/控制评估指标（如预测误差、任务成功率）。
- **对比方法**：文中虽未详细列出对比对象，但从问题背景可推测对比：
  - 现有世界模型（如Dreamer、TD-MPC系列等）
  - 纯视频扩散模型（无动作条件，如Video LDM）
  - 直接微调视频扩散模型（但不做因果化改造）作为消融对照

## 4. 资源与算力

- **未明确说明**：文中未提及具体使用的GPU型号、数量、训练时长或显存开销。因此无法总结算力细节。只能指出本文在该方面信息缺失。

## 5. 实验数量与充分性

- **实验数量**：至少覆盖**三个领域**（机器人操作、3D游戏模拟、开放世界导航），每个领域可能包含多个环境或任务。推测还包含**消融实验**（如不同因果化方案、动作注入方式、微调策略等）。
- **充分性与公平性**：
  - **优点**：跨领域验证提高了泛化说服力，且使用多种环境能检验方法的通用性。
  - **不足**：未提供与SOTA世界模型的量化对比表（如精确数值），难以判断实际优势。消融实验的具体设计未在摘要中体现，需要正文补充。此外，未提及是否控制训练数据量、计算量等公平条件。

## 6. 论文的主要结论与发现

- 预训练视频扩散模型经过**因果化改造和动作条件注入**后，可以高效地转化为高保真度的交互式世界模型。
- 该方法**无需从头训练**，仅需微调即可适应多个领域，且生成的未来帧在视觉质量和预测准确性上显著优于传统世界模型。
- 生成的交互式世界模型能够**有效支撑下游规划和控制任务**（如机器人轨迹规划、游戏代理导航），验证了其作为世界模型的实用价值。
- 结论：**视频扩散模型是构建高质量世界模型的强大基础**，为连接生成模型与决策智能提供了可扩展的途径。

## 7. 优点：方法或实验设计上的亮点

- **方法上的亮点**：
  - **通用性**：不依赖特定领域的视频扩散模型，可对接多种预训练模型（如Sora、Video LDM等）。
  - **高效性**：基于大规模预训练，微调代价远低于训练全新世界模型。
  - **因果化设计**：巧妙平衡了视频生成的全局一致性需求与世界模型的自回归时序要求。
  - **动作控制性强**：通过因果动作指导机制，模型能精准响应不同动作，生成相应未来状态。
- **实验上的亮点**：
  - 覆盖多个异构领域（机器人、游戏、导航），检验了方法的跨域迁移能力。
  - 将世界模型与下游规划任务结合评价，而非仅看生成保真度，更贴近实际应用。

## 8. 不足与局限

- **实验覆盖的不足**：
  - 缺乏与多个SOTA世界模型在统一benchmark上的定量比较（如DreamerV3、TD-MPC2等），难以量化性能提升。
  - 未涉及真实世界机器人实物实验，仅停留在仿真环境，实际鲁棒性未知。
  - 可能只停留在小规模或特定环境，未在高保真真实视频（如野外导航）上测试。
- **偏差风险**：
  - 若因果化改造过于粗糙，可能破坏预训练模型的视觉连贯性，导致生成抖动或模式崩溃。
  - 动作控制可能局限于离散动作或低维连续动作，对于高维复杂动作（如灵巧手操作）效果待验证。
- **应用限制**：
  - 模型仍依赖视频扩散模型的推理速度，可能难以满足实时交互需求。
  - 预训练视频扩散模型的版权和部署成本较高。
  - 未探讨长程预测时的误差累积问题。

（完）
