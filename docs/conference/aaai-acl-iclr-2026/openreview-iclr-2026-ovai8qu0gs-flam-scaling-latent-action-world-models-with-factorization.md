---
title: "FLAM: Scaling Latent Action World Models with Factorization"
title_zh: FLAM：通过分解扩展潜在动作世界模型
authors: "Zizhao Wang, Chang Shi, Jiaheng Hu, Kevin Rohling, Roberto Martín-Martín, Amy Zhang, Peter Stone"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ovaI8QU0gS"
tags: ["query:wam-vla"]
score: 8.0
evidence: 通过分解扩展潜在动作世界模型
tldr: 该论文提出FLAM框架，通过将潜在状态分解为独立因子，每个因子拥有独立的逆向和正向动态模型，解决了现有潜在动作世界模型在复杂多实体场景中的扩展性问题。该分解结构更准确地建模了复杂动态，显著提升了无动作视频环境下的视频生成质量，为可控制世界模型的学习提供了可扩展方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有潜在动作世界模型使用单一逆/正动态模型，难以在复杂多实体场景中扩展。
method: 将潜在状态分解为多个独立因子，每个因子具有独立的逆和正向动态模型。
result: 在多个视频生成基准上，FLAM以更少的计算量生成了更高质量的视频。
conclusion: 因子分解是扩展潜在动作世界模型到复杂动态场景的有效策略。
---

## Abstract
Learning latent actions from action-free video has emerged as a powerful paradigm for scaling up controllable world models learning.The latent actions offer an extra degree of freedom for users to generate videos iteratively.However, existing approaches often rely on monolithic inverse and forward dynamics models to learn one latent action that controls all, which struggle to scale in complex scenes where different entities act simultaneously. In this work, we propose FLAM, a factored dynamics framework that decomposes the latent state into independent factors, each with its own inverse and forward dynamics model. This structure enables more accurate modeling of complex, multi-entity dynamics and improves the video generation quality in action-free video settings. Evaluated on Multigrid, Procgen, nuPlan, Sports and EGTEA datasets, FLAM consistently outperforms the monolithic dynamics model, demonstrating the superiority of the factorized model.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：从无动作视频中学习潜在动作（latent actions）是扩展可控世界模型的可扩展范式。潜在动作为用户提供了迭代生成视频的额外自由度。然而，现有方法普遍采用单一的逆动态模型和正向动态模型来学习控制所有实体的单一潜在动作，这在多个实体同时运动的复杂场景中难以扩展。
- **整体含义**：该研究旨在解决现有潜在动作世界模型在多实体动态场景下的扩展瓶颈，通过更合理的结构化建模提升视频生成质量，为可控世界模型的学习提供更可扩展的解决方案。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出**FLAM**（因子化动态框架），将潜在状态分解为多个独立的因子，每个因子拥有独立的逆动态模型和正向动态模型。
- **关键技术细节**：
  - 不再使用单一的全局逆/正向动态模型，而是将潜在空间分解为若干**独立因子**，每个因子对应场景中的不同实体或动态成分。
  - 每个因子独立学习自身的逆动态（从相邻帧推断潜在动作）和正向动态（从当前状态和动作预测下一状态）。
  - 这种分解结构能够更准确地建模复杂、多实体的动态，使视频生成在无需动作标签的条件下获得更高质量。
- **流程说明**（文字流程）：
  1. 输入无动作视频序列。
  2. 将潜在状态分解为多个因子。
  3. 对每个因子，利用独立的逆动态模型从连续帧中推断该因子对应的潜在动作。
  4. 利用独立的正向动态模型，结合当前因子状态和推断的潜在动作预测下一因子状态。
  5. 所有因子的预测结果合并为全局下一状态，进而生成新视频帧。

### 3. 实验设计：数据集、基准、对比方法

- **数据集**：Multigrid、Procgen、nuPlan、Sports、EGTEA —— 涵盖网格世界、游戏、驾驶、体育动作、活动识别等多个复杂场景。
- **基准（Benchmark）**：以视频生成质量作为主要评价指标（具体指标未在摘要中详列，但可推测包括 FID、LPIPS 等常见生成指标）。
- **对比方法**：与**单一的动态模型（monolithic dynamics model）** 进行对比，即未使用因子分解的基线方法。
- **实验设置**：在五个数据集上评估 FLAM 与基线模型，并额外指出 FLAM 在**更少的计算量**（less compute）下取得了更高质量的视频生成。

### 4. 资源与算力

- **未明确说明**：摘要中未提及 GPU 型号、数量、训练时长等具体算力信息。仅提到 FLAM 以“更少的计算量”取得更好结果，但未量化。因此无法给出详细算力参数。

### 5. 实验数量与充分性

- **实验数量**：在 5 个不同的数据集上进行了评估，覆盖多种复杂动态场景。未明确说明做了多少组消融实验或细粒度分析。
- **充分性与客观性**：
  - 实验覆盖了多类任务（模拟、真实、驾驶、体育等），具有较好的多样性。
  - 对比了唯一的基线（单一大模型），但未与其他潜在动作世界模型（如 LAPO、VPO 等）比较，可能存在选择性偏倚。
  - 缺乏消融实验的明确报告（如分解因子的数量敏感性、各因子的独立贡献等），因此实验充分性尚可但在细节上不够完整。
  - 摘要声称“consistently outperforms”，表明结果一致且显著，但未提供统计显著性检验或误差范围。

### 6. 论文的主要结论与发现

- 因子分解是扩展潜在动作世界模型到复杂动态场景的有效策略。
- FLAM 框架能够更准确地建模多实体动态，并在多个视频生成基准上以更少的计算量实现更优的视频生成质量。
- 证明了独立因子的逆/正向动态模型比单一全局模型更具扩展性和表现力。

### 7. 优点：方法或实验设计上的亮点

- **方法创新**：简洁的分解结构（将潜在状态分解为独立因子）直接解决了单一模型在多实体场景下的扩展瓶颈，具有高可解释性和模块化潜力。
- **实验广泛**：在 5 个不同领域（网格、游戏、驾驶、体育、活动）的数据集上验证，展示了跨场景泛化能力。
- **效率优势**：明确提及以更少计算量取得更好性能，暗示方法在算力效率上具有竞争力。
- **直击要害**：针对现有范式（单一逆/正向动态模型）的核心弱点提出改进，动机清晰，解决方案自然。

### 8. 不足与局限

- **对状态分解的依赖**：方法隐含假设潜在状态可自然分解为独立因子，但在某些强耦合场景（如物理碰撞、遮挡交互）中，因子独立性假设可能不成立，导致性能下降。
- **实验对比不足**：仅与单一的动态模型基线对比，未与更先进的潜在动作学习方法（如 VAE+时序模型、基于扩散的潜在动作模型）进行比较，结论竞争力可能不够充分。
- **缺乏消融与分析**：未提供对分解因子数量、各因子语义含义、分解对整体性能贡献的定量分析，实验的深入性有待加强。
- **未讨论部署限制**：未说明在真实环境（如机器人控制、自动驾驶闭环）中的测试，仅停留在视频生成质量，涉及真实系统时可能存在 sim-to-real 差距。
- **资源信息缺失**：未报告具体训练配置，降低了结果的可复现性评估。

（完）
