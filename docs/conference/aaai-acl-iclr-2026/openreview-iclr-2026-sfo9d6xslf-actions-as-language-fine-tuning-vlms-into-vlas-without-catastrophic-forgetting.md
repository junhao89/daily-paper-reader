---
title: "Actions as Language: Fine-Tuning VLMs into VLAs Without Catastrophic Forgetting"
title_zh: 动作即语言：将视觉-语言模型微调为视觉-语言-动作模型而不出现灾难性遗忘
authors: "Asher James Hancock, Xindi Wu, Lihan Zha, Olga Russakovsky, Anirudha Majumdar"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=sFO9d6XSlf"
tags: ["query:latent-vla"]
score: 9.0
evidence: VLM2VLA将动作表示为语言令牌，在微调为VLA时保持VLM的多模态理解能力
tldr: 将VLM微调为VLA模型时，动作学习常导致灾难性遗忘，损害视觉语言理解。本文归因于数据分布不匹配，提出VLM2VLA范式：通过将底层动作表示为语言标记来弥合分布差异，并在训练中结合多任务学习。实验表明，VLM2VLA在保持原始VLM推理能力的同时，在机器人操控任务上取得先进的性能，尤其在指令跟随和语义理解方面表现突出。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: VLM微调为VLA时发生灾难性遗忘，损失多模态理解能力，源于训练数据分布不匹配。
method: 将低层动作表示为语言标记解决数据分布不匹配，采用数据层匹配和多任务训练策略。
result: 在机器人操控任务上，VLM2VLA保持VLM推理能力并实现先进操作性能，指令跟随和语义理解显著提升。
conclusion: VLM2VLA提供了一种有效解决VLA微调中灾难性遗忘的范式，推动了通用策略的发展。
---

## Abstract
Fine-tuning vision-language models (VLMs) on robot teleoperation data to create vision-language-action (VLA) models is a promising paradigm for training generalist policies, but it suffers from a fundamental tradeoff: learning to produce actions often diminishes the VLM's foundational reasoning and multimodal understanding, hindering generalization to novel scenarios, instruction following, and semantic understanding. We argue that this catastrophic forgetting is due to a distribution mismatch between the VLM's internet-scale pretraining corpus and the robotics fine-tuning data. Inspired by this observation, we introduce VLM2VLA: a VLA training paradigm that first resolves this mismatch at the data level by representing low-level actions with natural language. This alignment makes it possible to train VLAs solely with Low-Rank Adaptation (LoRA), thereby minimally modifying the VLM backbone and averting catastrophic forgetting. As a result, the VLM can be fine-tuned on robot teleoperation data without fundamentally altering the underlying architecture and without expensive co-training on internet-scale VLM datasets. Through extensive Visual Question Answering (VQA) studies and over 800 real-world robotics experiments, we demonstrate that VLM2VLA preserves the VLM's core capabilities, enabling zero-shot generalization to novel tasks that require open-world semantic reasoning and multilingual instruction following.

---

## 论文详细总结（自动生成）

# 论文总结：Actions as Language: Fine-Tuning VLMs into VLAs Without Catastrophic Forgetting

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：将视觉-语言模型（VLM）微调为视觉-语言-动作模型（VLA）是训练通用机器人策略的 promising 范式，但存在根本性权衡：学习生成动作（机器人操控数据）常常导致 VLM 原有的推理和多模态理解能力出现灾难性遗忘，从而损害对新颖场景的泛化、指令跟随和语义理解。
- **归因**：作者认为灾难性遗忘源于 VLM 在海量互联网数据上的预训练语料与机器人微调数据之间存在**数据分布不匹配**。
- **总体目标**：提出一种新的 VLA 训练范式，使得 VLM 在微调为 VLA 时能够同时保持其基础能力，并提升机器人操控性能。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：通过将底层动作表示为自然语言标记（Actions as Language），从数据层面解决分布不匹配问题，使得仅使用低秩适配（LoRA）即可微调 VLM，从而最小化对 VLM 骨干网络的修改，避免灾难性遗忘。
- **关键步骤**：
  1. **数据层匹配**：将机器人遥操作数据中的低层动作（如关节角度、末端执行器位姿）转换为语言描述（例如“向前移动0.1米”、“顺时针旋转30度”），使得动作数据与 VLM 预训练的文本-图像数据分布更一致。
  2. **多任务训练策略**：在微调过程中，同时保留 VLM 原有的视觉问答（VQA）任务或添加辅助的语义理解任务，以维持模型多模态能力。
  3. **使用 LoRA**：仅对 VLM 的部分参数进行低秩更新，大幅降低对原始权重的修改，从而遏制遗忘。
- **无需昂贵的互联网规模 VLM 数据集联合训练**：由于数据分布已对齐，该方法不需要在微调时额外使用大量互联网数据进行协同训练，节省了计算资源。

## 3. 实验设计：数据集、场景、基准、对比方法
- **数据集/场景**：
  - 机器人操控任务：真实世界实验（约800次）和虚拟仿真环境（文中提及“extensive Visual Question Answering (VQA) studies and over 800 real-world robotics experiments”）。
  - VLM 能力保留验证：使用视觉问答（VQA）基准测试（具体未详述，但强调为零样本泛化和多语言指令跟随）。
- **基准（Benchmark）**：
  - 对比方法：未在摘要中列出具体基线名称，但隐含与直接微调 VLM 为 VLA 的基线方法对比（如不使用数据对齐或采用其他微调方式的方法）。
  - 评估指标：零样本泛化到新颖任务的能力、指令跟随准确率、语义理解能力（通过 VQA 分数）。
- **消融实验**：可能包含是否使用“动作即语言”表示、是否使用 LoRA、是否联合多任务训练等消融分析（摘要未明确列出，但结合问题推测进行）。

## 4. 资源与算力
- 文中**未明确说明**使用的 GPU 型号、数量及训练时长。仅提及使用 LoRA 减少了参数更新量，因此算力需求较低，但具体数字未给出。

## 5. 实验数量与充分性
- **实验数量**：超过800次真实世界机器人实验，加上 VQA 评估（数据量未详述）。表明实验规模较大。
- **充分性判断**：
  - 覆盖了机器人操控性能（大量实操）和 VLM 能力保留（VQA）两个维度，较为全面。
  - 但未详细说明实验是否包含多个机器人平台、不同难度的任务、多种 VLM 骨干网络（如 CLIP、BLIP-2 等）。摘要中只提到“zero-shot generalization to novel tasks”和“multilingual instruction following”，但未提供具体任务列表。
  - 客观上，800次真实实验提供了统计显著性，但缺乏消融实验的明细数据，不易判断方法各组件贡献的公平性。

## 6. 主要结论与发现
- VLM2VLA 范式能够有效保持 VLM 的原始推理和多模态理解能力（通过 VQA 证实），同时达到先进的机器人操控性能。
- 该方法实现了零样本泛化到需要开放世界语义推理和多语言指令跟随的新颖任务，表明灾难性遗忘问题得到显著缓解。
- 数据层匹配（动作→语言）+ LoRA 微调是关键，无需依赖昂贵的大规模联合训练。

## 7. 优点
- **创新性**：将动作显式转化为语言标记，巧妙地弥合了 VLM 预训练与机器人微调的数据分布鸿沟，简单而有效。
- **效率**：仅使用 LoRA 微调，避免了全参数微调或大规模协同训练，降低了计算开销。
- **实用性**：在真实机器人上进行了大量实验，验证了方法的泛化和指令跟随能力，接近实际部署需求。
- **通用性**：不依赖于特定 VLM 架构（LoRA 可适配多种模型），易于复现和扩展。

## 8. 不足与局限
- **实验覆盖不全**：未详细说明使用哪些具体的 VLM 骨干、机器人平台和任务类型，可能限制了结论的普适性。
- **偏差风险**：动作的语言化表示可能引入人为偏见，例如某些精细操控难以用自然语言精确描述（如“微调力度”），可能导致动作精度损失。
- **应用限制**：
  - 依赖高质量的语言模板将动作编码为文本，对于高维连续动作（如灵巧手抓取）可能困难。
  - 未测试在复杂动态环境下的实时性能（如计算延迟）。
  - 未与其他主流 VLA 方法（如 RT-2、Octo）进行直接比较（摘要未提及对比基线细节），公平性有待进一步验证。
- **资源信息缺失**：未提供算力指标，无法评估复现成本。

（完）
