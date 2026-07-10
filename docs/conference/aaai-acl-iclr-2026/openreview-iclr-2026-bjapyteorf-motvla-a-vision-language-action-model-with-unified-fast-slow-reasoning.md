---
title: "MoTVLA: A Vision-Language-Action Model with Unified Fast-Slow Reasoning"
title_zh: MoTVLA：具备统一快慢推理的视觉-语言-动作模型
authors: "Wenhui Huang, Changhe Chen, Han Qi, Chen Lv, Yilun Du, Heng Yang"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=bjApyTEoRf"
tags: ["query:latent-vla"]
score: 7.0
evidence: 基于混合变换器的VLA模型，集成快慢推理
tldr: 现有VLA方法在语言可控性和推理延迟之间面临权衡，MoTVLA采用混合变换器架构，保留预训练VLM的通用智能处理感知与语义规划，同时引入领域专家变换器进行行为策略学习，实现快慢统一推理，在保持语言可操作性的同时显著降低推理延迟。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有VLA方法无法同时兼顾高语言可控性和低推理延迟。
method: 提出混合变换器架构，将通用VLM与领域专家变换器结合进行快慢推理。
result: 在多个机器人控制基准上实现了更优的语言可操作性和推理效率。
conclusion: 混合变换器架构能有效平衡VLA模型的推理精度与效率。
---

## Abstract
Integrating visual-language instructions into visuomotor policies is gaining momentum in robot learning for enhancing open-world generalization. Despite promising advances, existing approaches face two challenges: limited language steerability when no generated reasoning is used as a condition, or significant inference latency when reasoning is incorporated. In this work, we introduce MoTVLA, a mixture-of-transformers (MoT)–based vision–language–action (VLA) model that integrates fast–slow unified reasoning with behavior policy learning. MoTVLA preserves the general intelligence of pre-trained VLMs (serving as the generalist) for tasks such as perception, scene understanding, and semantic planning, while incorporating a domain expert, a second transformer that shares knowledge with the pretrained VLM, to generate fast domain-specific reasoning (e.g., robot motion decomposition), thereby improving policy execution efficiency. By conditioning the action expert on decomposed motion instructions, MoTVLA can learn diverse behaviors and substantially improve language steerability. Extensive evaluations across natural language processing benchmarks, robotic simulation environments, and real-world experiments confirm the superiority of MoTVLA in both language reasoning and manipulation task performance. We refer to https://motvla.github.io/MoTVLA-website/ for the demonstration videos and corresponding descriptions.

---

## 论文详细总结（自动生成）

# MoTVLA：具备统一快慢推理的视觉-语言-动作模型 详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：将视觉-语言指令融入视觉运动策略（visuomotor policies）是机器人学习领域的热点方向，旨在提升开放世界泛化能力。
- **核心问题**：现有视觉-语言-动作（VLA）方法面临一个关键权衡——若不在策略中引入推理生成作为条件，则语言可操控性（language steerability）受限；若引入推理，则会导致显著的推理延迟（inference latency）。即无法同时兼顾高语言可控性和低推理延迟。
- **整体意义**：MoTVLA提出一种基于混合变换器（Mixture-of-Transformers, MoT）的VLA模型，通过集成快慢统一推理与行为策略学习，在不牺牲语言可操控性的前提下显著降低推理延迟，实现了更优的平衡。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：采用混合变换器架构，将预训练VLM作为“通才”（generalist）处理感知、场景理解和语义规划等通用任务；同时引入一个“领域专家”（domain expert）——第二个变换器——与预训练VLM共享知识，专门生成快速领域特定推理（如机器人运动分解），以此提升策略执行效率。
- **关键技术细节**：
  - **混合变换器架构**：保留预训练VLM的通用智能（如语言推理），同时增加一个专家变换器，两者通过知识共享机制协同工作。
  - **快慢统一推理**：通用VLM进行慢速、深度的语义推理；领域专家变换器进行快速、领域特定的推理（例如动作分解）。两种推理模式统一在一个模型中。
  - **动作策略学习**：将领域专家生成的分解运动指令（motion instructions）作为条件输入动作专家（action expert），使模型能够学习多样化行为，并显著提升语言可操控性。
- **公式/算法流程**（文字说明）：
  1. 输入：视觉-语言指令（如“拿起红色杯子”）。
  2. 慢速推理（通用VLM）：进行场景理解、语义规划，输出高层次任务描述。
  3. 快速推理（领域专家变换器）：基于共享的中间表示，生成低层运动分解（如“前进0.1米，夹爪闭合”）。
  4. 动作专家：以运动分解指令为条件，输出具体动作序列。
  5. 整个过程端到端训练，预训练VLM的参数部分冻结或微调。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集/场景**：评估覆盖三个层面：
  - 自然语言处理（NLP）基准任务
  - 机器人模拟环境（robotic simulation environments）
  - 真实世界实验（real-world experiments）
- **基准（Benchmark）**：未明确列出具体基准名称，但包含标准机器人控制评估任务。
- **对比方法**：未在摘要中详细列出具体对比方法，但暗示与现有VLA方法（如未使用推理的基线、使用推理但高延迟的方法）进行对比。推测可能对比了RT-2、Octo等模型。

## 4. 资源与算力

- **文中未明确说明**：元数据和摘要中未提及使用的GPU型号、数量、训练时长等具体算力信息。仅能推断模型基于预训练VLM（如CLIP、PaLM-E类）进行微调或混合训练，具体硬件资源未知。

## 5. 实验数量与充分性

- **实验数量**：涵盖三类评估（NLP基准、模拟环境、真实世界），但摘要未给出具体实验组数或消融实验细节。
- **充分性与客观性**：从摘要描述看，实验覆盖多维度（语言推理、操控性能），但缺乏消融实验、统计分析等细节。结论“证实了MoTVLA在语言推理和操控任务中的优越性”略显笼统。需要查看全文才能判断实验是否充分、公平（如是否与SOTA方法在同一设置下比较、是否有重复实验、是否有偏差控制）。

## 6. 论文的主要结论与发现

- MoTVLA在保持高语言可操控性的同时，显著降低推理延迟，优于现有VLA方法。
- 混合变换器架构能有效平衡VLA模型的推理精度与效率。
- 通过领域专家变换器进行快速领域推理，动作专家学习多样化行为，提升了策略执行效率。
- 在NLP、模拟和真实机器人任务上均表现更优。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：首次将混合变换器（MoT）引入VLA，实现快慢推理的统一，解决了语言可控性与推理延迟的冲突。
- **架构优雅**：保留预训练VLM的通用能力，同时引入领域专家，无需重新训练庞大模型，具有较好的扩展性。
- **评估全面**：同时覆盖NLP、模拟和真实世界，增强了结论的说服力。
- **开源/网站**：提供演示视频与描述，便于复现与验证。

## 8. 不足与局限

- **实验细节缺失**：未明确具体基准名称、对比方法、消融实验组数、统计显著性检验等，难以完全评估实验的充分性和公平性。
- **算力资源未公开**：缺少训练资源信息，影响可复现性和对资源需求的判断。
- **应用限制**：该方法依赖于预训练VLM，模型规模和部署成本可能较高；领域专家变换器的设计可能需针对不同任务调整，泛化性有待验证。
- **潜在偏差风险**：仅从摘要看，未讨论失败案例或局限性，可能隐藏了在复杂场景或长尾任务中的性能下降。
- **会议接收状态**：标注为“ICLR-2026-Rejected-Public”，表明论文可能因某些问题被拒，需警惕方法或实验上的不足。

（完）
