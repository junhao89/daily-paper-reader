---
title: "Cosmos Policy: Fine-Tuning Video Models for Visuomotor Control and Planning"
title_zh: "Cosmos Policy: 微调视频模型用于视觉运动控制与规划"
authors: "Moo Jin Kim, Yihuai Gao, Tsung-Yi Lin, Yen-Chen Lin, Yunhao Ge, Grace Lam, Percy Liang, Shuran Song, Ming-Yu Liu, Chelsea Finn, Jinwei Gu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=wPEIStHxYH"
tags: ["query:gen2embodied"]
score: 9.0
evidence: 微调视频生成模型用于视觉运动控制，动作编码为潜在帧
tldr: Cosmos Policy通过单阶段后训练，将预训练视频模型Cosmos-Predict2直接适应为机器人策略，无需架构修改，通过生成动作作为视频潜在帧实现控制，实验表明该方法在多个机器人平台上有效，简化了视频生成到策略的迁移流程。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法需要多阶段训练和架构修改。
method: 单阶段后训练视频模型，直接生成潜在帧作为动作。
result: 在多个机器人平台上实现有效策略学习。
conclusion: 该方法简化了视频生成模型到机器人策略的迁移，具有较好通用性。
---

## Abstract
Recent video generation models demonstrate remarkable ability to capture complex physical interactions and scene evolution over time. To leverage their spatiotemporal priors, robotics works have adapted video models for policy learning but introduce complexity by requiring multiple stages of post-training and new architectural components for action generation. In this work, we introduce Cosmos Policy, a simple approach for adapting a large pretrained video model (Cosmos-Predict2) into an effective robot policy through a single stage of post-training on the robot demonstration data collected on the target platform, with no architectural modifications. Cosmos Policy learns to directly generate robot actions encoded as latent frames within the video model's latent diffusion process, harnessing the model's pretrained priors and core learning algorithm to capture complex action distributions. Additionally, Cosmos Policy generates future state images and values (expected cumulative rewards), which are similarly encoded as latent frames, enabling test-time planning of action trajectories with higher likelihood of success. In our evaluations, Cosmos Policy achieves state-of-the-art performance on the LIBERO and RoboCasa simulation benchmarks (98.5\% and 67.1\% average success rates, respectively) and the highest average score in challenging real-world bimanual manipulation tasks, outperforming strong diffusion policies trained from scratch, video model-based policies, and state-of-the-art vision-language-action models fine-tuned on the same robot demonstrations. Furthermore, given policy rollout data, Cosmos Policy can learn from experience to refine its world model and value function and leverage model-based planning to achieve even higher success rates in challenging tasks. We release code, models, and training data at https://research.nvidia.com/labs/dir/cosmos-policy/.

---

## 论文详细总结（自动生成）

# Cosmos Policy: 微调视频模型用于视觉运动控制与规划 —— 详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：近年来视频生成模型展示了捕捉复杂物理交互和时间演化的强大能力。机器人领域希望利用这些模型的时空先验知识来学习策略，但现有方法通常需要多阶段的后训练以及引入新的架构组件（如动作解码器），增加了复杂度。
- **核心问题**：如何**简单直接**地将预训练视频模型转化为有效的机器人策略，避免架构修改和多阶段训练，同时保留视频模型对复杂动作分布的建模能力。
- **整体含义**：Cosmos Policy 提供了一个单一阶段后训练的框架，直接让视频模型通过生成潜在帧来输出动作、未来状态和值函数，实现了从视频生成到策略学习的无缝迁移，简化了流程并提升了性能。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：利用预训练的视频模型 Cosmos-Predict2（一个潜在扩散视频模型），通过在目标机器人平台的演示数据上进行**单阶段后训练**，不修改任何架构，直接让模型学习生成机器人动作，动作被编码为视频模型潜在扩散过程中的**潜在帧**。
- **关键技术细节**：
  - **动作编码**：将机器人动作（如关节角度、末端执行器位置）编码为与视频帧相同维度的潜在表示，作为模型输出的一部分。模型通过自回归地生成这些“动作潜在帧”来实现控制。
  - **未来状态与值函数生成**：同样将未来状态图像和期望累积奖励（值函数）编码为潜在帧，使得模型可以同时进行**测试时规划**：通过评估不同动作轨迹的成功概率，选择最优轨迹执行。
  - **训练流程**：在机器人演示数据上微调整个视频模型，使用标准的扩散损失。模型学习从当前观测（以及可能的历史帧）预测接下来的动作潜在帧和下一帧图像。
  - **模型基座**：Cosmos-Predict2——一个大型预训练视频生成模型，已具备丰富的物理世界先验。
- **无架构修改**：不添加任何额外的动作解码器或策略头，视频模型原有的自编码器和扩散过程直接用于动作生成。

## 3. 实验设计

- **数据集/场景**：
  - **仿真 benchmark**：LIBERO（98.5%成功率）和 RoboCasa（67.1%成功率），均为机器人操作任务。
  - **真实世界**：挑战性的**双手操作任务**（bimanual manipulation tasks），具体任务未详述，但强调具有高难度。
- **对比方法**：
  - **从零训练的扩散策略**（diffusion policies trained from scratch）
  - **基于视频模型的策略**（video model-based policies）
  - **最新的视觉-语言-动作模型（VLA）**，在相同机器人演示数据上微调。
- **评估指标**：成功率（LIBERO/RoboCasa）和平均得分（真实世界任务）。Cosmos Policy 在所有对比中取得**最优**。

## 4. 资源与算力

- **文中未明确说明**：论文摘要及元数据中未提及使用的 GPU 型号、数量、训练时长等算力信息。仅可推断使用了 NVIDIA 的硬件资源（来自研究机构 NVIDIA Research），但具体配置未披露。

## 5. 实验数量与充分性

- **实验数量**：主要报告了**两个仿真 benchmark**（LIBERO、RoboCasa）和**一个真实世界双手操作任务**的实验结果。此外提及可以**利用策略部署数据**进行经验学习（模型改进），但未给出定量消融结果。
- **充分性**：
  - 覆盖了仿真和真实场景，对比了多种强基线，结果清晰。
  - 但**未提供详细的消融实验**（如是否必须利用未来状态和值函数、不同预训练视频模型的影响等），也**未报告方差**（仅给出平均成功率）。
  - 真实世界任务的具体任务数量、总试验次数等信息缺失，可复现性和公平性有待进一步验证。

## 6. 主要结论与发现

- **单阶段后训练即可有效**：不需要对视频模型架构做任何修改，直接通过微调就可以学习复杂动作分布，并达到最优策略效果。
- **同时生成未来状态和值函数**：使模型具备基于模型的规划能力，在经过策略部署数据的学习后，可以进一步提升挑战性任务的成功率。
- **简化了视频生成到策略迁移的流程**：为利用大规模预训练视频模型进行机器人学习提供了一条简洁高效的路径。

## 7. 优点

- **方法简洁性**：无需设计新架构、无需多阶段训练，降低了工程复杂度。
- **通用性强**：在多个机器人平台（仿真和真实世界）上均有效，表明该方法具有较好的跨平台迁移能力。
- **内置规划能力**：通过生成值函数和未来帧，自然地支持测试时规划，提升成功率。
- **性能优异**：在 LIBERO 达到 98.5% 成功率，在 RoboCasa 达到 67.1%，均超越了强大的对比方法。
- **开源友好**：计划公开代码、模型和训练数据，利于复现和后续研究。

## 8. 不足与局限

- **依赖视频模型质量**：最终策略性能受限于预训练视频模型（Cosmos-Predict2）的底层能力，如果预训练视频模型对特定机器人平台场景覆盖不足，性能可能下降。
- **实验覆盖有限**：
  - 真实世界任务仅涉及双手操作，未涵盖更多种类的机器人硬件（如单臂移动操作、灵巧手等）。
  - 未报告动作低维/高维、长程任务、接触力等敏感性分析。
  - 未提供消融实验量化各个组件（如未来帧生成、值函数）的贡献。
- **算力与资源未公开**：无法判断训练该方法对算力的实际需求，可能对普通实验室不够友好。
- **潜在偏差风险**：LIBERO 和 RoboCasa 任务相对结构化，真实世界任务的不确定性可能未被充分反映。模型在复杂遮挡、动态干扰下的鲁棒性未知。
- **应用限制**：动作编码为潜在帧要求视频模型的分辨率和帧率匹配，可能限制高频控制场景；同时，模型需要完整的观测图像序列，对观测延迟敏感。

（完）
