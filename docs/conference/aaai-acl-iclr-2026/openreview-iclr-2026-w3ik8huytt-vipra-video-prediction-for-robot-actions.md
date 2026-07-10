---
title: "ViPRA: Video Prediction for Robot Actions"
title_zh: 面向机器人动作的视频预测
authors: "Sandeep Routray, Hengkai Pan, Unnat Jain, Shikhar Bahl, Deepak Pathak"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=w3Ik8HUyTT"
tags: ["query:latent-vla"]
score: 8.0
evidence: 从视频中学习潜在动作用于机器人策略
tldr: 该论文提出ViPRA框架，从无动作标签的视频中学习连续机器人控制。通过训练视频-语言模型同时预测未来视觉观察和运动中心的潜在动作，利用感知损失和光流一致性确保物理合理性，最终将潜在动作用于下游策略学习，实现了从被动视频到主动控制的知识迁移。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 大量机器人视频无动作标签，限制了其在策略学习中的应用。
method: 训练视频-语言模型预测未来帧和潜在动作，潜在动作通过光流损失和感知损失学习物理一致性。
result: 在多个机器人操控任务上，ViPRA从无标签视频中学习到有效的策略，性能优于纯模仿学习。
conclusion: 潜在动作作为中间表征能够从视频中提取物理动态，为无标签视频数据用于机器人学习提供了可行的路径。
---

## Abstract
Can we turn a video prediction model into a robot policy? Videos, including those of humans or teleoperated robots, capture rich physical interactions. However, most of them lack labeled actions, which limits their use in robot learning. We present *Video Prediction for Robot Actions* (**ViPRA**), a simple pretraining-finetuning framework that learns continuous robot control from these actionless videos. Instead of directly predicting actions, we train a video-language model to predict *both future visual observations and motion-centric latent actions*, which serve as intermediate representations of scene dynamics. We train these latent actions using perceptual losses and optical flow consistency to ensure they reflect physically grounded behavior. For downstream control, we introduce a chunked *flow-matching decoder* that maps latent actions to robot-specific continuous action sequences, using only 100 to 200 teleoperated demonstrations. This approach avoids expensive action annotation, supports generalization across embodiments, and enables smooth, high-frequency continuous control upto 22 Hz via chunked action decoding. Unlike prior latent action works that treat pretraining as autoregressive policy learning, ViPRA explicitly models both what changes and how. Our method outperforms strong baselines, with a 16% gain on the SIMPLER benchmark and a 13% improvement across real world manipulation tasks. We have released models and code [here](https://vipra-project.github.io/).

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：机器人视频数据大多缺乏动作标签，难以直接用于策略学习。能否从这些无动作标签的视频（如人类或遥操作机器人视频）中提取物理动态信息，从而训练出可泛化的机器人控制策略？
- **研究动机**：大量机器人视频（包括人类演示）包含了丰富的物理交互信息，但缺乏动作标注，限制了其在机器人学习中的应用。现有方法要么依赖昂贵的动作标注，要么将预训练视为自回归策略学习，未能显式建模“场景中什么在变化”以及“如何变化”。
- **整体含义**：ViPRA 提出一种简单的预训练-微调框架，从无标签视频中学习连续的机器人控制，将视频预测模型转化为机器人策略，为利用海量无标签视频数据提供了可行路径。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程（用文字说明）

- **核心思想**：训练视频-语言模型同时预测未来视觉观察和运动中心的**潜在动作**，将潜在动作作为场景动态的中间表征。之后通过一个**分块流匹配解码器**将潜在动作映射为机器人特定的连续动作序列。
- **关键技术细节**：
  1. **视频-语言模型预测未来帧和潜在动作**：模型接收当前观察和语言指令，输出未来帧和潜在动作（latent action），潜在动作不是直接预测机器人关节命令，而是捕获场景中物理变化的抽象表示。
  2. **训练潜在动作的损失函数**：使用**感知损失**（perceptual loss）和**光流一致性**（optical flow consistency）来确保潜在动作反映物理上合理的行为。感知损失基于特征空间的距离，光流一致性约束预测帧的光流与潜在动作的对应关系。
  3. **分块流匹配解码器**：微调阶段，仅需100-200个遥操作演示，学习一个解码器将潜在动作映射为机器人连续动作序列。该解码器采用分块（chunked）方式，使得控制频率可达22 Hz，支持平滑高频控制。
- **算法流程**：
  - 预训练阶段：在大规模无标签视频数据上训练联合预测模型（视频-语言模型），输出未来帧和潜在动作。
  - 微调阶段：使用少量带动作标签的演示，训练流匹配解码器将潜在动作解码为具体机器人动作。
  - 推理阶段：输入当前观测和语言指令，模型先生成潜在动作，再经解码器输出连续动作序列，执行控制。

### 3. 实验设计：使用了哪些数据集/场景，它的 benchmark 是什么，对比了哪些方法

- **数据集/场景**：
  - SIMPLER 基准测试（模拟环境中的操控任务）
  - 真实世界操控任务（多种实际机器人操作场景）
  - 使用无标签视频数据进行预训练（可能包含人类视频或遥操作视频，但具体数据集名称未在摘要中明确，元数据也未列出）。
- **Benchmark**：
  - SIMPLER benchmark（模拟评估）
  - 真实世界操控任务（实际机器人评估）
- **对比方法**：
  - 强基线方法（具体名称未在摘要中给出，但提及 outperforms strong baselines with 16% gain on SIMPLER benchmark and 13% improvement across real world manipulation tasks）。推测对比了纯模仿学习、其他潜在动作方法（如之前的工作）等。

### 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。

- **文中未明确说明**：摘要和元数据中未提及具体使用的GPU型号、数量及训练时长。无法提供具体算力信息。需要进一步阅读全文才能获得。

### 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平

- **实验组数**：摘要提到在SIMPLER benchmark和真实世界操控任务上进行了评估，且取得显著提升（16%和13%）。元数据中未列出详细消融实验数量。基于典型ICLR论文，应包含多项消融实验（如预测模型变体、损失函数影响、解码器设计等），但摘要未详述。
- **充分性与公平性**：从结果性能提升来看，与强基线比较，实验设计较充分。但由于缺乏具体消融组数，无法完全判断。总体感觉实验覆盖了模拟和真实场景，对比方法有代表性，但需要阅读全文确认细节。

### 6. 论文的主要结论与发现

- 从无动作标签的视频中学习潜在动作作为中间表征，能够有效提取物理动态，并用于下游机器人策略。
- ViPRA 框架显著优于纯模仿学习基线，在SIMPLER基准上提升16%，在真实世界操控任务上提升13%。
- 仅需100-200个遥操作演示即可微调解码器，实现高达22 Hz的高频连续控制，支持跨本体的泛化能力。
- 显式建模“什么在变化”和“如何变化”比隐式自回归策略学习更有效。

### 7. 优点：方法或实验设计上有哪些亮点

- **方法亮点**：
  - 使用潜在动作作为中间表征，避免了直接预测动作带来的领域差异问题。
  - 感知损失+光流一致性确保了潜在动作的物理合理性，不依赖动作标签。
  - 分块流匹配解码器实现了高频连续控制，且只需极少量遥操作演示。
  - 模型可泛化到不同机器人本体（跨本体的泛化）。
- **实验亮点**：
  - 在模拟和真实环境均进行验证，覆盖多个操控任务。
  - 与强基线对比，性能提升显著。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验覆盖**：未明确说明预训练视频数据的规模和来源，是否覆盖多样化场景？是否只在单一类型的操控任务上测试？摘要未提及其他任务（如移动、导航）或更复杂的交互。
- **偏差风险**：潜在动作可能未完全编码所有动态信息（如细粒度接触力、物体形变），解码器可能只能依赖特定模态。
- **应用限制**：依赖视频-语言模型，可能对文本指令质量敏感；需要大量无标签视频预训练，但并非所有研究者都能获取；实际部署中22Hz控制频率可能不足以适应某些高动态任务；仅在100-200个演示微调，但对遥操作演示的质量有要求。

（完）
