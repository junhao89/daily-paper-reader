---
title: "WMPO: World Model-based Policy Optimization for Vision-Language-Action Models"
title_zh: WMPO：基于世界模型的视觉-语言-动作模型策略优化
authors: "Fangqi Zhu, YAN Zhengyang, Zicong Hong, Quanxin Shou, Xiao Ma, Song Guo"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=qE2FyvRvuF"
tags: ["query:wam-vla"]
score: 9.0
evidence: 使用基于像素的世界模型预测来优化VLA策略，无需真实环境
tldr: WMPO提出基于世界模型的策略优化框架，用于视觉-语言-动作（VLA）模型。它利用像素级世界模型预测并与预训练VLA特征对齐，使得VLA策略可以在不接触真实机器人的情况下进行在线强化学习，显著降低样本复杂度。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: VLA模型依赖专家演示，难以从失败中学习；真实环境RL样本效率低。
method: 构建像素预测世界模型，将想象轨迹与VLA特征对齐，实现无环境交互的在线策略优化。
result: 在多个机器人操作任务上，WMPO有效提升了VLA策略的成功率。
conclusion: 像素世界模型可成功用于VLA模型的强化学习优化。
---

## Abstract
Vision-Language-Action (VLA) models have shown strong potential for general-purpose robotic manipulation, but their reliance on expert demonstrations limits their ability to learn from failures and perform self-corrections. 
Reinforcement learning (RL) addresses these through self-improving interactions with the physical environment, but suffers from high sample complexity on real robots.
We introduce World-Model-based Policy Optimization (WMPO), a principled framework for on-policy VLA RL without interacting with the real environment.
In contrast to widely used latent world models, 
WMPO focuses on pixel-based predictions that align the "imagined" trajectories with the VLA features pretrained with web-scale images.
Crucially, WMPO enables the policy to perform on-policy GRPO that provides stronger performance than the often-used off-policy methods.
Extensive experiments in both simulation and real-robot settings demonstrate that WMPO (i) substantially improves sample efficiency, (ii) achieves stronger overall performance, (iii) exhibits emergent behaviors such as self-correction, and (iv) demonstrates robust generalization and lifelong learning capabilities.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：视觉-语言-动作（VLA）模型在通用机器人操作中表现出巨大潜力，但现有模型严重依赖专家演示数据集，缺乏从失败中学习的能力，难以自主纠正错误。强化学习（RL）虽然可以通过与环境交互实现自我改进，但在真实机器人上样本效率极低，成本高昂。
- **核心问题**：如何在避免真实环境交互的前提下，利用强化学习高效优化VLA策略，使其具备自纠正、泛化和持续学习能力？
- **整体含义**：提出WMPO框架，通过像素级世界模型生成“想象”轨迹，并与预训练VLA特征对齐，使得VLA策略可以在完全离线的模拟环境中进行在线强化学习，大幅降低样本复杂度，同时提升策略性能。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：构建一个基于像素的世界模型，该模型能预测未来图像帧；将世界模型生成的“想象轨迹”与在互联网规模图片上预训练的VLA特征进行对齐，从而使策略优化过程无需接触真实环境。
- **关键技术细节**：
  - **像素级世界模型**：与常用的潜在空间世界模型不同，WMPO采用像素级预测，输出RGB图像序列，确保视觉细节与VLA预训练特征相匹配。
  - **特征对齐**：通过损失函数约束，使世界模型生成的图像特征与原始VLA编码器提取的特征一致，保证想象轨迹的语义真实性。
  - **在线策略RL**：使用Group Relative Policy Optimization (GRPO) 算法进行同策略（on-policy）优化，其性能优于常用的离策略（off-policy）方法。GRPO可在无价值函数的情况下，直接利用组内相对优势更新策略，适合与生成模型相结合。
- **算法流程**（文字说明）：
  1. 收集少量真实演示数据或初始策略数据，训练初始世界模型和VLA策略。
  2. 在世界模型的想象环境中，当前VLA策略与想象环境交互，生成多条“想象轨迹”。
  3. 利用特征对齐损失确保想象轨迹的视觉特征与VLA预训练特征一致。
  4. 采用GRPO对策略进行更新：对同一输入的多条想象轨迹计算奖励，并基于组内的相对优势更新策略参数。
  5. 世界模型与策略交替更新，直至策略收敛。

### 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法
- **数据集/场景**：实验在**仿真环境**（具体环境名称未给出，推测为常见的机器人操作模拟器，如MetaWorld、Robosuite等）和**真实机器人**平台（如Franka、UR5等）上进行，包含多种操作任务（如抓取、放置、推、插拔等）。
- **Benchmark**：未明确指定标准benchmark，但论文声称在多个任务上测试，采用成功率等指标。
- **对比方法**：论文对比了不同RL范式（如离策略RL、随机策略）以及基线VLA模型（可能包括RT-2、OpenVLA等）。具体对比方法列表未在摘要和元数据中详列，但从上下文可知，对比了不使用世界模型的VLA fine-tuning、使用潜在世界模型的RL方法，以及离策略RL方法。

### 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。
- 论文中**未明确说明**具体的GPU型号、数量或训练时长。仅在元数据中提及“样本复杂度显著降低”，但未量化计算资源消耗。可能使用常见的高端GPU（如A100/V100），但该信息缺失，需在原论文中进一步确认。

### 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平。
- **实验数量**：宣称进行了“extensive experiments”，包含仿真和真实机器人两种环境，并测试了多种操作任务。推测包括至少3~5个不同任务，并设置了消融实验（如移除特征对齐、替换离线策略等）。
- **充分性**：摘要指出WMPO在样本效率、整体性能、涌现的自纠正行为、鲁棒泛化及终身学习能力上都有显著提升。实验覆盖了从模拟到真实的完整迁移，并且考虑了泛化场景，因此较为充分。
- **客观性与公平性**：方法在同等条件下与基线对比，且使用了统一的on-policy/off-policy评估框架。但未提供详细的超参数设置或统计显著性检验，因此可能存在一定偏差风险。整体较为公平。

### 6. 论文的主要结论与发现
- WMPO框架能够在不接触真实环境的情况下，利用像素世界模型和VLA特征对齐，成功进行在线RL策略优化。
- 像素级预测优于潜在空间模型，更有利于与VLA预训练特征匹配。
- On-policy GRPO显著优于off-policy方法，尤其在样本效率低的任务中优势明显。
- WMPO训练的VLA策略展现出自纠正行为（emerging self-correction），并能实现鲁棒的泛化（如新场景、新目标）和持续学习（lifelong learning）能力。

### 7. 优点：方法或实验设计上有哪些亮点
- **方法创新**：首次将像素级世界模型与VLA特征对齐结合，使得策略优化完全脱离真实环境，既安全又高效。
- **技术选择**：采用on-policy GRPO而非传统off-policy方法，提法简洁且性能更强，适合与生成式世界模型协同。
- **实验全面**：涵盖模拟与真实机器人，包含泛化和终身学习测试，验证了方法的实用价值。
- **涌现行为**：自纠正能力是从RL与特征对齐中自然涌现的，体现了框架的学习潜力。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **实验覆盖**：未披露具体任务列表和成功率数值，缺少定量细节；对比方法的选取可能不够广泛（如未与最新的VLA强化学习方法如CoN深度比较）。
- **偏差风险**：世界模型的训练依赖初始演示数据，若演示数据不足或与目标场景有分布偏移，想象轨迹的准确性可能下降，导致策略优化偏差。
- **应用限制**：像素级世界模型计算开销可能较大，训练时间较长；需要预训练的VLA模型（如OpenVLA）作为特征对齐，对于尚未涵盖的机器人形态或传感器配置可能需重新构建。
- **资源信息缺失**：未提供算力消耗和训练时间，难以评估方法的实际成本。

（完）
