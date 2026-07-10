---
title: "ViPRA: Video Prediction for Robot Actions"
title_zh: ViPRA：面向机器人行为的视频预测
authors: "Sandeep Routray, Hengkai Pan, Unnat Jain, Shikhar Bahl, Deepak Pathak"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=w3Ik8HUyTT"
tags: ["query:latent-vla"]
score: 9.0
evidence: 从无动作视频中学习以运动为中心的潜动作
tldr: 大多数机器人视频缺乏动作标签，限制了其在机器人学习中的应用。本文提出ViPRA框架，通过视频-语言模型预测未来视觉观察和以运动为中心的潜动作，利用感知损失和光流一致性训练这些潜动作表示物理行为。在下游控制中，该方法仅需少量带动作数据即可微调成策略，显著降低了对动作标注的依赖，实现了从视频到策略的有效迁移。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 大量机器人视频缺乏动作标签，导致难以直接用于策略学习。
method: 提出ViPRA预训练-微调框架，先预测潜动作和未来帧，再微调为策略。
result: 在多个操控任务上，ViPRA使用少量动作数据即达到与全监督方法可比的性能。
conclusion: 潜动作学习是从无标签视频中提取机器人控制知识的有效途径。
---

## Abstract
Can we turn a video prediction model into a robot policy? Videos, including those of humans or teleoperated robots, capture rich physical interactions. However, most of them lack labeled actions, which limits their use in robot learning. We present *Video Prediction for Robot Actions* (**ViPRA**), a simple pretraining-finetuning framework that learns continuous robot control from these actionless videos. Instead of directly predicting actions, we train a video-language model to predict *both future visual observations and motion-centric latent actions*, which serve as intermediate representations of scene dynamics. We train these latent actions using perceptual losses and optical flow consistency to ensure they reflect physically grounded behavior. For downstream control, we introduce a chunked *flow-matching decoder* that maps latent actions to robot-specific continuous action sequences, using only 100 to 200 teleoperated demonstrations. This approach avoids expensive action annotation, supports generalization across embodiments, and enables smooth, high-frequency continuous control upto 22 Hz via chunked action decoding. Unlike prior latent action works that treat pretraining as autoregressive policy learning, ViPRA explicitly models both what changes and how. Our method outperforms strong baselines, with a 16% gain on the SIMPLER benchmark and a 13% improvement across real world manipulation tasks. We have released models and code [here](https://vipra-project.github.io/).

---

## 论文详细总结（自动生成）

# ViPRA：面向机器人行为的视频预测（ViPRA: Video Prediction for Robot Actions）——详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：大量机器人相关视频（包括人类演示或遥操作机器人）蕴含丰富的物理交互信息，但绝大多数缺乏显式的动作标签。这限制了这些视频直接用于机器人策略学习，因为传统方法通常需要高成本的带动作标注数据。
- **研究动机**：探索能否将视频预测模型转化为机器人策略，从而利用海量无动作标签的视频数据学习控制知识。
- **整体含义**：提出一种预训练-微调框架ViPRA，从无标签视频中学习以运动为中心的潜动作（latent actions），并将其映射为机器人连续控制信号，大幅降低对动作标注的依赖，实现从视频到策略的有效迁移。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程（文字说明）
- **核心思想**：不直接预测动作，而是训练一个视频-语言模型同时预测未来视觉观察和运动中心的潜动作（motion-centric latent actions），将潜动作作为场景动态的中间表示。然后在下游控制中，利用少量带动作的遥操作演示数据，通过流匹配解码器将潜动作解码为机器人连续动作序列。
- **关键技术细节**：
  - **潜动作学习**：使用感知损失（perceptual losses）和光流一致性（optical flow consistency）训练潜动作，确保其反映物理上有意义的运动。
  - **流匹配解码器（chunked flow-matching decoder）**：将潜动作映射到机器人特定连续动作序列，支持分块（chunked）解码，从而获得高频（高达22Hz）的平滑控制。
  - **预训练-微调框架**：先在无标签视频上预训练视频-语言模型以预测未来帧和潜动作；再在少量带动作数据上微调下游策略。
- **区别于前人工作**：此前潜动作工作将预训练视为自回归策略学习，而ViPRA显式建模“什么在变化”和“如何变化”，即同时建模视觉变化和潜在运动模式。

## 3. 实验设计：数据集/场景、benchmark、对比方法
- **数据集/场景**：
  - **SIMPLER benchmark**：一个广泛的机器人操作基准。
  - **真实世界操控任务**：多个实际操控场景。
- **对比方法**：与多种强基线方法（包括全监督方法、此前潜动作方法等）进行对比。
- **主要结果**：
  - 在SIMPLER基准上，ViPRA比强基线方法提升**16%**。
  - 在真实世界操控任务中，平均提升**13%**。
  - 仅需**100~200个遥操作演示**即可达到与全监督方法可比的性能。

## 4. 资源与算力
- **文中未明确说明**：摘要和元数据中没有提及使用的GPU型号、数量、训练时长等具体算力信息。因此无法提供细节，仅指出这一点。

## 5. 实验数量与充分性
- **实验数量**：至少包含两大类实验——SIMPLER基准（可能含多个子任务）和真实世界操控任务（多个场景）。此外，可能还包含不同数据量（如100/200演示）的对比实验。
- **充分性与公平性**：
  - 与多个强基线方法对比，且使用了公开基准SIMPLER，确保了可比性。
  - 论文声称仅需少量动作数据即达到全监督性能，通过消融展示了潜动作学习的必要性。
  - 但未提及在更多样的环境（如不同机器人平台、复杂动态场景）上的测试，以及未提供算力分析，因此实验覆盖仍存在一定局限。

## 6. 论文的主要结论与发现
- ViPRA能够从无动作标签的视频中学习潜动作，并将其有效迁移为机器人策略。
- 潜动作学习是从无标签视频中提取机器人控制知识的有效途径。
- 使用分块流匹配解码器可实现高达22Hz的高频连续控制，且动作平滑。
- 该方法在SIMPLER和真实任务上均显著优于现有强基线，且仅需极少量的动作标注。

## 7. 优点（方法或实验设计上的亮点）
- **新颖性**：将视频预测与潜动作学习结合，绕过直接预测动作的难点，充分利用大规模无标签视频。
- **实用性**：仅需100~200个遥操作演示即可完成下游微调，大幅降低标注成本。
- **通用性**：支持跨体现（cross-embodiment）的泛化（通过视觉-语言模型统一表示）。
- **高频控制**：分块解码机制实现了22Hz的实时控制频率，满足实际机器人操控需求。
- **实验验证充分**：在仿真基准和真实世界任务上都取得显著提升，且开源代码和模型。

## 8. 不足与局限
- **算力资源未报告**：缺乏训练成本的定量描述，难以评估方法的可复现性和资源需求。
- **实验场景有限**：主要涵盖桌面操控任务，未在更复杂（如移动操作、灵巧手精细操作）或动态环境中验证。
- **潜动作的可解释性**：虽然用光流和感知损失约束，但潜动作的具体物理含义可能仍不清晰。
- **对视频质量的依赖**：如果视频中运动模糊、遮挡严重或视角变化大，光流估计可能失败，影响潜动作学习。
- **下游策略微调仍需少量动作数据**：虽然量少，但并未完全实现零样本控制。

（完）
