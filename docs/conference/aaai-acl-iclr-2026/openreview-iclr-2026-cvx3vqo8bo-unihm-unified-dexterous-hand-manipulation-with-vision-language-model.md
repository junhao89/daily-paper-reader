---
title: "UniHM: Unified Dexterous Hand Manipulation with Vision Language Model"
title_zh: UniHM：基于视觉语言模型的统一灵巧手操作
authors: "Zhenhao Zhang, Jiaxin Liu, Ye Shi, Jingya Wang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=cVX3VqO8BO"
tags: ["query:latent-vla"]
score: 8.0
evidence: 提出统一手-灵巧分词器，将手形态映射到共享码本，实现令牌化潜在动作表示
tldr: 针对灵巧手操作中不同手形态难以统一表示且依赖大规模数据的问题，提出UniHM框架，包含统一手-灵巧分词器将多种手形态映射到共享离散码本，以语言指令为条件训练VLA模型。仅使用人机交互数据训练，无需真实遥操作数据，在多个灵巧操作任务上实现零样本泛化到新物体和新指令。该工作展示了离散潜在动作令牌在VLA中的有效性。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有灵巧手操作依赖特定手形态和大规模数据，缺乏统一语言引导的表示。
method: 提出统一手-灵巧分词器将不同手形态映射到共享离散码本，结合VLA模型。
result: 仅用人类交互数据训练，在灵巧操作任务上实现零样本泛化到新物体和指令。
conclusion: 离散潜在动作令牌有效统一多种手形态，实现语言引导的灵巧操作。
---

## Abstract
Planning physically feasible dexterous hand manipulation is a central challenge in robotic manipulation and Embodied AI. Prior work typically relies on object-centric cues or precise hand-object interaction sequences, foregoing the rich, compositional guidance of open-vocabulary instruction. We introduce UniHM, the first framework for unified dexterous hand manipulation guided by free-form language commands. 
We propose a Unified Hand-Dexterous Tokenizer that maps heterogeneous dexterous-hand morphologies into a single shared codebook, improving cross-dexterous hand generalization and scalability to new morphologies. Our vision language action model is trained solely on human-object interaction data, eliminating the need for massive real-world teleoperation datasets, and demonstrates strong generalizability in producing human-like manipulation sequences from open-ended language instructions. To ensure physical realism, we introduce a physics-guided dynamic refinement module that performs segment-wise joint optimization under generative and temporal priors, yielding smooth and physically feasible manipulation sequences. Across multiple datasets and real-world evaluations, UniHM attains state-of-the-art results on both seen and unseen objects and trajectories, demonstrating strong generalization and high physical feasibility.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：灵巧手操作在机器人操作和具身智能中面临两大挑战：一是不同手形态（dexterous-hand morphologies）难以统一表示，导致模型难以跨形态泛化；二是现有方法通常依赖物体中心的线索或精确的手-物体交互序列，缺乏开放词汇语言指令的丰富组合指导，且需要大规模真实遥操作数据集。
- **整体含义**：本文旨在构建一个统一的、由自由形式语言指令引导的灵巧手操作框架，使机器人能够仅从人类-物体交互数据中学习，无需昂贵遥操作数据，并实现对新物体和新指令的零样本泛化。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：提出UniHM框架，通过将异构灵巧手形态映射到共享离散码本，结合视觉-语言-动作（VLA）模型，实现语言引导的统一灵巧手操作。
- **关键技术细节**：
  - **统一手-灵巧分词器（Unified Hand-Dexterous Tokenizer）**：将多种手形态映射到一个共享离散码本中，从而将不同手型的手势、抓取和操作模式统一为潜在动作令牌（latent action tokens），提高跨手形态的泛化能力和对新形态的可扩展性。
  - **视觉语言动作模型（VLA）**：以语言指令为条件，仅使用人类-物体交互数据训练，无需遥操作数据，生成类似人的操作序列。
  - **物理引导的动态优化模块（Physics-guided Dynamic Refinement Module）**：在生成和时序先验下执行分段的联合优化，产生平滑且物理可行的操作序列。
- **算法流程（文字说明）**：输入自由形式语言指令和视觉观测 → 通过统一手-灵巧分词器将手形态编码为离散潜在令牌 → VLA模型以语言为条件生成动作令牌序列 → 物理引导模块对序列进行分段优化，确保物理真实感 → 输出最终灵巧手操作序列。

## 3. 实验设计：使用的数据集/场景、Benchmark、对比方法
- **数据集与场景**：论文提及“多个数据集和真实世界评估”，但具体数据集名称未在摘要中列出。推测可能使用常见的灵巧手操作数据集（如Dexterous Manipulation Benchmark、HO3D、DexYCB等），并包含真实机器人环境。
- **Benchmark**：与多种现有方法对比，涵盖已见和未见物体、已见和未见轨迹。
- **对比方法**：未明确列出，但声称在已知和未知物体/轨迹上取得最先进结果，表明对比了多种基线（如基于物体中心的抓取规划、基于强化学习的方法、其他VLA变体）。

## 4. 资源与算力
- **未明确说明**：论文摘要和元数据中未提及具体的GPU型号、数量、训练时长或算力开销。需要阅读全文才能获知。

## 5. 实验数量与充分性
- **实验数量**：从摘要可知，进行了“跨多个数据集和真实世界评估”，并包含“已见和未见物体、已见和未见轨迹”的测试，暗示至少覆盖2-3个数据集和一组真实场景。消融实验可能包括：统一分词器的有效性、物理优化模块的效果、VLA训练数据来源等。
- **充分性与公平性**：论文声称实现了从单一数据源（人类-物体交互数据）训练并在多种设置下泛化，且强调了与SOTA的对比，实验设计较为合理。但由于未提供具体实验细节，严谨的公平性评价需阅读全文。注意：数据仅使用人类交互数据，避免了真实遥操作，降低了数据偏差风险。

## 6. 论文的主要结论与发现
- **主要结论**：离散潜在动作令牌（通过统一手-灵巧分词器）能够有效统一多种灵巧手形态，实现语言引导的灵巧操作。仅用人类交互数据训练的VLA模型即可在新物体和新指令上零样本泛化，且通过物理优化模块保证了操作序列的物理可行性。
- **发现**：跨手形态的共享码本显著提升了泛化能力，减少了对手型定制化数据的需求；物理引导的动态优化有效改善了生成序列的平滑性和可执行性。

## 7. 优点：方法或实验设计上的亮点
- **方法创新**：
  - 首次提出统一不同手形态的离散分词器，将异构手型映射至共享码本，实现了形态无关的操作表示。
  - 利用人类-物体交互数据（无需真实遥操作）训练VLA，降低了数据获取成本。
  - 引入物理引导的动态优化模块，在生成过程中保证物理合理性。
- **实验亮点**：
  - 零样本泛化到新物体和新指令，展示了语言引导的强大组合性。
  - 在多个数据集和真实世界评估中均达到最先进水平，验证了方法的通用性和实用性。

## 8. 不足与局限
- **实验覆盖**：摘要未提及失败案例、泛化边界的定量分析，也未说明是否在高度动态或复杂场景（如多物体交互、连续操作）上测试。
- **偏差风险**：仅使用人类-物体交互数据，可能隐含人类行为偏好（如惯用手、抓取习惯），导致对某些操作模式的过拟合。
- **应用限制**：依赖离散码本的分辨率，若码本大小有限，可能无法覆盖所有手部姿态；物理优化模块可能增加计算延迟，实时性未提及。
- **信息缺失**：未提供训练算力、超参数设置、消融实验的详细结果，限制了可复现性评估。

（完）
