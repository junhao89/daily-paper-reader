---
title: "LatentVLA: Taming Latent Space for Generalizable and Long-Horizon Bimanual Manipulation"
title_zh: LatentVLA：驯服潜空间实现可泛化长时域双臂操作
authors: Junming Wang
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38926/42888"
tags: ["query:latent-vla"]
score: 9.0
evidence: 学习连续潜动作空间用于VLA
tldr: 针对逆动力学模型潜动作空间与物理现实脱节导致时间纠缠等问题，提出LatentVLA框架，学习连续潜动作空间。通过将动作表征与物理状态紧密关联，消除视觉相似状态下的动作歧义，在长时程双臂操作任务中展现强泛化性和鲁棒性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38926/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 877, \"height\": 417, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38926/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1107, \"height\": 601, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38926/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1839, \"height\": 1123, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38926/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1848, \"height\": 651, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38926/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 661, \"height\": 417, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38926/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 884, \"height\": 565, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38926/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 384, \"height\": 459, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38926/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 344, \"height\": 451, \"label\": \"Table\"}]"
motivation: 现有潜动作空间与物理现实脱节，导致时间纠缠和离散化伪影，阻碍鲁棒规划。
method: 学习连续潜动作空间，将动作表征与物理状态紧密关联，消除视觉相似状态下的动作歧义。
result: 在长时程双臂操作任务中实现更强的泛化性和规划鲁棒性。
conclusion: 连续潜动作空间学习是提升VLA模型泛化和长时域操作能力的关键。
---

## Abstract
Current paradigms for robotic imitation learning face a stark trade-off between the motion fidelity of diffusion models and the data scalability of inverse dynamics models. The latter, while scalable, often learns a latent action space disconnected from physical reality. This flaw leads to critical failures: temporal entanglement, where the model cannot distinguish between visually similar states requiring distinct actions, e.g., a gripper approaching versus receding from an object. This ambiguity, compounded by discretization artifacts and sensitivity to task-irrelevant dynamics, renders robust planning infeasible. We introduce LatentVLA, a vision-language-action framework designed to overcome these limitations by learning a continuous and spatiotemporally grounded latent action representation. Its progressive three-stage architecture first employs a Temporal-Attentive Latent Action Model (TA-LAM) to resolve ambiguities using language-guided attention and explicit temporal encoding. Subsequently, a Latent Action Diffusion Transformer (LADT) performs planning via diffusion directly within this continuous latent space, preserving motion fidelity without tokenization. Finally, an expert policy head translates these latent plans into precise robot actions. Experiments show LatentVLA sets a new state-of-the-art across a suite of real-world bimanual tasks, outperforming prior methods and demonstrating superior zero-shot generalization and few-shot efficiency.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **背景**：机器人模仿学习面临两个矛盾范式——**扩散模型**运动保真度高但依赖大量标注数据，**逆动力学模型**数据可扩展但学到的潜动作空间与物理现实脱节。
- **核心问题**：逆动力学模型存在三个关键失效模式：
  - **时间纠缠**（Temporal Entanglement）：视觉相似的帧（如夹爪接近与远离物体）被映射到相同潜动作，导致动作歧义。
  - **离散化伪影**（Discretization Artifacts）：使用VQ-VAE等离散化时丢失连续运动信息，产生抖动轨迹。
  - **任务无关动力学敏感**（Task-irrelevant Dynamics）：环境噪声（光线变化、相机抖动）污染潜空间，误导策略。
- **整体含义**：需要设计一种**连续且时空上接地**的潜动作表示，从海量异构数据中学习，同时解决上述问题，实现可泛化、长时程的双臂操作。

## 2. 方法论

### 核心思想
将**表示学习**与**规划**解耦：先学习一个鲁棒、连续的潜动作空间（从有/无标签数据中），再在此高质量空间中执行规划，最后通过专家策略头落地。

### 三阶段架构

#### 阶段1：时间注意潜动作模型（TA-LAM）
- **逆动力学模型（IDM）**：推断单步潜动作 \(Z_t \in \mathbb{R}^{512}\)。
  - **语言引导空间注意力**：使用冻结的SigLIP模型，通过文本嵌入与图像特征的点积计算注意力掩码，聚焦任务相关区域。
  - **时间解缠**：将历史上下文（视觉、语言、本体感知特征）与绝对时间编码 \(\gamma(t)\) 和位置编码拼接，送入Transformer编码器，输出 \(Z_t\)。绝对时间编码告知模型当前任务阶段，相对位置编码建模短期动态。
- **前向动力学模型（FDM）**：预测下一观测 \(\hat{O}_{t+1}\)，用于无监督重构损失。
- **动作解码器**：轻量MLP将潜动作映射到连续机器人动作 \(\hat{a}_t \in \mathbb{R}^{14}\)。
- **联合优化**：总损失 \(L = L_{\text{recon}} + \lambda L_{\text{action}}\)，其中 \(L_{\text{recon}}\) 对所有序列（含无标签视频）计算重构MSE，\(L_{\text{action}}\) 仅对有标签数据计算动作MSE。

#### 阶段2：潜动作扩散Transformer（LADT）
- 使用**连续扩散**在潜空间内规划未来 \(H=16\) 步动作，避免离散化伪影。
- 条件：历史观测、先前潜动作、本体感知、语言指令。
- 训练目标：标准扩散去噪得分匹配损失。
- 语言条件通过跨层交替注入视觉与文本特征防止信息覆盖。

#### 阶段3：真实世界微调
- 冻结TA-LAM（IDM和动作解码器），仅微调LADT规划器。
- 使用1600小时新演示数据，最小化解码动作与专家动作的MSE，实现高效适配。

## 3. 实验设计

### 数据集
- **预训练数据集 \(D_{\text{pretrain}}\)**：超过250万条序列，来自52个源，包括无标签人类视频、多实体机器人数据（RDT-1B语料）、仿真数据、真实世界演示（AgiBot World Beta + LatentVLA-Dexterous约50万条）。
- **微调数据集 \(D_{\text{finetune}}\)**：1600小时新演示，覆盖8个特定双臂任务。

### Benchmark
- **真实世界**：8个挑战性双臂任务（如叠布、叠毛巾、拾取等），含分布内与零样本泛化测试（光照变化、背景变化、新物体、动态干扰）。
- **模拟**：
  - RoboTwin 1.0：复杂双臂协调；
  - SIMPLER：长时域操作；
  - CALVIN (ABC→D)：组合泛化。

### 对比方法
- **主要基线**：RDT-1B、LAPA（从零重训）。
- **其他**：DP、DP3、Moto、Octo、OpenVLA、RT-2、RoboFlamingo、GR-1等（在模拟基准中对比）。

## 4. 资源与算力
- **预训练阶段（Stage 1 & 2）**：在**112块NVIDIA A100-80G GPU**集群上，训练约**1个月**。
- **微调阶段（Stage 3）**：100,000次迭代，未明确说明具体配置，但提及单A100 GPU微调不到1小时（少样本实验）。
- 整体计算成本较高，但论文未详细给出总GPU时数。

## 5. 实验数量与充分性
- **实验数量**：充分。
  - 真实世界：8个任务 × 多种条件（分布内+零样本+少样本）。
  - 模拟：3个基准，每个含多个子任务（RoboTwin 1.0有9个子任务）。
  - 消融实验：1个核心消融（毛巾折叠任务，6个变体）。
  - 数据消融：比较不同数据规模（从1.3M vs 2.5M轨迹）。
  - 少样本：仅20个演示微调。
- **公平性**：
  - 所有基线均在同一数据集上从零重训，确保公平。
  - 消融中每个变体严格控制变量。
- **客观性**：结果以表格和柱状图呈现，误差分析（如最终成功率）明确。但未见置信区间或多次试验标准差报告，略显不足。

## 6. 主要结论与发现
1. **性能优越**：真实世界平均成功率63.8%，优于RDT-1B（+12.8%）和LAPA（+48.5%）；模拟基准上均达SOTA（RoboTwin 1.0平均提升26.0%，SIMPLER相较LAPA提升26.1%，CALVIN平均长度3.52→3.52，但需注意原文给出LatentVLA为3.52，Moto为3.10，提升13.5%）。
2. **零样本泛化强**：在光照、背景、干扰等OOD条件下保持56%成功率，优于离散方法的“表示悬崖”问题。
3. **少样本高效**：20个演示微调毛巾折叠任务达61%成功率，超过RDT-1B 27%。
4. **架构组件关键**：消融显示连续扩散规划器（-33%）和绝对时间编码（-27%）最重要，其次是视频+语言引导（-23%/-19%），再次是历史条件（-13%）。

## 7. 优点
- **新颖性**：首次系统性地同时解决逆动力学模型的三个失效模式，设计简洁且整合度高。
- **数据效率**：利用无标签视频联合训练，显著提升数据利用率。
- **连续潜空间**：避免离散化伪影，实现平滑、物理合理的运动。
- **语言引导时空注意力**：有效过滤任务无关动态，解决时间纠缠。
- **三阶段渐进学习**：表示→规划→执行，各模块可独立优化且冻结预训练表示，提高微调效率。
- **实验全面**：涵盖真实与模拟、分布内与OOD、少样本与消融，对比方法充分。

## 8. 不足与局限
1. **局限性提及**：对**完全物体遮挡**和**高反光表面**造成的视觉歧义较敏感。
2. **实验覆盖**：
   - 未报告多次运行的标准差或置信区间，结果稳健性难以全面评估。
   - 消融实验仅基于单一任务（毛巾折叠），结论泛化性需更多任务验证。
3. **偏差风险**：预训练数据中可能存在特定场景偏差（如AgiBot World Beta），导致泛化受限。
4. **应用限制**：
   - 模型规模大（2.5M序列，112 GPU月训练），资源需求高。
   - 需要明确的语言指令，无法处理隐式指令或未定义任务。
   - 仅针对双臂操作，未验证单臂或移动操作。
5. **计算成本**：虽论证了数据效率，但预训练成本极高，小团队可能难以复现。

（完）
