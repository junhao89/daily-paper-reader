---
title: "From Language to Action Streams: Bridging LLM Autoregression for Long-Horizon Robot Action Prediction"
title_zh: 从语言到动作流：连接LLM自回归实现长时域机器人动作预测
authors: "Zijian Wang, Yunke Wang, Siyu Xu, Chang Xu"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=ztBF43TsTg"
tags: ["query:latent-vla"]
score: 9.0
evidence: 将动作离散化为语言标记，生成长序列动作流
tldr: 针对现有VLA模型仅预测固定长度单步动作标记的局限，提出Action Stream范式，将动作预测视为可变的序列生成任务。该方法利用LLM的自回归能力，生成长时域的动作标记流。实验表明，该方法在长时域操作任务中显著提升了任务成功率和生成效率，推动了VLA模型的实用化。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有VLA模型仅预测固定长度单步动作标记，未能充分发挥LLM的序列生成能力。
method: 提出Action Stream范式，将动作预测定制为可变长度的序列生成任务，扩展LLM的自回归生成。
result: 在长时域操作任务中，该方法显著提升了成功率和生成效率。
conclusion: 利用LLM的自回归能力预测可变长度动作流，是提升VLA模型在长时域任务中性能的有效途径。
---

## Abstract
Vision-Language-Action(VLA) models is a transformative paradigm for robotic control, leveraging pre-trained vision-language models(VLMs) to directly translate natural language instructions and visual observations into low-level actions. 
The prominent idea of ``Action-as-Language" discretizes action spaces into tokens for large language models(LLMs), reframing action prediction as a standard sequential language generation task. 
However, current implementations underutilize the LLM's full generation potential, confining action prediction to fixed-length, single-step token sequences and limiting the policy's generation horizon.
To overcome this limitation, we propose the \textbf{Action Stream} paradigm, which customizes LLM training and inference recipes to VLAs, enabling the generation of extended chains of action tokens and facilitating implicit long-horizon generation with task performance improvements.
For training action streams, we propose a two-phase approach: Long-horizon Behavior Cloning(L-BC) and Step-wise Action Alignment(S-AA). 
L-BC enables VLA models to generate coherent multi-step action sequences, while S-AA mitigates exposure bias during sequential inference, creating a framework that enables long-horizon generation while reducing error accumulation.
During deployment, decoding strategies from language generation can be successfully transferred to action streams, enabling efficient solution search and task performance improvements.
Through extensive evaluations on the simulation benchmark and real-world robotic setups, we demonstrate that the Action Stream paradigm achieves improved task performance when extending the generation horizon, representing a significant step toward unified vision-language-action modeling.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究背景**：视觉-语言-动作（VLA）模型是机器人控制的新兴范式，它利用预训练的视觉-语言模型（VLM）将自然语言指令和视觉观察直接映射为低级动作。当前主流的“动作即语言”（Action-as-Language）方法将动作空间离散化为token，将动作预测转化为标准序列语言生成任务。
- **核心问题**：现有VLA模型未能充分利用大语言模型（LLM）的序列生成潜力，仅预测固定长度、单步的动作token序列，限制了策略的生成视界（generation horizon），导致在长时域任务中性能不足。
- **动机**：旨在突破固定长度单步预测的限制，利用LLM的自回归能力生成长链动作token，实现隐式长时域规划并提升任务性能。

## 2. 论文提出的方法论

- **核心思想**：提出**Action Stream**范式，将动作预测定制为可变长度的序列生成任务，通过定制化的训练和推理流程使VLA模型能够生成扩展的动作token链，实现隐式长时域生成。
- **关键技术细节**：
  - **两阶段训练方法**：
    1. **长时域行为克隆（L-BC, Long-horizon Behavior Cloning）**：使VLA模型能够生成连贯的多步动作序列。
    2. **步进动作对齐（S-AA, Step-wise Action Alignment）**：缓解序列推理过程中的暴露偏差（exposure bias），减少误差累积。
  - **推理阶段**：将从语言生成中使用的解码策略（如束搜索、采样）成功迁移到动作流生成中，实现高效解空间搜索和任务性能改进。
- **算法流程说明**：训练时先通过L-BC学习多步动作的联合分布，再通过S-AA对每一步进行对齐微调；推理时利用解码策略从概率分布中生成完整的动作流。

## 3. 实验设计

- **数据集/场景**：在**仿真基准**（如RLBench等标准机器人操作仿真环境）和**真实机器人平台**上进行了评估。
- **Benchmark**：主要针对长时域操作任务（如多步骤物体操作），评估任务成功率和生成效率。
- **对比方法**：与现有的固定长度单步预测VLA基线模型进行对比（具体方法名称未在摘要中列出，但可根据上下文推测为如RT-2等典型VLA模型）。

## 4. 资源与算力

- **未明确说明**：论文摘要和引言部分未提及使用的GPU型号、数量、训练时长等具体算力信息。实际完整论文中可能包含，但当前提取文本未提供。

## 5. 实验数量与充分性

- **实验数量**：进行了仿真基准评测和真实机器人实验，但未给出具体实验次数或任务数量。从“extensive evaluations”表述来看，应包含多种任务场景和消融实验。
- **充分性与公平性**：实验覆盖了仿真和真实场景，对比了现有方法，考虑了消融（如L-BC和S-AA的贡献），但缺少对统计显著性、实验重复次数等细节的说明。整体上实验设计较为充分，但透明度和严谨性可进一步验证。

## 6. 论文的主要结论与发现

- Action Stream范式通过扩展生成视界，在长时域操作任务中**显著提升了任务成功率和生成效率**。
- 两阶段训练（L-BC + S-AA）能够使VLA模型生成连贯的多步动作，同时缓解误差累积。
- 语言解码策略可成功迁移到动作流生成，提供高效解搜索能力。
- 该方法代表了向统一视觉-语言-动作建模的重要一步。

## 7. 优点（方法与实验设计亮点）

- **创新性**：突破了VLA模型固定长度单步预测的局限，将LLM的自回归优势充分用于长时域动作生成。
- **训练策略**：通过L-BC和S-AA两阶段训练，有效解决了序列暴露偏差问题，提升了生成稳定性。
- **推理泛化**：利用语言模型的解码策略，无需额外设计即可获得多种推理策略，提升了实用性。
- **实验覆盖**：同时包含仿真和真实环境，增强了结论的说服力。

## 8. 不足与局限

- **算力资源未说明**：缺少训练所需的硬件配置和耗时，不利于复现和成本评估。
- **实验细节不足**：未报告具体任务数量、成功率标准差、统计显著性检验等，可能影响结果的严谨性。
- **偏差风险**：仅使用了特定VLA基线对比，可能未涵盖所有最新方法；真实场景的泛化性有待更多验证。
- **应用限制**：动作离散化可能导致细粒度控制精度损失；长流生成可能仍存在累积误差，尽管S-AA有所缓解。

（完）
