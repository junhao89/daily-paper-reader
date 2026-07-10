---
title: Unified Latent Steering and Residual Refinement for Online Improvement of Diffusion Policy Models
title_zh: 统一潜空间引导与残差细化用于扩散策略模型的在线改进
authors: "Zhengbang Zhu, Ziyan Li, Xiu Yuan, Hanbo Zhang, Yuxiao Liu, Chongjie Zhang, Yong Yu, Weinan Zhang, Minghuan Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=DbBD2aT1OG"
tags: ["query:latent-vla"]
score: 7.0
evidence: 利用潜空间引导改进扩散策略
tldr: 纯模仿学习的扩散策略在分布外场景下脆弱，而直接微调大型策略样本效率低。本文提出USR统一框架，通过轻量级的潜空间引导和残差细化，高效地在线改进扩散策略。该方法在不改变预训练模型参数的情况下，利用在线交互数据调整潜编码，显著提升策略在分布偏移下的鲁棒性，为机器人策略的持续学习提供了实用方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 扩散策略在训练分布外表现不佳，且全参数微调成本过高。
method: 提出USR框架，在潜空间进行引导并使用残差细化模块实现高效在线适应。
result: 在多个操控任务上，USR显著提升了策略的泛化能力和样本效率。
conclusion: 潜空间操作是有效且高效的策略在线改进途径。
---

## Abstract
Imitation learning has driven major advances in robotic manipulation by exploiting large and diverse demonstrations, yet policies trained purely by imitation remain brittle under distribution shift and novel scenarios, making online improvement essential. 
Directly finetuning the parameters of modern large policies is prohibitively sample inefficient and computationally expensive,
while recent finetuning-free adaptation methods either fail to exploit the multimodal distributions learned by pretrained policies or remain confined to the coverage of demonstrations.
We propose USR, a Unified framework for latent Steering and residual Refinement that enables efficient online improvement of diffusion policy models. A lightweight actor jointly outputs latent noise to steer the diffusion process toward promising modes and residual corrections to adapt beyond the diffusion policy's support, combining stable mode selection with flexible refinement. This unified design stabilizes training and fully leverages both components. Experiments on standard benchmarks and our MultiModalBench demonstrate USR's state-of-the-art performance. Furthermore, we validate its real-world applicability by improving a Vision-Language-Action (VLA) model on a physical robot, setting a new paradigm for sample-efficient adaptation of diffusion-based policies.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）
- **研究动机**：模仿学习在大规模示教数据驱动下推动了机器人操控的显著进展，但纯模仿训练的策略在分布偏移（distribution shift）和新场景下表现脆弱，因此在线改进（online improvement）至关重要。
- **背景矛盾**：直接微调现代大型策略参数（如扩散策略）的样本效率极低且计算昂贵；而近期免微调（finetuning-free）的适应方法要么无法充分利用预训练策略学到的多模态分布，要么只能局限于示教覆盖的范围。
- **整体含义**：本文旨在探索一种高效、样本经济的在线改进方法，使得预训练的扩散策略能在不改变参数的前提下，通过轻量级干预适应分布外场景，提升鲁棒性与泛化能力。

## 2. 方法论
- **核心思想**：提出 **USR（Unified latent Steering and residual Refinement）** 统一框架，通过**潜空间引导（latent steering）** 和**残差细化（residual refinement）** 两个组件协同工作，实现对扩散策略模型的高效在线改进。
- **关键技术细节**：
  - 不修改预训练策略的参数。
  - 一个轻量级 **actor** 联合输出两部分：
    1. **潜噪声（latent noise）**：用于引导扩散过程朝向有前景的模式（promising modes），实现稳定的模式选择。
    2. **残差修正（residual corrections）**：在扩散策略支持范围之外进行适应，提供灵活的细化。
  - 两个组件统一训练，稳定了训练过程，并充分发挥各自优势。
- **算法流程（文字说明）**：
  - 预训练扩散策略固定不变。
  - 在线交互数据被用于训练轻量级 actor。
  - 每次扩散生成动作时，actor 向潜编码注入导向噪声，并输出残差项叠加在最终动作上，从而在不改变原始模型权重的情况下实现策略的在线适应与改进。

## 3. 实验设计
- **数据集 / 场景**：
  - **标准基准任务**（standard benchmarks）：未在元数据中明确列出具体任务名称。
  - **MultiModalBench**：本文自建的多模态操控基准，用于评估策略在多模态分布下的适应能力。
  - **真实机器人实验**：在物理机器人上改进一个视觉-语言-动作（VLA）模型，验证实际可用性。
- **对比方法**：文中声称与现有方法比较达到 **state-of-the-art** 性能，但元数据未列出具体对比基线（如扩散策略本身、其他免微调适应方法等）。
- **Benchmark**：标准基准 + MultiModalBench + 真实机器人任务。

## 4. 资源与算力
- 论文元数据中 **未明确说明** 使用的 GPU 型号、数量、训练时长等算力信息。因此无法总结。

## 5. 实验数量与充分性
- **实验数量**：包含三类实验——标准基准、自建多模态基准、真实机器人实验。元数据未给出具体任务数量或消融实验的详细组数。
- **充分性评价**：
  - 涵盖仿真和真实环境，具有一定说服力。
  - 但缺少对不同分布偏移程度、不同预训练策略规模的系统性消融。
  - 对比基线未能列全，难以判断实验公平性。
  - 总体而言，实验设计意图清晰，但元数据信息不足以全面评估充分性，需阅读全文确认。

## 6. 主要结论与发现
- USR 框架能显著提升扩散策略在分布偏移下的泛化能力和样本效率。
- 潜空间操作（latent steering）是一种有效且高效的策略在线改进途径。
- 在不改变预训练模型参数的前提下，通过联合使用引导和残差细化，可实现稳定的在线适应，达到当前最优性能。

## 7. 优点
- **方法亮点**：统一框架融合了模式选择（引导）与灵活适应（残差细化），避免了直接微调的高成本。
- **轻量级**：仅训练一个轻量级 actor，计算开销小。
- **通用性**：适用于任意扩散策略（包括 VLA 模型），并在真实机器人上验证。
- **免微调参数**：保留预训练模型的演示知识，防止遗忘。
- **实验亮点**：在标准基准和自建多模态基准上均达到 SOTA，并增设真实机器人场景增强说服力。

## 8. 不足与局限
- **实验覆盖有限**：元数据未提供具体任务数量、分布偏移类型、消融实验细节，无法判断方法对极端分布偏移或罕见场景的鲁棒性。
- **对比偏差风险**：未列出对比方法的详细设置，可能未与最新在线适应方法进行充分比较。
- **应用限制**：
  - 依赖预训练扩散策略的质量；若预训练策略本身多模态覆盖差，潜引导可能失效。
  - 残差细化模块的作用范围受限于 actor 的表达能力，可能无法处理超出训练分布过大的修正。
  - 真实机器人实验仅针对一个 VLA 模型，泛化到其他类型策略（如图像条件扩散策略）需进一步验证。
- **算力资源未报告**，不利于可重复性评估。

（完）
