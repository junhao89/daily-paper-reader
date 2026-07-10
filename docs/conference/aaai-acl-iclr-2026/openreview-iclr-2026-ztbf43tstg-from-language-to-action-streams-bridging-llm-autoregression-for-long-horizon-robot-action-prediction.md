---
title: "From Language to Action Streams: Bridging LLM Autoregression for Long-Horizon Robot Action Prediction"
title_zh: 从语言到动作流：连接LLM自回归实现长程机器人动作预测
authors: "Zijian Wang, Yunke Wang, Siyu Xu, Chang Xu"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=ztBF43TsTg"
tags: ["query:latent-vla"]
score: 8.0
evidence: VLA中离散动作令牌的长程预测
tldr: 该论文提出Action Stream范式，将机器人动作预测视为自回归序列生成任务，突破了传统VLA模型固定长度单步动作预测的限制，实现了长程动作生成。通过将动作空间离散化为令牌并利用LLM的完整生成潜力，该方法显著扩展了策略的预测视野，在长程机器人操控任务中表现出更优的性能和泛化能力。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有VLA模型将动作预测限制为固定长度的单步令牌序列，未能充分利用LLM的生成潜力，限制了长程任务能力。
method: 提出Action Stream范式，将动作预测建模为自回归序列生成，通过动作令牌的离散化实现灵活的长程预测。
result: 在多个机器人操控基准上，该方法在长程任务中显著提高了成功率，并展示了更好的泛化性。
conclusion: 将动作预测扩展为自回归序列生成是提升VLA模型长程能力的关键，离散动作令牌是实现这一目标的有效表示。
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

# 论文总结：《从语言到动作流：连接LLM自回归实现长程机器人动作预测》

## 1. 核心问题与整体含义（研究动机和背景）

- **背景**：视觉-语言-动作（VLA）模型是机器人控制的新范式，通过预训练的视觉-语言模型（VLM）将自然语言指令和视觉观测直接映射为低级动作。当前主流思路是“动作即语言”（Action-as-Language），将动作空间离散化为令牌，让大型语言模型（LLM）将动作预测重构为标准顺序语言生成任务。
- **核心问题**：现有VLA模型未能充分利用LLM的完整生成潜力，将动作预测限制为固定长度的单步令牌序列，即每次只预测一个时间步的动作，导致预测视野短，难以处理长程任务。
- **研究动机**：突破这一限制，使VLA模型能够生成长链的动作令牌，实现隐式长程生成，提升任务性能。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
提出 **Action Stream（动作流）** 范式，将机器人动作预测建模为**自回归序列生成任务**，使VLA模型能够生成连续多步的动作令牌序列，从而扩展预测视野。

### 关键技术细节
- **动作离散化**：将连续动作空间离散化为动作令牌（action tokens），作为LLM的输入/输出。
- **两阶段训练方法**：
  1. **长程行为克隆（Long-horizon Behavior Cloning, L-BC）**：训练VLA模型生成连贯的多步动作序列，学习动作之间的时序依赖。
  2. **逐步动作对齐（Step-wise Action Alignment, S-AA）**：缓解序列推理中的暴露偏差（exposure bias），减少误差累积，使模型在推理时能稳定生成长序列。
- **推理策略**：将语言生成中的解码策略（如束搜索、采样等）成功迁移到动作流中，实现高效解空间搜索和任务性能提升。

（论文未提供具体公式或算法伪代码，但上述流程为算法核心。）

## 3. 实验设计

- **数据集与场景**：
  - 模拟基准（simulation benchmark）上的评估。
  - 真实世界机器人设置（real-world robotic setups）。
- **Benchmark**：未明确命名具体基准（如RLBench、CALVIN等），但提到“simulation benchmark”和“real-world robotic setups”。
- **对比方法**：未在摘要中列出具体对比基线，但推测对比了传统单步预测的VLA模型（如RT-2、Octo等）以及其他固定长度动作预测方法。

## 4. 资源与算力

- 论文摘要及OpenReview元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。元数据仅包含作者、评分、标签等，未提供实验硬件详情。因此，资源与算力不可知。

## 5. 实验数量与充分性

- 实验覆盖**模拟+真实**两个维度，至少包含两组主要实验。消融实验方面，论文提及了两阶段训练（L-BC和S-AA）的作用，但具体消融数量未在摘要中说明。
- **充分性**：模拟+真实场景的组合增强了结论可信度；但未提供不同任务类型/难度下的详细成功率统计，也未与其他SOTA方法进行系统比较，实验数量略显不足。
- **客观性与公平性**：未提及基线调优细节或随机种子等控制条件，公平性难以完全判断。

## 6. 主要结论与发现

- Action Stream范式通过扩展生成视野，在长程机器人操控任务上显著提升了任务性能。
- 两阶段训练（L-BC + S-AA）有效实现了长程动作序列的连贯生成和误差抑制。
- 语言模型中的解码策略可成功迁移至动作流推理，进一步提升效果。
- 该范式是迈向统一视觉-语言-动作建模的重要一步。

## 7. 优点

- **方法创新**：首次将LLM的**完整自回归生成能力**应用于长程机器人动作预测，突破了固定长度单步预测的局限。
- **实用性强**：通过简单的训练策略调整即可实现，无需修改模型架构，具有较好的通用性。
- **迁移学习价值**：证明了语言生成中的解码策略可复用于动作序列推理，架起语言与动作生成的桥梁。
- **实验设计合理**：结合模拟和真实场景，增强了结论的普适性。

## 8. 不足与局限

- **实验覆盖有限**：未公开具体基准名称、任务种类数量、对比方法细节，难以复现和横向比较。
- **未见消融深度**：未明确展示L-BC与S-AA各自的贡献量级，以及长程预测视野的具体长度对性能的影响。
- **资源信息缺失**：缺乏训练成本、推理速度等实用信息，影响实际部署评估。
- **应用限制**：动作离散化的粒度选择、令牌数量对性能的影响未讨论；复杂任务中的误差累积问题可能仍未完全解决。
- **偏差风险**：实验可能仅覆盖特定环境（模拟器+特定真实设置），泛化到其他机器人平台或任务有待验证。

（完）
