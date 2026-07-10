---
title: Object-Centric Latent Action Learning
title_zh: 以物体为中心的潜在动作学习
authors: "Albina Klepach, Alexander Nikulin, Ilya Zisman, Denis Tarasov, Alexander Derevyagin, Andrei Polubarov, Nikita Lyubaykin, Igor Kiselev, Vladislav Kurenkov"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39423/43384"
tags: ["query:latent-vla"]
score: 8.0
evidence: 面向VLA策略的以物体为中心的潜在动作学习
tldr: 针对无标签视频中动作相关视觉干扰物导致潜在动作推断退化的问题，提出以物体为中心的潜在动作学习框架。通过自监督物体级预训练解耦智能体运动和背景干扰，生成更鲁棒的代理动作标签，从而提升下游模仿学习性能。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39423/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 865, \"height\": 401, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39423/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1469, \"height\": 628, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39423/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 709, \"height\": 707, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39423/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1451, \"height\": 724, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39423/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1876, \"height\": 1056, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39423/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1839, \"height\": 819, \"label\": \"Table\"}]"
motivation: 现有潜在动作学习方法在存在视觉干扰物时性能显著下降，限制了对海量无标签互联网视频的利用。
method: 利用自监督物体中心预训练，将智能体运动与背景动态解耦，聚焦于任务相关交互来学习潜在动作标签。
result: 在存在干扰物的场景下生成更鲁棒的代理动作标签，改善模仿学习效果。
conclusion: 物体中心先验能有效提升潜在动作学习的鲁棒性。
---

## Abstract
Leveraging vast amounts of unlabeled internet video data for embodied AI is currently bottlenecked by the lack of action labels and the presence of action-correlated visual distractors. Although recent latent action policy optimization (LAPO) has shown promise in inferring proxy action labels from visual observations, its performance degrades significantly when distractors are present. To address this limitation, we propose a novel object-centric latent action learning framework that centers on objects rather than pixels. We leverage self-supervised object-centric pretraining to disentangle the movement of the agent and distracting background dynamics. This allows LAPO to focus on task-relevant interactions, resulting in more robust proxy-action labels, enabling better imitation learning and efficient adaptation of the agent with just a few action-labeled trajectories. We evaluated our method in eight visually complex tasks across the Distracting Control Suite (DCS) and Distracting MetaWorld (DMW). Our results show that object-centric pretraining mitigates the negative effects of distractors by 50%, as measured by downstream task performance: average return (DCS) and success rate (DMW).

---

## 论文详细总结（自动生成）

### 论文中文详细总结

#### 1. 论文的核心问题与整体含义（研究动机和背景）
- **问题**：利用海量无标签互联网视频数据训练具身智能体时，面临两个主要瓶颈：一是缺乏动作标签，二是存在与动作相关的视觉干扰物（如动态背景、相机抖动、颜色变化）。这些干扰物会与智能体动作产生虚假相关，导致潜在动作模型过拟合于非因果模式，性能严重下降。
- **背景**：现有潜在动作学习方法（如LAPO）虽然能通过视觉观测推断代理动作标签，但在存在干扰物时失效。先前工作（如Nikulin等人）表明，LAPO在干扰环境下性能退化，需要标签监督来缓解，但互联网视频中标签通常缺失。物体中心学习（OCL）能够将场景分解为离散、可解释的物体槽，有望分离因果交互与非因果视觉噪声。
- **整体含义**：本文提出以物体为中心的潜在动作学习框架，通过自监督物体级预训练解耦智能体运动与背景干扰，提升潜在动作标签的鲁棒性，从而在低监督条件下实现更好的模仿学习和策略微调。

#### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：利用物体中心表示作为结构化先验，将潜在动作学习从像素空间转移到物体槽空间，过滤掉动作无关的视觉干扰。具体分为三个阶段：
  1. **物体中心预训练**：使用VideoSAUR（基于DINOv2编码器的自监督视频分解模型）将视频帧分解为K个时空物体槽（slot vectors），并生成对应的注意力掩码（object masks）。通过固定槽初始化提高不同episode间槽索引的语义一致性。
  2. **槽选择**：采用线性动作探针（Linear Action Probe）自动识别控制相关的槽。对少量带标签轨迹，对每个槽的表示做PCA降维，训练线性回归器预测真实动作，以平均MSE作为相关性指标，选出预测误差最低的槽（或槽组合）作为任务相关槽集合。
  3. **潜在动作学习与微调**：
     - **LAPO-slots**：在选定的槽嵌入空间上训练逆动力学模型（IDM）和前向动力学模型（FDM），预测下一时刻槽嵌入。
     - **LAPO-masks**：应用选定的槽掩码对原图进行过滤，得到仅含相关物体的图像，然后在该图像上训练LAPO。
     - 使用推断出的潜在动作训练行为克隆（BC）策略，再用少量带真实动作标签的轨迹微调（不超过总数据的2.5%）。
- **关键技术细节**：
  - 逆动力学模型：\(z_t \sim f_{\text{IDM}}^s(\cdot | s_t, s_{t+1})\)
  - 前向动力学模型：\(\hat{s}_{t+q} \sim f_{\text{FDM}}^s(\cdot | s_t, z_t)\)，最小化均方误差。
  - 固定槽初始化与线性探针结合，确保槽索引跨episode一致，便于选择。

#### 3. 实验设计：数据集/场景、benchmark、对比方法
- **数据集/场景**：
  - **Distracting Control Suite (DCS)**：基于DeepMind Control Suite，添加动态背景（DAVIS 2017视频）、颜色变化、相机扰动。使用其中四个任务（cheetah-run, walker-run, hopper-hop, humanoid-walk），分为仅背景干扰（DCS）和全部干扰（DCS-hard）。
  - **Distracting MetaWorld (DMW)**：在MetaWorld基础上添加动态背景，包含四个任务（hammer, bin-picking, basketball, soccer），涉及多物体交互与组合推理。
  - **数据收集**：专家策略（DCS用state-based专家，DMW用MetaWorld oracle）生成轨迹，包含观测-动作对。
- **Benchmark**：行为克隆（BC）策略在完整真实动作标签下达到的归一化得分（归一化回报或成功率）作为上限（1.0）。
- **对比方法**：
  - **LAPO**（基线）：标准LAPO，在含干扰的观测上训练。
  - **LAPO-clean**（上界）：LAPO在无干扰的干净数据上训练。
  - **LAPO-slots**（本文）：在物体槽嵌入上训练LAPO。
  - **LAPO-masks**（本文）：在物体掩码过滤后的图像上训练LAPO。

#### 4. 资源与算力
- **论文中未明确说明**使用的GPU型号、数量、训练时长等算力细节。仅在致谢中提到研究由俄罗斯联邦经济发展部资助，未涉及具体硬件配置。

#### 5. 实验数量与充分性
- **实验数量**：
  - 主要结果（表1和图1）：在DCS（4任务）、DCS-hard（4任务）、DMW（4任务）上共12个环境×4种方法×3个随机种子，每个方法在多个微调轨迹数量（4,8,16,32,64,128或32,64,128,256,512）下评估，生成大量数据点。
  - 槽选择研究（图5）：在DMW basketball任务上进行，包括线性探针相关性、不同K值（2到15）的性能、探针与下游性能相关性。
  - 消融研究：比较LAPO-slots与LAPO-masks在不同干扰难度下的表现；分析K值鲁棒性。
- **充分性与客观性**：
  - 实验覆盖了从简单到复杂的多种任务，以及不同干扰类型（背景、颜色、相机），对比了有/无干扰的上界和下界。
  - 所有结果均报告均值及标准差，随机种子为3个，统计可靠性可接受。
  - 消融实验验证了槽选择的有效性（探针与下游性能相关）、K值鲁棒性、不同干扰难度的影响。
  - 总体设计合理，对比公平，结论支持充分。

#### 6. 论文的主要结论与发现
- 物体中心预训练**显著提升**潜在动作学习在视觉干扰下的鲁棒性，将LAPO与干净数据上界之间的性能差距缩小约50%（在DCS和DMW上平均）：
  - DCS：LAPO-slots平均恢复52%的差距，LAPO-masks恢复50%。
  - DCS-hard：LAPO-slots恢复52%，LAPO-masks恢复26%（因LAPO-masks受CNN编码器鲁棒性限制）。
  - DMW：LAPO-slots恢复50%，LAPO-masks恢复55%。
- 线性动作探针可作为自动选择控制相关槽的有效指标，其得分与下游BC性能强相关。
- 无论槽数量K如何，LAPO-slots始终优于基线LAPO，表明方法对K值具有鲁棒性。
- 物体中心表示有助于将智能体运动与背景动态解耦，尤其在高难度干扰（如所有三类干扰组合）下仍保持相对改进。

#### 7. 优点
- **创新性**：首次将物体中心学习与潜在动作学习系统结合，针对动作相关干扰问题提出结构化先验，不同于现有方法依赖标签监督或干净数据。
- **实用性强**：几乎不需要额外标注（仅极少带标签轨迹用于微调和探针），可扩展至真实互联网视频。
- **自动化操作**：通过线性探针自动选择相关槽，减少人工依赖。
- **鲁棒性**：在多种干扰类型和多个复杂任务上验证，对槽数量K不敏感。
- **可解释性**：物体槽掩码提供可视化，便于理解模型关注的区域。

#### 8. 不足与局限
- **依赖OCL模型能力**：方法效果受限于底层物体中心模型（VideoSAUR）的性能。当前OCL模型在严重遮挡、多视角、物体出入场景等情况下仍存在不足，固定槽数K限制了灵活性（尽管实验显示对K鲁棒，但极端K值可能导致坍塌或冗余）。
- **无监督分解的局限性**：在DMW中，VideoSAUR倾向于将动作耦合的多个物体（如机械臂和工具）合并为一个槽，而非按语义分离。这可能是因为训练数据多样性不足，导致模型缺乏驱动细粒度分解的动机。
- **未考虑真实世界挑战**：实验仅在仿真环境进行，未在真实机器人或复杂真实视频上验证。真实世界中的干扰更复杂（光照变化、杂波、部分遮挡），OCL模型可能无法完美分解。
- **算力成本未提及**：论文未报告训练VideoSAUR或LAPO所需的计算资源，实际应用中OCL模型（如VideoSAUR）本身可能产生较高计算开销。
- **实验范围有限**：仅覆盖连续控制任务（DCS、DMW），未涉及离散动作或更长视野任务；未与更多最新VLA方法（如LAPA、Genie）进行直接对比（仅对比LAPO及其变体）。

（完）
