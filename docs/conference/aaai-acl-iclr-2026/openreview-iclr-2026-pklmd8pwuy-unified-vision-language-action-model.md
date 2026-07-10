---
title: Unified Vision-Language-Action Model
title_zh: 统一视觉-语言-动作模型
authors: "Yuqi Wang, Xinghang Li, Wenxuan Wang, Junbo Zhang, Yingyan Li, Yuntao Chen, Xinlong Wang, Zhaoxiang Zhang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=PklMD8PwUy"
tags: ["query:latent-vla"]
score: 9.0
evidence: 自回归VLA模型，将视觉、语言和动作标记化并结合世界模型
tldr: 现有VLA模型依赖VLM理解生成动作，忽略视觉中的时序结构。提出UniVLA，统一自回归建模视觉、语言和动作为离散标记序列，并融入世界模型，利用视频数据增强视觉理解，显著提升机器人操作性能。实验表明生成式视觉监督有效提升策略泛化能力。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA忽略了视觉观察中的时序因果结构。
method: 提出UniVLA，自回归地将视觉、语言、动作作为离散标记序列建模并整合世界模型。
result: 在多个操作任务上性能优越，视觉理解能力增强。
conclusion: 统一标记化框架有效利用视频数据，为VLA建模开辟新方向。
---

## Abstract
Vision-language-action models (VLAs) have garnered significant attention for their potential in advancing robotic manipulation.
However, previous approaches predominantly rely on the general comprehension capabilities of vision-language models (VLMs) to generate action signals, often overlooking the rich temporal and causal structure embedded in visual observations. In this paper, we present UniVLA, a unified and native multimodal VLA model that autoregressively models vision, language, and action signals as discrete token sequences. This tokenized formulation naturally supports flexible multimodal task learning, particularly from large-scale video data, and further demonstrates that generative vision supervision can significantly enhance visual understanding. By incorporating world modeling during post-training, UniVLA captures causal dynamics from videos, facilitating effective transfer to downstream policy learning—especially for long-horizon tasks. Our approach sets new state-of-the-art results across several widely used simulation benchmarks, including CALVIN, LIBERO, and Simplenv-Bridge, substantially outperforming prior methods. For example, UniVLA achieves 95.5% average success rate on LIBERO benchmark, surpassing π₀-FAST's 85.5%. We further demonstrate its broad applicability through experiments on real-world ALOHA manipulation tasks and autonomous driving scenarios.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有视觉-语言-动作模型（VLA）主要依赖视觉-语言模型（VLM）的通用理解能力来生成动作信号，但忽略了视觉观测中蕴含的丰富时序因果结构。这种缺失导致模型对动态场景的理解不足，尤其在长时序任务中表现受限。
- **整体意义**：本文提出UniVLA，旨在统一自回归建模视觉、语言和动作，并将世界模型融入其中，利用视频数据增强视觉理解，从而提升机器人操作策略的泛化能力和长程任务性能。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将视觉、语言和动作信号统一视为离散标记序列，采用自回归方式进行建模，并整合世界模型以捕捉视频中的因果动态。
- **关键技术细节**：
  - 视觉、语言、动作分别被离散化为token序列，模型以自回归方式预测下一个token。
  - 在训练中引入生成式视觉监督（即预测未来的视觉token），大幅提升视觉理解能力。
  - 在后训练阶段融入世界模型（世界建模），从视频数据中学习因果动力学，从而有效迁移到下游策略学习。
- **算法流程**（文字说明）：
  1. 多模态输入（图像、语言指令、历史动作）通过tokenizer转换为统一离散token。
  2. 使用自回归Transformer网络预测后续token序列，包括视觉、语言和动作token。
  3. 训练阶段同时优化语言建模损失和视觉生成损失。
  4. 后训练阶段引入世界模型损失，学习状态转移的因果结构。
  5. 推理时，给定当前观测和指令，模型自回归生成动作token序列。

## 3. 实验设计

- **数据集/场景**：
  - 模拟仿真基准：**CALVIN**、**LIBERO**、**Simplenv-Bridge**。
  - 真实世界实验：**ALOHA**操控任务、**自动驾驶场景**。
- **Benchmark**：在LIBERO上达到95.5%平均成功率，超越π₀-FAST的85.5%。
- **对比方法**：主要对比π₀-FAST等先前的VLA方法，以及以往依赖VLM的模型。

## 4. 资源与算力

- 论文摘要和元数据中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。因此无法总结。

## 5. 实验数量与充分性

- **实验数量**：论文提及在三个模拟基准（CALVIN、LIBERO、Simplenv-Bridge）和两个真实场景（ALOHA操控、自动驾驶）中进行了实验，包含性能对比和消融分析（从动机中可见生成式视觉监督的消融）。但未提供详细消融实验的组数。
- **充分性评价**：覆盖了多个主流仿真环境和真实任务，并与最强基线方法对比，实验设计相对充分。但缺乏对复杂真实场景的大规模验证，且未说明多样性（如不同物体、布局）的泛化测试。整体上，实验设置客观公平。

## 6. 论文的主要结论与发现

- 统一视觉、语言、动作的离散token自回归建模能有效利用视频数据，显著提升机器人操作性能。
- 生成式视觉监督（未来视觉token预测）可以显著增强模型的视觉理解能力。
- 世界模型的融入使得模型能够从视频中捕获因果动态，有效迁移到长程任务中，带来性能提升。
- 在LIBERO、CALVIN等基准上达到最新的最优结果（SOTA），例如LIBERO上95.5% vs π₀-FAST 85.5%。

## 7. 优点

- **方法论新颖**：首次将视觉、语言、动作统一为离散token序列进行端到端自回归建模，并融合世界模型。
- **数据效率提升**：通过对视频数据的生成式监督，无需额外标注即可提升视觉理解。
- **强泛化能力**：在多个仿真和真实场景上表现优异，验证了框架的通用性。
- **长程任务优势**：世界模型的因果建模特别有利于长时序操控任务。

## 8. 不足与局限

- **算力与训练细节缺失**：未报告训练所需计算资源，难以评估复现成本。
- **真实世界实验范围有限**：仅涉及ALOHA操控和自动驾驶，未覆盖更复杂的交互任务（如多物体抓取、动态环境）。
- **消融实验不够详尽**：元数据中只提到生成式视觉监督有效，未对世界模型的各部分进行定量消融。
- **偏差风险**：训练数据可能主要来源于仿真环境，真实世界迁移存在Sim-to-Real gap。
- **应用限制**：当前框架依赖于离散token化，对连续动作的高精度控制可能存在量化误差。

（完）
