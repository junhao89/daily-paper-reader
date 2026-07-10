---
title: "Ctrl-World: A Controllable Generative World Model for Robot Manipulation"
title_zh: Ctrl-World：用于机器人操作的可控生成世界模型
authors: "Yanjiang Guo, Lucy Xiaoyang Shi, Jianyu Chen, Chelsea Finn"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=748bHL2BAv"
tags: ["query:wam-vla"]
score: 9.0
evidence: 用于机器人操作推演的可控生成世界模型
tldr: 针对通用机器人策略评估需要大量真实推演的问题，提出可控生成世界模型Ctrl-World，支持多视角图像预测和多步交互，与现有通用策略兼容。在多个操作任务上，世界模型生成的想象推演与真实环境高度一致，有效用于策略评估和错误纠正。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 通用策略的评估与改进依赖大量真实数据，成本高且难以扩展。
method: 构建可控世界模型，支持多视角图像生成和条件控制，用于策略想象推演。
result: 生成的想象推演与真实环境高度一致，并成功提升策略在未见物体上的表现。
conclusion: 可控世界模型为通用机器人策略的评估与提升提供了可扩展方案。
---

## Abstract
Generalist robot policies can now perform a wide range of manipulation skills, but evaluating and improving their ability with unfamiliar objects and instructions remains a significant challenge. Rigorous evaluation requires a large number of real-world rollouts, while systematic improvement demands additional corrective data with expert labels. Both of these processes are slow, costly, and difficult to scale.
World models offer a promising, scalable alternative by enabling policies to rollout within imagination space. 
However, a key challenge is building a controllable world model that can handle multi-step interactions with generalist robot policies. 
This requires a world model compatible with modern generalist policies by supporting multi-view prediction, fine-grained action control, and consistent long-horizon interactions, which is not achieved by previous works.
In this paper, we make a step forward by introducing a controllable multi-view world model that can be used to evaluate and improve the instruction-following ability of generalist robot policies. Our model maintains long-horizon consistency with a pose-conditioned memory retrieval mechanism and achieves precise action control through frame-level action conditioning. Trained on the DROID dataset (95k trajectories, 564 scenes), our model generates spatially and temporally consistent trajectories under novel scenarios and new camera placements for over 20 seconds. We show that our method can accurately rank policy performance without real-world robot rollouts. Moreover, by synthesizing successful trajectories in imagination and using them for supervised fine-tuning, our approach can improve policy success by 44.7\%. Videos can be found at https://sites.google.com/view/ctrl-world.

---

## 论文详细总结（自动生成）

# Ctrl-World：用于机器人操作的可控生成世界模型

## 1. 论文的核心问题与整体含义

- **研究动机**：通用机器人策略（generalist robot policies）已能执行多种操作技能，但在面对未见过的物体和指令时，评估和改进其能力仍面临巨大挑战。传统的评估需要大量真实世界推演（rollouts），而系统性改进则需要额外的带专家标签的纠正数据。这两种过程都耗时、昂贵且难以扩展。
- **整体含义**：本文提出一种可扩展的替代方案——世界模型（world model），通过在想象空间中进行策略推演，从而降低对真实数据的依赖。核心贡献是构建了一个**可控的多视角世界模型**，能与现代通用策略兼容，支持多步交互、多视角预测、细粒度动作控制以及长时一致性，填补了先前工作在可控性和多步交互方面的空白。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：训练一个可控的多视角世界模型，用于评估和提升通用机器人策略的指令跟随能力。该模型在想象空间中生成与真实环境高度一致的轨迹，从而替代部分真实推演。
- **关键技术细节**：
  1. **姿态条件记忆检索机制**（pose-conditioned memory retrieval）：通过维护隐式的记忆，结合当前姿态条件，从训练数据中检索相关特征，维持长时一致性（超过20秒）。
  2. **帧级动作条件控制**（frame-level action conditioning）：在每个时间步将动作作为条件输入模型，实现对精细动作的精确控制。
  3. **多视角预测**：模型能够同时生成来自多个视角的图像序列，与现有通用策略的输出空间兼容。
  4. **训练数据**：在 DROID 数据集（包含95k条轨迹、564个场景）上训练，使模型能够应对新场景和新相机位姿下的空间及时间一致生成。
- **算法流程（文字说明）**：模型输入当前多视角图像、动作序列以及用户指令 → 通过姿态条件记忆检索模块编码历史信息 → 使用帧级动作条件逐步生成下一帧多视角图像 → 循环迭代直至生成完整轨迹（最长超过20秒）。生成的想象轨迹可用于策略评估（排名）或作为监督微调数据（supervised fine-tuning）以提升策略成功率。

## 3. 实验设计

- **数据集**：DROID 数据集（95k 条轨迹，564 个场景）。该数据集涵盖多种操作任务，真实世界数据量较大。
- **基准（Benchmark）**：论文未明确列出具体的 benchmark 名称，但评估内容包括：
  - 生成轨迹的时空一致性（空间和时间上的一致性）。
  - 使用想象轨迹对策略性能进行排名（与实际推演的排名对比）。
  - 通过想象轨迹进行监督微调后，策略成功率提升幅度。
- **对比方法**：摘要未列出具体对比方法，但可以推断与先前不可控或单视角的世界模型进行了对比。

## 4. 资源与算力

- **未明确说明**：摘要中未提及训练所用的 GPU 型号、数量、训练时长等具体算力信息。仅指出模型在 DROID 数据集上训练，但未提供硬件资源细节。因此无法评估算力开销。

## 5. 实验数量与充分性

- **实验数量**：摘要主要给出了两个定量结果：
  1. 可以**准确地对策略性能进行排名**（无需真实推演）。
  2. 通过想象轨迹进行监督微调，取得**44.7% 的成功率提升**。
- **充分性与客观性**：
  - 提供了一个大规模真实数据集（DROID）训练，覆盖 564 个场景，场景多样性较好。
  - 但未展示消融实验（如去掉记忆检索或帧级动作条件后的效果）、不同策略类型下的泛化测试、多步交互的定量指标（如 FID、PSNR）等。因此实验完整性一般，但针对核心主张（可控生成、策略评估与改进）给出了关键指标，具有一定说服力。
  - 由于摘要信息有限，难以完全判断是否公平对比（未列出 baseline）。

## 6. 论文的主要结论与发现

1. 提出了一种可控多视角世界模型，能在超过20秒的时长内生成空间和时间一致的轨迹，适用于新场景和新相机位姿。
2. 该模型可以准确地对通用策略的性能进行排序，从而替代部分真实机器人推演，降低评估成本。
3. 通过合成成功轨迹并进行监督微调，可将策略成功率提升 44.7%，证明其在策略改进中的实用性。
4. 模型与通用机器人策略兼容，支持多视角预测和细粒度动作控制，填补了可控世界模型在机器人操作领域的空白。

## 7. 优点

- **创新性**：首次提出与通用策略兼容的可控多视角世界模型，解决长时一致性和动作精确控制两个关键难点。
- **实用性**：直接用于策略评估（排名）和策略改进（微调），具有实际工程价值，可大幅减少真实数据收集成本。
- **数据规模**：使用大规模 DROID 数据集（95k轨迹），训练数据充分，有助于泛化。
- **可复现性**：提供了网站链接（https://sites.google.com/view/ctrl-world）展示视频，有助于直观理解生成效果。

## 8. 不足与局限

- **实验覆盖不足**：摘要未提供消融实验、不同场景下的定量指标（如 FVD、PSNR、LPIPS 等图像质量指标），也未与多种 baseline（如 Dreamer、VQ-VAE 等）进行系统性对比。
- **偏差风险**：仅基于 DROID 数据集训练，可能无法覆盖极端或长尾分布场景；若策略本身具有领域漂移，世界模型可能无法准确模仿。
- **应用限制**：模型依赖姿态条件记忆检索，可能对摄像头标定和姿态估计精度敏感；帧级动作条件需要访问精确的动作序列，对于部分策略可能获取困难。
- **资源未报告**：未说明训练算力要求，难以评估可复现成本和门槛。
- **长期稳定性**：虽然宣称超过20秒，但文献中未提供定量指标证明长时间生成中不会出现漂移或退化。

（完）
