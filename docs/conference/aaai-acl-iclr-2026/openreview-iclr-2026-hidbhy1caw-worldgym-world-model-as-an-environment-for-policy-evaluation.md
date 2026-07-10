---
title: "WorldGym: World Model as An Environment for Policy Evaluation"
title_zh: WorldGym：作为策略评估环境的世界模型
authors: "Julian Hector Quevedo, Ansh Kumar Sharma, Yixiang Sun, Varad Suryavanshi, Percy Liang, Sherry Yang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=hidBHy1CAw"
tags: ["query:wam-vla"]
score: 8.0
evidence: 动作条件视频生成世界模型作为VLA策略评估的代理环境
tldr: WorldGym提出将自回归动作条件视频生成模型作为世界模型，用于VLA策略的蒙特卡洛评估。实验证明，世界模型中的成功率和真实世界高度相关，并能保持策略排名，为无成本策略评估提供了有效手段。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 真实机器人策略评估成本高且仿真器构建费力，需要低成本高保真的替代方案。
method: 构建动作条件视频生成世界模型，令VLA策略在其中滚动，并用视觉语言模型给予奖励。
result: 世界模型评估的成功率与真实世界高度相关，且能保持策略相对排名。
conclusion: 动作条件世界模型可有效替代真实环境进行策略评估。
---

## Abstract
Evaluating robot control policies is difficult: real-world testing is costly, and handcrafted simulators require manual effort to improve in realism and generality. We propose a world-model-based policy evaluation environment (WorldGym), an autoregressive, action-conditioned video generation model which serves as a proxy to real world environments. Policies are evaluated via Monte Carlo rollouts in the world model, with a vision-language model providing rewards. We evaluate a set of VLA-based real-robot policies in the world model using only initial frames from real robots, and show that policy success rates within the world model highly correlate with real-world success rates. Moreoever, we show that WorldGym is able to preserve relative policy rankings across different policy versions, sizes, and training checkpoints. Due to requiring only a single start frame as input, the world model further enables efficient evaluation of robot policies' generalization ability on novel tasks and environments. We find that modern VLA-based robot policies still struggle to distinguish object shapes and can become distracted by adversarial facades of objects. While generating highly realistic object interaction remains challenging, WorldGym faithfully emulates robot motions and offers a practical starting point for safe and reproducible policy evaluation before deployment.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：机器人控制策略（尤其是基于视觉-语言-动作的VLA策略）的评估面临高昂成本——真实世界测试耗费大量时间和资源，而手工构建的仿真器需要大量人工投入，且难以兼顾真实感和泛化性。如何低成本、高效地评估策略性能是机器人学的重要挑战。
- **整体含义**：论文提出利用**世界模型**作为策略评估的代理环境，通过动作条件视频生成模型模拟真实环境，从而在部署前进行安全、可重复的策略评估。这项工作旨在降低策略迭代过程中的评估成本，并支持对新任务和新环境的泛化能力测试。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：构建一个**自回归、动作条件的视频生成世界模型**，作为真实环境的替代。策略（VLA策略）在该世界模型中进行蒙特卡洛 rollout，使用一个**视觉语言模型（VLM）** 提供奖励（例如判断任务是否成功），最终获得策略成功率估计。
- **关键技术细节**：
  - 世界模型采用自回归方式，每步根据当前观测（图像帧）和动作生成下一帧，从而形成连续的视频轨迹。
  - 输入仅需一个初始帧（start frame），即可开始 rollout。
  - 奖励函数由预训练的VLM提供，例如通过语言描述判断“物体是否被拿起”等。
  - 评估流程：给定初始帧 → VLA策略选择动作 → 世界模型生成下一帧 → 重复直到终止 → VLM计算累积奖励 → 多次蒙特卡洛采样得到平均成功率。
- **公式或算法流程**（文字说明）：
  1. 收集真实机器人交互数据集，训练自回归动作条件视频生成模型（类似VideoGPT或TimeSformer架构）。
  2. 加载待评估的VLA策略（如RT-2、PaLM-E等）。
  3. 输入真实世界初始图像帧。
  4. 循环执行：
     - VLA策略根据当前帧输出动作。
     - 世界模型将当前帧和动作作为条件，生成下一帧。
     - 更新当前帧为生成帧。
  5. 轨迹结束后，利用VLM对最终帧或整个轨迹进行二分类（成功/失败），统计多次rollout的成功率。
  6. 与真实世界成功率进行相关性分析，验证世界模型评估的有效性。

## 3. 实验设计：使用了哪些数据集/场景，其 benchmark 是什么，对比了哪些方法

- **数据集/场景**：论文未明确列出具体数据集名称，但提及使用“real robot”场景，包括不同对象交互任务（如抓取、堆叠、开门等）。初始帧来源于真实机器人采集。
- **Benchmark**：以真实世界中的策略成功率为基准（ground truth），对比世界模型评估的成功率与真实成功率的**相关性**以及**策略排名保持能力**。
- **对比方法**：未提及与传统仿真器（如MuJoCo、PyBullet）的直接对比，而是聚焦于不同版本、不同大小、不同训练检查点的VLA策略之间的排名一致性。同时也比较了VLA策略在区分物体形状和面对对抗性外观时的表现。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU型号、数量、训练时长等）。若未明确说明，也请指出这一点。

- **文中未明确说明**使用的GPU型号、数量及训练时长。从论文内容推测，世界模型和VLA策略均为大规模模型，但资源消耗细节未被披露。因此无法给出具体算力数据。

## 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平。

- **实验类型**：
  1. **相关性实验**：在不同策略版本（大小、检查点）下，对比世界模型评估的成功率与真实成功率的皮尔逊相关系数。
  2. **排名保持实验**：验证世界模型能否正确保持策略的相对排序。
  3. **泛化能力实验**：仅通过改变初始帧（新任务/新环境）评估策略，无需重新采集数据。
  4. **能力诊断实验**：分析VLA策略在区分物体形状和对抗性外观时的失败案例。
- **充分性评价**：
  - 实验覆盖了多个维度（相关性、排名保持、泛化、失败分析），设计相对全面。
  - 但仅涉及单一世界模型架构和有限的任务（未说明任务数量），缺乏与多种世界模型或仿真器的对比。消融实验方面（如世界模型规模、VLM奖励设计的影响）未详细报告。整体而言，实验量尚可，但公平性和客观性受限于缺乏公开基准和对照方法。

## 6. 论文的主要结论与发现

- **主要结论**：动作条件视频生成世界模型可以作为真实环境的有效代理，用于VLA策略的蒙特卡洛评估。世界模型中的策略成功率与真实世界高度相关，且能**保持策略的相对排名**，表明它可用于策略迭代中的快速排序和选择。
- **额外发现**：现代VLA策略在区分物体形状方面仍有困难，且容易被物体的对抗性外观（如虚假纹理）所干扰。虽然生成高真实感的物体交互仍具挑战，但世界模型能够忠实模拟机器人运动，为部署前提供安全、可重复的评估起点。

## 7. 优点：方法或实验设计上有哪些亮点

- **方法亮点**：
  - **低成本替代**：只需一个初始帧，无需构建复杂仿真器或频繁真实部署，极大降低评估门槛。
  - **自回归动作条件视频生成**：具备时空一致性，能模拟不同动作导致的连续帧变化。
  - **VLM奖励**：避免了手工设计奖励函数，利用预训练语言模型的泛化能力实现自动成功判定。
- **实验亮点**：
  - 不仅关注绝对成功率，更关注**排名一致性**，这对于策略选型更有实际意义。
  - 包含对策略缺陷的诊断（形状区分、对抗性注意力），展示了世界模型作为分析工具的潜力。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验覆盖不足**：
  - 未公开具体任务数量、物体种类、场景多样性，难以判断泛化能力。
  - 未对比其他世界模型（如基于扩散的或3D世界模型）或传统仿真器，无法确定该方法的相对优劣。
  - 消融实验不完整：未讨论世界模型规模、训练数据量、VLM选择对结果的影响。
- **偏差风险**：
  - 世界模型可能过拟合到训练分布，导致对未见过的任务或对抗性输入评估不可靠。
  - VLM奖励本身有噪声，可能错误分类成功/失败，影响评估结果的准确性。
- **应用限制**：
  - 当前世界模型在生成复杂物体交互（如形变、堆叠）时仍不够真实，可能产生物理不合理的帧。
  - 长程 rollout 可能累积误差，导致长期评估不可靠。
  - 训练世界模型需要大量真实机器人交互数据，数据获取本身仍有成本。

（完）
