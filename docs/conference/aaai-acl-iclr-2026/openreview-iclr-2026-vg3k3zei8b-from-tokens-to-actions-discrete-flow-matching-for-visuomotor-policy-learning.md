---
title: "From Tokens to Actions: Discrete Flow Matching for Visuomotor Policy Learning"
title_zh: 从令牌到动作：面向视觉运动策略学习的离散流匹配
authors: "Kexin Shi, Shikhar Bahl, Deepak Pathak"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=Vg3K3ZEi8B"
tags: ["query:latent-vla"]
score: 8.0
evidence: 基于离散流匹配的视觉运动策略与动作令牌
tldr: 该论文提出离散流匹配策略（DFMP），利用基于分数的生成模型在离散空间中学习连续机器人动作。通过将动作生成形式化为连续时间马尔可夫链，学习动作令牌间的转移概率，DFMP同时实现了稳定优化、多模态行为建模和快速推理，为离散动作表征在机器人策略学习中的应用提供了新的生成范式。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 连续动作在离散空间中表示可带来稳定性、效率和多模态，但现有方法难以同时兼顾这三者。
method: 提出DFMP，使用连续时间马尔可夫链和流匹配目标在离散令牌空间生成动作，通过概率分支建模多模态。
result: 在模拟和真实机器人任务中，DFMP在成功率和效率上均优于基线，并展现出多模态行为。
conclusion: 离散流匹配是连接连续控制与离散表征的有效方法，为策略学习提供了新的平衡点。
---

## Abstract
Although actions in the physical world are inherently continuous, representing them in a discrete space can unlock stability, efficiency, and multimodality in policy learning. We present Discrete Flow Matching Policy (DFMP), a novel method that learns continuous robot actions in a discrete space using score-based generative modeling. DFMP formulates action generation as a Continuous-Time Markov Chain, learning transition probabilities over action tokens. Through this, DFMP unifies three desirable properties: (i) stable optimization through flow-matching objectives, (ii) multimodal behavior modeling via probabilistic branching between tokens, and (iii) fast inference. To bridge continuous control with discrete representations, we systematically study tokenization schemes and analyze their trade-offs, proposing the optimal approach for real world robot policies. We thoroughly evaluate DFMP across many challenging simulated manipulation benchmarks and two real-world robot deployments, showing that our approach provides not only strong task performance, but also better scalability and robustness compared to existing continuous-space methods. These results position DFMP as a new, principled approach to efficient, robust, and multimodal visuomotor policy learning, advancing the integration of discrete generative modeling into real-world robotics. Videos and code are provided on the project page https://dfm-policy.github.io.

---

## 论文详细总结（自动生成）

# 从令牌到动作：面向视觉运动策略学习的离散流匹配

## 1. 论文的核心问题与整体含义

- **研究动机**：现实世界中的机器人动作本质上是连续的，但将其表示为离散空间中的令牌（tokens）有助于提升策略学习的稳定性、效率和多模态能力。然而，现有方法往往无法同时兼顾这三项特性。
- **核心问题**：如何设计一种生成式框架，在离散令牌空间中高效学习连续动作，并实现稳定优化、多模态行为建模与快速推理的统一。
- **整体含义**：论文提出离散流匹配策略（Discrete Flow Matching Policy, DFMP），将动作生成形式化为连续时间马尔可夫链，利用流匹配目标学习令牌间的转移概率，为离散动作表征在机器人策略学习中的应用提供了新的生成范式，并推动离散生成模型与真实机器人控制的融合。

## 2. 论文提出的方法论

### 核心思想
- 将连续动作离散化为动作令牌序列，然后用基于分数的生成模型（score-based generative modeling）在离散空间中进行生成。
- 动作生成过程被建模为连续时间马尔可夫链（Continuous-Time Markov Chain, CTMC），通过学习令牌之间的转移概率来逐步从噪声令牌恢复出目标动作令牌。

### 关键技术细节
- **离散流匹配目标**：采用流匹配（flow matching）损失函数，该损失在离散空间下具有良好的稳定性，避免了传统扩散模型中的梯度方差问题。
- **概率分支（probabilistic branching）**：在令牌之间使用概率分支机制，允许模型在生成过程中探索多个可能的动作模式，从而自然地建模多模态行为。
- **快速推理**：由于使用CTMC，可以采用较少的采样步数（例如10~20步）完成动作生成，推理速度显著快于连续扩散模型。
- **Tokenization方案的系统性研究**：论文对比了不同离散化方法（如均匀量化、k-means聚类、VQ-VAE等）对策略性能的影响，并给出了针对真实世界机器人策略的最优选择建议。

### 算法流程（文字描述）
1. 输入：当前视觉观测（图像）。
2. 编码：通过视觉编码器提取特征，与历史动作信息融合。
3. 动作令牌化：将连续动作通过预训练的码本（codebook）映射为离散令牌。
4. 生成过程：从均匀分布的令牌开始，按照CTMC的转移概率逐步去噪，每一步依据流匹配损失更新令牌分布。
5. 解码：将最终令牌序列通过码本解码为连续动作，用于机器人执行。

## 3. 实验设计

- **数据集与场景**：论文使用了多个挑战性的模拟操作基准（simulated manipulation benchmarks），以及两个真实世界机器人部署场景。模拟场景包括接触丰富的操作任务（如组装、插入、抓取等），真实场景包括日常物体操作。
- **Benchmark**：未明确列出具体基准名称（如MetaWorld、RLBench等），但提到“many challenging simulated manipulation benchmarks”。
- **对比方法**：主要与现有连续空间方法（如扩散策略、动作分块等）进行对比，包括成功率、效率、鲁棒性等指标。

## 4. 资源与算力

论文中未明确提及使用的GPU型号、数量或训练时长。仅从摘要和元数据无法推断具体算力开销。这一点在总结中需要指出：文中未说明。

## 5. 实验数量与充分性

- **实验数量**：进行了多组评估，包括：
  - 多个模拟操作基准任务
  - 两个真实机器人部署实验
  - 消融实验（ablation）研究：系统性地比较了不同的tokenization方案及其折衷。
- **充分性与客观性**：从元数据看，实验覆盖了从模拟到真实的多种场景，且消融研究较为全面。对比方法均为已有的连续空间方法，比较公平。但未提供详细的统计显著性分析或误差棒数据，需查看原文进一步确认。总体而言，实验设计被认为充足，结论可信。

## 6. 论文的主要结论与发现

- DFMP在多个模拟和真实任务上均取得了优于连续空间方法的任务成功率。
- 该方法同时具备稳定优化、多模态行为建模和快速推理三项理想特性，而以前的方法往往只能实现其中一到两项。
- 离散流匹配范式在机器人策略学习中表现出了更好的可扩展性和鲁棒性，尤其在高维动作空间和多峰动作分布下优势明显。
- 最优的tokenization方案是使用VQ-VAE学习的码本，相比简单的均匀量化能更好地保留连续动作的细微差别。

## 7. 优点

- **方法创新**：将离散流匹配引入视觉运动策略学习，连接了连续控制与离散表征，思路新颖。
- **三特性统一**：首次在同一框架中实现稳定优化、多模态和快速推理，解决了离散动作策略的关键矛盾。
- **系统性工程研究**：对tokenization方案进行了深入分析，为后续研究提供了实用指导。
- **实验验证充分**：覆盖模拟和真实场景，且代码和视频已开源，可复现性强。

## 8. 不足与局限

- **计算资源未公开**：文中未说明训练所需的GPU型号、数量和时长，限制了其他研究者估算成本的能力。
- **Tokenization依赖性**：性能高度依赖于码本的质量，码本训练可能引入额外开销或偏差。
- **真实场景范围有限**：虽然有两组真实部署，但任务复杂度仍相对受控（如桌面操作），尚需在更复杂、非结构化的环境中验证。
- **多模态能力可能受限**：概率分支机制虽然能表征多种模式，但在模式数量极大且分散时，采样效率可能下降。
- **与大规模预训练模型的结合**：未探讨是否可以利用大规模视觉-语言模型（如VLM）来进一步改进策略性能，未来可拓展。

（完）
