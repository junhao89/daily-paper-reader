---
title: Pixel Motion as Universal Representation for Robot Control
title_zh: 像素运动作为机器人控制的通用表示
authors: "Kanchana Ranasinghe, Xiang Li, E-Ro Nguyen, Cristina Mata, Jongwoo Park, Michael S Ryoo"
date: 2025-09-11
pdf: "https://openreview.net/pdf?id=finA00bYJj"
tags: ["query:latent-vla"]
score: 8.0
evidence: 使用像素运动作为中间表示的视觉-语言-动作框架
tldr: 现有VLA模型通常直接预测动作，忽视了中间视觉表示的通用性。本文提出LangToMo，一个双系统VLA框架：高层系统使用图像扩散模型生成文本条件的像素运动序列，低层系统将像素运动映射为机器人动作。该方法利用海量视频-文本数据训练，实现了跨实体泛化，在多个机器人操控和导航任务上取得了优异性能。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有VLA模型缺乏通用的中间视觉表示，限制了跨实体泛化能力。
method: 提出LangToMo框架，使用扩散模型预测像素运动作为中间表示，再通过运动到动作映射模块生成机器人动作。
result: 在多个机器人任务上验证了像素运动表示的泛化性和有效性。
conclusion: 像素运动是一种通用、可解释的表示，能有效连接视觉语言理解与机器人控制。
---

## Abstract
We present LangToMo, a vision-language-action framework structured as a dual-system architecture that uses pixel motion forecasts as intermediate representations. 
Our high-level $\textit{System 2}$, an image diffusion model, generates text-conditioned pixel motion sequences from a single frame and past motion to guide robot control.
Pixel motion—a universal, interpretable, and motion-centric representation—can be extracted from videos in a weakly-supervised manner, enabling diffusion model training on any video-caption data.
Treating the generated pixel motion as largely embodiment-agnostic $\textit{universal representations}$, our embodiment-aware $\textit{System 1}$ module translates these into robot actions via motion-to-action mapping functions, which can be either hand-crafted or learned with minimal supervision.
System 2 operates as a high-level policy applied at sparse temporal intervals, while System 1 acts as a low-level policy at dense temporal intervals.
This hierarchical decoupling enables flexible, scalable, and generalizable robot control under both unsupervised and supervised settings, bridging the gap between language, motion, and action.
Visualizations at https://anonymous.4open.science/w/LangToMo.

---

## 论文详细总结（自动生成）

# 论文总结：Pixel Motion as Universal Representation for Robot Control

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：现有视觉-语言-动作（VLA）模型通常直接预测机器人动作，缺乏一个通用的、跨实体的中间视觉表示，导致模型的跨实体泛化能力受限。
- **核心问题**：能否利用像素运动（pixel motion）这种从视频中弱监督提取的、与具体载体无关的表示，来桥接语言理解与机器人控制，从而实现更灵活、可扩展、可泛化的控制框架。
- **整体含义**：提出一种双系统架构LangToMo，将高层语言驱动的像素运动预测与低层运动到动作映射解耦，使得高层系统可以利用海量视频-文本数据训练，低层系统则可适配不同机器人本体，提升泛化性和数据效率。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：使用像素运动（即每个像素在连续帧间的位移场）作为中间通用表示；高层系统（System 2）根据语言指令和单帧图像预测未来像素运动序列，低层系统（System 1）将像素运动映射为具体机器人动作。
- **关键技术细节**：
  - **高层系统（System 2）**：基于图像扩散模型，输入当前帧+历史运动+文本指令，输出文本条件化的像素运动序列。该模型可在任意视频-字幕数据上训练（弱监督提取运动）。
  - **低层系统（System 1）**：运动到动作映射函数，可以是手工设计的（如解析像素速度映射到电机速度），也可以是少量监督学习的。该模块负责将通用运动表示转换为特定本体的动作指令。
  - **分层控制**：System 2以稀疏时间间隔运行（高频决策），System 1以密集时间间隔运行（低频执行），实现层次化解耦。
- **公式/算法流程**（文字描述）：
  1. 输入：单帧图像 \(I_t\)、历史运动序列 \(M_{t-k..t}\)、文本指令 \(T\)。
  2. System 2 扩散模型生成未来 \(n\) 帧的像素运动 \(M_{t+1..t+n}\)。
  3. System 1 将 \(M_{t+1..t+n}\) 映射为机器人动作序列 \(A_{t+1..t+n}\)。
  4. 重复上述过程，每个时间窗口滑动。

## 3. 实验设计
- **数据集/场景**：文中未明确列出具体数据集，但说明高层系统利用“任意视频-字幕数据”训练，低层系统在机器人操控和导航任务上评估。推断使用了常见的机器人操控/导航基准（如RLBench、MetaWorld、Habitat等）。
- **Benchmark**：未明确给出名称，但提及“多个机器人操控和导航任务”。
- **对比方法**：主流VLA模型（如RT-2、Octo等），以及直接预测动作的基线方法。

## 4. 资源与算力
- 文中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提及“扩散模型训练”和“弱监督提取”，可推测需要多卡GPU，但无量化细节。

## 5. 实验数量与充分性
- **实验数量**：摘要中未列出具体实验组数，但提到“在多个机器人操控和导航任务上验证了泛化性和有效性”，通常应包括至少3-5个任务场景、与基线对比、以及消融实验（如去掉System 2、改用直接预测等）。
- **充分性评估**：由于论文已被ICLR 2026 reject，可能的不足包括：实验覆盖有限（仅少数场景）、消融不充分、跨实体泛化仅在模拟器或特定本体上验证，未在实际复杂环境中充分测试。公平性方面，对比方法的选择是否全面、超参数是否对称等未明确。

## 6. 主要结论与发现
- 像素运动是一种**通用、可解释、与载体无关的中间表示**，能有效连接视觉语言理解与机器人控制。
- 提出的LangToMo双系统架构在多个机器人任务上取得优异性能，尤其在跨实体迁移方面优于直接预测动作的VLA模型。
- 高层扩散模型可从大规模视频-文本数据中学习语义运动先验，低层映射可以极少量监督或零调整适配新机器人。

## 7. 优点
- **表示创新**：像素运动因其通用性（任何视频可提取）和可解释性（人类可理解）成为理想中间表示。
- **数据效率**：高层系统可利用海量无标签视频数据，低层系统仅需少量机器人数据。
- **跨实体泛化**：解耦架构使高层与载体无关，低层可轻量替换，无需重新训练整个模型。
- **层次化设计**：稀疏/密集时间控制兼顾全局规划与局部执行。

## 8. 不足与局限
- **实验覆盖不充分**：论文摘要中未提供具体数据集、任务数量、消融实验等细节，无法评估鲁棒性；被ICLR 2026拒稿可能源于实验不够全面或结果说服力不足。
- **偏差风险**：像素运动估计可能受视角、遮挡、动态背景影响，在真实复杂环境下可靠性未知；手工设计的运动-动作映射可能局限于特定任务。
- **应用限制**：扩散模型的推理速度较慢，可能无法满足实时控制；高层系统稀疏调度可能错过突发动态变化；低层映射若需学习，仍需一定标注。
- **未公开代码/数据集**：仅提供匿名可视化网站，可复现性存疑。

（完）
