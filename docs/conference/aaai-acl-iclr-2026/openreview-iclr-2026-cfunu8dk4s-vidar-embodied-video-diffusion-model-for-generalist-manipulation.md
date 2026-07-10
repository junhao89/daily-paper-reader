---
title: "Vidar: Embodied Video Diffusion Model for Generalist Manipulation"
title_zh: Vidar：用于通用操作的具身视频扩散模型
authors: "Yao Feng, Hengkai Tan, Xinyi Mao, Chendong Xiang, Guodong Liu, Shuhe Huang, Hang Su, Jun Zhu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=CFuNu8dK4s"
tags: ["query:gen2embodied"]
score: 8.0
evidence: 视频扩散模型用于通用机器人操作
tldr: 现有机器人操作策略难以泛化到新平台，且像素到动作的端到端方法易受背景和视角干扰。为此，本文提出Vidar，采用互联网预训练的 embodied 视频扩散模型作为通用先验，并引入掩码逆动力学模型（MIDM）作为适配器。通过从三个真实机器人平台收集的75万条多视角轨迹进行连续预训练，Vidar实现了跨平台、跨任务的通用操作能力。实验表明，其视频先验显著提升了新平台上的零样本迁移性能。该工作展示了将视频生成模型迁移到机器人策略的潜力，属于 gen2embodied 方向的重要探索。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 克服多机器人平台泛化难题，避免像素到动作管道在背景和视角变化下的退化。
method: 利用互联网预训练视频扩散模型作为先验，结合掩码逆动力学模型适配器，在多视图轨迹上连续预训练。
result: 在三个真实机器人平台上，Vidar 实现了跨平台、跨任务的成功操作，零样本迁移性能优于基线。
conclusion: 视频扩散先验可有效提升通用操作策略的泛化性，为生成式模型用于具身智能提供新途径。
---

## Abstract
Scaling general-purpose manipulation to new robot embodiments remains challenging: each platform typically needs large, homogeneous demonstrations, and end-to-end pixel-to-action pipelines may degenerate under background and viewpoint shifts. Based on previous advances in video-based robot control, we present Vidar, consisting of an embodied video diffusion model as the generalizable prior and a masked inverse dynamics model (MIDM) as the adapter. We leverage a video diffusion model pre-trained at Internet scale, and further continuously pre-train it for the embodied domain using 750K multi-view trajectories collected from three real-world robot platforms. For this embodied pre-training, we introduce a unified observation space that jointly encodes robot, camera, task, and scene contexts. The MIDM module learns action-relevant pixel masks without dense labels, grounding the prior into the target embodiment’s action space while suppressing distractors. With only ∼20 minutes of human demonstrations on an unseen robot (∼1% of typical data), Vidar outperforms state-of-the-art baselines and generalizes to unseen tasks, backgrounds, and camera layouts. Our results suggest a scalable recipe for “one prior, many embodiments”: strong, inexpensive video priors together with minimal on-robot alignment.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 核心问题与整体含义（研究动机与背景）
- **核心问题**：通用机器人操作策略难以扩展到新的机器人平台。传统方法要么需要大量同质化演示数据，要么端到端的“像素到动作”管道在背景和视角变化时性能退化严重。现有视频扩散模型作为先验的工作（如Video-based robot control）尚未解决跨平台泛化问题。
- **研究动机**：希望利用互联网预训练的视频扩散模型作为通用先验，再通过少量机器人数据适配，实现“一个先验，多个本体”的通用操作能力，避免全量重新训练。
- **整体含义**：该工作属于“gen2embodied”（生成式模型到具身智能）方向的重要探索，表明视频生成先验可以显著提升跨平台零样本迁移性能，为低成本构建通用机器人策略提供了可扩展方案。

### 2. 方法论：核心思想、关键技术细节
- **核心思想**：将互联网预训练的视频扩散模型作为可泛化的先验，通过引入掩码逆动力学模型（MIDM）作为适配器，将视频先验对齐到目标机器人的动作空间，同时抑制背景/视角干扰。
- **关键技术细节**：
  - **Vidar整体架构**：包含两部分——a) 互联网预训练的视频扩散模型（作为embodied视频扩散模型）；b) **掩码逆动力学模型（MIDM）** 作为适配器。
  - **统一观察空间**：为进行具身领域的连续预训练，设计了一个统一的观察空间，联合编码机器人、相机、任务和场景上下文。
  - **连续预训练**：使用从三个真实机器人平台收集的 **75万条多视角轨迹** 对视频扩散模型进行连续预训练，使其适应具身领域。
  - **MIDM**：无需密集标注，学习与动作相关的像素掩码，将先验解码到目标本体的动作空间，同时抑制干扰物（distractors）。
  - **部署流程**：在新机器人上仅需约20分钟人类演示（约典型数据量的1%），即可完成对齐。
- **公式/算法流程**（文字说明）：  
  ① 互联网预训练视频扩散模型作为基本生成先验；  
  ② 在多机器人平台上收集多视角轨迹，构建统一观察空间；  
  ③ 在此数据上连续预训练视频扩散模型（embodied预训练）；  
  ④ 在目标新机器人上收集少量演示数据，训练MIDM适配器；  
  ⑤ 推理时，先由视频扩散模型生成未来帧（或潜码），再由MIDM解码为动作。

### 3. 实验设计
- **数据集与场景**：
  - 从三个真实世界机器人平台收集了75万条多视角轨迹，用于连续预训练。
  - 在未见过的机器人上测试，使用约20分钟的人类演示（约典型数据的1%）进行适配。
  - 测试任务包括未见过的任务、背景、相机布局。
- **基准（Benchmark）**：未明确提及标准化基准名称，但对比了最先进（state-of-the-art）的基线方法。
- **对比方法**：对比了现有的基于视频的机器人控制方法等基线（具体名称未在摘要中列出，但提到“outperforms state-of-the-art baselines”）。

### 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力资源。仅有“750K多视图轨迹”的数据量，“20分钟人类演示”的适配时间。未提及预训练视频扩散模型的计算量。

### 5. 实验数量与充分性
- **实验组数**：论文在三个真机平台上进行了实验，测试了跨平台、跨任务、跨背景、跨相机布局的泛化能力。包含与SOTA基线的对比实验，以及零样本迁移实验。
- **充分性与公平性**：
  - 优点：实验覆盖了多个平台和多种变化条件，且使用了统一的数据收集和处理流程。
  - 不足：摘要未提供详细的消融实验（如MIDM的设计选择、预训练规模的影响等），也未提供统计显著性检验或误差棒。对比基线是否公平（超参数调优、数据使用量等）未说明。总体实验数量偏少，但作为跨平台泛化初步验证是可以接受的。

### 6. 主要结论与发现
- **主要结论**：视频扩散先验可有效提升通用操作策略的泛化性。
- **具体发现**：
  - 仅需约20分钟人类演示（1%典型数据量），Vidar即可超越SOTA基线。
  - 成功泛化到未见任务、背景和相机布局。
  - 为“一个先验，多个本体”提供了可扩展方案：强大的低成本视频先验加上少量的机上对齐。

### 7. 优点
- **方法创新**：将互联网预训练视频扩散模型与掩码逆动力学模型结合，既利用了大规模先验，又通过轻量适配器应对不同机器人平台，无需从头训练。
- **数据高效**：仅需极少量的新机器人演示（约20分钟）即可完成适配，极大降低了部署成本。
- **跨平台适应**：在三个真实平台验证，并展示了对背景、视角变化的鲁棒性。
- **前景探索**：代表了gen2embodied方向的重要尝试，验证了生成式模型用于具身智能的潜力。

### 8. 不足与局限
- **实验覆盖有限**：仅三个平台，未在更广泛的机器人（如不同自由度、末端执行器类型）上验证。
- **消融不充分**：未明确报告MIDM掩码学习质量、预训练数据量对性能的影响、不同视频扩散架构的对比等。
- **风险与偏差**：
  - 可能对平台间相似性有隐含假设，如果新平台与预训练平台差异过大（如完全不同的运动学结构），效果可能下降。
  - 视频扩散模型生成质量可能受动作复杂性影响，快速动态任务可能失败。
  - 未提及安全性或失败模式分析。
- **应用限制**：需要机器人的RGB相机输入，对光照、遮挡等条件未详述；MIDM需要少量演示，但演示质量对适配影响未知。

（完）
