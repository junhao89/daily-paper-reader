---
title: "TTF-VLA: Temporal Token Fusion via Pixel-Attention Integration for Vision-Language-Action Models"
title_zh: "TTF-VLA: 通过像素注意力整合实现视觉-语言-动作模型的时间令牌融合"
authors: "Chenghao Liu, Jiachen Zhang, Chengxuan Li, Zhimu Zhou, Shixin Wu, Songfang Huang, Huiling Duan"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38910/42872"
tags: ["query:latent-vla"]
score: 8.0
evidence: 通过时间令牌融合改进VLA模型
tldr: 视觉-语言-动作模型逐帧处理视觉输入，丢弃了时间信息。TTF-VLA提出训练无关的时间令牌融合方法，结合灰度分析和注意力语义评估，选择性融合历史与当前视觉令牌。该方法无需额外训练，可提升VLA模型在机器人操作中的效率和鲁棒性。实验验证了在多种任务上的有效性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38910/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1619, \"height\": 913, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38910/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 845, \"height\": 820, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38910/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1797, \"height\": 558, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38910/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 878, \"height\": 252, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38910/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 873, \"height\": 871, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38910/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1430, \"height\": 320, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38910/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 809, \"height\": 246, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38910/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1748, \"height\": 320, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38910/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 860, \"height\": 248, \"label\": \"Table\"}]"
motivation: VLA模型逐帧处理视觉输入，忽略帧间时间连贯性，易受噪音影响。
method: 提出训练无关的时间令牌融合，基于灰度差异和注意力检测选择融合历史令牌。
result: 无需训练即可提升VLA推理质量，对视觉噪音更鲁棒。
conclusion: 时间信息融合能显著增强VLA模型的操作性能。
---

## Abstract
Vision-Language-Action (VLA) models process visual inputs independently at each timestep, discarding valuable temporal information inherent in robotic manipulation tasks. This frame-by-frame processing makes models vulnerable to visual noise while ignoring the substantial coherence between consecutive frames in manipulation sequences. We propose Temporal Token Fusion (TTF), a training-free approach that intelligently integrates historical and current visual representations to enhance VLA inference quality. Our method employs dual-dimension detection combining efficient grayscale pixel difference analysis with attention-based semantic relevance assessment, enabling selective temporal token fusion through hard fusion strategies and keyframe anchoring to prevent error accumulation. Comprehensive experiments across LIBERO, SimplerEnv, and real robot tasks demonstrate consistent improvements: 4.0 percentage points average on LIBERO (72.4% vs 68.4% baseline), cross-environment validation on SimplerEnv (4.8% relative improvement), and 8.7% relative improvement on real robot tasks. Our approach proves model-agnostic, working across OpenVLA and VLA-Cache architectures. Notably, TTF reveals that selective Query matrix reuse in attention mechanisms enhances rather than compromises performance, suggesting promising directions for direct KQV matrix reuse strategies that achieve computational acceleration while improving task success rates.

---

## 论文详细总结（自动生成）

# 论文总结：TTF-VLA: 通过像素注意力整合实现视觉-语言-动作模型的时间令牌融合

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：当前视觉-语言-动作（VLA）模型在机器人操作任务中，逐帧独立处理视觉输入，丢弃了相邻帧之间的时间连贯性。这种帧级处理方式使模型易受光照变化、运动模糊、传感器噪声等视觉噪声干扰，且无法利用操作序列中大量静态背景和缓慢变化区域的结构化信息。
- **研究动机**：机器人操作任务中，相邻帧的视觉内容大多一致，仅在局部任务相关区域发生显著变化。若能智能地融合历史与当前视觉表示，可提升推理质量，同时增强对噪声的鲁棒性。
- **整体含义**：提出一种无需训练的时间令牌融合方法（TTF），通过双维度检测机制选择性复用历史令牌，在保持对动态变化灵敏度的同时，利用时间连贯性改善VLA模型的性能，且适用于多种VLA架构。

## 2. 方法论

### 核心思想
- **训练无关**：无需微调或重新训练模型，仅在推理阶段对视觉令牌进行后处理融合。
- **硬融合策略**：对每个图像块（patch）做二值选择——若该块属于“重要区域”，则使用当前帧的令牌；否则复用前一帧的令牌。
- **关键帧机制**：每隔 K 帧强制全部重新计算所有令牌，防止长期误差累积。

### 关键技术细节
- **双维度检测**：
  1. **灰度像素差异检测**：将RGB帧转为灰度图，计算每个patch区域（14×14像素）内相邻帧的平均绝对灰度差。若差值超过阈值τ_pixel，则视为重要区域。计算复杂度低，对微小物体移动敏感。
  2. **注意力语义相关性检测**：利用前一帧transformer层的注意力权重，分别从“文本到视觉”和“动作到视觉”两个源汇聚注意力分数。选取top-k高注意力得分的patch作为重要区域。利用前一帧注意力（假设任务相关区域在时间上稳定）避免额外计算。
- **融合决策**：采用逻辑“或”组合两种掩码，确保任何一类变化都被更新。
- **算法流程**（Algorithm 1）：  
  输入当前帧、前一帧、前一帧令牌和注意力权重。若非关键帧，则提取当前帧令牌和注意力，对每个patch计算像素掩码和注意力掩码，取并集得到融合掩码，按硬融合原则输出融合后的令牌集。

## 3. 实验设计

### 数据集/场景
- **LIBERO**（模拟）：包含4个任务套件（Object、Spatial、Goal、Long），每个套件10个任务×20个episode，共200个episode/套件。
- **SimplerEnv**（模拟跨环境验证）：3个代表性任务（Move Near Object、Pick Coke Can、Drawer Operations），分别240、300、216个episode。
- **真实机器人**：Franka Research 3机器人，3个任务（单物体拾放、多物体顺序操作、接触式抽屉关闭），每个任务80个演示，测试20个episode。

### Benchmark与对比方法
- **基线模型**：OpenVLA（任务调优版和基础版）、VLA-Cache（带KV缓存机制的VLA架构）。
- **对比方式**：将TTF应用于基线，对比加与不加的成功率。同时，在消融研究中比较像素单独、注意力单独以及两者的组合。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等。由于TTF是训练无关方法，仅在推理阶段运行，因此主要开销来自额外的灰度差异计算和注意力提取，作者声称“引入少于2%的额外运行时开销”。未报告训练阶段（如微调OpenVLA）所耗费的算力。

## 5. 实验数量与充分性

- **实验数量**：
  - LIBERO上4个套件×2种模型（OpenVLA、VLA-Cache）= 8组主实验。
  - SimplerEnv上3个任务×1种模型 = 3组跨环境实验。
  - 真实机器人上3个任务×1种模型 = 3组。
  - 消融实验：分析维度消融（像素仅、注意力仅、组合）在LIBERO上覆盖4个套件。
  - 关键帧间隔消融：对K=2~200共14种配置，评估Object和Long任务。
- **充分性与公平性**：
  - 覆盖模拟和真实场景，任务类型多样（单步、多步、长程、接触性）。
  - 对比了不同架构（OpenVLA与VLA-Cache），验证模型无关性。
  - 消融实验完整：组件必要性、参数敏感性均被探讨。
  - 参数设置：LIBERO和SimplerEnv使用相同超参数（τ_pixel=0.03, top-k=70, K=3），真实机器人仅调整了阈值（0.01）和top-k（100），说明跨环境泛化性好。
  - 实验客观性尚可，但未报告多次运行的标准差，可能忽略随机性影响。

## 6. 主要结论与发现

- TTF在LIBERO上平均提升4.0个百分点（68.4%→72.4%），长程任务提升最显著（+11.5%相对）。
- VLA-Cache+TTF平均提升2.7个百分点（71.3%→74.0%），表明TTF可叠加于现有缓存方法。
- 跨环境SimplerEnv验证平均相对提升4.8%，真实机器人提升8.7%。
- **意外发现**：TTF导致VLA-Cache中Query矩阵被隐式复用，但性能不降反升，说明选择性Query复用可增强对视觉噪声的鲁棒性，且有望实现“免费午餐”——同时加速和提效。
- 像素检测和注意力检测互补：像素检测擅长捕捉空间变化，注意力检测擅长捕捉语义重要性，两者组合效果最好且融合率最低（42.8%）。
- 关键帧间隔K在≤15时性能稳定，K≥30后误差累积明显。

## 7. 优点

- **训练无关**：直接用于预训练模型，无需额外训练开销，易于部署。
- **模型无关**：在OpenVLA和VLA-Cache两种架构上均有效，通用性强。
- **轻量高效**：灰度差分复杂度O(1)/patch，注意力利用已有计算结果，额外运行时<2%。
- **双维度设计合理**：结合低级视觉变化与高级语义，互补性强。
- **消融充分**：验证了各组件必要性，以及关键帧参数的影响。
- **真实场景验证**：包含实际机器人实验，增强结论可靠性。
- **新发现**：揭示Query矩阵选择性复用的有益性，为后续高效推理研究提供方向。

## 8. 不足与局限

- **实验偏差风险**：未报告多次运行的标准差；真实机器人实验仅20个episode/任务，样本量较小。
- **超参数敏感性**：像素阈值和注意力top-k需根据场景微调（真实场景需降低阈值），未提供自适应调参方案。
- **对静态场景的依赖**：若任务涉及频繁全局场景变化（如移动相机、光照突变），TTF可能频繁触发关键帧，收益减小。
- **仅评估成功率**：未分析动作质量（如平滑性、碰撞率）、推理延迟等指标。
- **未讨论错误传播**：若历史令牌存在噪声或错误，复用可能滚动放大问题，虽有关键帧机制，但未深入分析最坏情况。
- **算力资源未报告**：微调基线模型所需的算力未提及，对复现成本缺乏指引。
- **局限在特定VLA模型**：实验仅基于OpenVLA和VLA-Cache，未在更多VLA（如RT-2、π0等）上验证。

（完）
