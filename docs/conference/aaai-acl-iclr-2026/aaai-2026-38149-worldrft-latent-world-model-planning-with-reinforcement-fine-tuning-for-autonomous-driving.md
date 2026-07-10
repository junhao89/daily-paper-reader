---
title: "WorldRFT: Latent World Model Planning with Reinforcement Fine-Tuning for Autonomous Driving"
title_zh: WorldRFT：用于自动驾驶的潜在世界模型规划与强化学习微调
authors: "Pengxuan Yang, Ben Lu, Zhongpu Xia, Chao Han, Yinfeng Gao, Teng Zhang, Kun Zhan, Xianpeng Lang, Yupeng Zheng, Qichao Zhang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38149/42111"
tags: ["query:latent-vla"]
score: 7.0
evidence: 用于自动驾驶的潜在世界模型规划，包含分层分解和强化学习微调
tldr: WorldRFT提出面向规划的潜在世界模型框架，通过层次化规划分解和局部感知交互细化机制对齐场景表示与规划，并利用强化学习微调提升安全关键策略性能，在自动驾驶任务上取得显著效果。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38149/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 869, \"height\": 536, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38149/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1835, \"height\": 545, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38149/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 875, \"height\": 468, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38149/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1439, \"height\": 469, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38149/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 703, \"height\": 415, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38149/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1836, \"height\": 806, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38149/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1832, \"height\": 572, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38149/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 902, \"height\": 420, \"label\": \"Table\"}]"
motivation: 重建导向的潜在世界模型将感知与规划纠缠在一起，导致规划优化次优。
method: 设计层次化规划分解和局部感知交互细化，将潜在世界模型与强化学习微调结合。
result: 在自动驾驶基准上，规划性能和安全指标均得到提升。
conclusion: 面向规划的潜在世界模型能有效提升安全关键策略表现。
---

## Abstract
Latent World Models enhance scene representation through temporal self-supervised learning, presenting a perception annotation-free paradigm for end-to-end autonomous driving. However, the reconstruction-oriented representation learning tangles perception with planning tasks, leading to suboptimal optimization for planning. To address this challenge, we propose WorldRFT, a planning-oriented latent world model framework that aligns scene representation learning with planning via a hierarchical planning decomposition and local-aware interactive refinement mechanism, augmented by reinforcement learning fine-tuning (RFT) to enhance safety-critical policy performance. Specifically, WorldRFT integrates a vision-geometry foundation model to improve 3D spatial awareness, employs hierarchical planning task decomposition to guide representation optimization, and utilizes local-aware iterative refinement to derive a planning-oriented driving policy. Furthermore, we introduce Group Relative Policy Optimization (GRPO), which applies trajectory Gaussianization and collision-aware rewards to fine-tune the driving policy, yielding systematic improvements in safety. WorldRFT achieves state-of-the-art (SOTA) performance on both open-loop nuScenes and closed-loop NavSim benchmarks. On nuScenes, it reduces collision rates by 83% (0.30% → 0.05%). On NavSim, using camera-only sensors input, it attains competitive performance with the LiDAR-based SOTA method DiffusionDrive (87.8 vs. 88.1 PDMS).

---

## 论文详细总结（自动生成）

# 论文详细中文总结：WorldRFT

## 1. 核心问题与整体含义（研究动机与背景）

- **核心问题**：现有潜在世界模型（Latent World Model）通过时序自监督学习增强场景表示，为端到端自动驾驶提供了一种无需感知标注的范式。然而，重建导向的表征学习将感知任务与规划任务纠缠在一起，导致对规划的优化是次优的。
- **具体体现**：
  - 缺乏空间感知能力：重建目标仅产生通用表征，缺乏关键的3D空间意识。
  - 规划交互机制低效：全局单一查询无法充分捕获局部结构关键特征。
  - 安全感知不足：基于模仿学习的范式缺乏主动碰撞避免能力，仅最小化轨迹偏差。
- **研究意义**：提出面向规划的潜在世界模型框架，使表征学习与规划需求深度对齐，并引入强化学习微调增强安全关键策略。

## 2. 方法论

### 核心思想
- **WorldRFT** 包含三大模块：空间感知世界编码器（SWE）、分层规划精化（HPR）、强化学习微调（RFT）。
- 总体流程：场景理解 → 规划决策 → 安全优化。

### 关键技术细节

#### （1）空间感知世界编码器（SWE）
- 利用几何基础模型 VGGT（冻结）提取多视图一致的3D token，通过交叉注意力与2D图像特征融合，构造空间感知的潜在世界表示。
- 同时使用 Grounded-SAM 进行语义监督。

#### （2）分层规划精化（HPR）
- 将端到端规划分解为三个并行子任务：
  - **目标区域定位**：输出目标区域的拉普拉斯分布（均值μ和尺度b），通过负对数似然损失训练。
  - **空间路径规划**：生成等间隔2米采样的空间路径点。
  - **时序轨迹预测**：预测未来T个时间步的轨迹点（0.5秒间隔）。
- **统一查询交互框架**：三个子任务使用独立的查询（Q_target, Q_path, Q_traj），通过交叉注意力从潜在空间中聚合特征，再经过自注意力实现任务间互通。
- **局部感知迭代精化**：将初始规划结果投影到潜在特征图，利用可变形卷积采样局部特征，并融合全局意图与不确定性表示（b），每轮迭代更新轨迹残差（步长α=0.1）。

#### （3）强化学习微调（RFT）
- **碰撞感知奖励**：根据自车与周围智能体的边界框距离定义奖励（碰撞-1，否则0）。
- **高斯化轨迹建模**：将确定性轨迹扩展为高斯分布（均值μ_θ，方差Σ_θ）。
- **策略优化**：使用组相对策略优化（GRPO），在每组G条轨迹内计算相对优势函数（考虑时间累积误差），优化目标是裁剪后的策略比加上KL散度正则项，KL使用参考模型（预训练模型）与当前策略的高斯分布的负对数似然计算。

### 训练损失
- 预训练阶段：语义损失（L_sem）、世界重建损失（L_rec）、目标区域损失（L_target）、轨迹L1损失（L_traj）。
- 微调阶段：RL损失（L_RL = -J(θ) + λD_KL）。

## 3. 实验设计

### 数据集与场景
- **nuScenes**：开环评估，报告L2误差（1s/2s/3s平均）和碰撞率（CR）。
- **NavSim**：闭环评估，使用PDM得分（PDMS），包含5个维度：no at-fault collision (NC), drivable area compliance (DAC), time-to-collision (TTC), ride comfort (Comf.), ego progress (EP)。

### 对比方法
- **感知基模仿学习（P-IL）**：ST-P3, OccNet, UniAD, VAD, UncAD, PPAD, PARA-Drive, GenAD, LAW（基于感知）, DiffusionDrive, Epona等。
- **自监督学习（SS-L）**：LAW（无感知）, World4Drive, SSR等。
- 部分方法使用LiDAR+相机输入。

### 实验设置
- 消融实验在nuScenes上进行（表3），包括VGGT、分层规划任务、迭代精化、RL微调等组件的有效性。
- NavSim结果在表2展示，与多个方法对比。
- 所有消融实验默认包含RL微调以确保安全评估一致性。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅在附录中提及更多实验细节，但未在主体部分给出资源消耗数据。

## 5. 实验数量与充分性

- **实验数量**：共进行了多组实验：
  - 开环nuScenes（表1）与闭环NavSim（表2）两个基准的主要结果。
  - 消融实验（表3）：8种配置，分别验证VGGT、各规划子任务、迭代精化、完整模块组合。
  - 定性结果（图4、图5）展示了轨迹对比和注意力可视化。
- **充分性与公平性**：
  - 实验覆盖全面，从指标（L2、CR、PDMS各维度）到视觉分析。
  - 对比方法均为近年SOTA，且分开「感知基」和「无感知」分组，公平比较。
  - 消融设计逐步分析每个组件贡献，逻辑清晰。
  - 但缺少对超参数灵敏度、RL微调对L2误差微小退化的分析。

## 6. 主要结论与发现

- WorldRFT在nuScenes上达到SOTA：平均L2误差0.48m（降低21%），碰撞率0.05%（降低83%）。
- 在NavSim上，纯相机输入达到PDMS 87.8，接近LiDAR基SOTA方法DiffusionDrive（88.1）。
- VGGT空间编码提升L2 7.7%，碰撞率下降37.5%。
- 分层规划分解和迭代精化显著降低碰撞和误差。
- RL微调使碰撞率从0.15%降至0.05%，代价是L2略有上升（0.47→0.48），体现了安全优先的权衡。

## 7. 优点

- **方法设计亮点**：
  - 首次提出面向规划的潜在世界模型，通过分层分解和局部感知迭代，使表征直接服务于规划。
  - 将强化学习微调引入潜在世界模型框架，利用GRPO和碰撞奖励实现主动安全避让，比单纯模仿学习更优。
  - 利用冻结的VGGT提供几何先验，无需额外深度监督，实现多视图一致的3D感知。
- **实验设计亮点**：
  - 同时涵盖开环和闭环评估，验证实际部署潜力。
  - 消融实验逐个验证组件有效性，且默认加入RL微调以统一安全评估基线。
  - 在无感知标注的设置下，超越了多种依赖感知标注的方法，展示了范式竞争力。

## 8. 不足与局限

- **局限性**：
  - 算力消耗未报告，不利于复现和资源评估。
  - RL微调环节导致L2误差轻微退化，文中仅归因于安全权衡，但未分析潜在的泛化风险或过度保守行为。
  - 消融实验仅基于nuScenes，未在NavSim上做同样细节的消融，可能缺乏闭环下的组件验证。
  - 方法依赖于VGGT和Grounded-SAM等外部基础模型，存在依赖性和部署成本。
  - 仅基于规划任务评估，未在多任务（如感知联合）场景下验证通用性。
  - 奖励函数仅考虑碰撞，未考虑交通规则、舒适度等其他安全因素，可能产生激进避让或异常行为。
- **应用限制**：当前框架是开环预训练+闭环模拟器评估，真实道路的鲁棒性和实时性尚未验证。

（完）
