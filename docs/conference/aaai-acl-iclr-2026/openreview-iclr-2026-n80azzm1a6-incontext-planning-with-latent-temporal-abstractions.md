---
title: In‑Context Planning with Latent Temporal Abstractions
title_zh: 上下文潜在时间抽象规划
authors: "Baiting Luo, Yunuo Zhang, Nathaniel S Keplinger, Samir Gupta, Abhishek Dubey, Ayan Mukhopadhyay"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=n80azZM1A6"
tags: ["query:latent-vla"]
score: 8.0
evidence: 通过RQ-VAE学习离散潜在动作令牌，用于上下文规划
tldr: I-TAP使用残差量化VAE（RQ-VAE）将观察-宏动作序列离散化为粗到细的残差令牌，并在潜在时间抽象空间中通过Transformer自回归预测这些令牌，实现在线上下文规划，有效应对长时间尺度和部分可观测性。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 原始时间尺度规划导致上下文长度和分支因子爆炸，且动态部分可观测。
method: 学习观测条件RQ-VAE将观察-宏动作序列离散为残差令牌，用Transformer自回归预测。
result: 在连续控制任务中，I-TAP实现了有效的在线规划与上下文适应。
conclusion: 潜在时间抽象令牌化可显著提升规划效率。
---

## Abstract
Planning-based reinforcement learning in real-world control faces two coupled obstacles: planning at primitive time scales explodes both context length and branching factor, and the underlying dynamics are often only partially observable. We introduce the In-Context Latent Temporal Abstraction Planner (I-TAP), which unifies in-context adaptation and online planning in a learned latent temporal-abstraction space. From offline trajectories, I-TAP learns an observation-conditioned residual-quantization VAE (RQ-VAE) that discretizes observation–macro-action sequences into a coarse-to-fine stack of residual tokens, together with a residual-quantized temporal Transformer that autoregressively predicts these tokens from recent observation and macro-action histories. This sequence model serves jointly as a context-conditioned prior over abstract actions and a latent-space dynamics model. At inference, I-TAP plans with Monte Carlo Tree Search directly in token space, leveraging short histories to implicitly infer latent factors without any test-time fine-tuning. Across deterministic and stochastic MuJoCo locomotion and high-dimensional Adroit manipulation, including partially observable variants, I-TAP consistently matches or outperforms strong model-free and model-based baselines, demonstrating effective in-context planning under stochastic dynamics and partial observability.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现实世界的强化学习（RL）控制面临两个耦合的挑战：一是原始时间尺度规划导致上下文长度和分支因子爆炸，二是底层动力学通常仅部分可观测。传统规划方法难以应对长时间尺度下的复杂控制和部分可观测性。
- **核心问题**：如何在不进行测试时微调的情况下，实现高效在线规划并自动适应动态变化和部分可观测环境。
- **整体含义**：提出一种在潜在时间抽象空间中进行上下文内规划的方法（I-TAP），通过离散化的潜在动作令牌和自回归Transformer，将规划从原始时间尺度提升到抽象的粗到细层次，显著提升规划效率和适应性。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：学习一个观测条件化的残差量化VAE（RQ-VAE），将观测-宏动作序列离散化为粗到细的残差令牌堆栈，并用一个残差量化时间Transformer自回归预测这些令牌。该序列模型同时作为上下文条件先验（对抽象动作）和潜在空间动力学模型。在推理时，I-TAP直接在令牌空间中使用蒙特卡洛树搜索（MCTS）进行规划，利用短历史隐式推断潜在因子，无需测试时微调。
- **关键技术细节**：
  - **RQ-VAE**：将连续观测-宏动作序列编码为离散的残差令牌，由粗到细层次化表示。
  - **残差量化时间Transformer**：基于历史观测和宏动作序列自回归预测残差令牌，提供上下文条件先验和动力学模型。
  - **在线MCTS规划**：在令牌空间（即潜在时间抽象空间）中进行搜索，利用短历史进行部分可观测性的隐式推断。
- **算法流程**（文字说明）：
  1. 离线数据中训练RQ-VAE和Transformer。
  2. 推理时，使用当前和近期观测-宏动作历史，由Transformer生成令牌先验。
  3. 在令牌空间运行MCTS，搜索最优令牌序列。
  4. 将令牌解码为实际动作执行。
  5. 循环步骤2-4进行在线规划。

## 3. 实验设计：使用了哪些数据集/场景，benchmark，对比方法

- **任务场景**：包括确定性和随机性的MuJoCo运动任务（locomotion）以及高维Adroit操控任务（manipulation），还包含部分可观测变体（partially observable variants）。
- **Benchmark**：未明确指定单一基准，但对比了强模型无关（model-free）和基于模型（model-based）的基线方法。
- **对比方法**：未在摘要中列出具体基线名称，但明确提到"strong model-free and model-based baselines"。

## 4. 资源与算力

- **声明**：论文摘要和提供的元数据中未明确提及使用的GPU型号、数量、训练时长等资源信息。可以推断该论文来自ICLR 2026，可能未在摘要中详细说明计算资源。

## 5. 实验数量与充分性

- **实验组数**：覆盖了多种任务类型：确定性/随机性运动、高维操控、部分可观测变体。至少包含4组不同场景（两组MuJoCo不同随机性，一组Adroit，一组部分可观测，可能还有消融实验）。具体消融实验未在摘要描述，但元数据提及"motivation: 原始时间尺度规划导致上下文长度和分支因子爆炸"，暗示可能有消融分析。
- **充分性与公平性**：实验覆盖了连续控制中典型的难易场景，对比多个基线，结论显示匹配或超越。但摘要未提供统计显著性、超参数分析等细节，没有明确提到基线是否公平调优。整体实验设计较充分，但无法完全判断公平性。

## 6. 论文的主要结论与发现

- I-TAP在多种连续控制任务（确定性和随机性运动、高维操控、部分可观测）中一致匹配或超越强模型无关和基于模型的基线，证明了其在随机动力学和部分可观测性下有效的上下文内规划能力。
- 潜在时间抽象令牌化可显著提升规划效率，减轻原始时间尺度规划的上下文长度和分支因子爆炸问题。
- 无需测试时微调，利用短历史即可隐式推断潜在因子。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：将RQ-VAE与Transformer结合，构建粗到细的潜在时间抽象空间，同时作为先验和动力学模型，统一上下文适应与在线规划。
- **高效规划**：直接在离散令牌空间进行MCTS，避免原始连续动作空间的搜索爆炸，利用层次化表示提升效率。
- **部分可观测性处理**：通过短历史隐式编码潜在因子，无需显式状态估计或额外记忆机制。
- **实验覆盖多样性**：包括确定性/随机性、高维、部分可观测等挑战性场景，验证方法鲁棒性。

## 8. 不足与局限

- **实验细节缺失**：未在摘要中提供具体基线名称、超参数设置、运行次数、统计显著性等信息，完整论文中需补充。
- **资源消耗未报告**：缺乏训练和推理的计算成本，难以评估实际部署可行性。
- **应用限制**：方法依赖离线轨迹学习RQ-VAE和Transformer，需要高质量离线数据；令牌空间搜索可能受限于离散化粒度；未在真实机器人或更复杂部分可观测环境中验证。
- **偏差风险**：只对比了MuJoCo和Adroit的连续控制任务，未涉及离散动作空间或混合任务，通用性有待进一步验证。
- **消融实验不明**：元数据提及动机中包含原始时间尺度规划的对比，但摘要未说明是否进行消融以分离各组件（RQ-VAE vs 直接VAE、Transformer vs 其他序列模型）的贡献。

（完）
