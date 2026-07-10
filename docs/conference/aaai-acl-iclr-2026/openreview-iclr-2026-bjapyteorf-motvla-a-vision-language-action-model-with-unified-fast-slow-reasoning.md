---
title: "MoTVLA: A Vision-Language-Action Model with Unified Fast-Slow Reasoning"
title_zh: MoTVLA：具有统一快慢推理的视觉-语言-动作模型
authors: "Wenhui Huang, Changhe Chen, Han Qi, Chen Lv, Yilun Du, Heng Yang"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=bjApyTEoRf"
tags: ["query:latent-vla"]
score: 7.0
evidence: 混合变换器VLA模型结合快慢推理
tldr: 现有VLA模型在语言可操纵性和推理延迟上存在权衡。MoTVLA采用混合变换器架构，将预训练VLM作为通用感知模块，并引入领域专家变换器进行行为学习，实现统一快慢推理。实验表明该方法在开放世界泛化任务上取得更好性能，同时保持低推理延迟。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 解决VLA模型语言可操纵性与推理延迟之间的矛盾。
method: 提出混合变换器架构，融合通用VLM和领域专家变换器，实现快慢推理统一。
result: 在开放世界机器人任务中实现了更好的泛化性能和更低的推理延迟。
conclusion: 统一快慢推理是提升VLA模型实用性的有效途径。
---

## Abstract
Integrating visual-language instructions into visuomotor policies is gaining momentum in robot learning for enhancing open-world generalization. Despite promising advances, existing approaches face two challenges: limited language steerability when no generated reasoning is used as a condition, or significant inference latency when reasoning is incorporated. In this work, we introduce MoTVLA, a mixture-of-transformers (MoT)–based vision–language–action (VLA) model that integrates fast–slow unified reasoning with behavior policy learning. MoTVLA preserves the general intelligence of pre-trained VLMs (serving as the generalist) for tasks such as perception, scene understanding, and semantic planning, while incorporating a domain expert, a second transformer that shares knowledge with the pretrained VLM, to generate fast domain-specific reasoning (e.g., robot motion decomposition), thereby improving policy execution efficiency. By conditioning the action expert on decomposed motion instructions, MoTVLA can learn diverse behaviors and substantially improve language steerability. Extensive evaluations across natural language processing benchmarks, robotic simulation environments, and real-world experiments confirm the superiority of MoTVLA in both language reasoning and manipulation task performance. We refer to https://motvla.github.io/MoTVLA-website/ for the demonstration videos and corresponding descriptions.

---

## 论文详细总结（自动生成）

# MoTVLA：具有统一快慢推理的视觉-语言-动作模型——详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有视觉-语言-动作（VLA）模型在语言可操纵性与推理延迟之间存在根本性权衡。若模型不使用生成的推理作为条件，则语言可操纵性有限；若引入推理过程，则显著增加推理延迟。
- **研究背景**：将视觉-语言指令集成到视觉运动策略中，是提升机器人开放世界泛化能力的重要方向。然而，现有方法难以同时兼顾高语言可操纵性和低推理延迟。
- **研究动机**：旨在设计一种VLA模型，能够统一快慢推理（fast-slow reasoning），在不牺牲语言可操纵性的前提下保持实时执行效率。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：提出混合变换器（Mixture-of-Transformers, MoT）架构，将预训练视觉-语言模型（VLM）作为通用感知模块（通用专家），同时引入一个领域专家变换器，两者共享知识，实现快慢推理的统一。
- **关键技术细节**：
  - **通用专家（Generalist）**：保留预训练VLM的通用智能，负责感知、场景理解、语义规划等慢速推理任务。
  - **领域专家（Domain Expert）**：第二个变换器，从预训练VLM共享知识，擅长生成快速的领域特定推理（如机器人动作分解），从而提高策略执行效率。
  - **动作专家（Action Expert）**：基于领域专家分解出的运动指令进行动作生成，学习多样化行为，提升语言可操纵性。
  - **工作流程**：先由VLM进行慢速通用推理，再由领域专家进行快速推理生成动作分解指令，最后动作专家据此执行策略，实现统一快慢推理。
- **公式/算法流程**（文字说明）：
  - 输入：视觉-语言指令
  - 步骤1：通用VLM处理输入，输出高层语义和感知结果（慢速）
  - 步骤2：领域专家利用共享知识，基于VLM输出生成快速动作分解指令（如机器人运动分解）
  - 步骤3：动作专家以分解指令为条件，生成具体动作序列
  - 最终输出：控制机器人执行动作

## 3. 实验设计：数据集/场景、基准、对比方法
- **实验场景**：
  - 自然语言处理基准（NLP benchmarks）
  - 机器人仿真环境（robotic simulation environments）
  - 真实世界实验（real-world experiments）
- **基准（Benchmark）**：未明确列出具体数据集名称，但涉及NLP、仿真和真实场景。
- **对比方法**：未在摘要中明确列出具体对比方法，但提及与现有VLA模型比较，验证了MoTVLA在语言推理和操作任务性能上的优越性。

## 4. 资源与算力
- **文中说明**：论文元数据和摘要中未明确提及使用的GPU型号、数量、训练时长等算力信息。仅提供了项目网站链接，可能包含更多细节，但在此处未提供。

## 5. 实验数量与充分性
- **实验数量**：摘要提到“广泛的评估”，包括NLP基准、机器人仿真环境和真实世界实验，至少三个主要场景。未给出具体消融实验数量，但暗示包含对比实验和可能的效果验证。
- **充分性评价**：实验覆盖了从抽象语言理解到具体机器人操作的多个层面，具有一定全面性。但由于论文被ICLR 2026拒稿，可能实验设计存在未公开的不足，或需更严格的消融和泛化测试。仅从摘要看，实验设置客观，但缺乏细节（如样本量、重复次数），无法完全判断公平性。

## 6. 论文的主要结论与发现
- **主要结论**：MoTVLA通过统一快慢推理的混合变换器架构，在保持低推理延迟的同时，显著提升了开放世界机器人任务中的泛化性能和语言可操纵性。
- **具体发现**：
  - 快慢推理的统一能够有效解决语言可操纵性与延迟之间的权衡。
  - 领域专家变换器通过共享知识实现了快速领域特定推理，提高了执行效率。
  - 基于分解动作指令的条件化学习，使模型能够更好地学习多样化行为。

## 7. 优点：方法或实验设计上的亮点
- **方法亮点**：
  - 独创的混合变换器（MoT）架构，将通用VLM与领域专家融合，实现快慢推理统一，思路清晰。
  - 同时保留预训练VLM的通用智能（用于感知和规划）和领域专家的快速推理能力（用于动作分解），兼顾了泛化性和实时性。
  - 通过条件化动作专家于分解指令上，极大提升了语言可操纵性。
- **实验设计亮点**：
  - 多维度评估（NLP、仿真、真实世界），验证了模型在不同层次的实用性。
  - 提供了项目网站和演示视频，便于直观理解。

## 8. 不足与局限
- **实验覆盖**：未公开具体数据集名称和评估指标，缺乏可复现的详细实验设定；消融实验数量不详，难以评估各组件贡献。
- **偏差风险**：真实世界实验可能依赖特定硬件或场景，泛化性需进一步验证；对比方法未列出，结果可能受选择偏差影响。
- **应用限制**：混合变换器架构需要预训练VLM和额外的专家变换器，模型参数量可能较大，在资源受限的机器人上部署存在挑战。
- **其他**：论文被ICLR 2026拒稿，可能表明存在未被充分论证或创新性不足的问题，或实验支持不够充分。

（完）
