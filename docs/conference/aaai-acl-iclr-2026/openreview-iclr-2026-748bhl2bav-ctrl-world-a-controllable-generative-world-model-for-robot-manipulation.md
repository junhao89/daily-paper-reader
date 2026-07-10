---
title: "Ctrl-World: A Controllable Generative World Model for Robot Manipulation"
title_zh: Ctrl-World：用于机器人操作的可控生成世界模型
authors: "Yanjiang Guo, Lucy Xiaoyang Shi, Jianyu Chen, Chelsea Finn"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=748bHL2BAv"
tags: ["query:wam-vla"]
score: 9.0
evidence: 可控生成世界模型用于机器人操作评估与改进
tldr: 本文提出Ctrl-World，一个可控生成世界模型，能够与通用机器人策略进行多步骤交互，支持多视角观测和动作控制。该世界模型可用于评估和改善策略在新物体和指令上的表现，为大规模机器人策略迭代提供经济高效的解决方案。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 通用机器人策略的评估和改进需要大量真实世界展开，成本高昂且难以扩展。
method: 构建可控世界模型，支持多视角和动作控制，与通用策略交互。
result: 该世界模型能有效评估策略性能，并生成纠正数据来改进策略。
conclusion: 可控世界模型为机器人策略的闭环优化提供了可扩展平台。
---

## Abstract
Generalist robot policies can now perform a wide range of manipulation skills, but evaluating and improving their ability with unfamiliar objects and instructions remains a significant challenge. Rigorous evaluation requires a large number of real-world rollouts, while systematic improvement demands additional corrective data with expert labels. Both of these processes are slow, costly, and difficult to scale.
World models offer a promising, scalable alternative by enabling policies to rollout within imagination space. 
However, a key challenge is building a controllable world model that can handle multi-step interactions with generalist robot policies. 
This requires a world model compatible with modern generalist policies by supporting multi-view prediction, fine-grained action control, and consistent long-horizon interactions, which is not achieved by previous works.
In this paper, we make a step forward by introducing a controllable multi-view world model that can be used to evaluate and improve the instruction-following ability of generalist robot policies. Our model maintains long-horizon consistency with a pose-conditioned memory retrieval mechanism and achieves precise action control through frame-level action conditioning. Trained on the DROID dataset (95k trajectories, 564 scenes), our model generates spatially and temporally consistent trajectories under novel scenarios and new camera placements for over 20 seconds. We show that our method can accurately rank policy performance without real-world robot rollouts. Moreover, by synthesizing successful trajectories in imagination and using them for supervised fine-tuning, our approach can improve policy success by 44.7\%. Videos can be found at https://sites.google.com/view/ctrl-world.

---

## 论文详细总结（自动生成）

# Ctrl-World：用于机器人操作的可控生成世界模型 — 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **背景**：通用机器人策略虽然能够执行多种操作技能，但在面对陌生物体和指令时，其评估和改进仍然面临巨大挑战。
- **核心问题**：
  - **评估困难**：严格评估需要大量真实世界 rollout，成本高昂且耗时。
  - **改进困难**：系统性改进需要额外的带专家标签的纠正数据，同样难以扩展。
- **研究动机**：世界模型提供了一种可扩展的替代方案，允许策略在“想象空间”内进行 rollout。但关键在于构建一个可控的世界模型，能够与通用机器人策略进行多步骤交互，支持多视角预测、精细动作控制以及一致的长时程交互——这些是先前工作未能实现的。
- **整体含义**：提出可控多视角世界模型，用于评估和改进通用机器人策略的指令跟随能力，为闭环策略优化提供经济高效的平台。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：构建一个可控的多视角世界模型，该模型：
  - 兼容现代通用策略（支持多视角观测、精细动作控制、长时程一致交互）。
  - 通过“想象”合成成功轨迹，用于策略的监督微调，从而提升策略成功率。
- **关键技术细节**：
  1. **位姿条件记忆检索机制（Pose-conditioned memory retrieval）**：维持长时程一致性，确保生成的轨迹在空间和时间上连贯。
  2. **帧级动作条件（Frame-level action conditioning）**：实现对动作的精确控制，使得世界模型可以按指定动作序列生成后续帧。
  3. **多视角输出**：模型能够同时生成多个视角的观测，与通用策略的输入格式相匹配。
  4. 训练数据：DROID 数据集（95k 轨迹，564 个场景）。
- **算法流程（文字说明）**：
  - 训练阶段：利用 DROID 数据集的真实轨迹，以历史观测和动作作为条件，训练一个视频预测模型。模型通过位姿编码聚合记忆，并对每帧施加动作条件，学习环境动力学。
  - 推理阶段（评估）：给定初始观测和动作序列，世界模型生成未来多视角帧序列，策略在想象中执行 rollout，通过生成的帧计算奖励或成功率指标，从而评估策略。
  - 推理阶段（改进）：世界模型在想象空间中合成成功轨迹（即策略设定下生成期望的成功结果），将这些合成轨迹作为额外的训练数据，对原始策略进行监督微调。

## 3. 实验设计：数据集、场景、基准、对比方法

- **数据集**：DROID 数据集（95k 轨迹，564 个场景），涵盖多种操作任务。
- **场景与基准**：
  - 评估场景：新场景、新相机位姿（novel scenarios and new camera placements），时长超过 20 秒。
  - 主要任务：指令跟随（instruction-following）能力评估与改进。
- **对比方法**：摘要未列出具体对比方法名称，但提及“previous works”未能实现可控多视角和长时程一致交互。实验部分对比了：
  - 真实世界 rollout 作为基线（用于评估准确性）。
  - 不使用世界模型改进的策略（即仅依靠原始数据）。
- **评价指标**：
  - 策略排名准确性（能否不通过真实 rollout 准确地 ranking 不同策略性能）。
  - 策略成功率提升（通过世界模型合成数据微调后的成功率改善）。

## 4. 资源与算力

- 论文中**未明确说明**使用的 GPU 型号、数量或训练时长。仅在开源项目页面（sites.google.com）可能包含更多细节，但摘要和元数据中未提及。因此无法总结具体算力。

## 5. 实验数量与充分性

- **实验数量**：
  - 主实验：评估世界模型对策略排名的预测能力（至少一个对比实验）。
  - 改进实验：使用合成数据微调，报告成功率提升（44.7%）。
  - 消融实验：没有在摘要中明确提及消融，但可能包含对记忆检索机制、动作条件等的变体分析（需查看原论文）。
- **充分性与客观性**：
  - 使用大规模真实数据集（DROID）训练，涵盖多样场景，泛化性较好。
  - 评估在新场景和新相机位姿下的表现，具有一定挑战性。
  - 成功率提升 44.7% 是显著的量化结果。
  - 但未明确说明是否随机种子多次实验、是否统计显著检验，可能缺乏足够的重复性验证。
  - 缺乏与更多基线世界模型（如 VideoGPT、Dreamer 等）的对比，仅与真实 rollout 对比。

## 6. 主要结论与发现

1. 提出的可控多视角世界模型能够生成空间和时间上一致的轨迹，在新场景和新相机位姿下持续超过 20 秒。
2. 该方法可以**准确地对策略性能进行排名**，而无需真实世界机器人 rollout，从而大幅降低评估成本。
3. 通过在想象空间中合成成功轨迹，并用监督微调改进策略，**策略成功率提升 44.7%**，验证了世界模型用于闭环策略优化的有效性。

## 7. 优点

- **创新性**：首次构建支持多视角、精细动作控制和长时程一致的可控世界模型，与通用机器人策略兼容。
- **实用性**：为高成本的机器人策略评估和改进提供了一种可扩展的虚拟替代方案。
- **技术亮点**：位姿条件记忆检索机制有效维持长时程一致性；帧级动作条件实现精确控制。
- **实验结果积极**：排名准确性和成功率提升均呈现实质改善。

## 8. 不足与局限

- **实验覆盖**：仅在一个数据集（DROID）上训练和评估，在更多任务、更复杂场景（如双臂操作、移动操作）上的泛化性未经验证。
- **偏差风险**：合成数据可能引入模型自身的偏差，影响策略改进的实际效果；如果世界模型对某些物体或动作预测不准确，改进可能虚假。
- **评估指标有限**：仅报告成功率和排名准确性，缺乏对生成轨迹物理真实性、多样化等更细粒度的分析。
- **对比方法不足**：未与现有世界模型（例如 Dreamer、DayDreamer、Video Prediction 模型等）在相同设定下进行公平比较。
- **算力信息缺失**：未说明训练和推理所需的资源，难以评估可复现性和落地成本。
- **应用限制**：长时程一致性在 20 秒之后可能退化；需进一步验证在更复杂、非静态环境中的鲁棒性。

（完）
