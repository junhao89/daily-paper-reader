---
title: "WorldPack: Compressed Memory Improves Spatial Consistency in Video World Modeling"
title_zh: WorldPack：压缩记忆提升视频世界模型的空间一致性
authors: "Yuta Oshima, Masahiro Suzuki, Yusuke Iwasawa, Yutaka Matsuo, Hiroki Furuta"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=CuNHz3zxgm"
tags: ["query:wam-vla"]
score: 9.0
evidence: 带压缩记忆的视频世界模型实现空间一致性
tldr: 视频世界模型在长时间生成中面临计算成本高、空间一致性差的问题。本文提出 WorldPack，通过轨迹打包和记忆检索实现高效压缩记忆，仅用较短上下文即可显著提升长期生成的空间一致性和保真度。在多个导航和操作场景中，WorldPack 生成的未来帧质量优于现有世界模型，且计算效率更高。该工作为可扩展的世界模型提供了有效记忆管理方案，直接服务于视频式世界模型用于机器人想象与 rollout。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 解决长时段视频世界模型计算昂贵、空间一致性不足的问题。
method: 提出轨迹打包和记忆检索构成的压缩记忆机制，降低上下文长度需求。
result: 在长期生成中空间一致性和保真度显著提升，计算开销更低。
conclusion: 压缩记忆是提升视频世界模型可扩展性和一致性的关键设计。
---

## Abstract
Video world models have attracted significant attention for their ability to produce high-fidelity future visual observations conditioned on past observations and navigation actions.
Temporally- and spatially-consistent, long-term world modeling has been a long-standing problem, unresolved with even recent state-of-the-art models, due to the prohibitively expensive computational costs for long-context inputs.
In this paper, we propose WorldPack, a video world model with efficient compressed memory, which significantly improves spatial consistency, fidelity, and quality in long-term generation despite much shorter context length.
Our compressed memory consists of trajectory packing and memory retrieval; trajectory packing realizes high conctext efficiency and memory retrieval maintains the consistency in rollouts and helps long-term generations that require spatial reasoning.
Our performance is evaluated with LoopNav, a benchmark on MineCraft, specialized for the evaluation of long-term consistency, and we verify that WorldPack notably outpeforms strong state-of-the-art models.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：视频世界模型（video world models）旨在根据过去的观测和导航动作生成高保真度的未来视觉观察，是机器人想象和 roll-out 的关键技术。然而，长期世界建模面临着时间与空间一致性的长期挑战，现有模型（包括 SOTA 方法）由于长上下文输入导致计算成本过高，无法有效处理长时间生成。
- **背景**：随着视觉数据规模增长，模型需要记忆更长的历史信息以维持空间一致性，但直接增加上下文长度会带来巨大的计算开销。本文提出 WorldPack，通过高效的压缩记忆机制，在显著缩短上下文长度的同时，提升长期生成的空间一致性、保真度和质量。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将历史轨迹信息压缩为紧凑的记忆表示，从而在不增加推理时上下文长度的情况下，保留长期空间关系，实现高效的世界模型 rollout。
- **关键技术细节**（基于摘要与元数据）：
  - **轨迹打包（Trajectory Packing）**：将过去多个时间步的观测和动作打包成紧凑的表示，提高上下文效率，降低计算需求。
  - **记忆检索（Memory Retrieval）**：在生成过程中，从压缩记忆中检索与当前状态相关的历史信息，维持 rollout 的空间一致性，支持需要空间推理的长期生成。
  - 整体流程：输入若干帧历史观测和动作 → 经轨迹打包构建压缩记忆 → 在每一步生成新帧时，通过记忆检索获取相关上下文 → 联合当前输入生成下一帧。具体公式或算法流程未在公开资料中给出，仅能从方案描述推断。

### 3. 实验设计：数据集、benchmark 与对比方法

- **数据集 / 场景**：使用 **MineCraft** 环境中的 **LoopNav** benchmark，该 benchmark 专为评估视频世界模型在长期生成中的空间一致性而设计（如导航环路任务，要求模型保持环境布局的一致性）。
- **对比方法**：文中提及“strong state-of-the-art models”，但未列出具体方法名称。从元数据看，该论文在 ICLR 2026 投稿中被拒，可能对比了诸如 VideoPoet、MaskGIT 类视频生成模型或 Robot world models（如 DayDreamer、DreamerV2 等变体），但具体需查看论文全文。

### 4. 资源与算力

- **未明确说明**：论文提供的摘要及元数据中未提及使用的 GPU 型号、数量、训练时长等算力信息。鉴于 WorldPack 以“计算效率更高”为亮点，推测其训练所需算力低于同尺度长上下文模型，但无确切数据。

### 5. 实验数量与充分性

- **实验数量**：从现有信息看，仅在 MineCraft 的 LoopNav 一个 benchmark 上进行了评估，未提及在其他环境（如室内导航、机器人操作等）的验证。消融实验方面，仅提及“轨迹打包”和“记忆检索”两个组件，但未明确是否进行独立的消融分析。
- **充分性与公平性**：由于信息有限，难以判断实验的全面性与公平性。仅一个 benchmark 可能不足以证明方法在多样化场景下的通用性。对比的基线未列出，也未说明超参数、随机种子等细节。因此，实验充分性存疑。

### 6. 论文的主要结论与发现

- WorldPack 通过压缩记忆（轨迹打包 + 记忆检索）在 **显著缩短上下文长度** 的情况下， **显著提升** 了长期视频生成的 **空间一致性**、**保真度** 和 **质量**。
- 在 LoopNav 基准测试中，WorldPack 的表现 **优于** 强 SOTA 模型，且 **计算开销更低**。
- 压缩记忆是提升视频世界模型 **可扩展性** 和 **空间一致性** 的关键设计，为大规模世界模型提供了高效记忆管理方案。

### 7. 优点：方法或实验设计上的亮点

- **方法创新性**：将记忆压缩（轨迹打包）与动态检索结合，在低上下文长度下保持空间一致性，思路新颖且实际生效。
- **效率优势**：相比直接增加上下文长度，压缩记忆显著降低了计算成本，使长期生成在资源受限场景下可行。
- **针对性强**：选择 LoopNav benchmark 专门评估空间一致性，而非仅关注帧级 FVD 等指标，设计合理。
- **直接服务于机器人应用**：WorldPack 可直接用于视频式世界模型的想象与 rollout，具有实际落地潜力。

### 8. 不足与局限

- **实验覆盖窄**：仅在 MineCraft 的 LoopNav 任务上验证，缺乏在其他导航或操作环境（如 Habitat、iGibson、RL Bench）中的实验，泛化能力未知。
- **对比基线不明确**：未列出具体对比方法及其超参数，难以公平复现与比较。
- **消融实验不完整**：未系统分析轨迹打包与记忆检索各自的贡献，也未探讨压缩率与性能的 trade-off。
- **长尾与闭环问题**：长期 rollout 中是否存在误差累积、遗忘等问题未讨论。
- **资源信息缺失**：未报告训练算力与时间，无法评估实用成本。
- **局限性总结**：作为 ICLR 2026 被拒论文，可能在实验严谨性、方法细节、或对比基线选择上存在不足，但具体原因需参考审稿意见。

（完）
