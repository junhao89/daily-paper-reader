---
title: "LatentVLA: Taming Latent Space for Generalizable and Long-Horizon Bimanual Manipulation"
title_zh: LatentVLA：驯服潜在空间实现泛化长时域双手机器人操作
authors: Junming Wang
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38926/42888"
tags: ["query:latent-vla"]
score: 10.0
evidence: LatentVLA学习连续潜在动作空间用于VLA双手机器人操作
tldr: 针对逆动力学模型学习潜在动作空间脱离物理真实导致时序混淆和离散化伪影的问题，LatentVLA框架学习连续的潜在动作空间，有效区分视觉相似但动作不同的状态，显著提升VLA在长时域双手机器人操作中的泛化能力和规划鲁棒性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38926/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 877, \"height\": 417, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38926/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1107, \"height\": 601, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38926/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1839, \"height\": 1123, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38926/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1848, \"height\": 651, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38926/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 661, \"height\": 417, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38926/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 884, \"height\": 565, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38926/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 384, \"height\": 459, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38926/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 344, \"height\": 451, \"label\": \"Table\"}]"
motivation: 现有逆动力学模型学习的潜在动作空间不连续且脱离物理真实，导致时序混淆。
method: 学习连续的潜在动作空间，并设计相应的训练目标以保持物理一致性。
result: 在长时域双手机器人操作任务上展现出色的泛化性能和规划鲁棒性。
conclusion: 连续潜在动作空间是提升VLA模型物理真实性和可规划性的有效途径。
---

## Abstract
Current paradigms for robotic imitation learning face a stark trade-off between the motion fidelity of diffusion models and the data scalability of inverse dynamics models. The latter, while scalable, often learns a latent action space disconnected from physical reality. This flaw leads to critical failures: temporal entanglement, where the model cannot distinguish between visually similar states requiring distinct actions, e.g., a gripper approaching versus receding from an object. This ambiguity, compounded by discretization artifacts and sensitivity to task-irrelevant dynamics, renders robust planning infeasible. We introduce LatentVLA, a vision-language-action framework designed to overcome these limitations by learning a continuous and spatiotemporally grounded latent action representation. Its progressive three-stage architecture first employs a Temporal-Attentive Latent Action Model (TA-LAM) to resolve ambiguities using language-guided attention and explicit temporal encoding. Subsequently, a Latent Action Diffusion Transformer (LADT) performs planning via diffusion directly within this continuous latent space, preserving motion fidelity without tokenization. Finally, an expert policy head translates these latent plans into precise robot actions. Experiments show LatentVLA sets a new state-of-the-art across a suite of real-world bimanual tasks, outperforming prior methods and demonstrating superior zero-shot generalization and few-shot efficiency.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

当前机器人模仿学习存在两个主流范式的权衡：
- **扩散策略**：运动保真度高，但依赖大量带标注的专家数据，可扩展性差。
- **逆动力学模型**：可利用海量无标签视频，数据可扩展性强，但学习到的潜在动作空间往往脱离物理真实，导致三个关键失效模式：
  - **时序纠缠（Temporal Entanglement）**：模型无法区分视觉相似但动作相反的状态（如夹爪靠近 vs. 远离物体），使状态-动作映射病态。
  - **离散化伪影（Discretization Artifacts）**：使用VQ-VAE等离散化表示破坏连续物理运动，产生抖动、不真实的轨迹。
  - **对任务无关动态敏感（Task-irrelevant Dynamics）**：光照、相机运动等视觉噪声污染潜在空间，误导策略。

因此，论文旨在构建一个**连续且时空锚定的潜在动作空间**，同时兼顾表示保真度和数据可扩展性，解决长时域双手机器人操作中的泛化与规划鲁棒性问题。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
通过**三阶段渐进式架构**解耦表示学习与规划：
1. **学一个连续、语义丰富、时间解耦的潜在动作空间**（利用海量异构数据，包括无标签视频和有标签机器人轨迹）。
2. **在此连续潜在空间内执行长时域扩散规划**（避免离散化伪影）。
3. **将潜在规划解码为精确机器人动作**（高效微调）。

### 关键技术细节

#### Stage 1：Temporal-Attentive Latent Action Model (TA-LAM)
- **组成**：逆动力学模型（IDM）+ 前向动力学模型（FDM）+ 动作解码器（Action Decoder），联合优化。
- **IDM 核心创新**：
  - **语言引导的空间注意力**：利用冻结的 SigLIP 模型对多视角图像进行文本嵌入注意力掩码，过滤任务无关干扰，聚焦相关物体。
  - **时间解耦**：输入历史观测（h=4），采用绝对时间编码（γ(t)，表示在整段任务中的时间索引）+ 相对位置编码（PE(k)），通过 Transformer 编码器输出连续潜在动作 Zt ∈ ℝ⁵¹²。
- **FDM**：从 Zt 和 IDM 最后隐藏状态预测下一帧观测 Ôₜ₊₁，提供无监督重建损失，使模型从未标记视频中学习世界动态。
- **动作解码器**：轻量 MLP 将 Zt 映射为连续机器人动作 âₜ ∈ ℝ¹⁴，仅在有标签数据上训练。
- **联合优化**：L_TA-LAM = L_recon + λ_act·L_action，其中 L_recon 对所有数据（含无标签视频）计算 MSE，L_action 仅对有标签数据计算 MSE。

#### Stage 2：Latent Action Diffusion Transformer (LADT)
- 使用冻结的 TA-LAM IDM 将标注轨迹编码为连续潜在动作序列。
- 条件：历史观测、历史潜在动作、本体感觉、语言指令。
- 预测未来 H=16 步潜在动作序列，采用标准去噪得分匹配损失。
- 在连续空间中扩散，避免离散化伪影；通过交替注入视觉和语言特征确保指令不淹没于视觉信息。

#### Stage 3：Real-World Fine-tuning
- 冻结整个 TA-LAM（IDM + Action Decoder），仅微调 LADT。
- 在 1600 小时新演示数据（D_finetune）上最小化解码动作与专家动作间的 MSE。
- 保持预训练表示通用性，实现高效样本适应。

## 3. 实验设计

### 数据集与场景
- **预训练数据集 D_pretrain**：来自 52 个来源的超 250 万序列，包括：
  - 无标签人类视频（学习视觉动态）
  - 有标签多本体机器人轨迹（RDT-1B 语料）
  - 仿真数据
  - 真实世界大规模演示（AgiBot World Beta 和 LatentVLA-Dexterous ~50 万序列）
- **微调数据集 D_finetune**：1600 小时 8 个特定双手任务的新演示。

### 基准（Benchmark）
- **真实世界**：8 个挑战性双手任务（如折布、折毛巾、拿瓶等）。
- **仿真**：
  - RoboTwin 1.0（复杂双手协调）
  - SIMPLER（长时域操作）
  - CALVIN（ABC→D 组合泛化）

### 对比方法
- RDT-1B（重新训练）
- LAPA（重新训练）
- 其他：DP、DP3、Octo、OpenVLA、RT-2、Moto、RoboFlamingo、GR-1 等。

### 评估指标
- 成功率（Success Rate）
- 平均长度（CALVIN）
- 零样本泛化测试（光照、背景、新物体、动态干扰）
- 少样本适应（新物体仅 20 个演示）

## 4. 资源与算力

- 训练集群：112 块 NVIDIA A100-80G GPU。
- 预训练阶段（Stage 1 & 2）耗时约一个月（～30 天）。
- 微调阶段（Stage 3）：100,000 次迭代。
- 少样本适应：在单块 A100 GPU 上微调不到 1 小时。

## 5. 实验数量与充分性

实验充分且系统，主要分为：
- **真实世界主实验**：8 个任务，对比 RDT-1B 和 LAPA，报告平均成功率及单任务结果。
- **仿真基准实验**：3 个 Benchmark（RoboTwin 1.0 包含 9 个子任务，SIMPLER，CALVIN）。
- **零样本泛化实验**：多干扰环境（光照变化等），定性/定量结果。
- **数据缩放实验**：两种消融（1）在原始 1.3M RDT-1B 数据上从头训练；（2）用 1600 小时数据微调 RDT-1B 再对比。
- **少样本适应实验**：仅 20 个演示（折叠长毛巾）。
- **消融实验**：在折毛巾任务上进行了 5 种变体消融，包括 VQ-VAE+GPT、无绝对时间编码、仅视频训练、无语言注意力、无历史潜在条件，并对比了重新训练的 RDT-1B 和 LAPA。

公平性：所有基线均在相同完整数据集上重新训练，严格控制变量。实验覆盖了真实与仿真、不同任务类型、不同干扰条件、样本效率等维度，结论可信。

## 6. 主要结论与发现

1. **性能领先**：真实世界 8 个双手任务平均成功率 63.8%，优于 RDT-1B（51.0%）和 LAPA（15.3%），尤其对可变形物体（折布 47% vs. 7%）提升显著。
2. **零样本泛化**：在多重干扰（光照变化等）下仍保持 56% 成功率，而离散化基线出现灾难性失败。
3. **少样本高效**：仅 20 个演示（长毛巾折叠）达到 61% 成功率，优于 RDT-1B（34%），验证了冻结预训练表示仅微调规划器的策略有效性。
4. **架构优势**：消融表明连续扩散规划器（-33%）和绝对时间编码（-27%）是最关键组件，语言引导注意力（-19%）与联合训练（-23%）也至关重要。
5. **数据缩放能力**：在等量数据下，LatentVLA 仍优于 RDT-1B 5.6%；使用 2.5M 数据后进一步拉开 7.2% 差距，证明架构可更有效利用大规模数据。

## 7. 优点

- **方法论创新**：首次系统性地同时解决三个关键失效模式（时序纠缠、离散化伪影、任务无关动态敏感），通过连续潜在表示+时空锚定+语言注意力实现。
- **三阶段解耦设计**：将表示学习、规划、执行分离，使各部分可独立优化，便于大规模预训练和高效微调。
- **实验全面且公平**：涵盖真实与仿真、多种干扰、少样本适应，基线重新训练，消融实验设计清晰。
- **资源信息充分**：明确报告了 GPU 数量和训练时长，可复现性强。
- **实际落地潜力**：在可变形物体、动态干扰等复杂真实场景下仍表现优异，零样本和少样本能力突出。

## 8. 不足与局限

- **感知局限**：对完全遮挡物体和高反光表面敏感（论文明确提及）。
- **计算开销**：预训练需要 112 块 A100 运行约一个月，资源要求较高，可能限制小团队复现。
- **任务覆盖**：虽包含 8 个真实任务，但均为桌面双手操作，未见移动操作、灵巧手精细操作等场景验证。
- **评价指标单一**：主要采用成功率，缺少任务完成时间、轨迹平滑度、能耗等其他维度。
- **未做误差累积分析**：长时域任务中连续潜在扩散是否累积误差未深入研究。
- **无标签视频利用效果**：消融表明仅视频训练（-23%）比有联合训练差，但未单独分析各类无标签数据（人类视频 vs 机器人视频）的贡献。

（完）
