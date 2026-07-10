---
title: "MIRTH: Mutual-Information Reasoning with Temporal Hubs for Vision-Language-Action Agents"
title_zh: MIRTH：基于互信息推理与时间枢纽的视觉-语言-动作智能体
authors: "Hao Sun, Yu Song, Teng Shiyu, Ziwei Niu, Yen-wei Chen"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.1016.pdf"
tags: ["query:latent-vla"]
score: 8.0
evidence: 潜在推理令牌和时间记忆枢纽实现紧凑的VLA表示
tldr: 针对视觉-语言-动作模型存在的时序短视、指令推理鸿沟和推理效率低下的问题，MIRTH框架提出双尺度时间记忆枢纽将长期场景演变和短期运动趋势压缩为紧凑嵌入，并通过互信息优化潜在推理令牌，从而弥合高层指令与低层动作之间的差距，显著提升动作预测的准确性和推理效率。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 783, \"height\": 481, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1649, \"height\": 432, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 807, \"height\": 682, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 785, \"height\": 571, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1638, \"height\": 865, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1643, \"height\": 368, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1318, \"height\": 720, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1016/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1654, \"height\": 736, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 811, \"height\": 438, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1281, \"height\": 286, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 692, \"height\": 342, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 722, \"height\": 216, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 611, \"height\": 179, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 664, \"height\": 288, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1582, \"height\": 1298, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1016/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1576, \"height\": 479, \"label\": \"Table\"}]"
motivation: 现有VLA模型受限于单帧架构，存在时序信息丢失、指令推理鸿沟和推理效率低下等问题。
method: 提出MIRTH框架，包含双尺度时间记忆枢纽和互信息优化的潜在推理令牌。
result: 在多个基准任务上验证了MIRTH有效提升动作预测准确性和推理速度。
conclusion: MIRTH通过时序压缩和潜在推理增强了VLA智能体的长期规划能力。
---

## Abstract
VLA models have emerged as a powerful paradigm for transferring semantic knowledge from web-scale data to physical robotic control. However, current single-frame architectures suffer from intrinsic limitations: temporal myopia that discards historical dynamics, reasoning gaps between high-level instructions and low-level motor commands, and inference inefficiency due to autoregressive scalar decoding. In this work, we propose MIRTH, a unified framework designed to address these challenges. MIRTH augments a pretrained VLA backbone with three key innovations: (1) dual-scale temporal memory hubs that compress long-term scene evolution and short-term motion trends into compact embeddings; (2) latent reasoning tokens optimized via a mutual-information objective carving out a semantic plan space to align multimodal context with action trajectories; and (3) a parallel action decoding scheme that replaces autoregressive generation with vector-wise prediction to maximize control throughput. Extensive evaluations on the LIBERO simulation benchmark and a real-world LeRobot platform demonstrate that MIRTH achieves state-of-the-art performance and exhibiting emergent error recovery capabilities. We will release our code and collected datasets to facilitate reproducible research in embodied AI upon publication.

---

## 论文详细总结（自动生成）

# 论文总结：MIRTH：基于互信息推理与时间枢纽的视觉-语言-动作智能体

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：当前视觉-语言-动作（VLA）模型在将网络知识迁移到物理机器人控制方面表现出巨大潜力，但主流单帧架构存在三大根本性限制：
  - **时序短视（Temporal Myopia）**：仅依赖当前观测，丢弃历史动态信息，导致物体遮挡后丢失状态、动作漂移，无法完成长时任务。
  - **推理鸿沟（Reasoning Gap）**：高层语言指令与底层运动命令之间存在语义鸿沟。已有方法要么直接映射（缺乏可解释性和内部规划），要么依赖离散动作语言（存在多对一歧义，且手工标注成本高）。
  - **推理效率低下（Inference Inefficiency）**：现有VLA采用自回归标量量化解码，每个动作维度需生成一个离散token，导致长序列低延迟，难以满足实时高频控制需求。
- **整体含义**：本文提出MIRTH统一框架，通过显式结构化的记忆、潜在推理和并行解码，同时解决上述三个问题，既保持VLM语义丰富性，又实现历史感知的高效精确控制。

## 2. 方法论：核心思想与关键技术细节

### 2.1 核心思想
在预训练VLA骨干（如OpenVLA）基础上，增加三个创新模块：
1. **双尺度时间记忆枢纽**：压缩长期场景布局和短期运动趋势为固定长度嵌入，避免上下文窗口膨胀。
2. **互信息优化的潜在推理令牌**：学习一组可训练的推理令牌，通过互信息最大化目标，使其成为连接多模态上下文与动作轨迹的紧凑语义规划空间，无需显式文本监督。
3. **并行动作解码**：将标量自回归解码替换为向量级并行预测，大幅提升吞吐量。

### 2.2 关键技术细节

#### (1) 时间记忆枢纽
- **工作空间（长期）记忆枢纽**：对每个补丁（patch）维护K个指数移动平均（EMA），衰减率βk对数分布在0.01~0.3之间。同时跟踪一阶和二阶运动统计量（速度μt、变化率σt²），通过小MLP预测每个补丁在多尺度上的混合权重，得到融合后的长期特征图 \(M^{work}_t\)。
- **短时（短期）记忆枢纽**：维护最近w帧的队列，对当前帧查询与过去帧键值做带有温度缩放和近因偏置的注意力聚合，得到 \(M^{short}_t\)。
- **融合**：基于sigmoid门控融合两个枢纽的输出。
- **集成策略**：有两种方式——**前缀式**（将记忆令牌展平后作为视觉前缀）和**注入式**（通过线性层直接调制当前帧补丁嵌入）。实验采用前缀式，性能更优。

#### (2) 潜在推理令牌与互信息目标
- 在输入序列中插入m个可学习推理令牌 \(T^{reas}_t\)，位于视觉/语言上下文与动作令牌之间。
- 从语言骨干输出隐藏状态中提取三个表示：推理表示 \(z^R\)、动作表示 \(z^A\)、上下文表示 \(z^X\)。
- 使用InfoNCE损失最大化推理表示与动作表示、上下文表示之间的互信息：
  - \(\mathcal{L}_{ra}\)：推理与动作的对比损失
  - \(\mathcal{L}_{rx}\)：推理与上下文的对比损失
  - 总损失 \(\mathcal{L}_{mi} = \lambda_{ra}\mathcal{L}_{ra} + \lambda_{rx}\mathcal{L}_{rx}\)
- 推理令牌成为连接感知与动作的潜在桥梁，无需任何文本链式推理或手工标签。

#### (3) 并行动作解码与训练目标
- 每个动作向量对应一个动作令牌位置，而非每个自由度一个令牌，大幅减少序列长度。
- 所有动作令牌隐藏状态通过轻量级两层投影头直接并行映射为连续动作向量。
- 训练目标：\(\mathcal{L} = \mathcal{L}_{l1} + \lambda_{mi}\mathcal{L}_{mi}\)，其中 \(\mathcal{L}_{l1}\) 为动作回归的L1损失。

#### (4) 公式与算法流程（文字说明）
- 记忆更新：EMA公式（1），运动统计更新（2），混合权重预测（3），记忆融合（4）。
- 短时注意力：公式（5）~（8）。
- 融合门控：公式（9）。
- 推理损失：公式（13）~（15）。
- 整体训练目标：公式（16）~（17）。

## 3. 实验设计

### 3.1 数据集与基准
- **LIBERO仿真基准**：包含Spatial、Object、Goal、Long四个套件，每个套件10个任务，评估长时鲁棒性和泛化能力。数据预处理：过滤静态帧、resize至224×224，垂直翻转增强。
- **LeRobot真实机器人平台**：物理单臂操作器，带腕部摄像头和主摄像头。收集126个不同任务，每个任务3组专家演示，共约1000条轨迹，分为5个难度组（基础操作、机构操作、场景重排、类别推理、语义配方）。评估时每个任务30次随机初始化。

### 3.2 对比方法
- **扩散策略（Diffusion Policy）**、**Octo**、**OpenVLA**、**OpenVLA-OFT**（带微调）。
- 消融实验中去除：工作空间记忆、短时记忆、推理令牌、MI损失。

### 3.3 评估指标
- 主要指标：任务成功率（每个任务30次试验，取平均）。
- 效率指标：推理吞吐量（Hz）。
- 其他分析：线性探测预测状态/速度的MAE、帧顺序打乱后的成功率、错误恢复率等。

## 4. 资源与算力

- **GPU**：2块 NVIDIA RTX Pro 6000 GPU，全局batch size 64（每卡32）。
- **训练时长**：约5天。
- **模型规模**：总参数量约80.2亿，仅约4.823亿（6.01%）可训练。其中：
  - LLM骨干通过LoRA（rank=32）更新7995万参数。
  - 视觉骨干（7.309亿）和主视觉投影器冻结。
  - 动作解码器2857.4万参数（占可训练参数最大比例59.24%）。
  - 记忆枢纽4686万参数，推理令牌5296万参数。
- **说明**：文中明确给出了GPU型号、数量、训练时长及参数量分布。

## 5. 实验数量与充分性

- **实验组数**：
  - 主对比实验：在LIBERO四个套件和LeRobot五个任务组上，每个条件30次试验，共约（4+5）×30×方法数。对比了5种方法，结果统计。
  - 消融实验：在LIBERO-Long上，去除记忆枢纽（3种）、去除推理（2种），共5组。
  - 时序接地分析：线性探测（两个目标）+ 帧顺序打乱（1组）。
  - 潜在推理分析：t-SNE可视化（1组）+ 错误恢复率比较（3种条件）。
  - 记忆集成策略比较：Prefix vs Infusion（1组）。
  - 解码范式比较（附录F）：4种范式在LIBERO-Object上收敛曲线。
  - 动作块大小消融（附录G）：5种尺寸，吞吐量和成功率。
  - 注意力掩码比较（附录H）：2种策略。
- **充分性评价**：
  - 实验覆盖了仿真和真实平台，任务从简单到复杂，含长时、推理、错误恢复等多维度评估。
  - 消融实验系统性地验证每个组件贡献。
  - 公平性：与其他方法的比较基于公开结果或标准设置；基线包括强基准OpenVLA-OFT等。
  - 不足：真实机器人评估仅在单一单臂平台上，未扩展到双机械臂或移动机器人；实验中仅采用30次试验，统计稳定性稍弱（可接受）。

## 6. 主要结论与发现

- MIRTH在LIBERO上达到98.1%平均成功率，远超单帧或简单多帧基线，尤其在长期任务中优势明显（LIBERO-Long 95.3%）。
- 在LeRobot真实平台上，随着任务难度增加，MIRTH性能差距进一步扩大，在语义推理任务中显著优于对比方法。
- 消融实验证实：**双尺度记忆枢纽**和**互信息推理**均显著提升性能；去掉两个记忆枢纽导致成功下降2.1%，去掉推理令牌下降1.4%。
- 线性探测证明记忆枢纽成功编码运动速度和动态信息，单帧基线无法做到。
- 帧顺序打乱造成成功率大幅下降（7.3%），说明模型利用时序因果结构而非静态纹理。
- t-SNE显示推理令牌自动聚类出语义相似任务，且错误恢复率（12.1%）优于基线（5.2%~8.7%），说明具备重新规划能力。
- 并行解码大幅提升吞吐量（达到64.4 Hz），同时保持或提高成功率。

## 7. 优点

- **方法创新性**：
  - 双尺度记忆枢纽在不增加上下文长度前提下实现历史感知，设计精巧（EMA混合+运动统计+门控融合）。
  - 互信息驱动的潜在推理避免昂贵的文本标注，且自动形成语义规划空间。
  - 并行向量解码显著加速，是VLA实用化的重要改进。
- **实验全面性**：
  - 同时覆盖仿真和真实平台，任务类型多样（原子操作到复杂推理）。
  - 多项消融和剖析实验（时序接地、注意力掩码、解码范式等），深入揭示机制。
- **资源效率**：冻结大部分预训练权重，仅6%参数可训练，计算成本合理。
- **开源精神**：承诺发布代码和数据集，有利于复现和后续研究。

## 8. 不足与局限

- **可解释性有限**：潜在推理令牌虽有效，但不如文本Chain-of-Thought可读，调试困难。
- **实验范围局限**：仅在固定单臂操作器上验证，未涉及双机械臂、移动操作或多传感融合（如触觉）。
- **长期记忆容量受限**：记忆压缩到固定长度，极端长程任务仍可能遗忘。
- **错误恢复率较低**：虽然达到12.1%，但仍有大量失败场景无法自主恢复。
- **统计稳定性**：部分实验仅30次试验，未报告置信区间或统计检验。
- **潜在的继承偏见**：基于预训练VLM，可能继承网络数据中的社会偏见，论文已意识到但未深入分析。

（完）
