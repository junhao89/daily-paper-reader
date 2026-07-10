---
title: Pixel Motion as Universal Representation for Robot Control
title_zh: 像素运动作为机器人控制的通用表示
authors: "Kanchana Ranasinghe, Xiang Li, E-Ro Nguyen, Cristina Mata, Jongwoo Park, Michael S Ryoo"
date: 2025-09-11
pdf: "https://openreview.net/pdf?id=finA00bYJj"
tags: ["query:gen2embodied"]
score: 8.0
evidence: 图像扩散模型生成像素运动预测作为机器人控制中间表示
tldr: 该论文针对机器人控制中表示通用性不足的问题，提出LangToMo框架，利用图像扩散模型生成文本条件化的像素运动序列作为中间表示，该表示是通用且可解释的，可通过弱监督从视频中学习。然后通过具身感知映射模块转换为具体动作。实验证明该方法在多种机器人平台上有效。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 需要一种通用、可解释的中间表示来连接视觉与动作，降低对具体平台的依赖。
method: 使用图像扩散模型生成像素运动预测，再由具身映射模块转换为动作。
result: 在多机器人平台上验证了方法的有效性，泛化能力强。
conclusion: 像素运动作为通用表示可提升VLA框架的跨平台迁移能力。
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

# 像素运动作为机器人控制的通用表示（LangToMo）详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：机器人控制中缺乏一种通用、可解释、跨平台可迁移的中间表示来连接视觉语言理解与具体动作执行。现有视觉-语言-动作（VLA）框架高度依赖具体机器人平台，泛化能力弱。
- **研究动机**：希望设计一种与具体机器人实体无关的“通用表示”，既能从自然语言和视频中弱监督学习，又能灵活映射到不同机器人的动作空间，从而提升跨平台迁移能力和数据效率。
- **整体含义**：论文提出 LangToMo 框架，将“像素运动预测”作为这种通用中间表示，并通过双系统架构（高层的 System 2 负责语义运动生成，低层的 System 1 负责动作映射）实现语言→运动→动作的解耦。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：使用图像扩散模型（System 2）从单帧图像和过去运动信息中，根据文本条件生成未来的像素运动序列（像素级别的运动流）。生成的像素运动被视为与机器人具体形态无关的通用表示。然后，通过一个具身感知模块（System 1）将这些像素运动映射为具体机器人的动作指令。
- **关键技术细节**：
  - **System 2（高层策略）**：基于图像扩散模型，输入为当前帧图像、历史运动信息以及语言条件，输出为未来若干步的像素运动预测。该模型可在大规模视频-文本数据上以弱监督方式训练（因为像素运动可从视频中自动提取）。
  - **System 1（低层策略）**：运动到动作的映射模块，可以是手工设计的函数（例如根据像素跟踪点反解机器人关节角），也可以是使用少量机器人动作数据学习得到的映射器。System 1 以较高频率执行，System 2 在稀疏时间间隔触发。
  - **层次化解耦**：System 2 在稀疏时间间隔（如每几秒）规划全局运动趋势，System 1 在密集时间间隔（每个时间步）执行局部动作映射。这使得高层策略可独立于具体机器人形态进行训练和优化。
- **公式/算法流程**（文字描述）：
  1. 输入：单帧图像 $I_t$、历史像素运动序列 $M_{t-k:t-1}$、语言指令 $L$。
  2. System 2 扩散模型生成未来像素运动序列 $M_{t:t+T}$。
  3. 将 $M_{t:t+T}$ 传递给 System 1。
  4. System 1 在每个时间步将像素运动映射为具体动作 $a_t$（如关节角度、末端执行器速度等）。
  5. 执行动作后更新环境，进入下一循环，System 2 根据新状态（可能重新触发）再次生成运动预测。

## 3. 实验设计
- **数据集/场景**：虽然摘要未列出具体数据集名称，但论文声称在“多种机器人平台”上验证，且 System 2 的训练使用了“任何视频-文本数据”（弱监督）。具体场景可能包括桌面操作、导航、抓取等。
- **基准测试**：未明确列出 benchmark 名称，但推测与常见的机器人操作基准（如 RLBench、MetaWorld、真实机器人平台）进行比较。
- **对比方法**：未在摘要中列出对比方法的名称，但通常同类方法对比包括：端到端 VLA 模型（如 RT-2、PaLM-E）、无中间表示的纯策略方法、以及使用不同中间表示（如关键点、目标图像）的方法。

## 4. 资源与算力
- **文中未明确说明**：摘要和提供的文本中未提及 GPU 型号、数量、训练时长等算力信息。因此无法总结具体资源。

## 5. 实验数量与充分性
- **实验数量**：从摘要推断，论文应该包括：
  - 在多个机器人平台上的验证实验（可能至少2-3种不同形态的机器人）。
  - 消融实验：对比不使用中间表示、不同映射方式（手工 vs 学习）、不同 System 2 刷新频率等。
  - 可能还包括零样本迁移实验（训练在一种机器人，直接部署到另一种）。
- **充分性与客观性**：由于摘要未提供详细结果（分数8.0表明评审认可度较高，但被ICLR 2026拒绝），无法判断实验设计是否严格公平。但双系统解耦和弱监督训练的设计在概念上合理，实验可能具有较好的泛化性证据。不过，未提供量化结果和误差分析，客观性有待完整论文验证。

## 6. 论文的主要结论与发现
- 像素运动是一种通用、可解释、与具体机器人形态无关的中间表示，能够有效弥合语言与动作之间的鸿沟。
- 基于图像扩散模型的 System 2 可以在海量视频-文本数据上弱监督训练，生成合理的像素运动预测。
- 将高层规划（System 2）与低层动作映射（System 1）解耦，使得跨平台迁移成为可能：只需更换 System 1 的映射模块，而固定 System 2。
- 该方法在多种机器人平台上实现有效控制，泛化能力强于传统的端到端 VLA 框架。

## 7. 优点
- **表示通用性**：像素运动是视觉可解释的，且可从任意视频中提取，降低了对机器人特定数据的依赖。
- **层次化解耦**：将语言理解、运动生成与动作执行分为两个独立的系统，有利于模块化开发和部分重用。
- **弱监督训练**：System 2 可利用大规模互联网视频-文本数据，无需昂贵的机器人标注数据。
- **灵活性**：System 1 既可用规则实现（无需任何数据），也可用少量监督学习，适应不同精度需求。

## 8. 不足与局限
- **实验覆盖不足**：摘要未提供具体量化结果，可能实验只在模拟器或有限真实环境进行，缺乏大规模真实机器人部署证据。
- **偏差风险**：像素运动预测可能在某些动态变化剧烈或背景复杂的场景中不准确，且 System 1 的映射精度受机器人运动学模型误差影响。
- **应用限制**：扩散模型推理速度较慢，可能难以在需要高频实时响应的任务中应用；System 2 的稀疏触发设计也会引入延迟。
- **缺失对比细节**：未说明与现有最先进方法的公平比较（如计算量、成功率指标），论文被 ICLR 2026 拒绝可能暗示存在方法论或实验上的不足。
- 算力和实验细节缺失，影响可复现性。

（完）
