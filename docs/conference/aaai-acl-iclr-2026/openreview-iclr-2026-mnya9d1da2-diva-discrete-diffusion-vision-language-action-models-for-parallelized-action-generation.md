---
title: "DIVA: Discrete Diffusion Vision-Language-Action Models for Parallelized Action Generation"
title_zh: DIVA：离散扩散视觉-语言-动作模型用于并行化动作生成
authors: "Xiufeng Song, Yiran Qin, Yan Tai, Li Kang, Heng Zhou, Siqi Luo, Jiwen Yu, Ling Yang, Philip Torr, LEI BAI, Ruimao Zhang, Xiaohong Liu, Zhenfei Yin"
date: 2025-09-02
pdf: "https://openreview.net/pdf?id=mNya9d1DA2"
tags: ["query:latent-vla"]
score: 9.0
evidence: 离散动作令牌化和潜在扩散用于VLA
tldr: 该论文针对自回归VLA模型误差累积和时序刚性等固有问题，提出离散扩散VLA模型DIVA，将动作生成视为离散潜在表示上的迭代去噪过程。DIVA包含可学习的离散动作令牌化、潜在驱动策略和并行非自回归生成，在多个机器人操作任务上取得更优性能。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 自回归VLA模型存在误差累积和生成时序固定的缺陷。
method: 提出离散扩散VLA框架，将连续动作离散化为令牌，并通过潜在表示上的迭代去噪并行生成动作。
result: 在仿真和真实操作任务上比自回归基线更优。
conclusion: 离散扩散范式为VLA模型提供了一种高效且鲁棒的动作生成方案。
---

## Abstract
Vision-Language-Action (VLA) models have shown promising results in robot control, yet prevailing auto-regressive frameworks suffer from inherent limitations, such as error accumulation and temporal rigidity in action generation. To address this, we introduce a DIscrete diffusion Vision-language-Action model (DIVA), a discrete diffusion-based VLA framework that reformulates action generation as an iterative denoising process over discrete latent representations. The innovation of DIVA lies in the unified discrete diffusion architecture that systematically integrates three core designs: first, a learnable discrete action tokenization process bridges continuous action with the structural multimodal space. Second, A latent-driven policy learning strategy is proposed to align the representative space of the vision-language backbone and the policy head through a joint optimization. Third, a selective group unmasking strategy is introduced during the discrete diffusion decoding to preserve spatiotemporal coherence. Extensive evaluation demonstrates that DIVA achieves state-of-the-art performance in both simulated and real-world environments, validating its advantages in generating coherent, precise, and generalizable robot behaviors. Our work establishes a robust and scalable paradigm for future embodied decision-making systems.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：现有的视觉-语言-动作（VLA）模型大多采用自回归（auto-regressive）框架生成动作序列。该类框架存在两大固有问题：
  - **误差累积**：自回归预测中，单步动作误差会沿着序列逐步放大，导致长程任务失败。
  - **时序刚性**：自回归生成的动作时间步长固定，无法灵活调整生成节奏或并行输出。
- **动机**：克服以上缺陷，探索一种非自回归、迭代精化的动作生成范式，提升机器人控制的鲁棒性和效率。
- **整体含义**：该论文提出的离散扩散VLA模型（DIVA）将动作生成重新定义为离散潜在空间上的迭代去噪过程，为具身智能决策系统提供了可扩展、更鲁棒的新路径。

## 2. 方法论
### 核心思想
将连续动作空间离散化为令牌（tokens），利用离散扩散模型在潜在空间上逐步去噪，并行生成整个动作序列，从而消除误差累积并实现灵活的输出时序。

### 关键技术细节
1. **可学习离散动作令牌化（Learnable Discrete Action Tokenization）**
   - 将连续动作（如关节角度、末端位置）映射到离散的令牌索引，建立连续动作与结构化的多模态令牌空间之间的桥梁。
2. **潜在驱动策略学习（Latent-driven Policy Learning）**
   - 通过联合优化视觉语言骨干网络（Vision-Language backbone）和策略头（policy head）的表征空间，使二者在潜在空间中对齐，促进跨模态理解。
3. **选择性组解掩码（Selective Group Unmasking）**
   - 在离散扩散解码阶段，每次去噪时仅针对部分组（group）的令牌进行解掩码，而非全体一次性生成，从而保留动作序列的时空一致性和上下文依赖。

### 算法流程（文字说明）
- **前向过程**：对离散动作令牌序列逐步添加噪声（如通过转移矩阵或掩码），使其变为纯噪声分布。
- **反向去噪**：利用条件信息（视觉观测、语言指令）作为条件，通过训练好的扩散模型逐步预测并还原每个令牌，最终并行输出完整的离散动作序列。
- **训练目标**：估计噪声/掩码的分布，最小化预测与实际令牌之间的交叉熵损失。

## 3. 实验设计
- **使用的环境/数据集**：论文摘要提到在**仿真环境**（如机器人操作模拟器）和**真实世界**中进行评估。但未明确给出具体数据集名称（如CALVIN、MetaWorld、RLBench等），因此无法确认benchmark细节。
- **对比方法**：主要与自回归VLA基线进行对比（例如基于Transformer的自回归策略）。具体方法名称未列出。
- **评估指标**：未明确。推测包括任务成功率、动作精度、泛化迁移能力等。
- **公平性与客观性**：仅凭摘要无法判断实验是否全面；需要查看全文了解是否在不同难度、不同机器人形态上进行了评估。

## 4. 资源与算力
- **文中未明确说明**所使用的GPU型号、数量、训练时长等硬件资源。也没有提及训练时间或显存消耗等细节。

## 5. 实验数量与充分性
- 根据摘要中“Extensive evaluation”以及“State-of-the-art performance in both simulated and real-world environments”的表述，可以推测实验涵盖多个场景和任务。
- 但缺乏具体的实验数量（例如做了多少组模拟任务、多少真实场景、消融实验的组数）。因此，**实验的充分性存疑**，需要完整论文才能客观判断。
- 若仅有少量的仿真任务和单一定制真实场景，则可能不足以证明泛化性。

## 6. 主要结论与发现
- DIVA在模拟和真实世界环境中均达到了**最先进性能**（State-of-the-art）。
- 能够生成**连贯、精确、可泛化**的机器人行为，验证了离散扩散范式相比自回归范式的优势：
  - 避免误差累积；
  - 支持并行动作生成（效率更高）；
  - 在时空一致性上表现更好。

## 7. 优点
- **创新范式**：首次将离散扩散模型系统性地引入VLA框架，解决自回归的固有问题。
- **三大核心设计环环相扣**：离散令牌化、潜在对齐、选择性解掩码，既有理论创新又具实际工程价值。
- **非自回归并行生成**：理论上可大幅提升生成速度，适合实时控制。
- **结构统一性强**：将视觉、语言、动作统一在离散扩散的潜在空间中进行处理，简化了多模态融合。

## 8. 不足与局限
- **实验信息不全**：摘要中未提供详细数据集、对比基线、超参数设置等，难以评估实验的可靠性。
- **资源消耗未披露**：离散扩散模型通常需要较长的训练时间和多次迭代采样，但文中没有提及推理延迟或算力成本，实际部署可能存在效率问题。
- **泛化性证据有限**：仅提到仿真和真实环境，未说明真实场景的多样性（如不同物体、光照、背景等），可能存在过拟合风险。
- **潜在精度损失**：连续动作离散化会引入量化误差，对高精度操作任务可能不够精细。
- **缺乏与连续扩散方案的对比**：本文只与自回归基线对比，未与更直接的连续扩散动作生成方法（如扩散策略Diffusion Policy）进行横向比较，削弱了论证力度。
- **未深入讨论失败案例与鲁棒性**：如面对未见过的指令或干扰时的行为退化情况未提及。

（完）
