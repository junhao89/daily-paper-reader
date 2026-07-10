---
title: "WorldRFT: Latent World Model Planning with Reinforcement Fine-Tuning for Autonomous Driving"
title_zh: WorldRFT：通过强化学习微调的潜在世界模型规划实现自动驾驶
authors: "Pengxuan Yang, Ben Lu, Zhongpu Xia, Chao Han, Yinfeng Gao, Teng Zhang, Kun Zhan, Xianpeng Lang, Yupeng Zheng, Qichao Zhang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38149/42111"
tags: ["query:wam-vla"]
score: 8.0
evidence: 潜在世界模型规划结合强化学习微调用于自动驾驶
tldr: 本文提出WorldRFT，面向规划的潜在世界模型框架。通过层次化规划分解和局部感知交互精炼机制，使场景表示学习与规划目标对齐，并使用强化学习微调提升安全关键性能。在自动驾驶任务中，它避免了重建导向表示学习对规划的干扰，显著提高了规划质量。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38149/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 869, \"height\": 536, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38149/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1835, \"height\": 545, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38149/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 875, \"height\": 468, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38149/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1439, \"height\": 469, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38149/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 703, \"height\": 415, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38149/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1836, \"height\": 806, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38149/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1832, \"height\": 572, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38149/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 902, \"height\": 420, \"label\": \"Table\"}]"
motivation: 重建导向的表示学习干扰规划优化。
method: 规划导向的潜在世界模型+层次规划分解+RL微调。
result: 在自动驾驶规划任务中超越先前的世界模型方法。
conclusion: 规划导向的表示学习是潜在世界模型的关键。
---

## Abstract
Latent World Models enhance scene representation through temporal self-supervised learning, presenting a perception annotation-free paradigm for end-to-end autonomous driving. However, the reconstruction-oriented representation learning tangles perception with planning tasks, leading to suboptimal optimization for planning. To address this challenge, we propose WorldRFT, a planning-oriented latent world model framework that aligns scene representation learning with planning via a hierarchical planning decomposition and local-aware interactive refinement mechanism, augmented by reinforcement learning fine-tuning (RFT) to enhance safety-critical policy performance. Specifically, WorldRFT integrates a vision-geometry foundation model to improve 3D spatial awareness, employs hierarchical planning task decomposition to guide representation optimization, and utilizes local-aware iterative refinement to derive a planning-oriented driving policy. Furthermore, we introduce Group Relative Policy Optimization (GRPO), which applies trajectory Gaussianization and collision-aware rewards to fine-tune the driving policy, yielding systematic improvements in safety. WorldRFT achieves state-of-the-art (SOTA) performance on both open-loop nuScenes and closed-loop NavSim benchmarks. On nuScenes, it reduces collision rates by 83% (0.30% → 0.05%). On NavSim, using camera-only sensors input, it attains competitive performance with the LiDAR-based SOTA method DiffusionDrive (87.8 vs. 88.1 PDMS).

---

## 论文详细总结（自动生成）

### 论文中文总结

#### 1. 核心问题与整体含义（研究动机与背景）

- **核心问题**：现有基于潜在世界模型（Latent World Model）的端到端自动驾驶方法，其表示学习以重建未来场景为目标（reconstruction-oriented），这与规划任务所需的决策导向表示之间存在根本性偏差。重建目标混杂了感知与规划，导致规划优化不充分，表现为三维空间感知弱、规划交互机制效率低、缺乏主动安全规避能力。
- **研究动机**：将表示学习与规划任务深度对齐，实现“规划导向”的潜在世界建模，并引入强化学习微调以提升安全关键策略性能。
- **整体含义**：提出一种无需感知标注（perception annotation-free）的新范式，通过几何先验融合、层次化规划分解与局部精炼、以及安全奖励驱动的强化微调，在保持高轨迹精度的同时大幅降低碰撞率。

#### 2. 方法论

- **核心思想**：构建“场景理解 → 规划决策 → 安全优化”的三阶段流水线：
  1. **Spatial-aware World Encoder (SWE)**：利用冻结的视觉几何基础模型VGGT提取3D空间token，通过轻量交叉注意力融合到2D图像特征中，得到空间增强的潜在世界表示 \( W_t^{latent} \)。
  2. **Hierarchical Planning Refinement (HPR)**：将端到端规划分解为三个并行子任务：
     - **目标区域定位**：用拉普拉斯分布建模目标区域 \((\mu, b)\)，通过负对数似然损失 \(L_{Laplace-NLL}\) 训练，\(b\) 反映场景不确定性。
     - **空间路径规划**：生成固定空间间隔（2米）的路径点 \(T_{path}\)。
     - **时间轨迹预测**：生成固定时间间隔（0.5秒）的轨迹点 \(T_{traj}\)。
     - **局部感知迭代精炼（K次迭代）**：将当前规划状态 \(F_s\)、轨迹点投影到潜在特征图、用可变形卷积采样局部特征 \(F_{local}\)，与全局意图及不确定性 \(b\) 融合，预测残差 \(\Delta T\) 并逐步更新轨迹。
  3. **Reinforcement Learning Fine-tuning (RFT)**：
     - 将轨迹建模为高斯分布（均值 \(\mu_\theta\)，方差 \(\Sigma_\theta\)）。
     - 设计碰撞感知奖励（碰撞-1，无碰撞0）。
     - 采用GRPO策略优化，利用组内相对优势函数，并加入KL散度正则化（参考策略为预训练模型）。
- **关键技术细节**：
  - 世界编码器通过未来潜在世界预测损失 \(L_{rec}\) 进行自监督预训练。
  - 总预训练损失：\(L_{Total} = \alpha L_{sem} + \beta L_{rec} + \gamma L_{target} + \eta L_{traj}\)。
  - RFT损失：\(L_{RL} = -J(\theta) + \lambda D_{KL}\)。

#### 3. 实验设计

- **数据集与基准**：
  - **nuScenes**（开环）：评估L2位移误差（1s/2s/3s）和碰撞率。
  - **NavSim**（闭环）：评估PDMS综合分数（包含无碰撞NC、可行驶区域合规DAC、碰撞时间TTC、乘坐舒适度Comf.、自车进展EP）。
- **对比方法**：
  - 感知基方法（P-IL）：ST-P3, OccNet, UniAD, VAD, LAW, DiffusionDrive, PARA-Drive等。
  - 自监督方法（SS-L）：LAW (perception-free), World4Drive, Epona等。
  - 含自车状态的方法：SSR, Ours (with RFT)†。
- **实验充分性**：
  - 主结果：Table 1 (nuScenes) 和 Table 2 (NavSim) 展示了与多种SOTA方法的对比。
  - 消融实验：Table 3 逐步验证VGGT编码、层次规划任务、局部精炼、RFT各组件贡献，共8组配置。
  - 定性分析：图4展示RFT前后轨迹对比，图5展示注意力热图。

#### 4. 资源与算力

- **未明确说明**。论文正文及附录中未提及GPU型号、数量、训练时长、显存消耗等算力信息。仅在致谢部分提及国家自然科学基金等资助，未提供具体计算资源细节。

#### 5. 实验数量与充分性

- **实验数量**：
  - 两个主流基准（nuScenes、NavSim）上的完整评估。
  - 消融实验共8组（Table 3），涵盖每个关键模块。
  - 包含与14+种基线方法的横向对比（Table 1, Table 2）。
  - 提供了带自车状态输入的额外结果（†标记）。
- **充分性评价**：
  - **优点**：实验设计完整，覆盖开环与闭环，消融实验逐组件分析，对比方法覆盖近期SOTA，结果具有说服力。
  - **公平性**：所有对比均在同一基准、相同评估协议下进行，且自监督方法与感知基方法分开比较，避免混淆。但部分基线（如LAW）同时报告了感知基和感知无版本，区分清晰。
  - **不足**：缺少对预训练阶段超参数选择、VGGT冻结与否的消融；未分析不同迭代次数K的影响；NavSim上的消融实验未在正文给出（仅提及在supplementary中）。

#### 6. 主要结论与发现

- WorldRFT在nuScenes上达到SOTA：平均L2误差0.48m（相比LAW降低21%），碰撞率0.05%（相比LAW降低83%），并且是所有方法中碰撞率最低的（含感知基方法）。
- 在NavSim上，仅用相机输入的WorldRFT达到PDMS 87.8，与LiDAR基SOTA DiffusionDrive（88.1）仅差0.3分，显著超越同类自监督方法。
- 层次化规划分解和局部精炼有效提取规划相关特征，VGGT几何先验显著提升3D空间理解，RFT在几乎不损失轨迹精度的情况下大幅降低碰撞风险。
- 定性结果证明WorldRFT的注意力更聚焦于关键交通参与者，RFT促使车辆主动保持安全距离。

#### 7. 优点

- **方法论创新**：首次提出规划导向的潜在世界模型，通过层次分解与局部精炼解决重建-规划错配问题，设计清晰。
- **性能卓越**：碰撞率降低幅度显著（83%），在闭环上达到与LiDAR方法相当的水平，体现了实际部署潜力。
- **无需感知标注**：完全自监督预训练，降低数据成本与系统复杂度。
- **安全强化微调**：从被动模仿学习转向主动碰撞规避，且通过高斯化轨迹实现概率探索，设计合理。
- **实验说服力强**：在开环与闭环两个标准基准上全面评估，消融实验验证了每个组件的必要性。

#### 8. 不足与局限

- **算力开销未报告**：未披露训练所需计算资源，难以评估复现成本和可扩展性。
- **依赖性风险**：依赖冻结的VGGT模型，VGGT本身的域漂移或性能上限可能限制下游表现。
- **泛化能力未知**：仅在nuScenes和NavSim上测试，后者为仿真环境，未在真实世界长尾场景中验证。
- **安全优化范围有限**：仅考虑了碰撞奖励，未纳入交通规则、行人意图等其他安全因素。
- **消融实验细节缺失**：NavSim上的消融结果仅提到在supplementary中，未在正文展示；迭代次数K、超参数\(\alpha, \beta, \gamma, \eta\)等未进行敏感性分析。
- **与部分强基线比较不够公平**：如DiffusionDrive使用了LiDAR+相机，而WorldRFT仅用相机，虽已达标但对比时仍需注意模态差异。

（完）
