---
title: "Cosmos Policy: Fine-Tuning Video Models for Visuomotor Control and Planning"
title_zh: Cosmos Policy：微调视频模型实现视觉运动控制与规划
authors: "Moo Jin Kim, Yihuai Gao, Tsung-Yi Lin, Yen-Chen Lin, Yunhao Ge, Grace Lam, Percy Liang, Shuran Song, Ming-Yu Liu, Chelsea Finn, Jinwei Gu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=wPEIStHxYH"
tags: ["query:gen2embodied"]
score: 9.0
evidence: 微调视频模型用于视觉运动控制与规划
tldr: 现有将视频模型用于机器人策略的方法需要复杂的多阶段后训练和架构修改。本文提出 Cosmos Policy，仅需单阶段后训练即可将大型预训练视频模型（Cosmos-Predict2）直接转化为有效机器人策略，无需任何架构改变。策略直接生成编码为潜在帧的机器人动作。实验表明，该方法在多种操作任务上达到或超越专门设计的策略，同时保持视频模型的生成能力。该工作简化了视频到策略的迁移流程，为生成式模型用于具身控制提供了简洁高效的范式。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 简化视频模型到机器人策略的复杂迁移过程，避免多阶段训练和架构改动。
method: 对预训练视频模型进行单阶段后训练，直接生成编码为潜在帧的动作，保留原始架构。
result: 在操作任务上性能与专门策略相当或更优，同时保持视频生成能力。
conclusion: 视频模型可通过简单微调直接用于机器人控制，降低了生成式模型在具身智能中的应用门槛。
---

## Abstract
Recent video generation models demonstrate remarkable ability to capture complex physical interactions and scene evolution over time. To leverage their spatiotemporal priors, robotics works have adapted video models for policy learning but introduce complexity by requiring multiple stages of post-training and new architectural components for action generation. In this work, we introduce Cosmos Policy, a simple approach for adapting a large pretrained video model (Cosmos-Predict2) into an effective robot policy through a single stage of post-training on the robot demonstration data collected on the target platform, with no architectural modifications. Cosmos Policy learns to directly generate robot actions encoded as latent frames within the video model's latent diffusion process, harnessing the model's pretrained priors and core learning algorithm to capture complex action distributions. Additionally, Cosmos Policy generates future state images and values (expected cumulative rewards), which are similarly encoded as latent frames, enabling test-time planning of action trajectories with higher likelihood of success. In our evaluations, Cosmos Policy achieves state-of-the-art performance on the LIBERO and RoboCasa simulation benchmarks (98.5\% and 67.1\% average success rates, respectively) and the highest average score in challenging real-world bimanual manipulation tasks, outperforming strong diffusion policies trained from scratch, video model-based policies, and state-of-the-art vision-language-action models fine-tuned on the same robot demonstrations. Furthermore, given policy rollout data, Cosmos Policy can learn from experience to refine its world model and value function and leverage model-based planning to achieve even higher success rates in challenging tasks. We release code, models, and training data at https://research.nvidia.com/labs/dir/cosmos-policy/.

---

## 论文详细总结（自动生成）

# Cosmos Policy: 微调视频模型实现视觉运动控制与规划

## 1. 论文的核心问题与整体含义

- **研究动机**：近年来，视频生成模型在捕捉复杂物理交互和场景随时间演化方面展现出强大能力。机器人领域尝试利用这些模型的时空先验进行策略学习，但现有方法通常需要多阶段后训练和新增架构组件来生成动作，流程复杂且成本高。
- **核心问题**：能否通过简单方式将大型预训练视频模型直接转化为有效的机器人策略，避免多阶段训练和架构修改？
- **整体含义**：本文提出Cosmos Policy，仅需单阶段后训练即可将视频模型变为高效策略，同时保持其生成能力，降低了生成式模型在具身智能中的应用门槛，为视频到策略的迁移提供了简洁范式。

## 2. 论文提出的方法论

- **核心思想**：利用预训练视频模型（Cosmos-Predict2）的潜在扩散过程，直接将机器人动作编码为“潜在帧”（latent frames）进行生成，无需改变原有架构。同时，模型还能生成未来状态图像和期望累积奖励（价值函数），这些同样编码为潜在帧，从而实现测试时规划。
- **关键技术细节**：
  - 单阶段后训练：仅对机器人演示数据进行一次微调，不涉及多阶段训练。
  - 动作表示：动作被编码为与视频帧相同维度的潜在表示，融入扩散模型的噪声预测过程。
  - 多任务能力：模型同时输出动作、未来状态和价值，这些信息作为附加潜在帧，利用模型原有学习算法捕获复杂动作分布。
  - 规划机制：在测试时，通过生成多个动作轨迹，并依据价值函数选择高成功概率的轨迹，实现模型基规划。
- **公式/算法流程**（文字说明）：
  - 训练阶段：给定机器人演示数据（观测图像、动作序列），将动作序列映射为潜在特征，与图像帧一起作为扩散模型的输入。模型学习去噪过程，同时预测动作潜在帧、未来图像帧和价值帧。
  - 测试阶段：输入当前观测，执行扩散采样生成一系列候选动作潜在帧，解码为实际动作；同时生成未来状态和价值。利用价值进行轨迹选择，执行最优动作。

## 3. 实验设计

- **使用数据集/场景**：
  - LIBERO仿真基准（包含多种桌面操作任务）
  - RoboCasa仿真基准（家庭场景操作任务）
  - 真实世界双臂操作任务（复杂操作挑战）
- **Benchmark**：在这些基准上评估成功率和平均得分。
- **对比方法**：
  - 从头训练的强扩散策略（Diffusion Policy）
  - 基于视频模型的策略（Video-based policy）
  - 最优的视觉-语言-动作模型（VLA）在相同演示数据上微调的结果
- **结果**：
  - LIBERO: 平均成功率98.5%
  - RoboCasa: 平均成功率67.1%
  - 真实双臂任务：最高平均分，超越所有对比方法

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量和训练时长。但推测可能使用NVIDIA GPU集群（因来自NVIDIA研究实验室）。具体算力信息需查阅论文全文或附录。

## 5. 实验数量与充分性

- 实验覆盖了三类基准：两个仿真（LIBERO、RoboCasa）和一个真实世界任务，共多个子任务。
- 对比了多种基线方法（扩散策略、视频基策略、VLA模型），并提供了成功率指标。
- 消融实验：文本未明确列出，但从摘要可知还进行了“从经验中学习”的测试——利用策略回滚数据进一步优化世界模型和价值函数，提升规划性能。
- **充分性**：实验设计较为全面，覆盖仿真和真实场景，对比基线有代表性。但缺少对模型规模、训练数据量、不同任务难度分布的详细分析，可进一步补充。

## 6. 论文的主要结论与发现

- 视频模型可通过简单的单阶段微调直接用于机器人控制，无需架构改动或多阶段训练。
- Cosmos Policy在多个基准上达到或超越专门设计的策略，且在真实世界复杂任务中表现最优。
- 模型不仅能生成动作，还能生成未来状态和值函数，支持测试时规划，进一步提升成功率。
- 通过利用策略回滚数据，模型可以持续改进其世界模型和价值函数，实现从经验中学习。

## 7. 优点

- **方法简洁**：单阶段后训练，无需架构修改，显著简化了视频模型到策略的迁移流程。
- **多输出能力**：同时输出动作、未来图像和值函数，支持规划与学习，增强鲁棒性。
- **性能优异**：在多个基准上达到SOTA，尤其是Libro场景下接近完美成功率。
- **泛化性**：在仿真和真实任务上均有效，验证了方法的通用性。
- **开放资源**：开源代码、模型和训练数据，有利于社区复现和进一步研究。

## 8. 不足与局限

- **算力信息不完整**：未公开训练所需GPU数量和时间，难以评估资源需求。
- **实验覆盖有限**：仅包含LIBERO、RoboCasa和真实双臂任务，未涉及更复杂的移动操作或动态环境。
- **潜在偏差**：视频模型预训练数据分布可能与机器人任务存在差异，对未见场景的泛化能力有待验证。
- **应用限制**：依赖高质量演示数据；动作编码为潜在帧可能引入信息损失；规划机制依赖于价值函数的准确性，在复杂随机环境中可能不稳定。
- **消融实验细节缺失**：摘要中未详细展示模型不同组件的贡献（如价值分支、规划模块的单独效果），需参考全文。

（完）
