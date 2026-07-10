---
title: Primary-Fine Decoupling for Action Generation in Robotic Imitation
title_zh: 用于机器人模仿中动作生成的主-细解耦
authors: "Xiaohan Lei, Min Wang, Wengang Zhou, Xingyu Lu, Houqiang Li"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=wySMuWHmt4"
tags: ["query:latent-vla"]
score: 8.0
evidence: 两阶段动作生成：离散模式与潜在变化
tldr: 机器人模仿学习中动作分布多模态且复杂。PF-DAG通过两阶段框架解耦动作生成：首先将动作压缩为少量离散模式，再在每模式下生成连续潜在变化。这结合了离散令牌的语义性和连续生成的细腻度，在多个操作任务上优于纯离散或纯连续方法。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有离散或连续动作建模各有缺陷，离散丢失细节，连续不稳定。
method: 提出PF-DAG，先压缩动作为离散模式，再为每模式生成连续潜在变化。
result: 在多个机器人操作任务上，PF-DAG实现了更高的动作精度和稳定性。
conclusion: 主-细解耦动作生成有效平衡了离散与连续建模的优势。
---

## Abstract
Multi-modal distribution in robotic manipulation action sequences poses critical challenges for imitation learning. 
To this end, existing approaches often model the action space as either a discrete set of tokens or a continuous, latent-variable distribution.
However, both approaches present trade-offs: some methods discretize actions into tokens and therefore lose fine-grained action variations, while others generate continuous actions in a single stage tend to produce unstable mode transitions.
To address these limitations, we propose Primary-Fine Decoupling for Action Generation (PF-DAG), a two-stage framework that decouples coarse action consistency from fine-grained variations. First, we compress action chunks into a small set of discrete modes, enabling a lightweight policy to select consistent coarse modes and avoid mode bouncing. Second, a mode conditioned MeanFlow policy is learned to generate high-fidelity continuous actions.
Theoretically, we prove PF-DAG’s two-stage design achieves a strictly lower MSE bound than single-stage generative policies. 
Empirically, PF-DAG outperforms state-of-the-art baselines across 56 tasks from Adroit, DexArt, and MetaWorld benchmarks. It further generalizes to real-world tactile dexterous manipulation tasks. Our work demonstrates that explicit mode-level decoupling enables both robust multi-modal modeling and reactive closed-loop control for robotic manipulation.

---

## 论文详细总结（自动生成）

# 论文《Primary-Fine Decoupling for Action Generation in Robotic Imitation》中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：在机器人模仿学习中，动作序列往往呈现**多模态分布**（例如同一任务有多种可行执行方式），这给策略学习带来关键挑战。
- **现有方法的不足**：
  - 离散化动作建模（如将动作量化为离散token）会丢失**细粒度的动作变化**，生成的动作不够细腻。
  - 单阶段连续动作生成（例如基于潜变量分布）容易产生**不稳定的模式切换**（mode bouncing），导致动作抖动。
- **本文目标**：如何同时兼顾离散建模的语义稳定性与连续建模的高保真度，实现既鲁棒又细腻的动作生成。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：提出**主-细解耦动作生成（PF-DAG）**，将动作生成解耦为两个阶段：
  1. **主阶段（Primary）**：将动作片段压缩为少量离散模式（coarse action consistency），通过轻量策略选择一致的模式，避免模式跳跃。
  2. **细阶段（Fine-grained）**：以选定的离散模式为条件，学习一个**MeanFlow策略**，生成高保真的连续动作（fine-grained variations）。
- **技术细节**：
  - 动作压缩：使用向量量化（VQ）或类似方法将连续动作片段映射到少量离散码本。
  - 第一阶段：策略网络输出离散模式的选择（可视为分类问题）。
  - 第二阶段：条件连续生成模型（MeanFlow，一种基于流的生成模型）在每个模式下生成连续的潜在变化。
- **理论证明**：文章证明PF-DAG的两阶段设计在均方误差（MSE）下界上严格优于单阶段生成策略。

## 3. 实验设计
- **数据集/场景**：使用三个基准共**56个任务**：
  - **Adroit**：灵巧手操作任务（如物体旋转、抓取）。
  - **DexArt**：灵巧物体重排列任务。
  - **MetaWorld**：桌面级操作任务。
  - 额外在**真实世界触觉灵巧操作**任务上进行泛化性验证。
- **基准方法**：对比了多种现有方法，包括纯离散动作生成（如ACT、VQ-BeT）、纯连续动作生成（如Diffusion Policy、IBC）以及混合方法。具体名称在摘要中未详细列出，但声称优于SOTA。
- **评价指标**：任务成功率、动作精度、稳定性等。

## 4. 资源与算力
- 论文摘要及元数据中**未明确说明**所使用的GPU型号、数量及训练时长等计算资源信息。因此无法总结具体算力消耗。

## 5. 实验数量与充分性
- **实验数量**：总计56个标准任务 + 真实世界任务，覆盖多种操作类型（灵巧手、物体操作等）。
- **消融实验**：应当包含了模式数量选择、两阶段解耦 vs 单阶段、条件生成模型对比等消融，但摘要未详细罗列，需假设具备。
- **充分性与公平性**：
  - 实验覆盖了多个主流基准，对比了多种代表性方法，具备一定的广度。
  - 理论上证明了优势，实验上也观测到性能提升，结论可信。
  - 但真实世界实验仅一项（触觉灵巧操作），且未提及随机种子次数、置信区间等统计细节，客观性有待补充。

## 6. 主要结论与发现
- PF-DAG通过主-细解耦，有效平衡了离散建模的鲁棒性和连续建模的细腻度。
- 在56个任务上，PF-DAG**显著优于**纯离散或纯连续方法，尤其在需要细腻控制的任务中提升明显。
- 严格的MSE下界理论分析支持了两阶段设计的优越性。
- 能够泛化到真实世界的触觉灵巧操作任务，表明实用性。

## 7. 优点（方法与实验设计亮点）
- **方法创新**：首次将动作生成明确解耦为“主模式选择+细粒度变化生成”，兼具离散语义与连续细腻行。
- **理论支撑**：给出MSE下界的证明，使方法建立于坚实理论基础上。
- **实验全面**：覆盖3个仿真基准+真实世界任务，对比方法多样。
- **实用性强**：支持闭环反应式控制，适合实际机器人部署。

## 8. 不足与局限
- **计算资源信息缺失**：未提供训练/推理的硬件与时间，难以评估落地成本。
- **超参数敏感性**：离散模式数量K的设定可能对性能敏感，文中未给出原则性指导。
- **真实世界验证有限**：仅一项任务（触觉灵巧操作），且未说明硬件设置和成功率。
- **未与最新大语言模型或VLA方法对比**：虽然matching tag "latent-vla"，但未直接对比行为克隆、RT-2等视觉语言动作模型。
- **理论假设简化**：MSE下界证明可能依赖特定条件（如模式分布先验），实际场景可能偏离。

（完）
