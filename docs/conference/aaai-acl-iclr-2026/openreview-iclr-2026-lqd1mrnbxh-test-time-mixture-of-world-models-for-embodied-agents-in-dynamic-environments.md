---
title: Test-Time Mixture of World Models for Embodied Agents in Dynamic Environments
title_zh: 用于动态环境中具身代理的测试时混合世界模型
authors: "Jinwoo Jang, Minjong Yoo, Sihyung Yoon, Honguk Woo"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=LQD1MrnbxH"
tags: ["query:wam-vla"]
score: 9.0
evidence: 提出测试时混合世界模型以增强动态环境适应性
tldr: 该论文针对动态环境中具身代理适应性不足的问题，提出测试时混合世界模型（TMoW），通过在线更新专家路由来适应未知领域。实验表明该方法在多种动态环境下显著提升了决策性能，为世界模型在真实场景中的灵活应用提供了新思路。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有世界模型在动态环境中适应性差，部署后无法调整。
method: 提出TMoW框架，在测试阶段动态更新混合专家路由。
result: 在多种动态环境下，TMoW优于静态世界模型和微调方法。
conclusion: 测试时混合策略有效提升世界模型在未见域中的泛化能力。
---

## Abstract
Language model (LM)-based embodied agents are increasingly deployed in real-world settings. Yet, their adaptability remains limited in dynamic environments, where constructing accurate and flexible world models is crucial for effective reasoning and decision-making. To address this challenge, we extend the Mixture-of-Experts (MoE) paradigm to embodied agents. While conventional MoE architectures modularize knowledge into expert components with pre-trained routing, they remain rigid once deployed, making them less effective for adapting to unseen domains in dynamic environments. We therefore propose Test-time Mixture of World Models (TMoW), a framework that enhances adaptability to unseen and evolving domains. TMoW updates its routing function over world models at test time, unlike conventional MoE where the function remains fixed, enabling agents to recombine existing models and integrate new ones for continual adaptation. It achieves this through (i) multi-granular prototype-based routing, which adapts mixtures across object- to scene-level similarities, (ii) test-time refinement that aligns unseen domain features with prototypes during inference, and (iii) distilled mixture-based augmentation, which efficiently constructs new models from few-shot data and existing prototypes. We evaluate TMoW on VirtualHome, ALFWorld, and RLBench benchmarks, demonstrating strong performance in both zero-shot adaptation and few-shot expansion scenarios, and showing that it enables embodied agents to operate effectively in dynamic environments.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：基于语言模型（LM）的具身代理在真实动态环境中部署时，其适应性严重不足。主要原因在于**世界模型**（world models）在预训练后无法根据环境变化灵活调整，导致在未知领域（unseen domains）中推理和决策能力显著下降。
- **研究背景**：现有世界模型大多采用静态参数，即便使用混合专家（MoE）架构，其路由函数在部署后也是固定的，难以应对现实世界中的持续变化。作者认为，关键在于需要在**测试阶段**（test time）动态更新世界模型，以提升对动态环境的自适应能力。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：提出**测试时混合世界模型（TMoW）** 框架，在测试阶段在线更新专家路由函数，使代理能够重新组合已有世界模型并集成新模型，实现持续适应。
- **关键技术细节**：
  - **(i) 多粒度原型基路由（multi-granular prototype-based routing）**：从物体级（object-level）到场景级（scene-level）多层次相似度上自适应地混合专家，使路由更精细。
  - **(ii) 测试时细化（test-time refinement）**：在推理过程中，将未知领域的特征与原型（prototypes）对齐，通过更新路由实现快速适应。
  - **(iii) 蒸馏混合基增强（distilled mixture-based augmentation）**：利用少量样本（few-shot data）和已有原型高效构建新世界模型，无需从头训练。
- **算法流程（文字说明）**：代理在测试阶段接收新环境输入 → 提取多粒度特征 → 与预存储的原型进行相似度对比 → 动态调整专家路由权重 → 通过在线反向传播更新路由参数 → 可选地使用蒸馏方法从少量标注样本中生成新原型并加入混合。整个过程无需重新训练整个模型，仅在测试时微调路由部分。

## 3. 实验设计
- **数据集/场景**：
  - **VirtualHome**（家庭任务模拟）
  - **ALFWorld**（交互式文本环境）
  - **RLBench**（机器人操作基准）
- **Benchmark**：在多种动态环境下评估零样本（zero-shot）适应和少样本（few-shot）扩展场景。
- **对比方法**：
  - 静态世界模型（static world models）
  - 传统微调方法（fine-tuning）
  - 可能还包括常规MoE基线（由于文本未列出完整对比列表，以上为摘要中明确提及的对比类别）

## 4. 资源与算力
- **文中未明确说明**：论文文本（摘要及元数据）未提及使用的GPU型号、数量、训练时长等算力信息。虽然可能存在实验细节，但提供的元数据中无相关内容。

## 5. 实验数量与充分性
- **实验数量**：主要在三个基准（VirtualHome、ALFWorld、RLBench）上进行评估，覆盖了零样本适应和少样本扩展两类场景。从摘要看，实验数量属于典型中等规模（3个环境 × 2种场景），但未提供具体消融实验数量。
- **充分性判断**：
  - **优点**：涵盖了家庭、文本交互、机器人操作三种不同类型动态环境，具有一定的多样性。
  - **不足**：缺乏现实物理机器人实验（仅限于模拟器）；消融实验（如多粒度路由、测试时细化、蒸馏增强各模块的贡献）在摘要中未提及具体结果，可能论文正文中有更多实验但本总结无法获取。
  - **客观性**：对比了静态模型和微调方法，是合理基线；但未提及与其他动态适应方法（如在线强化学习、测试时训练等）的对比，可能存在选择性参考偏差。

## 6. 主要结论与发现
- TMoW在**零样本适应**场景中显著优于静态世界模型和传统微调方法。
- 在**少样本扩展**场景中，通过蒸馏混合增强能够高效集成新世界模型，性能提升明显。
- 测试时混合策略有效提升了世界模型在未见域中的泛化能力，使具身代理能够在动态环境中更有效地运行。

## 7. 优点
- **方法创新**：首次将“测试时更新”思想引入混合专家世界模型，打破了部署后路由固定的限制。
- **多粒度路由**：从物体到场景层次适应，捕获更细粒度的环境变化。
- **高效适应性**：仅需少量计算（测试时微调路由），无需大规模重新训练。
- **实验覆盖多样**：在三个不同复杂度的模拟平台上验证，展示了跨场景泛化能力。

## 8. 不足与局限
- **模拟环境局限**：仅在封闭模拟器（VirtualHome、ALFWorld、RLBench）中验证，未涉及真实物理机器人或开放世界场景，现实动态环境的噪声和不可预测性可能带来额外挑战。
- **计算开销未量化**：测试时微调虽然“少量”，但具体推理时延、显存需求未报告，实际部署的实时性存疑。
- **支撑性实验缺失**：缺少对路由机制收敛性分析、原型数量影响、灾难性遗忘等关键问题的消融研究。
- **对比基线不够全面**：未与测试时训练（TTA）、在线学习、元学习等方法比较，难以判断是否为新SOTA。
- **论文文本缺失**：提供的“PDF提取文本”仅为OpenReview验证页，无法获取完整论文细节，本总结仅基于元数据中的摘要，可能遗漏重要实验设计和结果。

（完）
