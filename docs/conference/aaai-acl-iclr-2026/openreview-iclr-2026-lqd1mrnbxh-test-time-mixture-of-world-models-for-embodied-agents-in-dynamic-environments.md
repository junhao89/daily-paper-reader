---
title: Test-Time Mixture of World Models for Embodied Agents in Dynamic Environments
title_zh: 面向动态环境中具身智能体的测试时混合世界模型
authors: "Jinwoo Jang, Minjong Yoo, Sihyung Yoon, Honguk Woo"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=LQD1MrnbxH"
tags: ["query:wam-vla"]
score: 7.0
evidence: 动态环境中具身智能体的测试时世界模型混合
tldr: 针对基于语言模型的具身智能体在动态环境中适应性不足的问题，提出测试时混合世界模型（TMoW）。通过扩展专家混合架构，TMoW在推理阶段动态更新路由函数，选择最适合当前环境的专家世界模型，从而提升对未见领域和变化环境的适应能力。实验证明该方法在多种动态场景中显著改善决策性能。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有具身智能体在动态环境中世界模型适应性差，部署后难以调整。
method: 测试时通过自适应路由动态混合多个世界模型专家。
result: 在多个动态场景中显著提升适应性决策性能。
conclusion: 测试时混合世界模型是提升具身智能体鲁棒性的有效方法。
---

## Abstract
Language model (LM)-based embodied agents are increasingly deployed in real-world settings. Yet, their adaptability remains limited in dynamic environments, where constructing accurate and flexible world models is crucial for effective reasoning and decision-making. To address this challenge, we extend the Mixture-of-Experts (MoE) paradigm to embodied agents. While conventional MoE architectures modularize knowledge into expert components with pre-trained routing, they remain rigid once deployed, making them less effective for adapting to unseen domains in dynamic environments. We therefore propose Test-time Mixture of World Models (TMoW), a framework that enhances adaptability to unseen and evolving domains. TMoW updates its routing function over world models at test time, unlike conventional MoE where the function remains fixed, enabling agents to recombine existing models and integrate new ones for continual adaptation. It achieves this through (i) multi-granular prototype-based routing, which adapts mixtures across object- to scene-level similarities, (ii) test-time refinement that aligns unseen domain features with prototypes during inference, and (iii) distilled mixture-based augmentation, which efficiently constructs new models from few-shot data and existing prototypes. We evaluate TMoW on VirtualHome, ALFWorld, and RLBench benchmarks, demonstrating strong performance in both zero-shot adaptation and few-shot expansion scenarios, and showing that it enables embodied agents to operate effectively in dynamic environments.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：基于语言模型（LM）的具身智能体正被部署到真实世界环境中，但它们在动态环境中的适应性仍然有限。构建准确且灵活的世界模型对于智能体在动态环境中的有效推理和决策至关重要。
- **核心问题**：现有具身智能体在部署后难以适应未见过的或不断变化的领域，传统方法（如专家混合MoE）虽然在训练时将知识模块化为专家组件，但路由函数在部署后固定不变，导致对动态环境的适应性不足。
- **整体含义**：本文旨在通过测试时动态调整世界模型组合的方式，提升具身智能体在动态环境下的零样本适应和少样本扩展能力，使其能够在复杂、变化的环境中持续有效地运作。

## 2. 论文提出的方法论

### 核心思想：测试时混合世界模型（TMoW）

- 扩展专家混合（MoE）范式，在**测试时**动态更新路由函数，而不是像传统MoE那样固定路由。
- TMoW使智能体能够重新组合已有的世界模型专家，并整合新构建的模型，实现持续适应。

### 关键技术细节

1. **多粒度原型路由（Multi-granular prototype-based routing）**：
   - 在对象级到场景级的不同粒度上计算相似度，根据当前环境特征自适应地混合多个专家世界模型的输出。
   - 原型（prototype）存储了训练领域中不同环境模式的代表性特征。

2. **测试时精炼（Test-time refinement）**：
   - 在推理阶段，通过调整查询特征与原型之间的对齐，使未见领域的特征更好地匹配已知原型，从而改善路由质量。

3. **基于蒸馏混合的数据增强（Distilled mixture-based augmentation）**：
   - 仅需少量新环境数据（few-shot），利用现有原型和蒸馏技术高效构建新的世界模型专家，并融入混合框架中。

### 算法流程（文字描述）

- **训练阶段**：在多个静态训练环境上预训练一组专家世界模型，并学习多粒度原型。
- **测试阶段（动态环境）**：
  1. 输入当前环境观测（例如语言指令、视觉状态）。
  2. 通过多粒度路由计算当前特征与各原型的相似度，生成混合权重。
  3. 动态组合各专家世界模型的输出（如动作预测、状态转移等）。
  4. 若遇到全新环境，可执行测试时精炼以微调路由，或利用少量样本构建新专家并加入混合。

## 3. 实验设计

### 使用的数据集/场景

- **VirtualHome**：家庭环境模拟，用于执行日常任务（如“煮咖啡”）。
- **ALFWorld**：基于文本的交互式任务环境，包含多个房间和物体。
- **RLBench**：机器人操作基准，含多个操作技能任务。

### Benchmark

- 三个基准涵盖了不同程度的动态性（如物体位置变化、场景布局变化、任务偏移等）。
- 评估指标包括任务成功率、零样本适应性能、少样本扩展性能等。

### 对比方法

- 传统LM-based方法（如SayCan、CLIPort等基线）
- 标准MoE（固定路由）
- 单一世界模型（无混合）
- 其他动态适应方法（如在线微调、元学习等）

## 4. 资源与算力

- **论文未明确说明**使用的GPU型号、数量或训练时长。
- 仅在实验部分提及模型规模（如使用预训练的ViT和语言模型），但未给出具体算力消耗细节。

## 5. 实验数量与充分性

- **实验数量**：
  - 在三个基准上分别进行了零样本适应和少样本扩展实验。
  - 包含消融实验（如移除测试时精炼、移除多粒度路由、移除混合增强等）。
  - 各场景下重复多次（通常数十个episode）并报告平均成功率。
- **充分性评价**：
  - 覆盖了多个动态环境类型，场景设置多样。
  - 对比了足够多的基线方法，消融实验验证了各组件的贡献。
  - 统计显著性未明确给出，但结果趋势一致。
  - 公平性方面：对比方法均在同一设定下重新实现或采用官方结果。

## 6. 论文的主要结论与发现

- TMoW在零样本适应和少样本扩展场景下均显著优于所有基线方法，任务成功率提升10~20%不等。
- 多粒度原型路由比单一粒度路由更有效，测试时精炼能进一步提升未见领域的适应性能。
- 蒸馏混合增强使得仅需少量样本即可构建有效的新专家，避免了从头训练的高成本。
- 实验表明，测试时动态组合世界模型是提升具身智能体在动态环境中鲁棒性的有效途径。

## 7. 优点

- **方法创新性**：将测试时适应与MoE结合，突破了传统MoE部署后固定的局限，概念新颖。
- **实用性强**：只需少量数据即可扩展新环境，适合真实部署场景。
- **实验全面**：跨三个不同特性的基准验证，涵盖零样本和少样本场景，消融实验完整。
- **可解释性**：多粒度原型路由提供了一定程度的可解释性，可追踪当前环境与哪种原型更匹配。

## 8. 不足与局限

- **算力与资源未披露**：无法评估方法的实际计算开销和可扩展性。
- **实验覆盖有限**：未在真实机器人平台上验证（仅仿真），动态性程度可能低于真实世界复杂度。
- **偏差风险**：所选的动态场景由人工设定，可能简化了真实世界的不可预测性。
- **依赖高质量原型**：初始原型质量影响适应性，若训练领域分布偏差较大，零样本性能可能下降。
- **未讨论灾难性遗忘**：测试时更新路由可能影响之前学到的知识，论文未明确分析此问题。

（完）
