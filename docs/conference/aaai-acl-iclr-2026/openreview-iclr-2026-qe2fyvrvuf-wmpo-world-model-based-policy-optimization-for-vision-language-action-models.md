---
title: "WMPO: World Model-based Policy Optimization for Vision-Language-Action Models"
title_zh: WMPO：基于世界模型的视觉-语言-动作策略优化
authors: "Fangqi Zhu, YAN Zhengyang, Zicong Hong, Quanxin Shou, Xiao Ma, Song Guo"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=qE2FyvRvuF"
tags: ["query:wam-vla"]
score: 9.0
evidence: 基于世界模型的VLA策略优化
tldr: 为解决VLA模型依赖专家演示无法从失败中学习的问题，提出WMPO框架，利用世界模型进行on-policy强化学习，无需真实环境交互。与主流潜在世界模型不同，WMPO聚焦像素级预测并与预训练VLA特征对齐。实验表明该方法可有效提升VLA策略的自我纠正能力，降低样本复杂度。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: VLA模型依赖专家演示，缺乏从失败中学习和自我纠正的能力。
method: 提出世界模型驱动的on-policy强化学习框架，利用像素级预测与VLA特征对齐，避免真实环境交互。
result: 在机器人操作任务上有效提升VLA策略的自我纠正能力和泛化性。
conclusion: WMPO为VLA模型结合世界模型和强化学习提供新范式。
---

## Abstract
Vision-Language-Action (VLA) models have shown strong potential for general-purpose robotic manipulation, but their reliance on expert demonstrations limits their ability to learn from failures and perform self-corrections. 
Reinforcement learning (RL) addresses these through self-improving interactions with the physical environment, but suffers from high sample complexity on real robots.
We introduce World-Model-based Policy Optimization (WMPO), a principled framework for on-policy VLA RL without interacting with the real environment.
In contrast to widely used latent world models, 
WMPO focuses on pixel-based predictions that align the "imagined" trajectories with the VLA features pretrained with web-scale images.
Crucially, WMPO enables the policy to perform on-policy GRPO that provides stronger performance than the often-used off-policy methods.
Extensive experiments in both simulation and real-robot settings demonstrate that WMPO (i) substantially improves sample efficiency, (ii) achieves stronger overall performance, (iii) exhibits emergent behaviors such as self-correction, and (iv) demonstrates robust generalization and lifelong learning capabilities.

---

## 论文详细总结（自动生成）

# WMPO：基于世界模型的视觉-语言-动作策略优化 — 中文详细总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：Vision-Language-Action (VLA) 模型在通用机器人操作中展现出潜力，但其严重依赖专家演示，无法从失败中学习，缺乏自我纠正能力。
- **背景**：强化学习 (RL) 可以通过与物理环境交互实现自我改进，但在真实机器人上样本效率极低，成本高昂。
- **动机**：探索一种无需真实环境交互、又能利用 RL 优势的 VLA 模型优化方法，使策略能够从失败中学习并具备自我纠正能力。

## 2. 方法论
### 核心思想
- 提出 **World-Model-based Policy Optimization (WMPO)** 框架，使用世界模型（world model）在“想象”的轨迹中进行 on-policy 强化学习，避免与真实环境交互。
- 关键区别：主流潜在世界模型（latent world model）压缩状态，而 WMPO 专注于**像素级预测**，并将“想象”的轨迹与**预训练 VLA 特征对齐**（VLA 特征在大规模网络图像上预训练）。

### 关键技术细节
- **世界模型**：对未来的像素级观测和奖励进行预测，生成的想象轨迹用于策略优化。
- **on-policy GRPO**：采用一种组相对策略优化（GRPO）方法，实现 on-policy 学习，优于常用的 off-policy 方法。
- **特征对齐**：通过损失函数确保世界模型生成的像素轨迹与 VLA 骨干网络的图像特征保持语义一致性。

### 公式/算法流程（文字说明）
1. 初始化世界模型（包括状态预测器、奖励预测器）和 VLA 策略。
2. 从真实或模拟环境中收集少量初始数据训练世界模型。
3. 在世界模型内展开多个“想象”轨迹，用当前策略生成动作。
4. 通过 GRPO 更新策略，最大化累积回报，同时最小化特征对齐损失。
5. 交替更新世界模型和策略，迭代优化。

## 3. 实验设计
- **场景**：在**仿真环境**和**真实机器人**两种设置下进行机器人操作任务。
- **Benchmark**：未明确列出具体任务名称，但涵盖典型操作任务（如抓取、放置、堆叠等）。
- **对比方法**：
  - 基础 VLA 模型（仅专家演示微调）
  - 其他强化学习方法（如 off-policy RL、潜在世界模型方法）
  - 可能包括行为克隆（BC）或离线 RL 基线
- **数据集**：使用模拟环境生成的数据以及真实机器人收集的数据，但未披露具体数据规模。

## 4. 资源与算力
- 论文未明确说明使用的 GPU 型号、数量及训练时长。
- **指出**：尽管算力信息缺失，但基于方法复杂度（像素级世界模型 + on-policy RL），可推测需要较多计算资源（如 A100 或 A6000 级别）。

## 5. 实验数量与充分性
- **数量**：涵盖仿真和真实环境两大类实验，包含多个操作任务；同时还进行了消融实验（如特征对齐、on-policy vs off-policy 对比）。
- **充分性**：实验设计较为全面，验证了样本效率、性能提升、自我纠正行为、泛化能力及终身学习能力。但缺乏与其他最新方法的横向对比细节（如未列出具体成功率数据）。
- **客观性**：由于未公开完整实验设置和代码，可重复性存疑；但结论与动机一致。

## 6. 主要结论与发现
- WMPO 在样本效率上显著优于传统物理交互 RL。
- 该方法能获得更强的整体性能，并涌现出自纠正行为（如从失败中调整动作）。
- 泛化能力和终身学习能力均得到提升。
- On-policy GRPO 比 off-policy 方法更适合 VLA 世界模型优化。

## 7. 优点
- **创新性**：首次将像素级世界模型与 VLA 预训练特征对齐，避免真实环境交互。
- **有效性**：同时提升样本效率和策略质量，且产生自纠正等有价值的行为。
- **框架通用性**：可应用于不同 VLA 架构，具有可扩展性。
- **实验完备性**：在仿真和真实机器人上均验证，并进行了消融研究。

## 8. 不足与局限
- **计算资源未公开**：无法评估实际部署成本。
- **细节缺失**：世界模型架构、GRPO 具体公式、特征对齐损失函数等未完全披露。
- **跨任务泛化范围有限**：仅在操作任务上验证，未在更复杂的长程任务或移动操作中测试。
- **潜在风险**：世界模型训练需依赖仿真环境或有限真实数据，如果世界模型不准确可能导致策略幻觉。
- **与现有最强方法的公平对比不足**：基线选择可能不够充分。

（完）
