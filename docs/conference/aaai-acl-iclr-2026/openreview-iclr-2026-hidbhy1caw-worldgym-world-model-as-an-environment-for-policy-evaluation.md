---
title: "WorldGym: World Model as An Environment for Policy Evaluation"
title_zh: WorldGym：世界模型作为策略评估环境
authors: "Julian Hector Quevedo, Ansh Kumar Sharma, Yixiang Sun, Varad Suryavanshi, Percy Liang, Sherry Yang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=hidBHy1CAw"
tags: ["query:wam-vla"]
score: 9.0
evidence: 世界模型作为策略评估环境，通过蒙特卡洛展开
tldr: 本文提出WorldGym，将自回归动作条件视频生成模型作为策略评估环境，通过蒙特卡洛展开和VLM奖励函数评估VLA策略。在真实机器人上验证，世界模型中的成功率与真实世界高度相关，且能保持策略相对排名。该方法大幅降低评估成本。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 真实世界评估成本高，仿真器构建费力，需要可扩展的替代方案。
method: 利用动作条件视频生成模型作为环境，通过蒙特卡洛展开和VLM奖励评估策略。
result: 世界模型中的成功率与真实世界高度相关，能区分策略优劣。
conclusion: 世界模型可作为有效的策略评估代理，加速机器人研究闭环。
---

## Abstract
Evaluating robot control policies is difficult: real-world testing is costly, and handcrafted simulators require manual effort to improve in realism and generality. We propose a world-model-based policy evaluation environment (WorldGym), an autoregressive, action-conditioned video generation model which serves as a proxy to real world environments. Policies are evaluated via Monte Carlo rollouts in the world model, with a vision-language model providing rewards. We evaluate a set of VLA-based real-robot policies in the world model using only initial frames from real robots, and show that policy success rates within the world model highly correlate with real-world success rates. Moreoever, we show that WorldGym is able to preserve relative policy rankings across different policy versions, sizes, and training checkpoints. Due to requiring only a single start frame as input, the world model further enables efficient evaluation of robot policies' generalization ability on novel tasks and environments. We find that modern VLA-based robot policies still struggle to distinguish object shapes and can become distracted by adversarial facades of objects. While generating highly realistic object interaction remains challenging, WorldGym faithfully emulates robot motions and offers a practical starting point for safe and reproducible policy evaluation before deployment.

---

## 论文详细总结（自动生成）

好的，以下是根据提供的论文元数据（标题、摘要、TL;DR等）生成的中文总结。

---

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：真实世界中评估机器人控制策略成本高昂，且手工搭建仿真器需大量人力且难以提升真实感和泛化能力。需要一种可扩展、低成本且能反映真实性能的替代评估方案。
- **整体含义**：提出基于世界模型的策略评估环境WorldGym，利用自回归动作条件视频生成模型作为真实环境的代理，实现高效、安全的策略离线评估，加速机器人研究从仿真到部署的闭环。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将自回归动作条件视频生成模型视为“世界模型”，将其作为策略评估环境。策略通过在世界模型中进行蒙特卡洛展开（Monte Carlo rollouts）生成轨迹，然后使用视觉语言模型（VLM）为这些轨迹提供奖励分数，从而计算策略的成功率或其他指标。
- **关键技术细节**：
  - **世界模型**：基于动作条件的视频生成模型（自回归方式），输入为初始帧和动作序列，输出后续帧。
  - **评估流程**：
    1. 给定一个真实场景的初始帧（单个图像）。
    2. 运行待评估的VLA（视觉-语言-动作）策略，在世界模型中生成多步动作与对应的预测视频帧（蒙特卡洛展开）。
    3. 使用预训练的VLM对生成的视频序列进行奖励打分（如判断任务是否成功）。
    4. 重复多次展开，统计成功率作为策略性能指标。
  - **公式/算法**（文字说明）：无显式公式，算法流程为：策略π → 在世界模型M中生成轨迹τ → VLM R(τ) → 累积奖励/成功率。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集与场景**：使用真实机器人（如VLA基础策略）的初始帧，涉及多种操作任务（具体任务列表未在元数据中详述，但提到“区分物体形状”“对抗性外观干扰”等）。
- **Benchmark**：以真实世界实验中的策略成功率为金标准（ground truth），对比世界模型评估得到的成功率与其相关性。
- **对比方法**：主要对比不同版本、不同规模、不同训练检查点的策略在世界模型和真实世界中的相对排名一致性。未提及与其他世界模型评估方法（如传统模拟器Gauntlet等）的对比，但强调了与真实世界的高度相关性。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力；若未明确说明，也请指出这一点

- **文中未明确说明使用的GPU型号、数量及训练时长**。仅提到世界模型是“自回归动作条件视频生成模型”，但未给出其训练所需的算力细节。这是一个缺失信息。

## 5. 实验数量与充分性：大概做了多少组实验？是否充分、客观、公平？

- **实验数量**：元数据仅概括性描述，未给出具体实验组数。但提到评估了“一组VLA基础的真实机器人策略”、“不同策略版本、大小和训练检查点”，并进行了相关性分析。推测至少包含多个策略（如不同规模模型、不同训练阶段的检查点）的对比实验，以及可能涉及不同任务场景的泛化实验。
- **充分性**：实验覆盖了策略间的相对排名保持能力，这比单纯看绝对成功率更有说服力。但缺少与替代方法（如传统仿真器或纯物理引擎）的对比，也未提到消融实验（如不同VLM奖励模型的影响）。从元数据看，实验设计聚焦于“相关性”，方向合理，但完整性可进一步加强。

## 6. 论文的主要结论与发现

- **主要结论**：基于世界模型的策略评估环境（WorldGym）中得到的策略成功率与真实世界成功率高度相关，并且能够正确保持不同策略之间的相对排名（即世界模型中的表现排序与真实世界一致）。
- **其他发现**：
  - 现代VLA策略仍然难以区分物体形状，容易被物体的“对抗性外观”分散注意力。
  - 世界模型在生成高度真实的物体交互方面仍具挑战，但可以忠实地模拟机器人运动。
  - WorldGym仅需单个起始帧即可评估策略对新任务和新环境的泛化能力，降低了部署前的测试成本。

## 7. 优点：方法或实验设计上有哪些亮点

- **低成本与高效率**：只需一个初始帧，无需搭建完整仿真器或进行大量真实世界试验。
- **可扩展性与泛化性**：世界模型可以推广到未见过的场景，快速评估策略的泛化能力。
- **保持相对排名**：不仅预测绝对成功率，更能区分不同策略的优劣，这对策略开发过程中的调参和模型选型至关重要。
- **安全可复现**：评估过程在生成模型中完成，无物理风险，且结果可完全复现。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验覆盖局限**：
  - 未与现有仿真器（如Gazebo、MuJoCo）或真实人类评估进行大规模对比。
  - 未跨多个不同的世界模型架构验证结论的泛化性。
- **偏差风险**：
  - 世界模型可能产生与实际物理规律不一致的生成结果（尤其是物体交互部分），导致成功率估计出现偏差。
  - VLM奖励函数本身的质量可能影响评估准确性，但未做深入分析或消融。
- **应用限制**：
  - 当前世界模型在处理复杂物理交互（如物体形变、流体）时仍不够真实，限制了其在高精度任务上的适用性。
  - 依赖大规模视频生成模型，训练和推理成本较高（虽然评估时成本低，但世界模型训练本身昂贵）。
- **资源信息缺失**：未报告世界模型的训练算力，难以评估实际部署门槛。

（完）
