---
title: "VISOR: VIsual Spatial Object Reasoning for Language-driven Object Navigation"
title_zh: VISOR：基于视觉空间物体推理的语言驱动物体导航
authors: "Francesco Taioli, Shiping Yang, Sonia Raychaudhuri, Marco Cristani, Unnat Jain, Angel X Chang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=HhBxIQ0CA2"
tags: ["query:latent-vla"]
score: 8.0
evidence: 紧凑的视觉-语言-动作智能体用于语言驱动的物体导航
tldr: 语言驱动物体导航需要智能体解读包含内在和外在属性的自然语言描述。现有端到端模型泛化性差，模块化方法成本高且误差累积。本文提出一个紧凑的3B参数视觉-语言-动作（VLA）智能体VISOR，执行类人具身推理，在导航任务上实现了高效泛化。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有语言驱动导航方法在泛化性和计算效率上存在不足。
method: 提出一个紧凑的3B参数VLA智能体，融合视觉-语言-动作进行类人具身推理。
result: 在语言驱动导航任务上实现了优异的泛化性能和可解释性。
conclusion: 紧凑VLA模型是模块化导航流水线的有效替代。
---

## Abstract
Language-driven object navigation requires agents to interpret natural language descriptions of target objects, which combine intrinsic and extrinsic attributes for instance recognition and commonsense navigation. Existing methods either (i) use end-to-end trained models with vision–language embeddings, which struggle to generalize beyond training data and lack action-level explainability, or (ii) rely on modular zero-shot pipelines with large language models (LLMs) and open-set object detectors, which suffer from error propagation, high computational cost, and difficulty integrating their reasoning back into the navigation policy.
To this end, we propose a compact 3B-parameter Vision–Language–Action (VLA) agent that performs human-like embodied reasoning for both object recognition and action selection, removing the need for stitched multi-model pipelines. Instead of raw embedding matching, our agent employs explicit image-grounded reasoning to directly answer "Is this the target object?" and "Why should I take this action?" The reasoning process unfolds in three stages: "think", "think summary", and "action", yielding improved explainability, stronger generalization, and more efficient navigation. Code and dataset available upon acceptance.

---

## 论文详细总结（自动生成）

# 论文总结：VISOR: VIsual Spatial Object Reasoning for Language-driven Object Navigation

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：语言驱动物体导航要求智能体根据包含内在属性（如颜色、形状）和外在属性（如位置、上下文）的自然语言描述，识别目标物体并规划导航路径。现有方法存在两类主要局限：
  - 端到端训练的视觉-语言嵌入模型：难以泛化到训练数据之外的场景，且缺乏动作层面的可解释性。
  - 模块化零样本流水线（依赖大语言模型和开放集物体检测器）：存在误差传播、计算成本高、推理难以整合回导航策略等问题。
- **整体含义**：需要一种紧凑、高效、可解释且泛化能力强的智能体，能够在人类级别的具身推理中同时处理物体识别和动作选择，避免多模型拼接带来的缺陷。

## 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：提出一个紧凑的3B参数视觉-语言-动作（Vision-Language-Action, VLA）智能体VISOR，执行类人具身推理，直接融合视觉、语言和动作模态，替代传统的多模型流水线。
- **关键技术细节**：
  - 采用显式图像基础的推理（image-grounded reasoning）替代原始嵌入匹配，直接回答两个关键问题：“这是目标物体吗？”和“为什么我应该采取这个动作？”
  - 推理过程分为三个显式阶段：**“思考”（think）** → **“思考总结”（think summary）** → **“动作”（action）**。这一设计增强了可解释性，使模型不仅输出动作，还能输出推理链。
- **算法流程（文字说明）**：
  1. 输入：当前视觉观测（图像）和自然语言目标描述。
  2. 阶段1（Think）：模型基于视觉和语言信息，进行开放式推理，生成关于场景和目标物体的中间思考文本。
  3. 阶段2（Think summary）：将上一阶段的推理浓缩为简洁的摘要，突出关键证据。
  4. 阶段3（Action）：基于上述推理，生成具体的导航动作（如前进、转向、停止）以及选择该动作的理由。
- 整个流程由单个3B参数模型端到端完成，无需外部模块，因此避免了误差积累和计算开销。

## 3. 实验设计
- **数据集与场景**：摘要未明确提及具体数据集名称，但任务为“语言驱动物体导航”，通常涉及模拟环境（如Habitat、AI2-THOR、RoboTHOR等）中的室内场景导航。
- **Benchmark**：未明确说明。推测与现有语言导航任务（如ObjectNav、VLN-CE等）对标。
- **对比方法**：
  - 端到端模型（基于视觉-语言嵌入的方法）
  - 模块化零样本流水线（使用LLM和开放集检测器的方法）
- **具体实验细节缺失**：未提及评估指标（如成功率、SPL、推理时间等）、测试场景规模等。

## 4. 资源与算力
- **论文中未明确说明**：未给出使用的GPU型号、数量、训练时长等具体算力信息。
- **仅提及**：模型参数量为3B（30亿参数），属于紧凑型VLA模型。但完整训练所需算力未知。

## 5. 实验数量与充分性
- **实验数量**：摘要仅提及在语言驱动物体导航任务上进行了实验，但未列出具体实验组数、消融实验或跨数据集验证。
- **充分性评估**：
  - **不充分**：缺少定量结果（如成功率指标）、对比实验的详细表格、消融实验（如是否验证了三阶段推理的必要性）、跨域泛化测试等。论文结果为“优异的泛化性能和可解释性”，但未提供可量化的证据。
  - 存在**选择性报告风险**：仅强调自身优势，未披露可能存在的失败案例或不足。
- **公平性**：未说明是否对比了基线方法的公平设置（如相同训练数据、相同评估协议）。

## 6. 主要结论与发现
- 紧凑的3B参数VLA智能体VISOR可有效替代模块化导航流水线，在语言驱动物体导航任务上实现了更强的泛化性能和更好的可解释性。
- 三阶段推理（Think → Think summary → Action）使得模型能够输出推理链，提升了动作决策的透明度，有助于用户理解智能体行为。
- 端到端训练避免了多模型误差传播，同时降低了计算成本（相对于使用大语言模型+检测器的流水线）。

## 7. 优点
- **创新性**：将视觉-语言-动作融合为单一紧凑模型，并引入显式三阶段推理，同时实现了类人推理和可解释性。
- **实用性**：3B参数在VLA模型中属于紧凑规模，可能更适合实际部署（相比千亿级LLM）。
- **泛化潜力**：通过语言驱动推理而非固定嵌入匹配，理论上能更好地应对未见过的目标描述和场景。
- **可解释性**：显式生成推理文本，有助于调试和信任建立。

## 8. 不足与局限
- **实验证据不足**：缺乏公开基准上的定量结果、消融实验、统计显著性检验等，难以评估方法的实际性能提升幅度。
- **计算资源未披露**：无法判断训练该3B模型的实际开销，与其他方法的公平对比缺乏依据。
- **泛化性验证局限**：仅在单一任务（语言驱动物体导航）上声称泛化，但未在跨场景、跨机器人平台或真实环境中验证。
- **偏差风险**：可能依赖训练数据中的特定语言模式，对罕见或复杂属性描述（如抽象类比）的鲁棒性未知。
- **应用限制**：三阶段推理会增加推理延迟，可能不适合实时要求极高的系统；模型规模（3B）对于边缘设备仍可能过大。
- **结论强度不足**：论文被ICLR 2026拒绝，意味着审稿人可能指出了方法有效性的其他问题，如与SOTA相比无明显优势或贡献重复。

（完）
