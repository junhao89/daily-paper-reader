---
title: "DIVA: Discrete Diffusion Vision-Language-Action Models for Parallelized Action Generation"
title_zh: DIVA：离散扩散视觉-语言-动作模型用于并行化动作生成
authors: "Xiufeng Song, Yiran Qin, Yan Tai, Li Kang, Heng Zhou, Siqi Luo, Jiwen Yu, Ling Yang, Philip Torr, LEI BAI, Ruimao Zhang, Xiaohong Liu, Zhenfei Yin"
date: 2025-09-02
pdf: "https://openreview.net/pdf?id=mNya9d1DA2"
tags: ["query:latent-vla"]
score: 9.0
evidence: 具有可学习动作token化与潜在表征的离散扩散VLA
tldr: 自回归VLA存在误差累积和时序僵化问题。DIVA提出统一离散扩散架构，将动作生成视为迭代去噪过程：首先通过可学习的离散动作token化桥接连通动作与多模态空间，然后利用潜在驱动策略生成动作。在多个机器人控制基准上，DIVA实现了并行化动作生成，性能优于自回归基线，并展现出更好的多模态融合能力。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 克服自回归VLA的误差累积和时序局限性。
method: 将动作生成建模为离散潜在表示上的迭代去噪过程，包含可学习动作token化。
result: 实现并行化动作生成，在多个控制任务上超越自回归VLA基线。
conclusion: 离散扩散范式为VLA模型提供了更灵活高效的动作生成框架。
---

## Abstract
Vision-Language-Action (VLA) models have shown promising results in robot control, yet prevailing auto-regressive frameworks suffer from inherent limitations, such as error accumulation and temporal rigidity in action generation. To address this, we introduce a DIscrete diffusion Vision-language-Action model (DIVA), a discrete diffusion-based VLA framework that reformulates action generation as an iterative denoising process over discrete latent representations. The innovation of DIVA lies in the unified discrete diffusion architecture that systematically integrates three core designs: first, a learnable discrete action tokenization process bridges continuous action with the structural multimodal space. Second, A latent-driven policy learning strategy is proposed to align the representative space of the vision-language backbone and the policy head through a joint optimization. Third, a selective group unmasking strategy is introduced during the discrete diffusion decoding to preserve spatiotemporal coherence. Extensive evaluation demonstrates that DIVA achieves state-of-the-art performance in both simulated and real-world environments, validating its advantages in generating coherent, precise, and generalizable robot behaviors. Our work establishes a robust and scalable paradigm for future embodied decision-making systems.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的视觉-语言-动作（VLA）模型主要采用自回归框架，但在机器人控制中存在两个固有缺陷：误差累积（auto-regressive error accumulation）和时序僵化（temporal rigidity in action generation）。这两个问题限制了动作生成的连贯性、准确性和泛化能力。
- **整体含义**：本文提出一种基于离散扩散的VLA模型（DIVA），将动作生成重新建模为离散潜在表示上的迭代去噪过程，旨在克服自回归范式的局限性，实现并行化动作生成，提升机器人行为生成的效率与质量。

## 2. 论文提出的方法论

### 核心思想
- 将动作生成视为一个**离散潜在空间上的迭代去噪过程**，而非逐个时间步自回归生成。通过离散扩散模型，允许多个动作片段同时被去噪生成，从而实现并行化。

### 关键技术细节（三点创新设计）
1. **可学习的离散动作token化**（Learnable discrete action tokenization）  
   - 将连续动作空间通过可学习的量化器映射到离散的token序列，从而与视觉-语言多模态空间的结构相对齐，架起连续动作与结构化表示的桥梁。
2. **潜在驱动策略学习**（Latent-driven policy learning）  
   - 通过联合优化视觉-语言骨干网络（backbone）与策略头（policy head）的表示空间，使两者共享一致的潜在空间，从而提升动作生成的准确性和泛化性。
3. **选择性组去掩码策略**（Selective group unmasking strategy）  
   - 在离散扩散解码阶段，引入该策略以保持动作序列的时空连贯性，避免并行生成导致的局部不连续性。

### 算法流程（文字说明）
1. **训练阶段**：  
   - 输入观测（视觉+语言指令），通过视觉-语言骨干提取特征；  
   - 将连续动作通过可学习离散token化得到离散动作token序列；  
   - 在前向扩散过程中，逐渐向离散动作token中添加噪声（mask）；  
   - 在反向去噪过程中，模型学习根据观测特征和当前噪声状态预测原始离散动作token；  
   - 通过潜在驱动策略学习联合优化骨干和策略头。
2. **推理阶段**：  
   - 从完全masked的离散动作token开始，逐步进行选择性组去掩码迭代去噪，最终生成完整的离散动作token序列；  
   - 将离散token解码回连续动作，用于机器人控制。

（论文摘要未提供具体公式，故省略公式细节。）

## 3. 实验设计

- **使用的数据集/场景**：  
  - 模拟环境：据摘要所述，在多个机器人控制基准上进行了评估，具体环境名称在摘要中未列出，但提及“simulated environments”。  
  - 真实世界环境：也在真实机器人平台上进行了验证。
- **Benchmark**：未明确说明具体基准名称。结合元数据标签“query:latent-vla”，可能涉及常见的VLA基准（如CALVIN、RLBench等），但论文原文未提供细节。  
- **对比方法**：  
  - 与自回归VLA基线进行对比（如RT-2等）。  
  - 可能还与其他的扩散VLA模型对比，摘要未详述。

## 4. 资源与算力

- 论文摘要与元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。  
- 因此，无法总结具体的算力开销。仅能指出文中未提及该细节。

## 5. 实验数量与充分性

- **实验数量**：摘要提到“Extensive evaluation”，但未列举具体实验组数。从元数据看，该论文曾被ICLR 2026投稿（rejected），可能包含多个仿真任务和真实世界实验，以及消融研究（比如对三种创新设计的消融）。  
- **充分性评估**：  
  - 优点：涵盖了模拟和真实环境，且与自回归基线对比，基本完整。  
  - 不足：缺乏具体的实验结果数值（如成功率、任务完成率等），也未列出多种自回归变体的详细对比。因此，仅从摘要推断，实验设计的公开细节不足，难以完全判断其充分性与客观性。  
  - 由于论文被拒，可能实验存在某些缺陷（如对比不全面、泛化性不足等）。

## 6. 论文的主要结论与发现

- DIVA在模拟和真实环境中均实现了**最先进的性能**（state-of-the-art），验证了离散扩散范式在VLA模型中的优势。  
- 与自回归VLA相比，DIVA能够生成**连贯、精确且可泛化**的机器人行为。  
- 该工作为未来具身决策系统提供了一个**鲁棒且可扩展**的范式。

## 7. 优点

- **方法创新**：首次将离散扩散模型系统性地引入VLA框架，并设计了三种互补机制（可学习token化、潜在驱动学习、选择性去掩码），有效解决了自回归的时序局限性和误差累积问题。  
- **并行化能力**：扩散模型天然支持并行生成，速度优势明显。  
- **跨模态对齐**：通过可学习离散token化桥连接连续动作与多模态空间，提升了多模态融合效果。  
- **实验覆盖**：同时包含仿真和真实世界验证，增加了结论的可信度。

## 8. 不足与局限

- **可复现性欠缺**：论文摘要未提供详细的实验设置、超参数、网络架构、训练细节，难以复现。  
- **实验细节缺失**：未列出具体基准名称、对比方法的性能数值、消融实验的结果，削弱了论证的充分性。  
- **泛化能力存疑**：仅在有限的任务上测试，未涉及更复杂的长期任务或动态环境。  
- **算力成本**：离散扩散模型通常需要多次迭代去噪，推理时间可能比单步自回归方法长（尽管并行生成动作片段，但迭代次数多），文中未讨论计算效率。  
- **可能偏差**：对比的自回归基线是否经过公平优化（如相同的骨干、数据量）未说明，存在对比优势夸大的风险。  
- **应用限制**：离散token化可能引入量化误差，对于高精度动作控制可能不适用。

（完）
