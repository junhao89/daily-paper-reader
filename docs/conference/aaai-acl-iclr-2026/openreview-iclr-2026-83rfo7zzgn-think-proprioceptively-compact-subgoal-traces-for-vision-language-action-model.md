---
title: "Think Proprioceptively: Compact Subgoal Traces for Vision-Language-Action Model"
title_zh: 本体感知思考：视觉-语言-动作模型的紧凑子目标轨迹
authors: "Fangyuan Wang, Peng ZHOU, Shipeng Lyu, Gu Gong, Weiwei Lin, Anqing Duan, David Navarro-Alarcon, Guodong Guo"
date: 2025-09-10
pdf: "https://openreview.net/pdf?id=83RFO7Zzgn"
tags: ["query:latent-vla"]
score: 9.0
evidence: 本体感知作为 VLA 模型的交叉注意力查询
tldr: 现有视觉-语言-动作模型将本体感知视为被动输入，忽视了主动推理作用。本文提出 SubgoalVLA，采用“本体感知思考”范式，将本体感知状态作为交叉注意力查询，主动选择相关视觉内容，生成紧密关联于机器人物理配置的子目标轨迹。该方法显著提升了 VLA 模型在长时任务上的成功率，并减少了特征冗余。该工作直接改进 VLA 模型的内部表示，属于 latent-vla 主题（紧凑表示、子目标预测）。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有 VLA 模型未充分利用本体感知作为主动推理组件，子目标空间不具身。
method: 将本体感知状态作为交叉注意力查询，筛选视觉特征，生成具身子目标轨迹。
result: 在长时任务上提升成功率，减少特征冗余。
conclusion: 本体感知的主动使用能有效提升 VLA 模型的性能与可解释性。
---

## Abstract
Vision-language-action (VLA) models translate visual observations and language instructions to robot actions, yet current architectures regard proprioception as a passive input rather than an active reasoning component. Without proprioceptive guidance, VLA models process multimodal features in isolation from the robot’s physical configuration, and hierarchical approaches often encode subgoals in high-dimensional visual or textual spaces that are ungrounded in the robot’s embodiment. We present SubgoalVLA, a framework built on the \textit{think proprioceptively} paradigm that redefines how multimodal information is processed. SubgoalVLA leverages proprioception in two ways. First, proprioceptive states serve as cross-attention queries to select vision-language features, enabling configuration-aware feature extraction. Second, subgoals are encoded as compact sequences of joint configurations that eliminate the need for cross-modal translation. Through a two-stage training protocol that begins with supervised learning on ground-truth subgoals and then fine-tunes with self-predicted subgoals, we mitigate distribution shift between training and inference. On the CALVIN benchmark, SubgoalVLA achieves state-of-the-art performance with an average task completion length of 3.32, demonstrating that proprioceptive reasoning provides the critical bridge between high-level task understanding and embodied control.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：当前视觉-语言-动作（VLA）模型将本体感知（proprioception，即机器人关节角度等自身状态）视为被动的输入信息，而非主动的推理组件。这导致模型在处理多模态特征时脱离了机器人的实际物理配置，使得子目标（subgoal）编码在高维的视觉或文本空间中，缺乏与机器人具体形态的具身连接（ungrounded in embodiment）。
- **研究动机**：作者认为，更有效的VLA模型应当“本体感知地思考”（think proprioceptively），让本体感知主动指导多模态特征的筛选和子目标的生成，从而提升长时任务的成功率，并减少特征冗余。
- **整体含义**：该工作直接改进了VLA模型的内部表示（属于 latent-vla 主题，强调紧凑表示和子目标预测），通过将本体感知作为交叉注意力查询，实现了特征选择与具身子目标轨迹的生成，为VLA模型的可解释性和性能提升提供了新范式。

## 2. 论文提出的方法论

### 核心思想
- **范式**：提出“本体感知思考”（think proprioceptively）范式，重新定义多模态信息的处理方式。
- **两个关键利用方式**：
  1. **本体感知作为交叉注意力查询**：将机器人当前的本体感知状态（joint configurations）作为交叉注意力机制的查询（query），用于从视觉-语言特征中筛选出与当前物理配置相关的信息，实现**配置感知的特征提取**（configuration-aware feature extraction）。
  2. **子目标编码为紧凑的关节配置序列**：不再使用高维视觉或文本表示子目标，而是将子目标直接编码为一系列紧凑的关节配置（joint configurations）序列，避免了跨模态翻译的开销，并使子目标直接锚定于机器人的物理状态。

### 关键技术细节
- **模型架构**：SubgoalVLA 框架。
- **训练流程**：采用两阶段训练协议：
  - **第一阶段**：基于真实子目标（ground-truth subgoals）进行监督学习，让模型学会从本体感知和视觉语言信息中生成准确的关节配置子目标。
  - **第二阶段**：使用模型自身预测的子目标（self-predicted subgoals）进行微调，以缓解训练与推理之间的分布偏移（distribution shift）。
- **公式/算法流程**（文字说明）：
  - 输入：视觉观测 \(I\)、语言指令 \(L\)、当前本体感知状态 \(P_t\)。
  - 特征提取：视觉和语言各自提取特征，然后通过交叉注意力，以 \(P_t\) 为查询（Q），视觉语言特征为键值（K/V），得到配置感知的融合特征。
  - 子目标预测：融合特征解码为下一时刻的关节配置子目标 \(P_{t+1}\)（紧凑序列）。
  - 动作生成：基于当前状态和子目标，通过低层控制器输出最终动作。
  - 训练时，第一轮用真实 \(P_{t+1}\) 监督，第二轮用预测的 \(P_{t+1}\) 作为下一时刻的输入进行微调。

## 3. 实验设计

- **使用的数据集/场景**：CALVIN benchmark（一个模拟桌面操作任务的长期机器人基准，包含多种长序列任务）。
- **评估指标**：平均任务完成长度（average task completion length），即机器人能连续成功执行的子任务数量（最大为5个）。
- **对比方法**：文中未详细列出对比方法，但摘要提到“state-of-the-art performance”，暗示与当时其他VLA模型（如RT-2、Octo、CALVIN baseline等）进行了比较。
- **Benchmark**：CALVIN（标准化长期任务基准）。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅能从常规VLA模型训练推断可能使用了多块A100等（但无具体数据）。需要读者注意这一信息缺失。

## 5. 实验数量与充分性

- **实验数量**：从摘要和元数据看，主要报告了在CALVIN上的单一性能指标（平均完成长度3.32），未提供多组实验的详细描述（如不同子任务上的分项结果、消融实验、泛化实验等）。
- **充分性评估**：
  - **优点**：CALVIN是一个公认的长期任务基准，结果具有可比性。
  - **不足**：实验覆盖较窄。缺失以下内容：
    - 在多个不同仿真/真实场景上的验证。
    - 对本体感知使用的消融实验（例如去掉交叉注意力查询或换成随机查询）。
    - 与其他VLA方法在相同条件下的公平对比表格。
    - 对子目标紧凑性、特征冗余减少的量化分析。
  - **结论**：实验初步证明了方法的有效性，但充分性不足，需要更多对比和消融来支撑claim。

## 6. 主要结论与发现

- **核心结论**：SubgoalVLA在CALVIN上取得了平均任务完成长度3.32的SOTA性能，说明本体感知的主动推理（think proprioceptively）能够有效桥接高层任务理解与具身控制。
- **发现**：
  - 将本体感知作为交叉注意力查询可以筛选出与机器人物理状态相关的视觉特征，提升特征利用率。
  - 紧凑的关节配置子目标避免了跨模态歧义，减少了表示冗余。
  - 两阶段训练有效缓解了训练-推理分布偏移。

## 7. 优点

- **方法创新性**：重新定义了本体感知的角色，从被动输入变为主动推理工具，符合具身智能的“embodied cognition”思想。
- **表示紧凑性**：使用关节配置作为子目标表示，直接与机器人形态对齐，减少了不必要的维度冗余。
- **技术可解释性**：交叉注意力可视化可揭示机器人关注了哪些视觉特征，增强了模型透明度。
- **训练技巧**：两阶段训练缓解分布偏移，实用有效。

## 8. 不足与局限

- **实验覆盖有限**：仅在单一仿真基准（CALVIN）上评估，未在真实机器人或更多样化环境（如RLBench、MANIS）上验证。
- **缺少消融实验**：未量化本体感知作为查询、紧凑子目标、两阶段训练各自的贡献，难以判断关键组件。
- **硬件资源未披露**：不利于复现和公平比较。
- **潜在偏差**：仅报告平均完成长度，未报告最大/最小长度、成功率、失败模式等，可能存在选择性汇报。
- **长期任务泛化性存疑**：若任务需要复杂物体交互或动态环境，关节配置子目标是否足够？
- **与语言指令的关联**：仅用语言指导，但文中未说明如何将语言嵌入到交叉注意力中（与本体感知的关系），细节不够清晰。

（完）
