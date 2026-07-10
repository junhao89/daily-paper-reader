---
title: "WorldAgen: Unified State-Action Prediction with Test-Time World Model Training"
title_zh: WorldAgen：具备测试时世界模型训练的统一状态-动作预测
authors: "Chi Wan, Kangrui Wang, Yuan Si, Pingyue Zhang, Manling Li"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38925/42887"
tags: ["query:wam-vla"]
score: 9.0
evidence: 统一的VLA与世界模型，具备测试时训练以适应新环境
tldr: 现有VLA模型在静态数据集上预训练后难以适应动态新环境。本文提出WorldAgen，一个统一框架，通过共享Transformer主干和两个预测头（世界模型头和动作预测头）联合学习状态预测与动作生成，并引入测试时训练（TTT）机制，使模型能在推理时快速适应环境变化。实验显示该方法在多种操控和导航任务上泛化能力显著提升。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38925/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1839, \"height\": 1866, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38925/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1541, \"height\": 808, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38925/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 753, \"height\": 799, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38925/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 882, \"height\": 390, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38925/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1673, \"height\": 952, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38925/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 745, \"height\": 283, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38925/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 454, \"height\": 303, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38925/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 646, \"height\": 265, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38925/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 735, \"height\": 181, \"label\": \"Table\"}]"
motivation: 现有VLA模型缺乏适应新环境动态变化的机制，泛化能力有限。
method: 提出WorldAgen，共享Transformer主干联合学习世界模型与动作预测，并引入测试时训练实现快速适应。
result: 在多个操控和导航任务上，模型泛化能力显著优于基线。
conclusion: 测试时世界模型训练是提升VLA模型适应性的有效策略。
---

## Abstract
How can vision-language-action (VLA) models adapt to new environments where world dynamics shift?
While recent research has combined world modeling and action prediction to improve VLA performance, existing methods largely rely on pretraining in static datasets, without mechanisms for active adaptation to new environments. As a result, these models often fail to generalize when deployed in unseen scenarios with novel object configurations or dynamics.

We present WorldAgen, a unified framework that jointly learns world modeling and action prediction while enabling test-time training (TTT) to adapt to new environments. WorldAgen employs a shared Transformer backbone with two heads: (1) a world-model head that predicts future states from past state-action trajectories, and (2) an agent-model head that predicts actions conditioned on task instructions. During test time, WorldAgen samples exploratory actions, collects ground-truth state transitions, and performs lightweight TTT updates to refine its world model. This adaptation improves the model's understanding to the environments and leads to more accurate action predictions.

Experiments on the CALVIN and LIBERO benchmarks demonstrate that our baseline model achieves comparable, and in some cases superior, performance to current state-of-the-art approaches. Moreover, with TTT on a small number of samples, our method surpasses existing state-of-the-art models, highlighting the effectiveness of adapting world models at inference time.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：当前视觉-语言-动作（VLA）模型在静态数据集上预训练后，缺乏主动适应新环境动态变化的机制。当部署到具有新物体布局、光照条件或物理属性的未见场景时，模型泛化能力严重下降。
- **核心问题**：如何使VLA模型在测试时（inference time）能够在线调整其对环境动态的理解，从而鲁棒地适应分布偏移？
- **整体含义**：本文提出VLA模型应具备“测试时训练”能力，将世界模型从被动的预训练目标转变为主动的测试时适应机制，从而弥合离线训练与真实世界泛化之间的鸿沟。

## 2. 方法论：核心思想、关键技术细节、公式/算法

### 2.1 核心思想
- WorldAgen 采用共享的Transformer主干，联合学习两个任务：
  - **世界模型头（World Model Head）**：根据历史状态-动作轨迹预测未来的状态（观测）。
  - **智能体模型头（Agent Model Head）**：根据任务指令预测当前应执行的动作。
- 在测试时，通过**轻量级测试时训练（Test-Time Training, TTT）**，仅对世界模型部分进行LoRA微调，使模型快速适应新环境，进而间接提升动作预测的准确性。

### 2.2 关键技术细节
- **任务形式化**：将问题建模为部分可观测马尔可夫决策过程（POMDP）。每个轨迹被划分为一系列轨迹单元 \(U_i = (G_i, O_i, S_i, A_i, A_p, O_p)\)，其中 \(G_i\) 为任务指令，\(O_i\) 为观测块，\(S_i\) 为机器人状态块，\(A_i\) 为动作块，\(A_p\) 和 \(O_p\) 分别为动作和观测占位符。
- **联合状态-动作预测**：
  - 智能体模型：\(\hat{A}_i \sim p_a(\hat{A}_i | G_i, O_i, S_i, h^a_i)\)
  - 世界模型：\(\hat{O}_i \sim p_w(\hat{O}_i | O_i, S_i, \hat{A}_i, h^w_i)\)
  - 两个模型共享同一Transformer主干，但通过**混合单向注意力掩码（Mixed Unidirectional Attention Mask）** 防止信息泄露：本地掩码禁止当前单元内动作块访问占位符；全局掩码禁止世界模型头观察任务指令，保证世界模型的任务无关性。
- **轨迹分割与动作分块**：支持不同的观测块长度 \(n\) 和动作块长度 \(m\)，通过均匀子采样构建观测/状态块，减少视觉冗余。例如 \(n=2, m=3\) 时，每个轨迹单元包含2个观测步、3个动作步。
- **两阶段测试时训练（TTT）**：
  - **阶段1：随机探索**：在新环境中执行无任务指令的随机轨迹，收集 \((O_i, S_i, A_i)\) 三元组，并用“no-lang”标记替换语言指令。
  - **阶段2：世界模型适应**：仅对共享主干进行LoRA微调（作用于注意力投影和MLP投影层），冻结智能体模型头，更新规则为 \(\vartheta_w \leftarrow \vartheta_w - \eta \nabla_{\vartheta_w} \mathcal{L}_o\)，其中 \(\mathcal{L}_o\) 为观测预测损失。
- **预训练损失**：\(\mathcal{L} = \mathcal{L}_a + \varepsilon \mathcal{L}_o\)，其中 \(\mathcal{L}_a\) 为动作交叉熵，\(\mathcal{L}_o\) 为观测重建损失，\(\varepsilon\) 为平衡权重。训练时使用教师强制（teacher forcing）。

## 3. 实验设计：数据集、Benchmark、对比方法

- **数据集**：
  - **CALVIN**：模拟长时域语言条件机器人操控基准，要求代理按序理解一连串无约束的语言指令完成5个连续任务。
  - **LIBERO**：机器人终身学习基准，包含多种操控任务，强调跨任务知识迁移。
- **对比方法**：
  - CALVIN上对比：RoboFlamingo、SuSIE、GR-1、3D Diffusor Actor、CLOVER、Seer、Seer-Large。
  - LIBERO上对比：MT-ACT、MVP、MPL、OpenVLA、Seer。
- **评估指标**：任务成功率（%）和平均成功序列长度（CALVIN的ASL）。

## 4. 资源与算力

- 文中明确提及：测试时训练（包括采样和LoRA适应）可在单张 **RTX 4090 GPU** 上运行，CALVIN每个任务约需 **8分钟**，LIBERO每个场景约需 **2分钟**。
- 预训练阶段和具体总GPU数量、训练时长未说明。模型使用Qwen3作为Transformer主干，具体配置未详述。

## 5. 实验数量与充分性

- **主要实验**：在CALVIN（表1）和LIBERO（表2）两个基准上，对比了多种SOTA方法，每个基准报告了多项子任务或连续任务的成功率。
- **消融实验**（正文及附录）：
  - 世界模型贡献：有/无图像预测，平均成功率提升30%~67%。
  - LoRA秩的影响（16/32/64/128/256）：变化幅度很小（±0.02），表明对超参数不敏感。
  - TTT数据量的影响（6/90/204/340样本）：适当增加数据提升性能，但过多（340）轻微下降，存在过拟合。
  - 模拟真实噪声：添加高斯噪声后，TTT仍优于基线，验证鲁棒性。
- **补充实验**（附录提及）：轨迹分割方式、图像/动作块长度、不同Transformer主干、全微调 vs LoRA、TTT前后可视化等。
- **充分性评估**：实验覆盖两个主流基准，对比了多个强基线，消融实验系统考察了关键组件（世界模型、LoRA、数据量、噪声）。**但缺乏真实机器人验证**，仅通过模拟噪声模拟真实条件。总体实验设计较充分、客观公平。

## 6. 论文的主要结论与发现

- 联合世界建模与动作预测能显著提升VLA模型性能（带世界模型比不带世界模型，CALVIN ASL从2.96→3.87，LIBERO成功率从46.5%→78.0%）。
- 测试时训练（TTT）进一步改进环境理解，使WorldAgen在CALVIN和LIBERO上均超越现有SOTA。
- 轻量级LoRA微调世界模型即可有效适应新环境，无需修改任务策略头。
- 适当的TTT数据量是关键，过多数据可能过拟合于图像生成而非动作预测。
- 模型对LoRA秩不敏感，鲁棒性强。

## 7. 优点

- **创新性**：首次将测试时训练引入VLA联合模型，将世界模型从静态预训练变为在线适应信号，概念新颖。
- **方法简洁性**：共享主干+双头架构，结合单向注意力掩码实现任务分离，设计轻巧有效。
- **效率**：TTT仅需LoRA微调世界模型，可在消费级GPU上快速完成，适合实际部署。
- **实验全面**：在两大标准基准上验证，消融实验覆盖多个超参数和变体，结果一致。
- **鲁棒性验证**：通过添加高斯噪声模拟真实环境扰动，证明方法对感知噪声具有较好的容忍度。

## 8. 不足与局限

- **模型规模单一**：仅测试了基于Qwen3的一种模型架构和大小，未探索更大或不同架构的适用性。
- **TTT仅作用于世界模型**：未尝试联合优化动作预测和世界模型，可能限制了适应上限。
- **缺乏真实机器人实验**：所有评估均在模拟中进行（CALVIN/LIBERO），虽加了噪声模拟，但真实物理世界中的硬件延迟、控制噪声、感知噪声等复合因素未被覆盖。
- **TTT的泛化代价**：TTT需要额外进行随机探索和微调，对实时性敏感的任务可能存在瓶颈（每次部署需数分钟）。
- **潜在偏差**：CALVIN和LIBERO均为桌面操控场景，结论是否适用于更复杂的环境（如移动操作、动态物体）有待验证。

（完）
