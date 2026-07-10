---
title: "MIRTH: Mutual-Information Reasoning with Temporal Hubs for Vision-Language-Action Agents"
title_zh: "MIRTH: 面向视觉-语言-动作代理的互信息推理与时序中枢"
authors: "Hao Sun, Yu Song, Teng Shiyu, Ziwei Niu, Yen-wei Chen"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.1016.pdf"
tags: ["query:latent-vla"]
score: 7.0
evidence: 面向未来感知VLA的潜在推理标记与时序记忆
tldr: 提出MIRTH框架，通过双尺度时序记忆压缩长期场景演化和短期运动趋势，并引入由互信息优化的潜在推理标记，弥补单帧VLA架构的时间短视和指令-动作推理鸿沟，提升推理效率。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 783, \"height\": 481, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1649, \"height\": 432, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 807, \"height\": 682, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 785, \"height\": 571, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1638, \"height\": 865, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1643, \"height\": 368, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1318, \"height\": 720, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1654, \"height\": 736, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 811, \"height\": 438, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1281, \"height\": 286, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 692, \"height\": 342, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 722, \"height\": 216, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 611, \"height\": 179, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 664, \"height\": 288, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1582, \"height\": 1298, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1576, \"height\": 479, \"label\": \"Table\"}]"
motivation: 当前单帧VLA架构忽略历史动态，存在高层指令到底层动作的推理间隙和推理效率低下。
method: 设计双尺度时序记忆中枢和潜在推理标记，以互信息优化推理信号。
result: 改善了时序推理能力和推理效率。
conclusion: 潜在推理标记和时序记忆是增强VLA能力的关键组件。
---

## Abstract
VLA models have emerged as a powerful paradigm for transferring semantic knowledge from web-scale data to physical robotic control. However, current single-frame architectures suffer from intrinsic limitations: temporal myopia that discards historical dynamics, reasoning gaps between high-level instructions and low-level motor commands, and inference inefficiency due to autoregressive scalar decoding. In this work, we propose MIRTH, a unified framework designed to address these challenges. MIRTH augments a pretrained VLA backbone with three key innovations: (1) dual-scale temporal memory hubs that compress long-term scene evolution and short-term motion trends into compact embeddings; (2) latent reasoning tokens optimized via a mutual-information objective carving out a semantic plan space to align multimodal context with action trajectories; and (3) a parallel action decoding scheme that replaces autoregressive generation with vector-wise prediction to maximize control throughput. Extensive evaluations on the LIBERO simulation benchmark and a real-world LeRobot platform demonstrate that MIRTH achieves state-of-the-art performance and exhibiting emergent error recovery capabilities. We will release our code and collected datasets to facilitate reproducible research in embodied AI upon publication.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

当前视觉-语言-动作（VLA）模型虽能将大规模多模态语义知识迁移至物理机器人控制，但主流单帧架构存在三大固有限制：
- **时间短视（Temporal Myopia）**：仅依赖当前观测和指令，丢弃历史动态信息，导致物体被遮挡时丢失跟踪，动作漂移甚至失败。
- **推理鸿沟（Reasoning Gap）**：高层语言指令与低层动作命令之间缺乏显式规划，直接映射或离散动作词汇易产生歧义（多对一问题），且缺少可解释性。
- **推理效率瓶颈（Inference Inefficiency）**：自回归式标量量化解码产生冗长token序列，延迟高，难以满足实时高频控制需求。

论文旨在统一解决上述挑战，提出**MIRTH**（Mutual-Information Reasoning with Temporal Hubs）框架，增强预训练VLA主干的能力。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
在预训练VLA骨干基础上，引入**三个关键创新**：
- **双尺度时序记忆中枢**：压缩长期场景演化与短期运动趋势为固定长度嵌入，避免上下文窗口膨胀。
- **潜在推理令牌**：通过互信息最大化解耦出语义计划空间，对齐多模态上下文与动作轨迹，无需文本监督。
- **并行动作解码**：用向量级预测替代标量级自回归生成，提升控制吞吐量。

### 关键技术细节
#### (1) 双尺度时序记忆中枢
- **工作空间（长期）记忆中枢**：使用K个指数移动平均（EMA）维护不同衰减率的历史特征图 \(M_{t,k}\)，并通过MLP预测每块混合权重 \(\alpha_t\)，融合多尺度记忆和投影后的运动统计量（一阶速度 \(\mu_t\)、二阶变化率 \(\sigma_t^2\)）。
- **短期记忆中枢**：维护最近w帧队列，通过当前帧查询，计算时间注意力（含近期偏置 \(\gamma^j\)），加权求和得短时记忆。
- **融合**：通过可学习的sigmoid门控融合长短期记忆，再经由**前缀（Prefix）** 或**注入（Infusion）** 方式接入VLM主干。前缀方式性能更优（95.3% vs 93.1%），但注入方式吞吐更高（70 Hz vs 64.4 Hz）。

#### (2) 潜在推理令牌与互信息目标
- 在输入序列中插入可学习的推理令牌 \(L_t^{\text{reas}}\)，位于上下文（视觉+指令）与动作令牌之间。
- 提取三个池化表示：推理表示 \(z^R_i\)、动作表示 \(z^A_i\)、上下文表示 \(z^X_i\)，投影至对比空间。
- 使用InfoNCE损失最大化推理表示与动作表示、推理表示与上下文表示之间的互信息：
  \[
  \mathcal{L}_{\text{mi}} = \lambda_{ra} \mathcal{L}_{ra} + \lambda_{rx} \mathcal{L}_{rx}
  \]
  其中 \(\mathcal{L}_{ra}\) 拉近推理与动作，\(\mathcal{L}_{rx}\) 拉近推理与上下文，无文本监督。

#### (3) 并行动作解码
- 每个动作向量用一个令牌表示（而不是每个自由度一个令牌），隐藏状态经轻量级两层投影头映射为连续动作。
- 全部动作令牌并行解码，单次前向预测一个动作块（chunk size=10），消除自回归开销。
- 训练采用L1回归损失 \(\mathcal{L}_{l1}\) 加互信息正则项 \(\lambda_{\text{mi}} \mathcal{L}_{\text{MI}}\)。

## 3. 实验设计

### 数据集/场景
- **LIBERO仿真基准**：包括四个子套件（Spatial, Object, Goal, Long），每个10个任务，共40个任务。评价随机初始化，每任务30次滚动，报告平均成功率。
- **LeRobot真实平台**：单臂机械臂，含1000条专家轨迹（126个任务，分5组：基本操作、机构操作、场景重排、类别推理、语义配方），外加未见指令泛化验证（每组1个）。每任务30次试验。

### 对比方法
- **LIBERO仿真**：Diffusion Policy、Octo、OpenVLA、OpenVLA-OFT。
- **LeRobot真实**：Diffusion Policy、OpenVLA、Octo（图3画了对比）。

## 4. 资源与算力

论文明确说明：
- 训练硬件：**2块NVIDIA RTX Pro 6000 GPU**，混合精度训练。
- 全局批大小64（每GPU 32）。
- 训练时长：**约5天**训练单个模型。
- 模型总参数量约**80.2亿**，可训练参数约**4.82亿**（6.01%），通过LoRA高效微调。

## 5. 实验数量与充分性

### 实验组数
- **LIBERO仿真**：4个套件，每套件10任务，共500次评估（不同seed），结果取平均±标准差。
- **LeRobot真实**：5个任务组，每任务30次试验。
- **消融实验**：在LIBERO-Long上进行，包括：
  - 去除work空间记忆、短期记忆、同时去除（单帧基线）
  - 去除MI损失、去除推理令牌
  - 记忆集成策略（前缀vs注入）
  - 解码范式对比（4种）
  - 动作块大小影响（1~30）
  - 注意力掩码类型（全因果 vs 混合）
- **时间接地分析**：线性探测运动状态/速度、帧顺序打乱敏感性。
- **潜在推理分析**：t-SNE可视化推理令牌语义聚类、错误恢复率比较。

### 充分性与公平性
- 实验覆盖仿真和真实场景，任务难度多样（从短时到长时、从原子操作到语义推理）。
- 消融系统全面，验证每个组件贡献。
- 对比方法均采用已发表SOTA开源模型，结果引用文献或有复现。
- 重复30次取平均，报告标准差，统计可靠。

## 6. 主要结论与发现

- MIRTH在LIBERO四个套件上均达到**最高成功率**（平均98.1%），尤其在LIBERO-Long（95.3%）显著优于单帧基线（OpenVLA仅53.7%）。
- 在LeRobot真实任务中，随难度增加优势扩大，尤其在复杂推理任务上大幅超越对比方法。
- **时序记忆中枢**使模型能追踪运动动态，线性探测速度估计误差（MAE）从0.15降至0.04；帧顺序打乱导致成功率下降7.3%，证实因果结构利用。
- **潜在推理令牌**自发形成语义聚类（如开容器与放置任务分离），且赋予模型**12.1%的错误恢复率**，远高于基线（5.2~8.7%），体现闭环比额重规划能力。
- **并行动作解码**显著提升吞吐量（10块时达62 Hz），且全局拼接向量解码收敛最快。

## 7. 优点

- **方法创新集成**：统一解决时间短视、推理鸿沟、效率瓶颈三大问题，而非孤立改进。
- **数据高效**：仅需50条示范/任务，无需文本中间标注，互信息目标自监督。
- **推理效率高**：不使用视频Transformer（避免二次复杂度），通过压缩记忆和并行解码实现接近实时控制（62~70 Hz）。
- **泛化与鲁棒性**：在未见指令、物体重排、故障恢复上表现突出；t-SNE可视化显示推理令牌语义结构化。
- **开源可复现**：代码和数据集将发布；提供详细实验设置、超参数、硬件配置。

## 8. 不足与局限

- **可解释性有限**：潜在推理令牌非人可读（对比文本CoT），调试困难。
- **具身范围局限**：仅验证于固定单臂操控，未拓展至双臂、移动机器人。
- **记忆容量受限**：长期记忆仍受固定压缩大小限制，超过数百步的极长任务可能发生灾难性遗忘。
- **真实场景覆盖**：仅一个真实平台（LeRobot单臂），物体种类和场景多样性有限，可能高估泛化。
- **安全与伦理**：并行解码提升响应但仍可能产生不可预测行为；预训练VLM可能继承社会偏见；强调需硬件级紧急停止与人类监督。

（完）
