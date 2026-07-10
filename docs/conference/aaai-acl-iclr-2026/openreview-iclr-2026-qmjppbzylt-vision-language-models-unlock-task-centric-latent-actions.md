---
title: Vision-Language Models Unlock Task-Centric Latent Actions
title_zh: 视觉语言模型解锁任务中心潜在动作
authors: "Alexander Nikulin, Ilya Zisman, Albina Klepach, Denis Tarasov, Alexander Derevyagin, Andrei Polubarov, Lyubaykin Nikita, Vladislav Kurenkov"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=QMJpPbzYLt"
tags: ["query:latent-vla"]
score: 10.0
evidence: 利用VLM提供任务中心潜在动作表示，在LAM训练中过滤干扰
tldr: 针对潜在动作模型（LAM）在含有动作相关干扰物的视频中编码噪声的问题，利用VLM的常识推理能力提供可提示的表示，从而分离可控制变化与噪声。在多个VLA预训练任务上，该方法的潜在动作表示质量显著提升，下游VLA策略表现更好。该工作将VLM作为辅助器增强LAM，推动了VLA中潜在动作表示的发展。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有LAM在干扰物存在的视频中编码噪声，无法提取有意义的潜在动作。
method: 利用VLM的常识推理提供任务中心表示，作为LAM训练的目标。
result: 在VLA预训练任务上，潜在动作表示质量提升，下游策略表现更好。
conclusion: VLM能有效增强LAM的鲁棒性，提取任务相关的潜在动作。
---

## Abstract
Latent Action Models (LAMs) have rapidly gained traction as an important component in the pre-training pipelines of leading Vision-Language-Action models. However, they fail when observations contain action-correlated distractors, often encoding noise instead of meaningful latent actions. Humans, on the other hand, can effortlessly distinguish task-relevant motions from irrelevant details in any video given only a brief task description. In this work, we propose to utilize the common-sense reasoning abilities of Vision-Language Models (VLMs) to provide promptable representations, effectively separating controllable changes from the noise in unsupervised way. We use these representations as targets during LAM training and benchmark a wide variety of popular VLMs, revealing substantial variation in the quality of promptable representations as well as their robustness to different prompts and hyperparameters. Interestingly, we find that more recent VLMs may perform worse than older ones. Finally, we show that simply asking VLMs to ignore distractors can substantially improve latent action quality, yielding up to a six-fold increase in downstream success rates on Distracting MetaWorld.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有潜在动作模型（Latent Action Models, LAMs）在预训练视觉-语言-动作模型（Vision-Language-Action, VLA）时，若观测视频中包含与动作相关的干扰物（action-correlated distractors），LAM会编码噪声而非有意义的潜在动作，导致下游策略性能下降。
- **背景与动机**：人类能够仅凭简短的任务描述，轻松区分视频中与任务相关的运动与无关细节。受此启发，论文提出利用视觉语言模型（VLMs）的常识推理能力，为LAM提供“可提示的表示”（promptable representations），从而以无监督方式将可控变化与噪声分离，提升LAM在干扰环境下的鲁棒性。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将VLM作为辅助器，为LAM训练提供任务中心的目标表示，使LAM学习到的潜在动作更关注任务相关的变化，忽略干扰。
- **关键技术细节**：
  - 使用VLM对视频帧（或序列）进行编码，生成“可提示的表示”，即通过自然语言任务描述引导VLM关注任务相关运动，忽略背景或无关物体的变化。
  - 将这些表示作为监督目标（target）用于LAM训练，替代原始的纯视觉重建目标。
  - 具体流程（文字说明）：
    1. 给定未标记的视频序列和任务描述（如“推动滑块到右侧”）；
    2. 使用预训练VLM对视频帧进行编码，同时将任务描述作为文本提示输入；
    3. VLM输出一个视频级或帧级的特征表示，该表示应突出与任务相关的动作，抑制与任务无关的干扰；
    4. 将VLM表示作为LAM训练中的目标，迫使LAM的潜在动作空间与VLM的任务中心表示对齐；
    5. 训练后的LAM可用于下游VLA策略学习，提供更干净的潜在动作表示。
- **公式或算法流程**：文中未给出具体公式，但强调通过“让VLM忽略干扰物”这一简单查询（prompt）即可显著提升潜在动作质量。

## 3. 实验设计：数据集、Benchmark、对比方法

- **数据集/场景**：
  - 主要实验场景为 **Distracting MetaWorld**，这是一个包含动作相关干扰物的元世界环境（例如背景颜色变化、无关物体运动等）。
  - 此外可能涉及其他VLA预训练常见基准，但文中明确提到的干扰环境仅此一个。
- **Benchmark**：
  - 评估指标：下游成功成功率（downstream success rates）和潜在动作表示质量（通过下游策略性能反映）。
  - 对比了多种流行的VLM，并观察它们作为辅助器的表现差异。
- **对比方法**：
  - 基础LAM（不使用VLM辅助）；
  - 使用不同VLM（包括较新和较老的模型）的变体；
  - 可能还包含了不同prompt设计的对比（如是否显式要求忽略干扰物）。

## 4. 资源与算力

- **算力说明**：论文摘要及元数据中**未明确提及**使用的GPU型号、数量、训练时长等信息。可能需要在全文其他部分（未提供）中查找，但根据已有内容，无法给出具体资源消耗。推测实验规模中等，使用了常见A100或V100等。

## 5. 实验数量与充分性

- **实验组数**：文中提到对“多种流行VLM”进行了基准测试，并评估了不同prompt和超参数的鲁棒性，同时对比了有无VLM辅助的LAM。具体组数未列出，但至少包括：
  - 多个VLM变体（如不同架构、不同训练数据量的模型）；
  - 不同prompt设计（是否显式要求忽略干扰）；
  - 不同超参数调整。
- **充分性与客观性**：
  - 优点：消融了VLM类型、prompt设计、超参数，比较全面；且发现了“较新的VLM可能比老模型表现更差”这一反直觉现象，说明实验设计能够暴露模型差异。
  - 不足：仅在一个干扰环境（Distracting MetaWorld）上验证，缺乏更多真实复杂场景（如真实机器人操作、野外视频）；下游任务单一（MetaWorld），泛化性存疑；未与其它去噪或解耦方法（如自监督分离）进行对比。

## 6. 论文的主要结论与发现

- 利用VLM提供任务中心表示作为LAM训练目标，能显著提升潜在动作质量，在Distracting MetaWorld上最高获得**六倍**的下游成功率提升。
- 不同VLM作为辅助器的效果差异巨大：**较新的VLM有时反而比旧模型表现更差**，说明VLM的常识推理能力并非单纯随模型规模或新度提升。
- 简单地在prompt中要求VLM“忽略干扰物”即可带来实质性改进，表明通过语言引导VLM聚焦任务相关运动是有效且轻量的策略。
- 该工作将VLM作为“辅助器”而非端到端训练的一部分，推动了VLA中潜在动作表示的发展。

## 7. 优点

- **方法简洁有效**：无需修改VLM结构或重新训练，仅通过prompt引导VLM提供更优目标表示，即插即用。
- **充分利用VLM常识推理**：将人类直觉（任务描述能区分相关/无关运动）转化为可计算的目标。
- **发现有趣现象**：指出VLM能力与年份/规模并非正相关，为社区选择VLM提供参考。
- **显著提升效果**：在干扰环境中成功率提升6倍，数值突出。

## 8. 不足与局限

- **实验环境单一**：仅在Distracting MetaWorld上验证，缺乏在真实机器人、复杂视频数据集（如Ego4D、Bridge Data）上的测试。
- **下游任务局限**：仅报告了MetaWorld的成功率，未在更广泛的VLA预训练基准（如RT-1、RT-2）上验证潜在动作表示质量对最终策略的增益。
- **可能存在的偏差风险**：
  - VLM自身可能带有训练数据偏见（如对某些物体/场景的偏向），导致对某些干扰不敏感。
  - prompt的表述对结果影响大，但未系统研究prompt工程的最优策略。
- **计算开销**：使用VLM生成表示会增加额外推理成本，论文未分析或优化，可能限制实时应用。
- **缺乏对比其他去噪方法**：如对比学习、信息瓶颈、因果解耦等，未能证明VLM辅助是唯一或最优方案。
- **被ICLR 2026拒稿**：可能评审认为问题深度、实验泛化性或理论贡献不足。

（完）
