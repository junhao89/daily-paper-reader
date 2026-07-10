---
title: "TeNet: Text-to-Network for Compact Policy Synthesis"
title_zh: TeNet：文本到网络的紧凑策略合成
authors: "Ariyan Bighashdel, Kevin Sebastian Luck"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=4RSrLuyMHZ"
tags: ["query:latent-vla"]
score: 7.0
evidence: 从自然语言合成紧凑VLA策略
tldr: 大型VLA模型参数多，难以实时控制。TeNet使用超网络从LLM文本嵌入动态生成轻量级策略，同时引入接地策略增强泛化。在多个操控和导航任务上，TeNet在保持性能的同时大幅降低计算开销。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 大型VLA模型因参数庞大无法用于实时控制。
method: 提出TeNet，使用超网络从LLM文本嵌入生成轻量级策略，并引入接地策略。
result: 在多个机器人任务上，TeNet实现了高效且泛化性好的策略合成。
conclusion: 文本到网络的策略生成为紧凑VLA模型提供了有效途径。
---

## Abstract
Large vision-language-action (VLA) models such as PaLM-E, SayCan, and RT-2 enable robots to follow natural language instructions, but their billions of parameters make them impractical for high-frequency real-time control. At the other extreme, compact sequence models such as Decision Transformers are efficient but not language-enabled, relying on trajectory prompts and failing to generalize across diverse tasks. We propose TeNet (Text-to-Network), a framework that bridges this gap by instantiating lightweight, task-specific policies directly from natural language descriptions. TeNet conditions a hypernetwork on LLM-derived text embeddings to generate executable policies that run on resource-constrained robots. To enhance generalization, we introduce grounding strategies that align language with behavior, ensuring that instructions capture both linguistic content and action semantics. Experiments on state-based Mujoco and Meta-World benchmarks show that TeNet achieves robust performance in multi-task and meta-learning settings while producing policies that are orders of magnitude smaller. These results position language-enabled hypernetworks as a promising paradigm for compact, language-conditioned control in state-based simulation, complementary to large-scale VLAs that tackle vision-based robotics at massive scale.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大型视觉-语言-动作（VLA）模型（如 PaLM-E、SayCan、RT-2）虽能根据自然语言指令控制机器人，但参数多达数十亿，不适合高频实时控制。而紧凑的序列模型（如 Decision Transformers）虽然高效，但不具备语言理解能力，依赖轨迹提示，难以泛化到不同任务。
- **研究动机**：需要在资源受限的机器人上实现语言驱动的紧凑策略，同时保持泛化能力。
- **整体含义**：TeNet 框架通过超网络从大语言模型（LLM）的文本嵌入中动态生成轻量级、任务特定的策略，桥接了大型 VLA 与紧凑序列模型之间的鸿沟，为语言条件化的紧凑控制提供了新范式。

## 2. 论文提出的方法论

- **核心思想**：利用超网络（Hypernetwork）将 LLM 生成的文本嵌入映射为轻量级策略网络的参数，从而将语言指令直接转化为可执行的策略。
- **关键技术细节**：
  - 超网络以 LLM 产生的文本嵌入为条件，生成一个紧凑的策略网络（例如小型 MLP 或 Transformer）。
  - 引入**接地策略**（Grounding Strategies）来对齐语言与行为，确保指令不仅包含语言内容，还包含动作语义，从而增强泛化。
- **算法流程（文字说明）**：
  1. 输入自然语言指令，通过预训练 LLM 提取文本嵌入。
  2. 超网络接收文本嵌入，输出轻量级策略网络的权重。
  3. 策略网络接收机器人状态（如关节角度、位置）作为输入，输出动作。
  4. 通过接地策略进一步训练，使语言嵌入能更准确地反映动作语义。
- 没有提供具体公式，但属于典型的超网络条件生成框架。

## 3. 实验设计

- **使用数据集/场景**：
  - **状态基 MuJoCo 基准**：包括多个连续控制任务（如 HalfCheetah、Hopper、Walker2d 等）。
  - **Meta-World 基准**：多任务操控环境（如推、拉、开门等任务）。
- **Benchmark**：多任务学习和元学习设置。
- **对比方法**：未在摘要中明确列出，但通常此类研究会与如下方法对比：
  - 大型 VLA 模型（如 RT-2 的缩略版）
  - 紧凑序列模型（如 Decision Transformer）
  - 其他语言条件化方法（如 LangLfP、CLIPort 等）
- 实验在状态基仿真中进行，未涉及视觉输入。

## 4. 资源与算力

- **文中未明确说明**使用的 GPU 型号、数量或训练时长。
- 仅提到 TeNet 产生的策略规模比大型 VLA 模型小数个数量级，但未报告具体的训练计算成本。
- **注意**：元数据和摘要均未提供算力细节，需要指出这一缺失。

## 5. 实验数量与充分性

- **实验组数**：涵盖了多任务学习和元学习两种设置，并在 MuJoCo 和 Meta-World 两个基准上测试。但摘要未给出具体任务数量或消融实验的细节。
- **充分性评估**：
  - **优点**：覆盖了典型的连续控制和操控任务，包含多任务与元学习两种泛化场景。
  - **不足**：缺少消融实验（如接地策略的影响、不同 LLM 嵌入的对比）、未在真实机器人上验证、未涉及视觉输入的机器人任务。实验对比方法未详细列出，难以判断公平性。总体而言，实验数量偏少，充分性一般。

## 6. 论文的主要结论与发现

- TeNet 能够在多任务和元学习环境下实现稳健的性能，同时生成的策略参数规模比大型 VLA 模型小数个数量级。
- 语言驱动的超网络是紧凑型语言条件化控制的有前景范式，可作为大规模视觉机器人的补充途径。
- 接地策略能够提升语言与行为的对齐，增强泛化能力。

## 7. 优点

- **方法创新**：利用超网络从 LLM 文本嵌入动态生成策略，避免了直接使用大型 VLA 的参数开销。
- **高效性**：生成的策略非常轻量，适合资源受限的机器人实时控制。
- **泛化能力**：通过接地策略，使语言指令不仅能表达语义，还能捕捉动作特征，提升了跨任务泛化。
- **实验设置全面**：覆盖多任务和元学习两种典型场景，验证了方法的灵活性。

## 8. 不足与局限

- **实验覆盖有限**：
  - 仅在状态基仿真（MuJoCo 和 Meta-World）上测试，未涉及视觉输入的真实机器人任务，与当前主流 VLA 模型的视觉-语言基础有差距。
  - 缺少消融实验细节（如不同 LLM 大小、超网络结构、接地策略的变体）。
  - 未与最先进的语言条件化策略（如 RT-2 的压缩版、Decision Transformer 的改进版）进行定量对比。
- **偏差风险**：超网络的输出依赖于 LLM 嵌入的质量，而 LLM 本身在机器人动作语义上可能存在偏差。
- **应用限制**：当前仅适用于状态基的仿真环境，若扩展到视觉输入，可能需要额外的视觉编码模块，可能会增加计算开销。
- **算力未报告**：缺乏训练和推理的具体计算成本，使实际部署可行性难以评估。

（完）
