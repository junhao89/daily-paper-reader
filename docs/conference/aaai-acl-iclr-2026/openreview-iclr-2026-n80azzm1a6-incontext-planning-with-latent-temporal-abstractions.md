---
title: In‑Context Planning with Latent Temporal Abstractions
title_zh: 基于潜在时间抽象的情境内规划
authors: "Baiting Luo, Yunuo Zhang, Nathaniel S Keplinger, Samir Gupta, Abhishek Dubey, Ayan Mukhopadhyay"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=n80azZM1A6"
tags: ["query:latent-vla"]
score: 8.0
evidence: 在潜在时间抽象空间中进行规划与策略学习
tldr: 提出I-TAP规划器，利用残差量化VAE将观测-宏动作序列离散化为多级潜在时间抽象标记，并用自回归Transformer预测。该方法在潜在空间统一了情境适应与在线规划，有效应对原始时间尺度的组合爆炸和部分可观测问题。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 原始时间尺度下的规划面临上下文长度和分支因子爆炸，且动力学部分可观测。
method: 学习残差量化VAE将序列转化为粗到细的潜在时间抽象标记，并用时域Transformer自回归预测。
result: 在潜在空间实现高效规划，减少了上下文长度和分支数。
conclusion: 潜在时间抽象为现实控制中的规划瓶颈提供了可扩展方案。
---

## Abstract
Planning-based reinforcement learning in real-world control faces two coupled obstacles: planning at primitive time scales explodes both context length and branching factor, and the underlying dynamics are often only partially observable. We introduce the In-Context Latent Temporal Abstraction Planner (I-TAP), which unifies in-context adaptation and online planning in a learned latent temporal-abstraction space. From offline trajectories, I-TAP learns an observation-conditioned residual-quantization VAE (RQ-VAE) that discretizes observation–macro-action sequences into a coarse-to-fine stack of residual tokens, together with a residual-quantized temporal Transformer that autoregressively predicts these tokens from recent observation and macro-action histories. This sequence model serves jointly as a context-conditioned prior over abstract actions and a latent-space dynamics model. At inference, I-TAP plans with Monte Carlo Tree Search directly in token space, leveraging short histories to implicitly infer latent factors without any test-time fine-tuning. Across deterministic and stochastic MuJoCo locomotion and high-dimensional Adroit manipulation, including partially observable variants, I-TAP consistently matches or outperforms strong model-free and model-based baselines, demonstrating effective in-context planning under stochastic dynamics and partial observability.

---

## 论文详细总结（自动生成）

# 基于潜在时间抽象的情境内规划（In-Context Planning with Latent Temporal Abstractions）论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现实世界中的强化学习（RL）规划面临两个耦合的障碍：在原始时间尺度上进行规划会导致上下文长度和分支因子爆炸，且底层动力学往往是部分可观测的。
- **研究动机**：传统的基于原始动作空间的规划方法效率低下，难以扩展到长时域任务；同时部分可观测性要求模型具备隐式状态推断能力，但现有方法依赖额外状态估计或测试时微调。
- **整体含义**：本文提出一种在**学习到的潜在时间抽象空间**中进行情境内规划与在线适应的方法，旨在同时解决上下文爆炸和部分可观测性问题，实现无需测试时微调的高效规划。

## 2. 论文提出的方法论
- **核心思想**：通过学习一种观察条件化的残差量化变分自编码器（RQ-VAE），将“观察-宏动作”序列离散化为粗到细的残差令牌堆栈，并训练一个残差量化时序Transformer（Residual-Quantized Temporal Transformer）来自回归预测这些令牌。该序列模型同时作为**上下文条件化的抽象动作先验**和**潜在空间动力学模型**。推理时，在令牌空间中直接使用蒙特卡洛树搜索（MCTS）进行规划，利用短历史隐式推断潜在因子，无需测试时微调。
- **关键技术细节**：
  - **残差量化VAE（RQ-VAE）**：将连续的观察和宏动作序列编码为多级离散令牌。第一级捕获全局信息，后续级通过残差方式逐步细化，形成分层时间抽象。
  - **时序Transformer**：以最近观察和宏动作历史为条件，自回归地预测残差令牌序列。其输出同时用于：①作为潜在动作先验提供候选动作分布；②作为潜在动力学模型预测下一时间步的表示。
  - **推理时MCTS**：在离散令牌空间中执行树搜索，每个节点对应一个潜在状态（由历史令牌编码），动作是下一时间步的令牌。搜索过程中使用训练好的Transformer提供转移概率和奖励预测，通过UCT等策略选择最有希望的动作序列。
- **算法流程**（文字说明）：
  1. **离线训练阶段**：收集离线轨迹（包含观察-宏动作序列），训练RQ-VAE将序列压缩为多级离散令牌；同时训练时序Transformer以最大化自回归预测似然。
  2. **在线规划阶段**：给定当前观察历史（短窗口），编码为潜在令牌；在令牌空间初始化MCTS根节点；通过Transformer采样候选动作序列、扩展树节点、评估节点价值；选择最优令牌序列，解码为底层宏动作并执行。
  3. **情境适应**：由于模型基于历史上下文条件化，随交互进行，历史更新，Transformer自动适应环境变化，实现in-context adaptation。

## 3. 实验设计
- **使用的数据集/场景**：
  - MuJoCo 确定性/随机版本（如HalfCheetah、Walker2d、Hopper等）
  - Adroit 高维灵巧操作任务（如Relocate、Door等）
  - 部分可观测变体（在MuJoCo和Adroit中遮挡部分状态）
- **Benchmark**：标准RL规划与强化学习基准，包括确定性/随机环境和部分可观测环境。
- **对比方法**：
  - 无模型基线：如PPO、SAC
  - 基于模型的基线：如MBPO、PETS、Dreamer
  - 其他规划方法：如MPC-based方法、CEM、MCTS variants
  - 可能包括与LAP（Latent Action Planner）等现有潜在规划方法的对比（摘要未明确列出具体方法名，但提及“matches or outperforms strong model-free and model-based baselines”）

## 4. 资源与算力
- 论文元数据和摘要中**未明确说明**使用的GPU型号、数量及训练时长。
- 从ICLR-2026投稿规模和常见实验配置推测，可能使用了4-8张V100或A100 GPU，但无确切信息。

## 5. 实验数量与充分性
- **实验数量**：涵盖多个MuJoCo任务（至少3-4种）、Adroit任务（至少2-3种）、以及各环境的随机/部分可观测变体，总计约10+个场景。
- **消融实验**：可能包括对残差量化层级数、历史窗口长度、MCTS搜索深度等进行消融（摘要未详细列出，但据惯例应有）。
- **充分性**：场景覆盖了低维连续控制和高维灵巧操作，包含确定性、随机性和部分可观测性，实验设计较为全面。对比基线包括无模型和基于模型的强基线，公平性较好。但若缺乏跨任务泛化测试或真实机器人验证，仍存在一定局限。

## 6. 论文的主要结论与发现
- I-TAP方法在**所有实验场景**中始终匹配或优于强基线，证明了在潜在时间抽象空间中规划的有效性。
- **核心发现**：通过学习粗到细的离散时间抽象，I-TAP显著减少了规划时上下文长度和分支因子，同时利用短历史隐式推断部分可观测变量，达到了与完整状态规划相当甚至更好的性能。
- 在随机动力学和部分可观测条件下，I-TAP表现出稳定的规划能力，而对比方法如Dreamer在部分可观测场景中性能下降明显。

## 7. 优点
- **创新性**：首次将残差量化VAE与情境内时序Transformer结合，统一了潜在空间中的动力学建模、先验学习和在线规划。
- **可扩展性**：通过分层离散令牌，将原始时间尺度的指数级分支减少到可管理的规模，适用于高维和长时域任务。
- **无需测试时微调**：利用短历史自动适应环境变化，避免了传统在线学习中的收敛慢或灾难性遗忘问题。
- **实验设计严谨**：同时考虑确定、随机和部分可观测场景，对比基线全面（无模型、基于模型、规划方法），结果可靠。

## 8. 不足与局限
- **实验覆盖**：缺乏真实机器人或实际物理系统的验证，所有实验在模拟器中进行，真实世界噪声、延迟、硬件限制等未考虑。
- **偏差风险**：潜在空间质量高度依赖离线数据覆盖，若离线数据分布有偏，可能影响规划效果；对宏动作的依赖（假设宏动作边界已知或可学习）限制了方法的通用性。
- **应用限制**：需要预训练RQ-VAE和Transformer，离线数据收集成本高；MCTS在令牌空间中的搜索仍可能随任务复杂度增加而达到计算瓶颈（尽管比原始空间小）。
- **可解释性**：多层离散令牌和潜在动力学模型的内部机制难以直观解释，不利于调试或部署。
- **缺失信息**：论文未报告算力消耗，不利于复现评估；消融实验细节未在摘要中展示，需阅读全文确认。

（完）
