---
title: "Vidar: Embodied Video Diffusion Model for Generalist Manipulation"
title_zh: Vidar：面向通用操控的具身视频扩散模型
authors: "Yao Feng, Hengkai Tan, Xinyi Mao, Chendong Xiang, Guodong Liu, Shuhe Huang, Hang Su, Jun Zhu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=CFuNu8dK4s"
tags: ["query:gen2embodied"]
score: 8.0
evidence: Vidar利用互联网预训练的视频扩散模型作为通用先验，结合掩蔽逆动力学模型，实现通用操控
tldr: 通用操控面临跨平台泛化难题，末端到像素的管道易受背景和视角变化影响。Vidar由预训练视频扩散模型作为通用先验和掩蔽逆动力学模型（MIDM）作为适配器组成。在三个真实机器人平台的75万条多视角轨迹上进行持续预训练，引入统一观测空间。实验表明，Vidar在未见物体和场景中表现出强大的泛化能力，大幅超越此前基于视频的机器人控制方法。该工作展示了将大规模视频生成迁移到机器人控制的潜力。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 通用机器人操控需要跨平台泛化，现有方法依赖大量同构数据且对视角变化敏感。
method: 利用互联网预训练视频扩散模型作为通用先验，通过具身持续预训练和掩蔽逆动力学模型适配机器人动作。
result: 在三个机器人平台的多个任务上，Vidar展现了卓越的跨平台和跨场景泛化能力。
conclusion: Vidar验证了视频扩散模型作为通用机器人先验的有效性，为生成式世界模型提供了新思路。
---

## Abstract
Scaling general-purpose manipulation to new robot embodiments remains challenging: each platform typically needs large, homogeneous demonstrations, and end-to-end pixel-to-action pipelines may degenerate under background and viewpoint shifts. Based on previous advances in video-based robot control, we present Vidar, consisting of an embodied video diffusion model as the generalizable prior and a masked inverse dynamics model (MIDM) as the adapter. We leverage a video diffusion model pre-trained at Internet scale, and further continuously pre-train it for the embodied domain using 750K multi-view trajectories collected from three real-world robot platforms. For this embodied pre-training, we introduce a unified observation space that jointly encodes robot, camera, task, and scene contexts. The MIDM module learns action-relevant pixel masks without dense labels, grounding the prior into the target embodiment’s action space while suppressing distractors. With only ∼20 minutes of human demonstrations on an unseen robot (∼1% of typical data), Vidar outperforms state-of-the-art baselines and generalizes to unseen tasks, backgrounds, and camera layouts. Our results suggest a scalable recipe for “one prior, many embodiments”: strong, inexpensive video priors together with minimal on-robot alignment.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：通用机器人操控面临跨平台泛化难题。现有方法通常需要为每个机器人平台收集大量同构演示数据，且端到端的“像素到动作”管道在背景和视角变化时容易退化。
- **整体含义**：Vidar旨在利用互联网规模预训练的视频扩散模型作为通用先验，通过少量具身数据对齐，实现“一个先验，多种本体”的可扩展操控方案，大幅降低对新机器人平台的适应成本。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将视频扩散模型作为通用视觉-运动先验，通过具身持续预训练和轻量级适配器（掩蔽逆动力学模型）将先验对齐到目标机器人的动作空间，同时抑制无关干扰。
- **关键技术细节**：
  - **具身视频扩散模型**：基于互联网预训练的视频扩散模型，进一步在三个真实机器人平台收集的75万条多视角轨迹上进行持续预训练，学习机器人的运动、摄像头视角、任务和场景上下文。为此引入**统一观测空间**，联合编码机器人、相机、任务和场景信息。
  - **掩蔽逆动力学模型（MIDM）**：作为适配器，不依赖密集标注，自动学习与动作相关的像素掩码，从而将视频先验的预测结果映射到目标本体的动作空间，并抑制背景等干扰因素。
- **流程说明**：① 互联网预训练视频扩散模型 → ② 在机器人多视角轨迹上持续预训练 → ③ 新机器人上仅需约20分钟人类示范（约1%典型数据量）→ ④ 使用MIDM对齐到具体动作空间 → ⑤ 实现零样本或小样本泛化。

## 3. 实验设计

- **数据集/场景**：三个真实世界机器人平台，共75万条多视角轨迹数据。实验任务包括未见物体和场景、不同背景和摄像头布局。
- **Benchmark**：与当前最先进的基于视频的机器人控制方法进行对比。
- **对比方法**：未列出具体方法名称，但提及“大幅超越此前基于视频的机器人控制方法”。

## 4. 资源与算力

- **文中未明确说明**：使用的GPU型号、数量、训练时长等算力信息。元数据和摘要中未提及相关细节。

## 5. 实验数量与充分性

- **实验数量**：主要对比了多个基线方法，并在多个任务（未见物体、场景、背景、相机布局）上验证泛化能力。元数据提到“多组实验”，但未给出具体消融实验列表。
- **充分性评估**：实验设计覆盖了跨平台、跨场景和跨任务泛化，且使用真实机器人平台，具有较高可信度。但缺乏对MIDM模块的详细消融、对预训练数据规模影响的探讨，以及更广泛的真实世界场景（如动态环境）测试。总体而言，实验对于验证核心论点较为充分，但可进一步丰富。

## 6. 论文的主要结论与发现

- Vidar在仅用约20分钟（约1%典型数据）的人类演示下，显著优于当前最先进的基线方法，并成功泛化到未见任务、背景和摄像头布局。
- 验证了**视频扩散模型作为通用机器人先验的有效性**，为生成式世界模型迁移至机器人控制提供了可扩展的配方：“一个强先验 + 少量本体对齐”。

## 7. 优点：方法或实验设计上的亮点

- **利用互联网预训练先验**：将大规模视频生成知识迁移到机器人操控，大幅减少对新平台的专属数据需求。
- **统一观测空间**：编码机器人、相机、任务和场景上下文，提高跨平台泛化能力。
- **MIDM自适应掩码**：无需密集标注，自动学习动作相关像素，兼具强泛化和抗干扰能力。
- **少量对齐数据**：仅需20分钟示范即可适配新机器人，实用性强。

## 8. 不足与局限

- **资源算力未披露**：无法评估训练可重复性和成本。
- **实验细节有限**：元数据未提供完整消融实验、对比方法名称、任务难度分级等，难以充分判断方法在极端条件下的表现。
- **潜在偏差风险**：MIDM的掩码学习可能依赖特定数据分布，在高度动态或杂乱场景中效果未知。
- **应用限制**：当前仅验证了三个机器人平台，对更复杂本体（如灵巧手、非刚体）的泛化性尚未证明；且视频扩散模型推理速度可能影响实时控制。
- **未讨论失败案例**：缺少对泛化失败场景的分析，限制了方法的鲁棒性理解。

（完）
