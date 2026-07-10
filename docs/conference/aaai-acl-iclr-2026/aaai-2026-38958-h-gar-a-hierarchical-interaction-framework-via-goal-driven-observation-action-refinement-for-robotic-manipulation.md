---
title: "H-GAR: A Hierarchical Interaction Framework via Goal-Driven Observation-Action Refinement for Robotic Manipulation"
title_zh: H-GAR：面向机器人操作的目标驱动观测-动作精化层次化交互框架
authors: "Yijie Zhu, Rui Shao, Ziyang Liu, Jie He, Jizhihui Liu, Jiuru Wang, Zitong Yu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38958/42920"
tags: ["query:wam-vla"]
score: 7.0
evidence: 面向统一视频和动作预测的目标驱动观测-动作精化
tldr: 针对现有方法将观测和动作生成视为整体且缺乏目标导向的问题，提出H-GAR层次化交互框架。首先生成目标观测和粗略动作草图作为高层路线，然后通过显式交互精化预测。框架统一了视频预测和动作生成，在机器人操作任务中实现了语义对齐的行为序列。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38958/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 878, \"height\": 664, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38958/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1819, \"height\": 721, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38958/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 830, \"height\": 300, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38958/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 866, \"height\": 586, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38958/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1652, \"height\": 672, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38958/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1646, \"height\": 549, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38958/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 838, \"height\": 282, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38958/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1816, \"height\": 509, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38958/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1824, \"height\": 350, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38958/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 858, \"height\": 274, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38958/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 854, \"height\": 210, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38958/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 854, \"height\": 476, \"label\": \"Table\"}]"
motivation: 现有方法将观测和动作预测视为整体，缺乏目标导向，导致语义不对齐。
method: 提出层次化框架，先生成目标观测和动作草图，再通过交互精化。
result: 在多个机器人操作任务中生成了语义对齐且连贯的行为序列。
conclusion: 目标驱动的层次化精化是统一视频和动作预测的有效方法。
---

## Abstract
Unified video and action prediction models hold great potential for robotic manipulation, as future observations offer contextual cues for planning, while actions reveal how interactions shape the environment. However, most existing approaches treat observation and action generation in a monolithic and goal-agnostic manner, often leading to semantically misaligned predictions and incoherent behaviors.
To this end, we propose H-GAR, a Hierarchical interaction framework via Goal-driven observation-Action Refinement. To anchor prediction to the task objective, H-GAR first produces a goal observation and a coarse action sketch that outline a high-level route toward the goal. To enable explicit interaction between observation and action under the guidance of the goal observation for more coherent decision-making, we devise two synergistic modules. (1) Goal-Conditioned Observation Synthesizer (GOS) synthesizes intermediate observations based on the coarse-grained actions and the predicted goal observation. (2) Interaction-Aware Action Refiner (IAAR) refines coarse actions into fine-grained, goal-consistent actions by leveraging feedback from the intermediate observations and a Historical Action Memory Bank that encodes prior actions to ensure temporal consistency. By integrating goal grounding with explicit action-observation interaction in a coarse-to-fine manner, H-GAR enables more accurate manipulation. Extensive experiments on both simulation and real-world robotic manipulation tasks demonstrate that H-GAR achieves state-of-the-art performance.

---

## 论文详细总结（自动生成）

基于提供的论文内容，以下是对该论文的结构化、深入总结。

---

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的统一视频和动作预测模型在机器人操作中大多采用**整体式、无目标导向**的生成策略，即直接从当前观测和指令预测整个未来观测和动作序列，而不显式建模动作如何影响观测、目标如何引导轨迹。这导致两个关键局限：
  - **目标无关的观测生成**：缺乏语义锚点，模型可能生成视觉合理但任务无关的序列，降低可解释性和下游规划精度。
  - **隐式观测-动作建模**：观测和动作通常并行或松散耦合，缺乏因果交互建模，削弱时间连贯性和自适应性。
- **整体含义**：H-GAR通过引入**显式目标接地**和**结构化的观测-动作双向交互**，实现更准确、语义对齐、时间连贯的机器人操作规划，填补了现有工作在目标导向和交互建模上的空白。

## 2. 论文提出的方法论：核心思想、关键技术、算法流程

- **核心思想**：采用**粗到细（coarse-to-fine）的层次化精化**范式，先预测目标观测和粗略动作草图作为高层路线，再通过两个协同模块（GOS和IAAR）进行显式交互精化，使最终动作序列与目标语义一致、时间连贯。
- **关键技术细节**：
  1. **整体框架**（如图2所示）：输入过去观测序列和任务指令，通过Transformer编码得到联合潜在表示。首先预测**目标观测**（最终视觉状态）和**粗略动作序列**（基于联合潜在表示）。然后：
     - **目标条件观测合成器（GOS）**：基于预测的目标观测潜在和粗略动作潜在，通过学习查询（Q_Inter）经过自注意力（融合目标信息）和交叉注意力（注入动作上下文），合成中间观测潜在，提供目标对齐的视觉引导。
     - **交互感知动作精化器（IAAR）**：将粗略动作潜在精化为细粒度动作。包括两个步骤：① 历史交互层（HisInter），利用**历史动作记忆库**（存储先前细粒度动作潜在）作为key/value，对粗动作进行时间一致性增强；② 交叉注意力层，以中间观测潜在为key/value，注入视觉反馈，输出精化后的细粒度动作。
  2. **历史动作记忆库更新策略**：基于余弦相似度选择最相似相邻对进行平均合并，保持紧凑且多样的历史信息。
  3. **训练目标**：总损失包括目标观测重建损失（L_goal）、粗动作损失（L_coarse）、中间观测重建损失（L_inter）、细动作损失（L_fine），全部基于扩散噪声预测目标。
- **算法流程（文字说明）**：
  - 输入：任务指令I，过去观测O_{t-h+1...t}，掩码未来观测。
  - 编码：使用预训练VAE + CLIP文本编码器，经Transformer得到未来步骤联合潜在Z_{t+1:t+h'}。
  - 生成目标观测：基于Z_{t+h'}通过扩散解码器重建目标帧O_{t+h'}。
  - 生成粗略动作：基于Z_{t+1:t+h'}通过扩散预测动作序列。
  - GOS合成中间观测：查询Q_Inter与目标潜在自注意力，再与粗动作潜在交叉注意力，得到中间观测潜在。
  - IAAR精化动作：粗动作潜在与历史记忆库HisInter，再与中间观测潜在交叉注意力，得到精化动作。
  - 各损失基于扩散过程计算，总损失求和训练。

## 3. 实验设计

- **数据集/场景**：
  - **仿真**：
    - 单任务：PushT（经典桌面推盘任务）。
    - 多任务：PushT-M（多目标变体）、Libero-10（10种生活操作任务）。
  - **真实世界**：Cobot Agilex ALOHA平台，四种任务：
    - 物体放置（Object Placement，长时，包含Cube→Plate和+Toy→Bowl两个子阶段）
    - 抽屉操作（Drawer Manipulation，长时，包含Open、+Place、+Close三个子阶段）
    - 毛巾折叠（Towel Folding）
    - 鼠标整理（Mouse Arrangement）

- **Benchmark**：任务成功率（SR），以及观测生成质量（Fréchet Video Distance, FVD）。

- **对比方法**：
  - 仿真：Diffusion Policy-C/T, UniPi, OpenVLA, STAR, CoT-VLA, SpatialVLA, PD-VLA, UVA（其中部分为自复现）。
  - 真实世界：VQ-BeT, QueST, STAR, PD-VLA, UVA。
  - 观测生成：UniPi, UVA（不同步数下对比FVD）。

## 4. 资源与算力

- **文中未明确说明**：未提及使用的GPU型号、数量、训练时长等具体算力信息。仅在致谢中提到计算资源由松山湖高性能计算中心（SSL-HPC）在Great Bay University提供。因此无法量化训练成本。

## 5. 实验数量与充分性

- **实验数量**：
  - 仿真：单任务（PushT）、多任务（PushT-M、Libero-10）各1组，共3个场景。
  - 真实世界：4个任务，含长时任务的阶段成功率。
  - 消融实验：
    - 模型组件（GOS、IAAR、历史记忆库）共4种配置。
    - 目标条件观测输入策略（Uniform Multi-Frame、Single Random Frame、Goal Frame Only）3种。
    - 历史动作记忆库大小（8/16/32/64）和更新策略（随机/FIFO/相似度）共4×3=12种。
    - 观测生成质量与任务成功率的相关性分析。
  - 总计超过20组对比实验。
- **充分性与公平性**：
  - 充分：覆盖了不同难度（单/多任务）、不同环境（仿真/真实）、不同模块影响、不同设计选择。
  - 公平：对比方法均为SOTA，且部分方法进行了自复现；实验设置遵循标准协议（如PushT、Libero-10标准评估）；消融实验严格控制变量。
  - 但仍然存在未公开代码和未提供资源消耗细节的问题。

## 6. 论文的主要结论与发现

- H-GAR在仿真和真实世界所有任务上均取得**最佳或最优**成功率（仿真PushT: 0.99, PushT-M: 0.90, Libero-10: 0.94；真实世界各任务成功率显著高于对比方法）。
- 在观测生成质量（FVD）上同样最优，且FVD与任务成功率呈**强负相关**，说明高质量观测预测有利于下游执行。
- 消融实验证明：
  - GOS和IAAR两个模块均对性能有显著贡献，二者协同效果最佳。
  - 使用预测的目标观测作为GOS输入优于随机单帧或多帧策略，凸显目标锚定的关键性。
  - 历史动作记忆库的相似度更新策略优于FIFO和随机，且适当增大记忆大小（如32）可提升性能。
- 定性分析（图5、图6）显示H-GAR能生成语义对齐、时间连贯的中间观测，并在复杂多阶段任务中成功完成所有子步骤，而UVA会失败。

## 7. 优点

- **方法创新**：提出层次化目标驱动的粗到细精化范式，将目标观测显式引入观测合成和动作精化，解决了现有方法的目标无关和隐式建模缺陷。
- **模块设计巧妙**：GOS和IAAR形成双向交互循环——GOS提供视觉反馈，IAAR利用历史动作和视觉反馈精化动作，结构清晰且可解释。
- **实验全面**：涵盖仿真单/多任务、真实世界多任务，消融实验覆盖多个设计维度（模块、输入策略、记忆库大小及更新策略），并分析了观测质量与任务成功率的相关性。
- **性能显著**：在几乎所有场景下均超越SOTA方法，尤其在长时任务和复杂操作上优势明显。
- **可落地的真实部署**：在Cobot Agilex ALOHA平台上验证，具有实际应用潜力。

## 8. 不足与局限

- **资源消耗未报告**：缺乏训练所需的GPU型号、时长、能耗等关键信息，难以评估方法的计算效率。
- **依赖外部预训练模型**：使用预训练VAE（来自Rombach et al.）和CLIP文本编码器，其性能可能受预训练质量影响，且可能引入领域偏差。
- **记忆库大小选择敏感**：实验显示太大会引入噪声、太小会丢失信息，最佳值需针对具体场景调优，通用性有待验证。
- **真实世界任务数量和多样性有限**：仅4个任务（其中2个为长时子阶段），且均为桌面机械臂操作，未涵盖更复杂的移动操作或强动态环境。
- **未进行泛化性测试**：例如迁移至不同的机器人平台、未见过的物体类别或光照条件。
- **长时任务的成功率仍非100%**：真实世界任务中最高为9/10或8/10，说明存在失败案例，需要进一步分析错误模式。

---

（完）
