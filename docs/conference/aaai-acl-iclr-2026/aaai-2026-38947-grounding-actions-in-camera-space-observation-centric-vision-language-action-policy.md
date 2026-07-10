---
title: "Grounding Actions in Camera Space: Observation-Centric Vision-Language-Action Policy"
title_zh: 在相机空间锚定动作：以观察为中心的视觉-语言-动作策略
authors: "Tianyi Zhang, Haonan Duan, Haoran Hao, Yu Qiao, Jifeng Dai, Zhi Hou"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38947/42909"
tags: ["query:latent-vla"]
score: 8.0
evidence: 以观察为中心的VLA策略，将动作锚定在相机空间
tldr: VLA模型常在机器人基坐标系预测动作，导致不同相机视角下的空间不一致。OC-VLA将末端执行器位姿转换到相机坐标系，使动作预测与观察对齐。实验表明，该方法显著提升了VLA模型在多种操控场景中的跨视角泛化能力。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38947/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 858, \"height\": 506, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38947/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1808, \"height\": 571, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38947/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 810, \"height\": 1187, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38947/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 855, \"height\": 643, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38947/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 855, \"height\": 588, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38947/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 854, \"height\": 1062, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38947/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1441, \"height\": 244, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38947/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1778, \"height\": 855, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38947/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1829, \"height\": 265, \"label\": \"Table\"}]"
motivation: VLA模型因动作空间与观察空间不一致导致泛化困难。
method: 提出OC-VLA，利用相机外参将动作预测统一到相机坐标系。
result: 在多个操控任务上，跨视角泛化性能显著提升。
conclusion: 观察空间对齐是提升VLA模型泛化性的关键。
---

## Abstract
Vision-Language-Action (VLA) models frequently encounter challenges in generalizing to real-world environments due to inherent discrepancies between observation and action spaces. Although training data are collected from diverse camera perspectives, the models typically predict end-effector poses within the robot base coordinate frame, resulting in spatial inconsistencies. To mitigate this limitation, we introduce the Observation-Centric VLA (OC-VLA) framework, which grounds action predictions directly in the camera observation space. Leveraging the camera’s extrinsic calibration matrix, OC-VLA transforms end-effector poses from the robot base coordinate system into the camera coordinate system, thereby unifying prediction targets across heterogeneous viewpoints. This lightweight, plug-and-play strategy ensures robust alignment between perception and action, substantially improving model resilience to camera viewpoint variations. The proposed approach is readily compatible with existing VLA architectures, requiring no substantial modifications. Comprehensive evaluations on both simulated and real-world robotic manipulation tasks demonstrate that OC-VLA accelerates convergence, enhances task success rates, and improves cross-view generalization.

---

## 论文详细总结（自动生成）

### 论文详细中文总结

#### 1. 核心问题与整体含义（研究动机和背景）
- **问题核心**：现有的Vision-Language-Action (VLA) 模型在预测机器人动作时，通常将末端执行器位姿定义在**机器人基坐标系**（robot base）中；而视觉观测则来自不同视角的**相机坐标系**（camera space）。这种空间不一致导致模型在训练时面对不同相机视角下相同的动作需要学习隐含的坐标系变换，限制了跨视角泛化能力。
- **背景**：VLA模型（如RT-2、OpenVLA、π0等）在大规模机器人数据上预训练，但数据采集视角多样（如Droid数据集包含1417个视角），而现有方法仍以机器人基坐标系作为动作监督信号，迫使模型从2D图像中推理3D位姿，形成“ill-posed”问题。
- **研究目标**：消除观察空间与动作空间之间的错位，提升模型对相机视角变化的鲁棒性，实现更好的跨视角泛化。

#### 2. 方法论：核心思想、关键技术细节
- **核心思想**：将动作预测目标从机器人基坐标系转换到**相机坐标系**，使动作空间与观察空间对齐。即**Observation-Centric VLA (OC-VLA)**。
- **关键技术细节**：
  - 利用相机的外参矩阵 \( T \in \mathbb{R}^{4 \times 4} \)（world-to-camera变换），将机器人基坐标系下的末端执行器位姿 \( P_{\text{world}} \) 转换为相机坐标系下的位姿 \( P_{\text{cam}} = T P_{\text{world}} \)。
  - 动作定义为相邻两步位姿的相对变换：\( A_{\text{world}} = P_{\text{world},2} P_{\text{world},1}^{-1} \)，转换为相机空间动作：\( A_{\text{cam}} = T A_{\text{world}} T^{-1} \)。
  - 最终预测的动作为7维向量：\( \langle x, y, z, \text{roll}, \text{pitch}, \text{yaw}, \text{gripper} \rangle \)，均定义在相机坐标系下。
  - 推理时，再通过相机外参将预测的相机坐标动作逆变换回机器人基坐标以执行。
- **架构兼容性**：该方法是一种轻量级即插即用策略，不改变原有VLA模型架构（如Dita，使用DINOv2+Q-Former+Transformer），只需修改动作标签和推理时的坐标转换。

#### 3. 实验设计
- **模拟环境**：使用**ManiSkill2**，选取5个任务（PickCube、StackCube、PickSingleYCB、PickClutterYCB、PickSingleEGAD）。构建包含30万随机相机视角的数据池，每个轨迹采样20个随机视角，共4万+轨迹，19:1划分训练/验证。评估时每任务100条轨迹，共500条。
- **真实机器人**：Franka Emika Panda + Robotiq 2F-85，配备3个RealSense D435i相机（2个用于训练+微调，1个用于零样本测试）。
  - **数据收集**：从Camera1收集15个任务（固定视角），从Camera2收集8个任务（轻微视角扰动），每任务10条示范（10-shot微调）。
  - **评估设置**：
    - A. 固定视角：训练和测试使用相同固定相机位置。
    - B. 轻微视角扰动：使用Camera2的数据，测试时相机位置略有变化。
    - C. 零样本新视角：用Camera1训练，但用从未见过的第三个相机评估。
- **对比方法**：基线包括OpenVLA-OFT、π0，以及将动作保持在机器人基坐标系的同等模型（Dita架构）。所有方法在相同的数据上进行微调或从Droid预训练初始化。
- **指标**：任务成功率（Success Rate）。

#### 4. 资源与算力
- **文中明确说明**：模型训练在**8块NVIDIA A100 GPU**上进行，batch size为2048（每GPU 256样本）。
- **预训练/从头训练**：在模拟实验（ManiSkill2）中，模型从零训练30,000步；在真实机器人实验中，基于Droid预训练模型微调20,000步。
- 优化器：AdamW，学习率：Transformer和Q-Former 1e-4，DINOv2 1e-5。

#### 5. 实验数量与充分性
- **实验数量**：
  - 模拟实验：2种动作空间（离散/连续）×2种坐标（机器人基/相机基）=4组，在5个任务上报告成功率（表1）。
  - 真实机器人：对比了OpenVLA-OFT、π0、机器人基模型、相机基模型（OC-VLA），在15个任务上各10次测试，同时做零样本测试（表2）；另加入相机扰动实验（8个任务，表3）。
- **充分性评价**：
  - 覆盖了模拟和真实场景，考虑了固定视角、扰动视角、零样本迁移等多种设置，实验设计较为全面。
  - 对比方法包括当前主流VLA模型（OpenVLA、π0），并控制了架构因素（使用同一Dita基础模型仅改变动作坐标），公平性较好。
  - 但缺少消融实验（如仅使用部分数据、不同预训练策略等），且仅在一个模拟benchmark（ManiSkill2）上测试，泛化性仅针对视角变化，未测试任务类别变化。

#### 6. 主要结论与发现
- **核心发现**：将动作预测空间从机器人基坐标系迁移到相机坐标系，能显著提升VLA模型的成功率和跨视角泛化能力。
- **模拟结果**：离散动作空间下，相机基坐标相比机器人基坐标成功率提升约14%（如PickCube从71%→88%）；连续动作空间下也有稳定提升。
- **真实结果**：
  - 固定视角：OC-VLA平均成功率68%，优于OpenVLA-OFT（63.3%）和机器人基（58%）。
  - 零样本新视角：OC-VLA下降最小（14%），其他基线下降超20%，OC-VLA仍保持54%，大幅领先。
  - 相机扰动：OC-VLA取得73.8%，高于机器人基的61.3%，表明对视角变化更鲁棒。
- **定性表现**：OC-VLA在抓取定位（grasp pose）上更精确，尤其在新视角下能避免失败。

#### 7. 优点
- **方法简洁有效**：仅需使用相机外参进行坐标变换，不引入额外网络参数或计算开销，即插即用。
- **显著提升跨视角泛化**：尤其在新视角和扰动场景下性能领先，解决了VLA模型的一个关键短板。
- **兼容现有架构**：适用于离散和连续动作空间的各种VLA模型（如Diffusion、Token分类）。
- **理论分析合理**：从优化角度解释了为何相机坐标相比机器人坐标更容易学习（图像坐标与相机坐标的直接关联，且外参多样导致学习困难）。

#### 8. 不足与局限
- **依赖相机外参**：方法要求已知相机与机器人基座的标定矩阵，在无标定或动态相机场景下无法直接使用。
- **仅支持第三视角相机**：当前框架只适用于第三视角，未探索第一视角或腕部相机。
- **实验覆盖有限**：仅在一个模拟benchmark（ManiSkill2）上验证，未在更多多样化的模拟环境（如RLBench、MetaWorld）上测试；真实场景任务数15个，样本量较小（10次/任务），统计意义可能有限。
- **模型规模单一**：所有实验基于Dita（334M参数），未在更大模型（如7B级别）上验证。
- **未讨论无标定适配**：对于没有相机外参的数据集，方法无法直接应用，需额外标定或估计。
- **缺乏对失败案例的深入分析**：未详细分析在哪些任务或视角下仍失败，以及失败原因。

（完）
