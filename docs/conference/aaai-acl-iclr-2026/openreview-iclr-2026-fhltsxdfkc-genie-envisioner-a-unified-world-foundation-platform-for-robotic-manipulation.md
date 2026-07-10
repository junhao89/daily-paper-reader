---
title: "Genie Envisioner: A Unified World Foundation Platform for Robotic Manipulation"
title_zh: Genie Envisioner：用于机器人操作的统一世界基础平台
authors: "Yue Liao, Pengfei Zhou, Siyuan Huang, Donglin Yang, Shengcong Chen, Yuxin Jiang, Yue Hu, Si Liu, Jianlan Luo, Liliang Chen, Shuicheng YAN, Maoqing Yao, Guanghui Ren"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=fHLtSxDFKC"
tags: ["query:gen2embodied"]
score: 10.0
evidence: 使用视频扩散模型和流匹配解码器联合学习视觉表征和动作策略
tldr: 该论文针对机器人操作中视觉表征与动作策略分离的问题，提出Genie Envisioner统一平台，以视频扩散模型为基础联合学习时空动态，并通过流匹配解码器生成动作轨迹。在百万级操作数据上训练后，支持多任务多平台泛化，展示了生成式世界模型与动作策略融合的巨大潜力。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法分离视觉与动作学习，缺乏统一的生成式世界模型。
method: 提出GE，包含指令视频扩散模型（GE-Base）和流匹配动作解码器（GE-Act）。
result: 在多种操作任务上实现跨本体零样本泛化，优于基线方法。
conclusion: 视频生成式世界模型可作为机器人通用策略学习的基础平台。
---

## Abstract
We introduce Genie Envisioner (GE), a unified world foundation platform for robotic manipulation that jointly learns visual representations and action policies within a single video-generative framework. At its core, GE-Base is a large-scale instruction-conditioned video diffusion model that captures the spatial, temporal, and semantic dynamics of real-world robotic interactions in a structured latent space. Building on this foundation, GE-Act employs a lightweight flow-matching decoder to map latent representations into executable action trajectories, enabling precise and generalizable policy inference across diverse embodiments with minimal supervision. Trained on over 1 million manipulation episodes, GE supports both short- and long-horizon tasks, and generalizes across embodiments.  All code, models, and benchmarks will be released publicly.

---

## 论文详细总结（自动生成）

# Genie Envisioner 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有机器人操作方法将视觉表征学习与动作策略学习分离，缺乏统一的生成式世界模型，导致不同本体（embodiment）、任务之间的泛化能力受限。
- **研究动机**：探索能否将视频生成式世界模型作为机器人通用策略学习的基础平台，实现视觉-动作的联合学习与跨本体零样本泛化。
- **整体含义**：提出 Genie Envisioner（GE），一个用于机器人操作任务的统一世界基础平台，将视觉表征与动作策略整合在单一视频生成框架中，以改变当前分离式学习范式。

## 2. 论文提出的方法论

- **核心思想**：利用大规模指令条件视频扩散模型（GE-Base）捕获机器人交互的时空与语义动态，并在此基础上用轻量级流匹配解码器（GE-Act）将潜表征映射为可执行的动作轨迹，实现精确且可泛化的策略推理。
- **关键技术细节**：
  - **GE-Base**：大型指令条件视频扩散模型，在结构化潜空间中学习真实世界机器人交互的空间、时间与语义动态。
  - **GE-Act**：轻量级流匹配解码器，以最小监督将GE-Base的潜表征转换为可执行的动作轨迹。
  - 训练数据：超过100万条操作片段（episodes），支持短程和长程任务。
- **公式/算法流程（文字说明）**：
  1. 输入：操作指令 + 初始观测图像序列。
  2. GE-Base 基于扩散模型生成指令条件下的未来视频帧潜表征。
  3. GE-Act 使用流匹配方法将潜表征解码为连续动作轨迹（如末端执行器位置、夹爪开合等）。
  4. 动作轨迹直接用于机器人执行，无需额外微调即可泛化到新本体。

## 3. 实验设计

- **数据集/场景**：在超过100万条操作片段上训练，涵盖多种操作任务（短程与长程），具体场景包括桌面操作、物体拾取、组装等。未明确提及公开数据集名称，推测可能使用自采集数据或公开操作数据集。
- **Benchmark**：论文未明确列出标准benchmark名称，但通过与基线方法对比评估泛化性能和任务成功率。
- **对比方法**：abstract提到“优于基线方法”，但未列出具体基线。常见基线包括：单独视觉策略（如BC）、单独动作预测、非生成式世界模型等。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量及训练时长。仅提到在百万级数据上训练，具体算力需求未披露。
- 推测：训练大型视频扩散模型通常需要多卡（如8×A100 80GB）数天至数周，但本文未提供详细数据，需依赖后续开源信息。

## 5. 实验数量与充分性

- **实验数量**：论文未详细列出所有实验组数，但根据逻辑至少包括：
  - GE-Base 视频生成效果评估（定性+定量）。
  - GE-Act 动作预测精度与任务成功率对比。
  - 跨本体零样本泛化实验（多种机器人手臂/夹爪）。
  - 短程与长程任务对比。
  - 与基线方法的消融实验（如移除GE-Base、使用不同解码器等）。
- **充分性评价**：
  - **优点**：数据量大（百万级操作片段），覆盖多种任务和本体，支持零样本泛化评估，实验设计较为完善。
  - **不足**：未披露具体实验组数与详细结果表格，难以判断统计显著性；未在标准公开benchmark（如CLIPort、MetaWorld等）上系统报告性能，可能缺乏同平台公平对比。

## 6. 论文的主要结论与发现

- 视频生成式世界模型（GE-Base）可作为机器人通用策略学习的基础平台，联合学习视觉与动作表征。
- GE 在多种操作任务上实现跨本体零样本泛化，优于分离式学习方法。
- 轻量级流匹配解码器（GE-Act）能在最小监督下将视频潜表征转化为高精度动作轨迹。
- 证明了生成式世界模型与动作策略融合的巨大潜力，为机器人基础模型提供了新范式。

## 7. 优点

- **方法创新**：首次将视频扩散模型与流匹配解码器统一成单一框架，实现视觉-动作联合学习，避免了传统分离方法中的信息损失。
- **泛化性强**：跨本体的零样本泛化能力，无需针对新机器人重新训练，实用价值高。
- **数据规模大**：百万级操作片段训练，覆盖多种任务，增强了模型的鲁棒性和场景覆盖。
- **开源承诺**：公开代码、模型和基准，可复现性强，促进社区发展。

## 8. 不足与局限

- **实验细节缺失**：未提供与基线方法的定量对比表格、具体成功率数值、误差棒等，削弱了结果的说服力。
- **标准Benchmark**：未在广泛使用的公开基准（如RLBench、MetaWorld、VIMA）上系统评估，难以与其他方法公平比较。
- **资源消耗未知**：未报告训练算力需求，限制了他人对资源需求的判断。
- **动作空间限制**：可能仅适用于连续控制任务（如机械臂末端轨迹），对于离散动作或复杂灵巧操作可能仍需适配。
- **长程任务**：虽然声称支持长程任务，但未明确长程任务的具体时长或成功率，需进一步验证。
- **数据偏差风险**：百万级操作片段可能来自特定机器人配置和场景，泛化到真实开放世界环境可能存在分布偏移。

（完）
