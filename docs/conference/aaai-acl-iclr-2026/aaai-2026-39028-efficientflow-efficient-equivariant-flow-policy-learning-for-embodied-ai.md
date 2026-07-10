---
title: "EfficientFlow: Efficient Equivariant Flow Policy Learning for Embodied AI"
title_zh: EfficientFlow：面向具身AI的高效等变流策略学习
authors: "Jianlei  Chang, Ruofeng Mei, Wei Ke, Xiangyu Xu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39028/42990"
tags: ["query:gen2embodied"]
score: 7.0
evidence: 等变流匹配用于具身策略学习
tldr: 生成式策略面临数据效率和采样效率双重挑战。本文提出EfficientFlow框架，将等变性引入流匹配策略学习，理论证明等变速度预测网络可保持动作分布的等变性，从而提升数据利用率和生成速度。在多个具身任务中，EfficientFlow以更少演示达到同等性能，且推理速度更快，推动了生成式模型在具身智能中的实用化。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39028/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1643, \"height\": 739, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39028/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1620, \"height\": 787, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39028/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1708, \"height\": 888, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39028/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 937, \"height\": 456, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39028/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 883, \"height\": 209, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39028/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 890, \"height\": 278, \"label\": \"Table\"}]"
motivation: 现有生成式策略数据效率低、采样速度慢，限制实际应用。
method: 提出等变流匹配框架，利用等变网络结构提升数据效率和采样速度。
result: 在多个机器人任务上，以更少训练样本实现更快的动作生成和更好的泛化。
conclusion: 等变性是提升生成式策略效率的关键原理。
---

## Abstract
Generative modeling has recently shown remarkable promise for visuomotor policy learning, enabling flexible and expressive control across diverse embodied AI tasks. However, existing generative policies often struggle with data inefficiency, requiring large-scale demonstrations, and sampling inefficiency, incurring slow action generation during inference. We introduce EfficientFlow, a unified framework for efficient embodied AI with flow-based policy learning. To enhance data efficiency, we bring equivariance into flow matching. We theoretically prove that when using an isotropic Gaussian prior and an equivariant velocity prediction network, the resulting action distribution remains equivariant, leading to improved generalization and substantially reduced data demands. To accelerate sampling, we propose a novel acceleration regularization strategy. As direct computation of acceleration is intractable for marginal flow trajectories, we derive a novel surrogate loss that enables stable and scalable training using only conditional trajectories. Across a wide range of robotic manipulation benchmarks, the proposed algorithm achieves competitive or superior performance under limited data while offering dramatically faster inference. These results highlight EfficientFlow as a powerful and efficient paradigm for high-performance embodied AI.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心挑战**：生成式模型（如扩散策略）在具身智能的视动策略学习中展现出灵活性和表达能力，但面临两大瓶颈：**数据效率低**（需要大量示范数据）和**采样效率低**（推理时需多步迭代去噪，计算开销大）。
- **研究动机**：如何在保持生成式模型灵活性的同时，显著提升数据效率和推理速度，使其适用于实时机器人控制。
- **整体含义**：本文提出 **EfficientFlow**，一个统一的基于流匹配的策略学习框架，通过引入**等变性**（equivariance）提高数据效率，并通过**加速度正则化**加速采样，最终实现快速、高效、泛化能力强的具身策略。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：结合流匹配（Flow Matching）与群等变性，并引入正则化迫使流轨迹更平滑，减少推理步数。
- **关键技术细节**：
  - **等变性融入流匹配**：
    - 理论证明：若初始分布各向同性高斯（\(p_0\)），且速度场网络\(u_\theta\)关于观测变换等变，则整个流过程诱导的条件动作分布保持等变（定理1）。
    - 实现：使用`escnn`库构建等变网络，将动作的6D旋转表示、3D平移、夹爪宽度按SO(2)群表示分解（\(\rho_1,\rho_0\)组合），观测编码器和动作编码器均设计为等变的。
  - **加速度正则化（FABO）**：
    - 问题：直接计算边际流轨迹的二阶导数不可行。
    - 解决方案：提出**流加速度上界（Flow Acceleration Upper Bound）**，利用条件流轨迹上相邻时间点的速度差作为代理损失，最小化该上界即可近似惩罚加速度。
    - 时间依赖权重\(\lambda(t)=(1-t)^2\)鼓励早期平滑、后期保真。
  - **推理时时间一致性策略**：生成多候选轨迹，选择与上一段重叠部分欧氏距离最小的轨迹，并每10步随机重置以避免模式坍塌。
- **算法流程**（文字说明）：
  1. 训练阶段：从数据集中采样观测\(o\)和动作\(x_1\)，随机时间\(t\)，从条件流路径采样\(x_t\)；计算条件速度场\(u(t,x_t|x_1)\)；预测速度\(u_\theta(t,x_t|o)\)；损失 = CFM损失 + \(\lambda\)× FABO正则项。
  2. 推理阶段：采样高斯噪声作为初始动作序列\(x_0\)，通过等变网络定义的ODE从\(t=0\)积分到\(t=1\)得到动作\(x_1\)；使用多候选选择提高时间平滑性。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集/场景**：使用 **MimicGen** 基准的12个机器人操作任务，涵盖不同难度、时间跨度和物体布局（如 Stack D1, Square D2, Threading D2, Kitchen, Pick Place, Nut Assembly 等）。
- **基准（Baselines）**：
  - 主要对比方法：**EquiDiff**（等变扩散策略）、**ACT**、**Diffusion Policy**（CNN版DP-C和Transformer版DP-T）、**DP3**（使用点云）。
  - 所有方法在同一输入条件下比较：RGB图像（agent视角+腕部相机）。
- **实验变量**：
  - 训练示范数量：100、200、1000个。
  - EfficientFlow的NFE（函数评估次数）：1、3、5步。
  - 基线方法使用其原始配置（如EquiDiff使用1000 NFE）。

## 4. 资源与算力

- **文中未明确说明**训练所使用的GPU型号、数量或训练时长。
- 仅提及推理时间（如1 NFE时平均12.22ms），但未提供训练成本信息。
- *注：论文在开源仓库和致谢中提及国家自然科学基金，但无具体算力细节。*

## 5. 实验数量与充分性

- **实验组数**：
  - 主实验：在12个任务上，对3个数据量（100/200/1000）、3个NFE（1/3/5）进行测试，每个结果取3个种子平均，表格1包含大量数值。
  - 学习效率实验：表3比较了达到最终性能50%所需训练轮数（在100个演示下）。
  - 消融实验（表4）：在12个任务100个演示下，对比4种变体（NoAcc、NonEqui、EquiCFM、EquiMF），全面解耦等变性和加速度正则化的贡献。
- **公平性**：
  - 基线结果直接引用自EquiDiff论文，保证了复现一致性。
  - 所有方法使用相同的输入模态（RGB），DP3额外使用点云作为额外对比。
  - 实验覆盖多种任务类型（抓取、组装、拧螺丝等），难度梯度明显。
- **充分性评价**：实验数量充足，任务多样，消融设计完整，基线对比充分。但缺少真实机器人实验（仅在仿真中），且未给出训练时间等资源信息。

## 6. 论文的主要结论与发现

- **核心发现**：
  - EfficientFlow在**数据有限（100次演示）**时，成功率达52.61%（1 NFE）至54.18%（5 NFE），优于EquiDiff（53.77%），推理速度提升19.9×~56.1×。
  - 在200次演示下，平均成功率可达EquiDiff的98.4%，显著超过DP-T、DP3、ACT。
  - **等变性**和**加速度正则化**各自独立且互补地提升性能：去除其中任意一项均导致成功率下降。
  - 与Consistency Flow Matching和MeanFlow等高效流变体相比，EfficientFlow效果更优。
  - 学习效率更高：仅需更少轮数即可达到最终性能的50%（如Hammer任务只需1/5轮数）。
- **结论**：EfficientFlow为高性能具身AI提供了一种高效、实用的生成式策略范式。

## 7. 优点：方法或实验设计上的亮点

- **方法论优点**：
  - **理论贡献**：严格证明了在流匹配框架中等变性成立的条件，为后续工作提供理论基础。
  - **创新正则化**：FABO上界是一种巧妙的替代损失，避免了边际轨迹不可得的问题，使平滑化训练可行。
  - **系统化设计**：融合等变网络、加速度正则化、时间一致性策略，整体框架统一而高效。
- **实验设计亮点**：
  - 对比了多种生成式策略（扩散、ACT、流策略变体），且测算了不同NFE下的性能-速度权衡。
  - 消融实验覆盖等变性和正则化的各自贡献，并比较了其他流变体（CFM、MF），体现方法先进性。
  - 学习效率的测量（达到50%性能的轮数）更直观地展示了等变性的优势。

## 8. 不足与局限

- **实验覆盖不足**：
  - 仅在**仿真环境**（MimicGen）中评估，未在真实机器人上验证。仿真中的对称性可能与真实场景有差距。
  - 使用agent-view相机并非正交视角，可能削弱等变性的效果，但作者认为这是更严格的测试，然而仍与真实部署存在差异。
- **偏差风险**：
  - 基线结果仅引自一篇论文（EquiDiff），其他方法（DP-C等）的配置未公开验证一致性，可能引入复现偏差。
  - 仅测试了SO(2)等变性，未扩展到SE(3)或更一般的群，限制了在三维非平面操作上的直接应用。
- **应用限制**：
  - 需要预定义群的表示（如旋转阶数），对非结构化环境适应性有限。
  - 加速度正则化仅用上界近似，当条件流与边际流差异大时可能效果下降。
- **缺乏资源透明度**：未报告训练成本，难以评估实际部署的计算开销。

（完）
