---
title: "H-GAR: A Hierarchical Interaction Framework via Goal-Driven Observation-Action Refinement for Robotic Manipulation"
title_zh: "H-GAR: 基于目标驱动的观察-动作细化分层交互框架用于机器人操作"
authors: "Yijie Zhu, Rui Shao, Ziyang Liu, Jie He, Jizhihui Liu, Jiuru Wang, Zitong Yu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38958/42920"
tags: ["query:wam-vla"]
score: 8.0
evidence: 目标驱动的观察和动作联合预测
tldr: H-GAR通过目标驱动的分层观察-动作细化框架，先生成目标观测和粗动作草图，再逐步细化，实现语义对齐的预测和连贯行为，实验证明在多种操作任务上优于统一预测方法。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38958/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 878, \"height\": 664, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38958/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1819, \"height\": 721, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38958/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 830, \"height\": 300, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38958/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 866, \"height\": 586, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38958/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1652, \"height\": 672, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38958/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1646, \"height\": 549, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38958/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 838, \"height\": 282, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38958/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1816, \"height\": 509, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38958/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1824, \"height\": 350, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38958/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 858, \"height\": 274, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38958/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 854, \"height\": 210, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38958/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 854, \"height\": 476, \"label\": \"Table\"}]"
motivation: 现有方法忽视任务目标，导致预测语义不对齐。
method: 分层框架：先预测目标观测和粗略动作草图，再细化。
result: 在机器人操作任务上实现更连贯的行为。
conclusion: 目标驱动的分层预测可提高操作任务的一致性和语义对齐。
---

## Abstract
Unified video and action prediction models hold great potential for robotic manipulation, as future observations offer contextual cues for planning, while actions reveal how interactions shape the environment. However, most existing approaches treat observation and action generation in a monolithic and goal-agnostic manner, often leading to semantically misaligned predictions and incoherent behaviors.
To this end, we propose H-GAR, a Hierarchical interaction framework via Goal-driven observation-Action Refinement. To anchor prediction to the task objective, H-GAR first produces a goal observation and a coarse action sketch that outline a high-level route toward the goal. To enable explicit interaction between observation and action under the guidance of the goal observation for more coherent decision-making, we devise two synergistic modules. (1) Goal-Conditioned Observation Synthesizer (GOS) synthesizes intermediate observations based on the coarse-grained actions and the predicted goal observation. (2) Interaction-Aware Action Refiner (IAAR) refines coarse actions into fine-grained, goal-consistent actions by leveraging feedback from the intermediate observations and a Historical Action Memory Bank that encodes prior actions to ensure temporal consistency. By integrating goal grounding with explicit action-observation interaction in a coarse-to-fine manner, H-GAR enables more accurate manipulation. Extensive experiments on both simulation and real-world robotic manipulation tasks demonstrate that H-GAR achieves state-of-the-art performance.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的机器人操作统一视频-动作预测模型（如UVA、UniPi等）采用“整体式、目标无关”的生成范式，即直接一次性预测未来所有观测和动作序列，缺乏对任务最终目标的显式引导，也没有建模观测与动作之间的结构化因果关系。这导致预测结果在语义上可能与任务目标不对齐（产生视觉合理但任务无关的序列），且动作与观测之间的时序连贯性差，限制了规划准确性和适应性。
- **整体含义**：为提升机器人操作中的长期规划与精细操控能力，需要一种既能锚定任务目标（目标观测）又能显式建模观测与动作交互的框架，使预测更加语义对齐、行为连贯。H-GAR正是为解决这一需求而提出。

### 2. 论文提出的方法论

- **核心思想**：采用分层次、由粗到细的生成方式，先预测任务最终的目标观测（goal observation）和粗略动作草图（coarse action sketch），再通过两个协同模块——目标条件观测合成器（GOS）和交互感知动作细化器（IAAR）——显式地让观测和动作在目标引导下相互交互，逐步生成精细、一致的中间观测和动作序列。
- **关键技术细节**：
  - **Transformer编码器**：将历史观测、未来观测（训练时随机mask）和语言指令经过VAE/CLIP编码后，拼接成多模态序列，由Transformer生成联合潜在表示 $Z_{t+1:t+h'}$。
  - **目标观测预测**：使用轻量级视频扩散解码器，以最后一个时间步的潜在 $Z_{t+h'}$ 为条件，通过去噪过程重建目标帧 $O_{t+h'}$，损失 $L_{goal}$（公式2）。
  - **粗略动作生成**：将联合潜在 $Z_{t+1:t+h'}$ 输入扩散模型生成粗略动作序列，损失 $L_{coarse}$（公式3）。
  - **目标条件观测合成器（GOS）**：引入可学习查询 $Q_{Inter}$，先与目标观测潜在做自注意力，再以粗略动作潜在为键值做交叉注意力，输出中间观测潜在 $Z_{Inter}$（公式4,7,8），损失 $L_{inter}$。
  - **交互感知动作细化器（IAAR）**：先通过历史动作记忆库（$H_t$）作为键值、粗略动作潜在作为查询进行历史交互层（HisInter），增强时间一致性；再以 $Z_{Inter}$ 为键值进行交叉注意力，输出细化后的动作潜在 $\hat{A}$（公式5,9,10），损失 $L_{fine}$。
  - **历史动作记忆库管理**：采用基于余弦相似度的冗余感知压缩策略，当记忆超过阈值时合并最相似相邻对以保持多样性（公式11，图3）。
- **总损失**：$L_{total} = L_{goal} + L_{coarse} + L_{inter} + L_{fine}$（公式6）。

### 3. 实验设计

- **数据集/场景**：
  - **仿真**：
    - 单任务：PushT（推积木任务）
    - 多任务：PushT-M（多目标变体）和Libero-10（10种操作任务）
  - **真实世界**：使用Cobot Agilex ALOHA平台，包含4个任务：物体放置（Object Placement）、抽屉操作（Drawer Manipulation）、折毛巾（Towel Folding）、鼠标整理（Mouse Arrangement）。对长程任务报告阶段完成率。
- **Benchmark**：与以下方法对比：
  - Diffusion Policy-C/T（CNN/Transformer版）、UniPi、OpenVLA、STAR、CoT-VLA、SpatialVLA、PD-VLA、UVA等（部分结果引用原论文，部分为作者复现）。
  - 观测生成对比：UniPi、UVA（不同自回归步数）。
- **评估指标**：任务成功率（SR）和秩（RK），以及观察生成的Fréchet Video Distance（FVD）。

### 4. 资源与算力

- 论文中未明确说明所使用的GPU型号、数量及训练时长。仅在致谢中提到计算资源由“SongShan Lake HPC Center in Great Bay University”支持，但未给出具体硬件配置或训练时间。因此这部分信息缺失。

### 5. 实验数量与充分性

- **实验数量**：论文进行了大量实验，包括：
  - 仿真单任务和多任务主实验结果（Table 1）
  - 真实世界4个任务的结果（Table 2，包含长程任务阶段完成率）
  - 观测生成质量对比（Table 3，在Libero-10和Mouse Arrangement上比较FVD）
  - 消融实验：
    - 模型组件消融：GOS和IAAR（含记忆库）的贡献（Table 4）
    - 目标条件观测生成策略消融：对比目标帧、均匀多帧、随机单帧（Table 5）
    - 历史动作记忆库的容量和更新策略消融（Table 6）
    - FVD与成功率相关性分析（Figure 4）
  - 定性可视化（Figure 5,6）
- **充分性与公平性**：
  - 覆盖了仿真和真实世界多种难度的任务，对比方法包括近年来主流模型，复现部分时使用相同设置。
  - 消融实验全面，证明了各模块的必要性。
  - 但未报告标准差或多次运行结果（如多次随机种子），存在一定偏差风险。此外，真实世界实验每个任务仅10次测试，统计量有限。

### 6. 论文的主要结论与发现

- H-GAR在仿真（PushT、PushT-M、Libero-10）和真实世界（4个任务）上均达到最佳成功率，显著优于基线方法。
- 目标引导的观测生成策略（先预测目标观测）优于均匀多帧或随机单帧策略，表明目标观测提供了语义锚点，有助于生成语义对齐的中间帧。
- GOS和IAAR模块协同工作，尤其IAAR中的历史动作记忆库采用相似度合并策略比FIFO和随机更好，表明保持多样性有利于细化。
- 观测生成质量（低FVD）与任务成功率呈强负相关（Figure 4），说明更好的视觉预测直接提升下游规划准确度。
- 定性结果（Figure 5,6）展示H-GAR能够生成时间连贯的多阶段操作过程，而UVA在关键步骤失败。

### 7. 优点

- **方法论创新**：提出“先预测目标、再细化”的分层交互框架，解决了现有方法目标无关和隐式建模的缺陷，具有较强新颖性。
- **模块设计合理**：GOS和IAAR通过交叉注意力显式耦合观测与动作，且历史记忆库压缩策略考虑了多样性，细节扎实。
- **实验全面**：覆盖仿真单/多任务、真实世界多任务，对比模型多，消融实验丰富（组件、策略、记忆库大小/更新方式、FVD分析）。
- **性能领先**：在多个指标上均获得SOTA，尤其真实世界任务提升明显。

### 8. 不足与局限

- **算力资源未公开**：无法复现训练成本或判断资源需求。
- **统计可靠性不足**：真实世界实验仅10次测试，未报告成功率的置信区间或方差，可能受随机性影响。
- **任务类型有限**：仅涉及桌面操作（抓取、放置、折叠等），未测试移动操作、多机器人协同等更复杂场景。
- **泛化性验证不足**：跨任务分布（如从未见过的物体或环境）未进行零样本或少样本测试。
- **依赖预训练编码器**：使用预训练VAE和CLIP，可能引入领域偏差，且在未见过的视觉域上性能可能下降。
- **未探讨长程记忆压缩的极限**：记忆库容量增加导致性能提升，但未提供理论分析或更大容量的实验。

（完）
