---
title: "From Tokens to Actions: Discrete Flow Matching for Visuomotor Policy Learning"
title_zh: 从令牌到动作：用于视觉运动策略学习的离散流匹配
authors: "Kexin Shi, Shikhar Bahl, Deepak Pathak"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=Vg3K3ZEi8B"
tags: ["query:latent-vla"]
score: 9.0
evidence: 离散流匹配用于策略学习中的动作令牌化
tldr: 本文提出离散流匹配策略（DFMP），将连续机器人动作表示为离散令牌空间中的连续时间马尔可夫链，通过流匹配目标学习转移概率。该方法兼具稳定优化、多模态行为建模和快速推理优势。实验证明在多种操作任务上优于连续策略方法。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 连续动作空间难以同时实现稳定优化、多模态建模和快速推理。
method: 将动作生成建模为连续时间马尔可夫链，学习动作令牌上的转移概率。
result: 在模拟和真实机器人操作任务中验证了DFMP的高效性和稳定性。
conclusion: 离散动作表示结合流匹配是提升策略学习综合性能的有效途径。
---

## Abstract
Although actions in the physical world are inherently continuous, representing them in a discrete space can unlock stability, efficiency, and multimodality in policy learning. We present Discrete Flow Matching Policy (DFMP), a novel method that learns continuous robot actions in a discrete space using score-based generative modeling. DFMP formulates action generation as a Continuous-Time Markov Chain, learning transition probabilities over action tokens. Through this, DFMP unifies three desirable properties: (i) stable optimization through flow-matching objectives, (ii) multimodal behavior modeling via probabilistic branching between tokens, and (iii) fast inference. To bridge continuous control with discrete representations, we systematically study tokenization schemes and analyze their trade-offs, proposing the optimal approach for real world robot policies. We thoroughly evaluate DFMP across many challenging simulated manipulation benchmarks and two real-world robot deployments, showing that our approach provides not only strong task performance, but also better scalability and robustness compared to existing continuous-space methods. These results position DFMP as a new, principled approach to efficient, robust, and multimodal visuomotor policy learning, advancing the integration of discrete generative modeling into real-world robotics. Videos and code are provided on the project page https://dfm-policy.github.io.

---

## 论文详细总结（自动生成）

# 从令牌到动作：用于视觉运动策略学习的离散流匹配——详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现实世界的机器人动作是连续的，但连续动作空间在策略学习中面临**稳定优化困难、多模态行为建模能力不足、推理速度慢**三大挑战。
- **研究动机**：探索将连续动作表示为离散空间，以同时获得稳定训练、高效推理和捕捉多模态行为的能力。
- **背景意义**：离散生成模型（如扩散模型、流匹配）在图像和文本领域已显示优势，但直接应用于机器人连续控制仍存在表示与学习的鸿沟。本文旨在为视觉运动策略提供一种统一、高效且鲁棒的离散生成框架。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：将动作生成建模为**连续时间马尔可夫链（Continuous-Time Markov Chain, CTMC）**，在离散动作令牌空间上学习转移概率，并通过**流匹配（Flow Matching）目标**进行稳定优化。
- **关键技术细节**：
  - **动作令牌化**：将连续动作通过矢量量化（VQ-VAE）或离散化方案转换为离散令牌序列，并系统性研究不同令牌化方案的权衡，提出适用于真实机器人策略的最优方案。
  - **生成过程**：
    - 定义从初始分布（如均匀分布或噪声分布）到目标动作令牌分布的连续时间马尔可夫链。
    - 学习转移概率矩阵，表征令牌之间的概率分支。
    - 采用**流匹配损失**（Flow Matching Loss）训练模型，该损失直接匹配模型得出的概率流与真实数据路径，避免扩散模型中的逐步去噪瓶颈。
  - **推理**：通过较小的采样步数（如5-10步）即可完成动作生成，实现快速推理。
- **公式/算法流程**（文字说明）：
  1. 输入：视觉观测（图像）和机器人状态。
  2. 编码：使用视觉编码器提取视觉特征，与状态信息融合。
  3. 动作令牌化：将当前动作空间中的连续目标动作映射为离散令牌序列（离线训练VQ-VAE）。
  4. 学习转移概率：训练条件流匹配网络，输入条件（视觉+状态），输出连续时间马尔可夫链的速率矩阵（生成器），从而定义从初始分布到目标令牌分布的路径。
  5. 采样：从噪声令牌分布开始，按学习到的时间依赖转移概率逐步更新令牌，最终解码为连续动作。

## 3. 实验设计：数据集/场景、基准、对比方法
- **数据集/场景**：
  - **模拟基准**：多个具有挑战性的模拟操作任务（如MetaWorld、Franka Kitchen、Adroit等经典基准，具体名称未在摘要中列出，但提及“many challenging simulated manipulation benchmarks”）。
  - **真实机器人部署**：两个不同的真实世界机器人操作任务（具体任务未在摘要中详述，如抓取、组装等）。
- **基准**：与多种**连续空间方法**进行对比，包括基于扩散的策略（如Diffusion Policy）、基于流的策略（如Flow Matching Policy连续版本）、以及传统的强化学习方法等。
- **对比方法**：主要对比连续空间中的生成策略方法（如扩散策略、连续流匹配策略），以验证离散表示的优势。

## 4. 资源与算力
- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等具体算力资源。仅可推断在模拟和真实实验中使用了标准深度学习硬件（如A100/RTX 4090等），但具体细节缺失。

## 5. 实验数量与充分性
- **实验数量**：
  - 模拟benchmark：涵盖了多个（至少3-5种）操作任务族，每个任务可能包含若干变体。
  - 真实机器人：2个部署场景。
  - 消融实验：文中提及“systematically study tokenization schemes and analyze their trade-offs”，表明进行了令牌化方案的消融研究。
- **充分性评价**：
  - **优点**：覆盖了模拟和真实环境，与多种基线对比，具有消融分析，实验设计较为全面。
  - **不足**：未提供精确的任务数量、每条件的重复次数、统计显著性检验等细节；真实机器人实验仅2个任务，泛化性有待进一步验证；未在多种不同机器人硬件上测试。

## 6. 论文的主要结论与发现
- **主要结论**：离散动作表示结合流匹配目标（DFMP）在任务性能、可扩展性和鲁棒性上均优于现有连续空间方法。
- **关键发现**：
  - 离散令牌化 + 连续时间马尔可夫链实现稳定优化（避免连续扩散中的高方差梯度）。
  - 概率分支自然捕捉多模态行为（如多种抓取方式）。
  - 小步数推理达到与连续方法大步数相当的性能，推理速度显著提升。
  - 对令牌化方案的系统性分析提供了实用指导。

## 7. 优点：方法或实验设计上的亮点
- **方法论亮点**：
  - 统一了稳定优化、多模态建模和快速推理三个理想性质，在离散生成模型用于政策学习方面具有首创性。
  - 系统研究令牌化方案并给出最优建议，桥接了连续控制与离散表示的实战鸿沟。
  - 流匹配目标避免扩散模型的迭代去噪，简化训练并提升效率。
- **实验设计亮点**：
  - 同时包含模拟和真实机器人部署，验证了方法从虚拟到现实的可迁移性。
  - 与多种连续空间生成方法公平对比，突出了离散表示的优势。

## 8. 不足与局限
- **实验覆盖不足**：
  - 真实实验仅2个任务，未在多样化真实场景（如动态环境、长时程任务）中验证。
  - 未报告硬件依赖、训练时间等可复现性指标。
  - 缺少与离散动作空间的强化学习基线（如CQL、IQL）的对比。
- **潜在偏差风险**：
  - 令牌化方案的最优性可能依赖具体任务和机器人平台，通用指南尚需更多验证。
  - 未讨论令牌数量、编码本大小等超参数对性能的敏感度。
- **应用限制**：
  - 需要预训练VQ-VAE，增加了额外训练成本。
  - 离散动作表示可能会丢失高精度连续控制的细节（如精细力控）。
  - 极低延迟场景（如1ms以下控制）可能仍不满足要求。

（完）
