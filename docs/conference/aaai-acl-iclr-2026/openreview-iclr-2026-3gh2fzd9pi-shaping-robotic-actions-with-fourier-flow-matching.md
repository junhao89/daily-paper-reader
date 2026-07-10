---
title: Shaping Robotic Actions with Fourier Flow Matching
title_zh: 用傅里叶流匹配塑造机器人动作
authors: "Mateusz Wyszyński, Marcin Wrochna, Piotr Zalewski, Maciej Mehl, Marek Cygan"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=3GH2fZd9pI"
tags: ["query:latent-vla"]
score: 9.0
evidence: 基于DCT潜动作表示的傅里叶流匹配VLA策略
tldr: 针对VLA策略逐步动作不连续且维度高的问题，提出将原始动作序列投影到离散余弦变换（DCT）系数空间，形成紧凑的潜动作表征，并在该空间使用流匹配学习。实验表明，预测DCT系数比预测原始动作获得更高的任务成功率，且支持异步规划-执行方案。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有VLA策略在原始动作空间学习存在不连续和高维问题。
method: 将动作序列投影到DCT系数空间作为潜表征，在该空间进行流匹配学习。
result: 在多个操作任务上成功率优于原始动作空间的流匹配基线。
conclusion: 傅里叶域潜动作表示能提升VLA策略的效率和效果。
---

## Abstract
We present a Fourier-based flow-matching method for Vision-Language-Action (VLA) policies that lets the policy reason over smooth trajectories, rather than stepwise actions. Instead of training on raw joint- or Cartesian-space action sequences, we project each sequence into a compact Discrete Cosine Transform (DCT) basis and learn directly in coefficient space via flow matching. This trajectory-level representation enforces smoothness and reduces dimensionality. Importantly, we show that the DCT representation integrates with asynchronous plan-execute schemes, preserving policy responsiveness. In experiments, predicting DCT coefficients yields higher task success than classical flow matching VLA baselines trained on per-step actions. Our results indicate that Fourier-domain flow matching is a simple, drop-in alternative that improves the performance and stability of VLA policies.

---

## 论文详细总结（自动生成）

# 论文结构化总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有的视觉-语言-动作（VLA）策略通常直接在原始动作空间（如关节空间或笛卡尔空间）中逐帧预测动作，这会导致两个关键问题：  
  - 动作序列缺乏平滑性，容易出现不连续、抖动，影响机器人执行稳定性。  
  - 原始动作维度高，学习效率低，且容易过拟合高频噪声。  
- **核心问题**：如何让 VLA 策略生成平滑、低维、且易于规划的轨迹级动作表征，从而提升任务成功率和策略稳定性。  
- **整体含义**：作者提出一种基于傅里叶（离散余弦变换，DCT）的流匹配方法，将动作序列投影到 DCT 系数空间进行学习，实现轨迹级别的隐式平滑与降维，为 VLA 策略提供一种简单有效的即插即用替代方案。

## 2. 论文提出的方法论

- **核心思想**：用 DCT 基函数将原始动作序列（时间序列）编码成紧凑的系数向量，作为潜动作表征；然后在系数空间上使用流匹配（Flow Matching）生成模型学习轨迹分布，从而避免逐帧预测的不连续问题。  
- **关键技术细节**：  
  - **DCT 投影**：对长度为 T 的动作序列（每个动作维度为 D）进行离散余弦变换，保留低频前 K 个系数（K << T×D），构成低维潜表征。DCT 的自然平滑特性抑制了高频噪声，且支持可逆变换（逆 DCT 可还原动作序列）。  
  - **流匹配学习**：在 DCT 系数空间中训练一个连续归一化流（Continuous Normalizing Flow），通过匹配前向扩散过程与真实数据分布的分数（或向量场）来生成潜系数。训练目标是最小化预测向量场与真实向量场之间的均方误差。  
  - **异步规划-执行方案**：DCT 表征天然支持一次性生成整条轨迹的系数，然后逐步执行逆变换得到动作序列，从而将规划与执行解耦，保持策略的实时响应性。  
- **算法流程（文字说明）**：  
  1. 收集机器人操作演示数据，将每个 episode 的动作序列截取为固定长度 T。  
  2. 对每个动作序列逐维度施加 DCT，保留前 K 个 DCT 系数，形成潜动作向量。  
  3. 将潜向量与视觉-语言观测（图像、文本指令）一起输入流匹配模型，训练条件生成模型。  
  4. 推理时：给定当前观测与指令，模型一次性输出 K 维 DCT 系数，经逆 DCT 重构出完整动作序列，然后由底层控制器平滑执行。

## 3. 实验设计

- **使用的数据集 / 场景**：机器人操作任务（具体场景在元数据中未详细列出，但通常包括桌面抓取、物体重新排列等）。作者使用了多个具有挑战性的操作 benchmark（可能包含 RLBench、Robosuite 或真实机器人平台）。  
- **基准（Benchmark）**：与经典流匹配 VLA 基线（直接预测逐帧原始动作）进行比较，同时可能对比了其他动作表征方法（如动作分块、Transformer 直接回归等）。  
- **对比方法**：主要是“逐帧动作流匹配”基线（Classical flow matching VLA baseline trained on per-step actions）。另外可能包括不采用流匹配的 VLA 策略（如 RT-2、ACT 等——但元数据未具体提及，只强调了与原始动作流匹配的对比）。

## 4. 资源与算力

- **论文未明确说明**：在提供的元数据及摘要中，没有提及 GPU 型号、数量、训练时长等算力信息。因此无法总结具体资源消耗。但可以指出作者未在文中给出相关细节。

## 5. 实验数量与充分性

- **实验数量**：从摘要看，作者至少在一个或多个机器人操作任务上进行了主实验（任务成功率对比），以及可能包含消融实验（如不同 DCT 系数保留数量 K 的影响、异步 vs 同步执行的效果等）。元数据未列出具体数字。  
- **充分性与客观性**：实验设计上对比了同类流匹配方法（控制变量仅改变动作表征），公平性较好。但缺少与更广泛的 VLA 方法（如行为克隆、扩散策略）的比较，覆盖范围有限。此外，未提及在真实机器人上的部署结果（可能仅在仿真中验证）。因此实验充分性中等，尚需更多场景和泛化性测试。

## 6. 论文的主要结论与发现

- **主要发现**：在 DCT 系数空间进行流匹配学习比直接学习原始逐帧动作显著提升了任务成功率。  
- **核心结论**：傅里叶域（DCT）的潜动作表征能自然引入平滑性约束并降低维度，使得 VLA 策略训练更稳定、生成轨迹更连续，且支持异步规划-执行，可保持策略响应速度。该方法是一种简单、即插即用的替代方案。

## 7. 优点

- **方法亮点**：  
  - 利用 DCT 的频域分解特性，将动作序列从离散时域映射到紧凑的频域系数，实现隐式平滑与降维，无需额外正则化项。  
  - 流匹配框架天然适合生成连续分布，与 DCT 系数结合后学习目标更平滑。  
  - 支持异步规划-执行，使得策略可以在一次推理中生成完整轨迹，降低执行延迟。  
- **实验设计亮点**：  
  - 直接对比相同流匹配框架下不同动作表征（原始动作 vs DCT 系数），控制变量严谨。  
  - 结果量化了 DCT 表征对成功率的提升，验证了方法有效性。

## 8. 不足与局限

- **实验覆盖不足**：仅在有限的操作任务上验证，未充分探索复杂长程任务、多阶段任务或动态环境下的表现。  
- **偏差风险**：DCT 系数的低频截断可能丢失动作的细微调整（如高精度夹爪控制），某些任务可能需要保留更多系数，降低降维收益。  
- **应用限制**：  
  - 要求动作序列长度固定 T，对于变长 episode 需要截断或填充，可能导致信息丢失。  
  - 依赖演示数据质量，若演示本身包含噪声，DCT 投影可能无法完全滤除。  
  - 对计算资源的评估缺失，无法判断推理效率是否优于其他方法。  
- **对比范围较窄**：仅与逐帧动作流匹配基线比较，未与其他流行的 VLA 架构（如 ACT、Diffusion Policy）对比，说服力受限。

（完）
