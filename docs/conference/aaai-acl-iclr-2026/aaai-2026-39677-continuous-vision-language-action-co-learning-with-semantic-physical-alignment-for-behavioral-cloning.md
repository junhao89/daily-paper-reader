---
title: Continuous Vision-Language-Action Co-Learning with Semantic-Physical Alignment for Behavioral Cloning
title_zh: 连续视觉-语言-动作联合学习与语义-物理对齐的行为克隆
authors: "Xiuxiu Qi, Yu Yang, Jiannong Cao, Luyao Bai, Chongshan Fan, Chengtai Cao, Hongpeng Wang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39677/43638"
tags: ["query:latent-vla"]
score: 9.0
evidence: 提出连续视觉-语言-动作联合学习与语义-物理对齐进行行为克隆
tldr: 针对行为克隆中复合误差和语义-物理不对齐的问题，提出连续视觉-语言-动作联合学习框架CCoL，通过语义-物理对齐和连续动作表示，有效降低了复合误差，提升了操作性能。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39677/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1724, \"height\": 739, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39677/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 860, \"height\": 461, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39677/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 864, \"height\": 383, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39677/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 592, \"height\": 312, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39677/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 591, \"height\": 173, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39677/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 844, \"height\": 365, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39677/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 862, \"height\": 444, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39677/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 870, \"height\": 486, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39677/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 862, \"height\": 373, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39677/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 880, \"height\": 409, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39677/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1779, \"height\": 499, \"label\": \"Table\"}]"
motivation: 现有行为克隆方法存在物理不连续和语义-物理不对齐，导致动作克隆不准确。
method: 提出CCoL框架，实现连续的视觉-语言-动作联合学习，并通过语义-物理对齐改善动作生成。
result: 在语言条件操作任务上减少了复合误差，提高了行为克隆的准确性。
conclusion: 连续联合学习与对齐能有效克服行为克隆中的关键挑战。
---

## Abstract
Language-Conditioned Manipulation (LCM) facilitates human-robot interaction via Behavioral Cloning (BC), which learns control policies from human demonstrations and serves as a cornerstone of embodied AI. Overcoming compounding errors in sequential action decisions remains a central challenge to improving BC performance. Existing approaches mitigate compounding errors through data augmentation, expressive representation, or temporal abstraction. However, they suffer from physical discontinuities and semantic-physical misalignment, leading to inaccurate action cloning and intermittent execution. In this paper, we present Continuous vision-language-action Co-Learning with Semantic-Physical Alignment (CCoL), a novel BC framework that ensures temporally consistent execution and fine-grained semantic grounding. It generates robust and smooth action execution trajectories through continuous co-learning across vision, language, and proprioceptive inputs (i.e., robot internal states). Meanwhile, we anchor language semantics to visuomotor representations by a bidirectional cross-attention to learn contextual information for action generation, successfully overcoming the problem of semantic-physical misalignment. Extensive experiments show that CCoL achieves an average 8.0% relative improvement across three simulation suites, with up to 19.2% relative gain in human-demonstrated bimanual insertion tasks. Real-world tests on a 7-DoF robot further confirm CCoL’s generalization under unseen and noisy object states.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：语言条件操作（LCM）中的行为克隆（BC）因**复合误差**（compounding errors）导致性能下降。具体表现为：  
  - **物理不连续性**：离散动作建模（如时间抽象）导致轨迹突变、高加速度振荡。  
  - **语义-物理不对齐**：静态多模态融合无法动态适应任务阶段的语义变化，造成动作克隆不准确。

- **研究背景**：LCM 是实现具身智能的关键范式，BC 从专家演示中学习策略，但小误差会随序列长度二次增长（covariate shift）。现有方法通过数据增强、表达性表征或时间抽象缓解问题，但未从根本上解决时序一致性和语义-物理对齐。

- **整体含义**：本文提出连续联合学习与语义-物理对齐的新框架 CCoL，旨在生成**平滑、鲁棒**的动作轨迹，克服复合误差，提升长周期操作成功率。

### 2. 论文提出的方法论

- **核心思想**：通过**多模态连续联合学习**（MCC）在隐空间中用神经常微分方程（Neural ODE）建模本体感知动态，保持时间一致性；同时用**跨模态语义-物理对齐**（CSA）以双向交叉注意力实现每步语义锚定，指导动作生成。

- **关键技术细节**：
  - **上下文感知表征学习**：分别用 ViT、RoBERTa 和 CVAE 编码视觉、语言和本体感知（关节位置）特征。
  - **多模态连续联合学习 (MCC)**：  
    - 将本体感知特征经 CVAE 映射为初始隐状态 \(z_0\)。  
    - 以 Neural ODE 演化 \(z(t) = z_0 + \int_{0}^{t_\delta} f(z(t), t; \psi) dt\)，得到连续隐轨迹 \(Z_t\)，替代离散步特征。  
  - **跨模态语义-物理对齐 (CSA)**：  
    - 对文本嵌入 \(\tilde{l}_t\) 和视觉-本体联合上下文 \(X_t = (\tilde{x}_t, \tilde{Z}_t)\) 进行双向交叉注意力计算：  
      \[
      F_t^{(\iota)}(\tilde{l}_t, X_t) = \mathrm{softmax}\left(\frac{(W_q^{(\iota)}\tilde{l}_t)(W_k^{(\iota)}X_t)^\top}{\sqrt{d_k}}\right)
      \]
      （同理有 \(F_t^{(\iota)}(X_t, \tilde{l}_t)\)）。  
    - 融合特征 \(\tilde{F}_t = \sum_{\iota=1}^n \left(F_t^{(\iota)}(\tilde{l}_t, X_t) X_t + F_t^{(\iota)}(X_t, \tilde{l}_t) \tilde{l}_t\right)\)。  
    - 加入位置编码保持时间连贯，得到最终多模态表征 \(\xi_t\)。  
  - **上下文动作生成**：目标条件解码器预测未来 \(k\) 步动作序列 \(a'_{t:t+k}\)，包含残差连接、层归一化和 Dropout。

- **优化目标**：  
  总损失 \(\mathcal{L} = \frac{1}{N}\sum(\mathcal{L}_{\text{BC}} + \mathcal{E}_{\text{disc}})\)，其中  
  - \(\mathcal{L}_{\text{BC}}\) 为行为克隆损失（包含重建误差 + KL 散度）。  
  - 不连续性惩罚 \(\mathcal{E}_{\text{disc}} = \sum_{t}^{t+k} \int_{t_0}^{t_\delta} \| \frac{dz(t)}{dt} - f(z(t),t;\psi) \|_2^2 dt\)，强制隐轨迹平滑。

### 3. 实验设计

- **数据集与场景**：
  1. **Aloha MuJoCo**：双臂协作任务（Cube Transfer、Bimanual Insertion），含脚本和人类演示。
  2. **RLBench**：多场景单臂任务（Lamp On、Grill Meat、Phone、Open Bottle）。
  3. **Franka Kitchen**：多阶段长周期任务（Turn Knob、Open Door、Open Microwave 及其组合）。

- **基准方法**：
  - 时序建模：BCCNN、RT-1、BeT、VINN。
  - 动作块/时间抽象：ACT、AWE。
  - 扩散模型：DP、DIC、HDP、3DDiff。
  - 表征增强：R3M、Voltron、MPI。

- **评价指标**：成功率（%），对任务完成条件有明确界定（如物品转移要求 1cm 碰撞余量）。

### 4. 资源与算力

- 文中明确提到：  
  - 训练硬件：RTX 4090 GPU。  
  - 训练时长：5.3 小时（Aloha MuJoCo 背景？实际为总训练时间？原文“Our model trains in 5.3 hours on an RTX 4090 GPU”）。  
  - 推理速度：每动作序列 0.015 s（±0.003 s），对应约 67 Hz 策略频率。  
  - 模型参数量未具体给出，但提及使用了 ViT-S（22M）和 ViT-B（86M）作为视觉骨干。

- 未提及使用的其他算力细节（如 CPU、内存、数据并行度等）。

### 5. 实验数量与充分性

- **实验组数**：
  - 三大仿真套件共超过15个子任务（Aloha 2个×2种演示 = 4组，RLBench 4个任务，Franka Kitchen 3个单任务+3个长周期组合）。
  - 消融实验（Table 4）：7种变体，覆盖 MCC、CSA、Edisc、注意力替换、TCN 等。
  - 超参数实验（Fig.5）：3种 ODE 时间步长。
  - 真实世界实验（Fig.6）：3个任务×每个任务15次试验，含4类失败原因统计。
  - 定性分析（Fig.4）：注意力可视化、阶段注意分数。

- **充分性与公平性**：
  - 对比方法覆盖主流类别，且与 AWE、ACT、3DDiff 等保持相同设置（如 Aloha 与 AWE 一致，RLBench 与 3DDiff 一致）。
  - 消融实验逐步验证各组件贡献；超参数实验证明时间步长影响。
  - 真实世界测试检验泛化能力（未见过的物体、噪声状态）。
  - 所有实验均报告多个随机种子（如 RLBench 3种子）以降低随机误差。

- **可能不足**：未在更多难任务（如 cloth folding、multi-step assembly）上验证，且真实世界任务仅3个、每任务15次试验，样本量较小。

### 6. 论文的主要结论与发现

- CCoL 在三套仿真环境中平均相对提升 8.0%，在人类演示双臂插入任务上最高提升 19.2%（相对）。
- MCC 通过 Neural ODE 实现连续隐轨迹，显著降低速度/加速度抖动（分别降低 30.8% 和 32.7%）。
- CSA 动态调整注意力（如从抓取右夹爪→红立方体→左夹爪），实现阶段性语义锚定。
- 真实世界中，在物体位置、大小、噪声干扰下仍保持高成功率（如 cubes placement 86.7%）。
- 大时间步长（2.0）比小步长（0.5）更稳定，加速收敛。
- 冻结视觉编码器仍能取得竞争力（ViT-B frozen 34.5% vs. MPI 30.9%），表明 CCoL 不依赖视觉微调。

### 7. 优点

- **方法创新**：将 Neural ODE 引入隐空间建模本体感知时间依赖，结合双向交叉注意力实现动态语义对准，从本质上缓解复合误差和物理不连续。
- **实验全面**：覆盖仿真和真实世界，包含多样化任务（双臂、长周期、3D场景）、多种骨干网络、详尽的消融和超参分析。
- **结果显著**：在多个 SOTA 基线（如 DIC、3DDiff、MPI）上一致领先，尤其在人机协作双臂任务上提升明显。
- **代码与项目公开**：提供项目网站，但论文未明确开源代码（元数据中 project website 可用）。
- **实时性**：推理速度 67 Hz，满足机器人实时控制需求。

### 8. 不足与局限

- **实验覆盖**：未在更多灵巧操作或动态交互任务（如开门、衣物处理）中测试，且真实世界任务数较少、样本量有限（每任务仅15次），泛化性结论需更多验证。
- **方法局限**：依赖离线专家演示，未探索在线互动纠正；Neural ODE 求解器选择固定步长（2.0、1.0、0.5），未与其他自适应方法对比。
- **资源与实现**：虽提及训练时间，但未说明模型参数总量、FLOPs 或内存占用，难以完全复现效率；未公开完整代码仓库（仅有项目网站）。
- **偏差风险**：所有仿真任务均来自特定基准套件（Aloha、RLBench、Franka Kitchen），可能存在数据偏差；语言指令为模板化英语，未涉及多样化自然语言。
- **应用限制**：假设视觉和本体感知数据完美对齐，实际中传感器噪声或延迟可能影响性能；未讨论多机器人或动态障碍物情况。

（完）
