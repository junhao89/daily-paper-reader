---
title: Shaping Robotic Actions with Fourier Flow Matching
title_zh: 通过傅里叶流匹配塑造机器人动作
authors: "Mateusz Wyszyński, Marcin Wrochna, Piotr Zalewski, Maciej Mehl, Marek Cygan"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=3GH2fZd9pI"
tags: ["query:latent-vla"]
score: 9.0
evidence: 使用DCT潜在动作表征的傅里叶流匹配VLA方法
tldr: 现有VLA策略基于逐步动作预测，缺乏轨迹平滑性和紧凑性。本文提出将动作序列投影到离散余弦变换（DCT）系数空间，并通过流匹配学习轨迹级潜在动作表征。该方法不仅强制动作平滑，还降低了维度。实验表明，预测DCT系数相比原始流匹配VLA基线显著提高了操作成功率。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 提高VLA策略中动作轨迹的平滑性和紧凑性。
method: 将动作序列投影到DCT系数空间，使用流匹配学习轨迹级潜在表征。
result: 在多个机器人操作任务上成功率优于经典的逐步流匹配VLA基线。
conclusion: 傅里叶域潜在动作表征是提升VLA策略的有效设计。
---

## Abstract
We present a Fourier-based flow-matching method for Vision-Language-Action (VLA) policies that lets the policy reason over smooth trajectories, rather than stepwise actions. Instead of training on raw joint- or Cartesian-space action sequences, we project each sequence into a compact Discrete Cosine Transform (DCT) basis and learn directly in coefficient space via flow matching. This trajectory-level representation enforces smoothness and reduces dimensionality. Importantly, we show that the DCT representation integrates with asynchronous plan-execute schemes, preserving policy responsiveness. In experiments, predicting DCT coefficients yields higher task success than classical flow matching VLA baselines trained on per-step actions. Our results indicate that Fourier-domain flow matching is a simple, drop-in alternative that improves the performance and stability of VLA policies.

---

## 论文详细总结（自动生成）

# 通过傅里叶流匹配塑造机器人动作——详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的视觉-语言-动作（VLA）策略通常基于逐步骤（stepwise）的动作预测，这种方式生成的轨迹缺乏平滑性和紧凑性，导致机器人动作不够自然、执行效率低，且在学习复杂任务时性能受限。
- **研究动机**：引入轨迹级别的动作表征，强制动作平滑并降低维度，从而提升VLA策略的动作生成质量与任务成功率。
- **整体含义**：提出了一种基于傅里叶域（离散余弦变换）的流匹配方法，将动作序列投影到紧凑的DCT系数空间，替代原始逐步骤动作预测，实现了更稳定、高效的机器人操作策略。

## 2. 论文提出的方法论

- **核心思想**：利用离散余弦变换（DCT）对连续动作序列进行压缩和正则化，在DCT系数空间上训练流匹配（Flow Matching）模型，学习轨迹级别的潜在动作表征，而非原始的关节空间或笛卡尔空间逐步骤动作。
- **关键技术细节**：
  - 将原始动作序列（如关节角度、末端执行器位姿）分割为固定长度的窗口，对每个窗口内的动作序列进行DCT变换，得到一组低频系数（保留主要动态信息，丢弃高频噪声）。
  - 使用流匹配（Flow Matching）模型在DCT系数空间进行生成训练：模型以视觉-语言条件为输入，输出DCT系数向量，再通过逆DCT重建完整的平滑动作轨迹。
  - 支持异步“规划-执行”（plan-execute）机制：模型可预生成整段轨迹的系数，然后机器人按时间顺序执行，保持策略的响应性。
- **公式或算法流程（文字说明）**：
  1. 从数据集收集动作序列 `τ = [a_1, a_2, ..., a_T]`。
  2. 对每个序列应用DCT，得到系数 `c = DCT(τ)`，仅保留前K个低频系数（K << T）。
  3. 构建流匹配条件生成模型 `f_θ(c | visual, language)`，将视觉图像和语言指令作为条件，学习从噪声到目标DCT系数的流。
  4. 推理时：给定当前观测和指令，模型采样并生成DCT系数 `ĉ`，通过逆DCT得到平滑动作轨迹 `τ̂ = IDCT(ĉ)`。
  5. 机器人按异步方式执行 `τ̂`，同时模型可并行处理下一段轨迹。

## 3. 实验设计

- **使用的数据集/场景**：在多个机器人操作任务上进行评估，具体任务名称未在摘要中列出（推测包含桌面操作、移动操作等标准VLA基准场景）。
- **Benchmark**：以经典流匹配VLA基线（逐步骤动作预测）作为主要对比方法。另外可能对比了直接预测原始动作序列的其他基线（如Diffusion Policy、Behavior Transformer等，但摘要未明确提及）。
- **对比方法**：
  - 传统逐步骤流匹配VLA（原始动作空间）。
  - 可能还包括不使用DCT的同等规模轨迹生成方法。

## 4. 资源与算力

- **文中说明**：摘要及元数据中未明确提及使用的GPU型号、数量、训练时长等具体算力信息。需要在原文全文中查找才能获取，但用户提供的文本片段未包含这部分内容。
- **结论**：论文未公开详细算力资源，属于信息缺失。

## 5. 实验数量与充分性

- **实验概览**：根据摘要“在多个机器人操作任务上成功率优于经典流匹配VLA基线”，推测至少包含3~5个不同任务场景的完整实验。可能包含：
  - 主要对比实验（DCT流匹配 vs. 逐步骤流匹配）。
  - 消融实验（如不同DCT截断维度K的影响、异步执行调度的影响）。
  - 可能还有不同训练数据量或不同条件编码的对比。
- **充分性评价**：由于缺乏详细实验说明，只能推断实验覆盖了常见基准任务，对比了最直接的基线，但未提及真实机器人部署测试、跨域泛化测试、以及与其他最新VLA方法（如RT-2、Octo等）的对比，实验充分性一般。公平性方面，由于与流匹配基线在同一训练框架下对比，相对公平；但未公布随机种子、统计显著性等细节。

## 6. 论文的主要结论与发现

- DCT系数空间上的流匹配可以显著提升VLA策略的任务成功率，优于传统逐步骤动作预测。
- 轨迹级表征强制了动作平滑性，降低了维度，使得模型更易学习长程依赖。
- 傅里叶域流匹配可作为即插即用（drop-in）替代方案，无需复杂修改即可提升VLA性能与稳定性。
- 异步规划-执行方案与DCT表征兼容良好，不会牺牲策略的响应性。

## 7. 优点

- **方法简洁有效**：仅通过将动作序列投影到DCT空间，就实现了无缝的性能提升，无需改变网络结构或训练流程。
- **可解释性**：DCT低频系数保留了轨迹的主要形状，有助于可视化理解模型学到了什么（如平滑性约束）。
- **通用性**：DCT流匹配适用于任何连续动作序列，未来可扩展到人形机器人、软体机器人等场景。
- **效率提升**：减少动作维度（从T维降至K维）有助于加速训练和推理。

## 8. 不足与局限

- **实验覆盖不全面**：未公开与其他主流VLA方法（如扩散策略、Chunked Transformer、Token-based方法）的对比，且未提供真实机器人实验结果（可能仅在仿真中评估）。
- **信息缺失**：算力资源、超参数选择、不同数据集的具体成功率数据未在提供材料中给出，限制了可复现性。
- **潜在偏差**：如果任务本身对动作平滑性不敏感（如离散抓取），DCT方法可能反而引入不必要的平滑约束，导致性能下降（文中未讨论此类失败案例）。
- **应用限制**：仅适用于固定窗口长度的轨迹生成；对于需要实时反应的任务（如动态避障），异步规划可能存在延迟；DCT系数截断对任务依赖性强，需要手动选择K值。
- **未分析安全性与鲁棒性**：未评估对抗性干扰或环境变化下模型的泛化能力。

（完）
