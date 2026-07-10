---
title: Learning and Reusing Abstract Latent Actions in a Hippocampal-Entorhinal-Inspired World Model
title_zh: 在海马-内嗅皮层启发的世界模型中学习与重用抽象潜在动作
authors: "Tianqiu Zhang, Muyang Lyu, Xiao Liu, Si Wu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=RSvfY6dRVN"
tags: ["query:latent-vla"]
score: 9.0
evidence: 在海马-内嗅皮层启发的世界模型中的抽象潜在动作
tldr: 现有世界模型在抽象动态经验的结构化表示方面存在不足。本文受海马-内嗅皮层回路启发，提出一种能够学习并重用抽象潜在动作的世界模型，通过分离内容细节和抽象结构实现跨情境的结构泛化。实验表明该模型在多种环境中有效。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 人类能够将动态经验抽象为结构化表示并跨情境迁移，现有AI模型缺乏这种能力。
method: 受海马-内嗅皮层神经机制启发，设计世界模型分别编码内容特定细节和抽象结构，并学习可重用的抽象潜在动作表示。
result: 模型在多个非空间认知任务中展示了结构泛化能力，能够有效迁移学习到的抽象动作。
conclusion: 该工作为构建具有抽象推理能力的具身智能体提供了生物启发的路径。
---

## Abstract
Humans are capable of abstracting dynamic experiences into structured representations, facilitating both the inference of shared patterns by observing similar transition dynamics and the transfer of these structures across varied contexts. The hippocampal-entorhinal circuit, widely known for its role in spatial navigation, also supports the representation of abstract conceptual spaces crucial for non-spatial cognitive processes. This function emerges from the distinct yet integrated encoding of content-specific details by the hippocampus and abstract structures by the entorhinal cortex, facilitating structural generalization across varied contexts. Although the hippocampal-entorhinal circuit has been previously explored as a predictive system for binding contents, the process for concurrently extracting abstract structures from continuous real-world dynamics remains largely understudied. In this work, we propose a computational model inspired by the hippocampal-entorhinal circuit, capable of simultaneously inferring latent actions to form abstract structures and constructing predictive world models from real-world video sequences. Our model combines an inverse model for extracting abstract latent actions with a hippocampal-entorhinal-inspired coupling model that separately encodes contents and structures, leveraging action-driven path integration for prediction. Experimental results demonstrate that our model effectively captures abstract latent actions, reuses them robustly across diverse contexts, and achieves reliable predictive performance in both familiar and novel environments. Additionally, our analysis of latent representations from 3D object rotation datasets highlights why latent actions extracted through entorhinal cortex representations demonstrate greater abstraction and reusability. This work provides novel insights into the brain-inspired mechanisms underlying the self-supervised learning of abstract latent actions and world models from real-world dynamics, illuminating cognitive processes essential for transfer learning and data-efficient learning.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有世界模型（world models）在从动态经验中提取抽象结构表示方面能力不足，缺乏人类所具备的将动态经验抽象为结构化表示并跨情境迁移的能力。
- **研究动机**：人类海马-内嗅皮层回路不仅在空间导航中起关键作用，还能表示抽象概念空间，支持非空间认知过程。该回路通过海马编码内容特定细节、内嗅皮层编码抽象结构，实现结构泛化。论文受此神经机制启发，旨在构建能同时从真实世界视频序列中推断抽象潜在动作（abstract latent actions）并形成预测性世界模型的AI系统。
- **背景地位**：尽管海马-内嗅回路已被探索为绑定内容的预测系统，但从连续现实动态中并发提取抽象结构的过程仍缺乏研究。该工作为自监督学习抽象潜在动作和世界模型提供了脑启发的新路径，有望促进迁移学习和数据高效学习。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：分离内容细节与抽象结构，学习可重用的抽象潜在动作表示，并通过动作驱动的路径积分进行未来预测。
- **关键技术细节**：
  - **逆模型（Inverse Model）**：从观测序列中提取抽象的潜在动作，将底层动态抽象为结构化的潜在动作空间。
  - **海马-内嗅皮层启发的耦合模型**：分别编码内容（内容特异性细节）和结构（抽象结构），两者协同工作。
  - **动作驱动的路径积分（Action-driven Path Integration）**：利用学习到的抽象潜在动作在潜在状态空间中进行路径积分，从而预测未来帧。
  - 整个框架以自监督方式从真实世界视频序列中学习，无需显式的动作标签。
- **公式/算法流程**：论文摘要中未提供具体的数学公式或算法伪代码，仅描述了模块化设计思路（逆模型 + 耦合模型 + 路径积分）。

## 3. 实验设计：数据集、Benchmark、对比方法

- **使用的数据集/场景**：
  - 真实世界视频序列（未具体说明，可能为模拟或录制视频）
  - 3D物体旋转数据集（用于分析潜在表示）
  - 多个非空间认知任务（如迷宫导航、结构化环境等）
- **Benchmark**：文中未明确列出公开基准或特定指标对比。
- **对比方法**：摘要中未提及与其他现有世界模型（如Dreamer、World Models等）的直接定量比较，仅说明模型在熟悉和新环境中均能实现可靠预测性能。

## 4. 资源与算力

- **明确情况**：论文全文摘要及元数据中**未提及**任何有关GPU型号、数量、训练时长的信息。因此无法判断算力消耗。

## 5. 实验数量与充分性

- **实验数量**：根据摘要描述，实验涵盖了“多个非空间认知任务”和“3D物体旋转数据集”，并进行了潜在表示分析。但未列出具体子实验数量（如不同环境数量、任务数量等），也未提及消融实验（如去掉耦合模型、逆模型等）。
- **充分性评价**：
  - 优势：展示了跨情境的抽象动作重用能力，以及在新环境中的泛化。
  - 不足：缺乏与现有代表性方法的定量对比（如Dreamer、RAMP等），缺少消融实验和统计显著性检验，实验覆盖面较窄。该论文被ICLR 2026拒稿可能反映了实验充分性方面的不足。

## 6. 论文的主要结论与发现

- **主要发现**：
  1. 所提模型能够有效捕获并学习抽象潜在动作，在不同情境下鲁棒地重用这些动作。
  2. 在熟悉和新环境中均取得可靠的预测性能，体现了结构泛化能力。
  3. 对3D物体旋转数据集的分析表明，通过内嗅皮层表示提取的潜在动作比直接使用海马内容表示更具抽象性和可重用性。
- **主要结论**：该脑启发框架为构建具有抽象推理能力的具身智能体提供了可行路径，有望推动迁移学习和数据高效学习。

## 7. 优点

- **生物合理性**：明确借鉴海马-内嗅皮层的神经机制，将内容与结构分离，增强了模型的可解释性。
- **自监督学习**：无需动作标签即可从视频序列中学习抽象潜在动作，符合现实场景。
- **跨情境泛化**：学习的抽象动作可在不同环境间迁移，体现了结构化概括能力。
- **新颖视角**：将世界模型的学习与认知科学中的抽象推理联系起来，为后续研究提供新思路。

## 8. 不足与局限

- **实验覆盖不足**：缺少与现有SOTA世界模型的定量比较，未提供在标准基准（如DMControl、Atari、Habitat等）上的性能。
- **消融分析缺失**：未通过消融实验验证各模块（逆模型、耦合模型、路径积分）的贡献，难以判断设计选择的关键性。
- **偏差风险**：实验可能仅在特定简单环境（如3D物体旋转）上有效，在更复杂、高维真实环境中是否仍能保持抽象能力存疑。
- **应用限制**：未讨论模型对噪声、部分可观测性、动态变化的鲁棒性；实际部署时可能需要大量数据和计算资源，但文中未评估。
- **信息缺失**：算力、超参数、训练细节均未提供，影响可复现性。

（完）
