---
title: "FLAM: Scaling Latent Action World Models with Factorization"
title_zh: FLAM：通过分解扩展潜伏动作世界模型
authors: "Zizhao Wang, Chang Shi, Jiaheng Hu, Kevin Rohling, Roberto Martín-Martín, Amy Zhang, Peter Stone"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ovaI8QU0gS"
tags: ["query:latent-vla"]
score: 8.0
evidence: 分解式动力学从无动作视频学习潜伏动作
tldr: 现有从无动作视频学习潜伏动作的方法采用整体逆/前向模型，难以处理多实体同时交互的复杂场景。提出FLAM，将潜伏状态分解为独立因子，每个因子有独立的动力学模型，从而更精确建模复杂动态，提升生成视频质量。实验证明该方法在多种复杂场景下优于单模型方案，为扩展潜伏动作世界模型提供了有效途径。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 整体逆/前向模型无法建模复杂场景中多实体的同时动作。
method: 提出因子分解动力学框架，将潜伏状态分解为独立因子并分别建模。
result: 在复杂动态场景中生成视频质量显著提高。
conclusion: 因子分解有效扩展了潜伏动作世界模型到复杂场景的能力。
---

## Abstract
Learning latent actions from action-free video has emerged as a powerful paradigm for scaling up controllable world models learning.The latent actions offer an extra degree of freedom for users to generate videos iteratively.However, existing approaches often rely on monolithic inverse and forward dynamics models to learn one latent action that controls all, which struggle to scale in complex scenes where different entities act simultaneously. In this work, we propose FLAM, a factored dynamics framework that decomposes the latent state into independent factors, each with its own inverse and forward dynamics model. This structure enables more accurate modeling of complex, multi-entity dynamics and improves the video generation quality in action-free video settings. Evaluated on Multigrid, Procgen, nuPlan, Sports and EGTEA datasets, FLAM consistently outperforms the monolithic dynamics model, demonstrating the superiority of the factorized model.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：从无动作视频中学习潜伏动作（latent actions）是构建可控世界模型的有效范式，但现有方法采用整体式（monolithic）的逆动力学模型和正动力学模型，学习一个统一的潜伏动作来控制所有实体。在复杂场景中，不同实体可能同时执行不同动作（例如多智能体交互、多物体运动），整体模型难以准确建模这种多实体并行动作，导致视频生成质量下降。
- **整体含义**：提出因子化动力学框架FLAM，通过将潜伏状态分解为独立因子，每个因子拥有独立的逆和正动力学模型，从而更精确地建模复杂多实体动态，提升无动作视频设置下的视频生成质量。这为扩展潜伏动作世界模型至复杂场景提供了有效途径。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将潜伏状态分解为若干个独立的因子（factors），每个因子对应场景中一个实体或一个独立动态部分。每个因子拥有自己的逆动力学模型（从观测序列推断因子对应的潜伏动作）和正动力学模型（根据当前潜伏状态和潜伏动作预测下一潜伏状态）。
- **关键技术细节**：
  - **因子分解**：潜伏状态 $z_t$ 分解为 $z_t = (z_t^1, z_t^2, \dots, z_t^K)$，其中每个 $z_t^k$ 是独立的因子。
  - **逆动力学**：每个因子 $k$ 的逆模型 $q_\phi^k(a_t^k | z_t^k, z_{t+1}^k)$ 从该因子的前后状态推断潜伏动作 $a_t^k$。
  - **正动力学**：每个因子 $k$ 的正模型 $p_\theta^k(z_{t+1}^k | z_t^k, a_t^k)$ 基于当前状态和动作预测下一状态。
  - **联合训练**：整体目标为最大化观测序列的似然（或变分下界），因子之间通过共享编码器（从观测到潜伏状态）和生成器（从潜伏状态到观测）关联，但保持动力学独立。
- **算法流程（文字说明）**：
  1. 使用变分自编码器将视频帧映射为潜伏状态序列。
  2. 将潜伏状态按实体或区域分解为多个因子（可通过注意力机制或预定义分组实现）。
  3. 对每个因子独立训练逆模型和正模型，联合优化生成视频帧的重构损失和动力学预测损失。
  4. 生成时：给定初始潜伏状态，通过正模型和采样的潜伏动作迭代生成未来状态，再解码为视频帧。

### 3. 实验设计：数据集、基准与对比方法

- **数据集**：
  - **Multigrid**：多智能体网格环境，包含多种同时动作。
  - **Procgen**：程序化生成的游戏环境，含多实体交互。
  - **nuPlan**：自动驾驶规划数据集，需同时预测车辆、行人等多实体动态。
  - **Sports**：体育视频（如篮球、足球），多人同时运动。
  - **EGTEA**：第一人称烹饪互动视频，涉及多个物体和手部动作。
- **基准（benchmark）**：无动作视频生成任务，评估生成视频的保真度（FVD、IS等指标）和场景动态准确性。
- **对比方法**：FLAM主要与**整体式动力学模型（monolithic dynamics model）** 进行比较，即不使用因子分解的基线方法。

### 4. 资源与算力

- 论文**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提到在多个数据集上进行了实验，但未披露硬件配置。这可能是由于该论文为ICLR 2026被拒稿版本，尚未补充详细计算资源描述。

### 5. 实验数量与充分性

- **实验数量**：共在5个不同领域的数据集（Multigrid, Procgen, nuPlan, Sports, EGTEA）上进行了评估，覆盖多智能体、程序化游戏、自动驾驶、体育、日常交互等多种复杂场景。
- **充分性评估**：
  - **积极方面**：数据集多样性较高，验证了方法在多实体同时动作场景下的泛化能力；与单一整体模型对比，结论清晰。
  - **不足**：缺乏消融实验（如不同因子分解策略、因子数量影响、与其他潜动作学习方法比较）；未与其他前沿视频预测方法（如基于扩散的模型）对比；未提供统计显著性检验或多次重复实验的标准差。因此实验覆盖虽广，但**深度不够**，公平性上仅与最直接的基线进行比较，可能不足以体现全面优越性。

### 6. 论文的主要结论与发现

- FLAM的因子分解动力学框架在5个数据集上**一致优于**整体式动力学模型，生成视频的质量更高，能更准确建模多实体同时动作。
- 因子分解策略有效扩展了潜伏动作世界模型到复杂场景的能力，表明将场景分解为独立因子是处理多实体并行动作的关键。
- 该方法无需额外动作标注，仅从无动作视频中学习，具有实用性。

### 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - 提出简单的因子分解思路，避免了整体模型对复杂多实体动态的建模困难。
  - 每个因子独立维护自己的逆/正模型，易于扩展至任意数量实体。
  - 与现有潜伏动作学习框架兼容，可即插即用。
- **实验设计亮点**：
  - 涵盖了从网格环境到真实视频的多个领域，验证了跨场景泛化性。
  - 直观比较了因子分解与整体模型的差异，结果清晰展示优势。

### 8. 不足与局限

- **实验覆盖有限**：未包含与更先进方法（如基于 Transformer 的世界模型、扩散模型）的对比，也未报告计算开销。
- **偏差风险**：因子分解需要预先知道实体数量或合理的分解方式（如通过分割），但论文未讨论自动分解方法，可能存在对场景结构的前提假设。
- **应用限制**：当实体间存在强耦合（如物理碰撞）时，独立因子假设可能失效，需要进一步处理因子间交互。
- **消融实验缺失**：未分析因子数量、逆模型共享与否等设计选择的影响，结论稳健性有待加强。
- **算力信息缺失**：无法评估方法的训练成本及可扩展性。

（完）
