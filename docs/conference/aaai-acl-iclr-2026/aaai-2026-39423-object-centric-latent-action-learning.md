---
title: Object-Centric Latent Action Learning
title_zh: 以对象为中心的潜在动作学习
authors: "Albina Klepach, Alexander Nikulin, Ilya Zisman, Denis Tarasov, Alexander Derevyagin, Andrei Polubarov, Nikita Lyubaykin, Igor Kiselev, Vladislav Kurenkov"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39423/43384"
tags: ["query:latent-vla"]
score: 9.0
evidence: 以对象为中心的潜在动作学习框架获取鲁棒的代理动作标签
tldr: 从无标签互联网视频学习动作时，视觉干扰物严重降低潜在动作学习质量，本文提出以对象为中心的潜在动作学习框架，利用自监督对象分离预训练将智能体运动与背景动态解耦，从而获得更鲁棒的代理动作标签，有效提升下游模仿学习性能。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39423/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 865, \"height\": 401, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39423/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1469, \"height\": 628, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39423/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 709, \"height\": 707, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39423/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1451, \"height\": 724, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39423/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1876, \"height\": 1056, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39423/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1839, \"height\": 819, \"label\": \"Table\"}]"
motivation: 现有潜在动作学习方法受视觉干扰物影响导致动作标签质量下降。
method: 通过自监督对象分离预训练，以对象为中心解耦智能体运动与背景动态。
result: 在包含干扰物的视频数据上获得更鲁棒的代理动作标签，提升模仿学习效果。
conclusion: 对象为中心的建模能显著提升潜在动作学习对视觉干扰的鲁棒性。
---

## Abstract
Leveraging vast amounts of unlabeled internet video data for embodied AI is currently bottlenecked by the lack of action labels and the presence of action-correlated visual distractors. Although recent latent action policy optimization (LAPO) has shown promise in inferring proxy action labels from visual observations, its performance degrades significantly when distractors are present. To address this limitation, we propose a novel object-centric latent action learning framework that centers on objects rather than pixels. We leverage self-supervised object-centric pretraining to disentangle the movement of the agent and distracting background dynamics. This allows LAPO to focus on task-relevant interactions, resulting in more robust proxy-action labels, enabling better imitation learning and efficient adaptation of the agent with just a few action-labeled trajectories. We evaluated our method in eight visually complex tasks across the Distracting Control Suite (DCS) and Distracting MetaWorld (DMW). Our results show that object-centric pretraining mitigates the negative effects of distractors by 50%, as measured by downstream task performance: average return (DCS) and success rate (DMW).

---

## 论文详细总结（自动生成）

### 论文详细中文总结

#### 1. 核心问题与整体含义（研究动机和背景）
- **问题**：互联网上有大量无动作标注的视频数据，可用于训练具身智能体，但缺少动作标签且存在与动作相关的视觉干扰物（如动态背景、相机抖动、颜色变化等），导致现有潜在动作学习方法（如LAPO）性能严重下降。
- **背景**：Latent Action Policy Optimization (LAPO) 通过从视频中推断潜在动作来学习策略，但在存在干扰物时，模型会过拟合非因果的视觉模式，无法有效提取动作信息。现有方法或依赖精心筛选的无干扰数据集，或需要昂贵的人工标注，难以扩展到真实场景。
- **本文意义**：提出**以对象为中心（Object-Centric）的潜在动作学习框架**，通过自监督对象分离预训练，将智能体运动与背景动态解耦，从而获得更鲁棒的动作代理标签，提升下游模仿学习性能。

#### 2. 方法论
- **核心思想**：利用对象中心表征（object-centric representations）将场景分解为离散、可解释的对象槽（slots），使潜在动作模型聚焦于任务相关的对象交互，过滤掉干扰信息。
- **关键技术细节**：
  - **对象中心表征学习**：采用 VideoSAUR 模型对视频帧进行时空对象槽分解，每个槽对应一个实体，并可通过注意力掩码（mask）可视化。采用固定槽初始化以保证不同片段中相同槽索引对应一致语义。
  - **槽选择（Slot Selection）**：通过**线性动作探针（Linear Action Probe）**自动选出与动作最相关的槽。具体做法：使用少量带标签轨迹，对每个槽的表征进行PCA降维，训练线性回归器预测真实动作，根据MSE选出预测误差最小的槽集。
  - **潜在动作建模**：
    - **LAPO-slots**：直接在槽嵌入空间上训练逆动力学模型（IDM）和前向动力学模型（FDM），最小化预测槽与真实槽的MSE。
    - **LAPO-masks**：先利用选定槽的注意力掩码生成过滤后的图像（只保留与动作相关对象的区域），然后在像素空间进行潜在动作学习。
  - **行为克隆与微调**：用学习到的潜在动作训练BC策略，再使用少量（≤2.5%）带真实标签的轨迹进行微调。

#### 3. 实验设计
- **数据集/场景**：
  - **Distracting Control Suite (DCS)**：基于DeepMind Control，包含动态背景、颜色变化、相机抖动三类干扰。实验中区分只含背景干扰（DCS）和全干扰（DCS-hard）。任务：cheetah-run, walker-run, hopper-hop, humanoid-walk。
  - **Distracting MetaWorld (DMW)**：在MetaWorld基础上加入DCS风格的动态背景。任务：hammer, bin-picking, basketball, soccer。
- **基准（Benchmark）**：以**归一化回报（DCS）**和**归一化成功率（DMW）**为指标，1.0对应使用全部真实动作标签的BC策略。
- **对比方法**：
  - **LAPO（基线）**：直接在有干扰的观测上训练。
  - **LAPO-clean（上界）**：在无干扰的干净数据上训练。
  - **LAPO-slots（本文）**：使用槽嵌入。
  - **LAPO-masks（本文）**：使用槽掩码过滤后的图像。
- **评估方式**：改变微调用的标注轨迹数量（0.1%~2.5%），对比各方法的性能。

#### 4. 资源与算力
- 论文未明确说明使用的GPU型号、数量或训练时长，仅提到代码已开源（GitHub）。因此无法具体量化算力消耗。

#### 5. 实验数量与充分性
- 共涉及**8个视觉复杂任务**（4个DCS + 4个DMW），每个任务报告了不同标注量下的性能曲线。
- 进行了**槽选择研究**：分析了线性探针与下游性能的相关性、槽数K的影响（从2到15）。
- 对**干扰难度递增**进行了消融：只含背景 vs. 全干扰（DCS-hard vs. DCS）。
- 每个实验使用3个随机种子取平均值，结果附带标准差。
- **评估公平性**：所有方法使用相同数据集，上界为无干扰的干净数据训练，基线为同数据集下有干扰的LAPO，消融完整。
- **结论**：实验设计较为充分，覆盖不同干扰类型、任务复杂度、标注预算，统计稳健性好。

#### 6. 主要结论与发现
- 对象中心预训练**将干扰造成的性能差距缩小约50%**（DCS和DMW平均），在复杂任务（如humanoid-walk）中改善更显著（+174%）。
- **LAPO-slots 比 LAPO-masks 更鲁棒**，尤其在多种干扰叠加时（DCS-hard），因为VideoSAUR的DINOv2编码器对视觉外观变化更稳健。
- **线性探针可以自动、可靠地选择任务相关槽**，探针得分与下游性能强相关。
- **对槽数K不敏感**：从K=2到15，LAPO-slots始终保持优于基线，说明方法鲁棒。
- 对象中心模型倾向于将功能上耦合的实体（如机械臂与工具）合并为一个槽，而非严格按几何分离，这在大数据偏差下是合理行为。

#### 7. 优点
- **创新性**：首次将对象中心表示与潜在动作学习结合，有效解决干扰问题，且无需额外标注。
- **实用性**：仅需极少标注（≤2.5%）即可大幅提升性能，符合真实应用场景。
- **鲁棒性**：对干扰类型、标注量、槽数均表现稳定。
- **可解释性**：槽掩码和线性探针提供了可视化与自动选择机制。
- **开源**：代码已公开，便于复现和扩展。

#### 8. 不足与局限
- **依赖对象中心模型**：性能受限于VideoSAUR自身能力（如对遮挡、多视角处理弱，假设固定槽数）。
- **槽数固定**：无法处理对象数量动态变化（如新物体出现/消失），当前有相关工作但未采用。
- **无监督分解的不确定性**：在数据多样性有限时，对象中心模型可能产生“坍塌”分解（如将多个对象合并），影响解耦效果。
- **算力未报告**：缺少训练成本信息，不利于资源评估。
- **应用范围**：仅在仿真环境验证，真实世界视频（如YouTube）的复杂度和多样性远高于本设置，需进一步验证。
- **与现有方法正交性**：未与Nikulin等人(2025)提出的“借助少量标签提供监督”的方法直接对比，互补性有待探讨。

（完）
