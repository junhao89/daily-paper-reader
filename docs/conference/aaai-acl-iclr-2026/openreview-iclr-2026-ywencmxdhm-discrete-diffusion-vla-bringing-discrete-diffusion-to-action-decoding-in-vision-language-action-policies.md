---
title: "Discrete Diffusion VLA: Bringing Discrete Diffusion to Action Decoding in Vision-Language-Action Policies"
title_zh: 离散扩散VLA：将离散扩散引入视觉-语言-动作策略的动作解码
authors: "Zhixuan Liang, Yizhuo Li, Tianshuo Yang, Chengyue Wu, Sitong Mao, Tian Nian, Liuao Pei, Shunbo Zhou, Xiaokang Yang, Jiangmiao Pang, Yao Mu, Ping Luo"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=YWeNCMxdhM"
tags: ["query:latent-vla"]
score: 10.0
evidence: 将离散扩散引入VLA动作解码，建模离散化动作块
tldr: 针对现有VLA策略中动作生成方式碎片化的问题，提出离散扩散VLA，利用离散扩散模型对离散化动作块进行建模，并采用统一的Transformer架构，实现了自适应解码顺序和更好的可扩展性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有VLA策略动作生成方式碎片化，导致信息路径断裂和训练复杂。
method: 提出离散扩散VLA，使用离散扩散模型对离散化动作块进行建模，并集成到统一Transformer中。
result: 在机器人操作任务上实现了自适应解码顺序和更好的性能。
conclusion: 离散扩散与VLA的兼容为构建统一可扩展的策略提供了新方向。
---

## Abstract
Vision–Language–Action (VLA) models adapt large vision–language backbones to map images and instructions into robot actions. However, prevailing VLAs either generate actions autoregressively in a fixed left-to-right order or attach separate MLP or diffusion heads outside the backbone, leading to fragmented information pathways and specialized training requirements that hinder a unified, scalable architecture. We present Discrete Diffusion VLA, a unified-transformer policy that models discretized action chunks with discrete diffusion. The design retains diffusion's progressive refinement paradigm while remaining natively compatible with the discrete token interface of VLMs. Our method achieves an adaptive decoding order that resolves easy action elements before harder ones and uses secondary re-masking to revisit uncertain predictions across refinement rounds, which improves consistency and enables robust error correction. This unified decoder preserves pretrained vision-language priors, supports parallel decoding, breaks the autoregressive bottleneck, and reduces the number of function evaluations. Discrete Diffusion VLA achieves 96.3% avg. success rates on LIBERO, 71.2% visual matching on SimplerEnv-Fractal and 54.2% overall on SimplerEnv–Bridge, improving over autoregressive, MLP decoder and continuous diffusion baselines. These findings indicate that discrete-diffusion VLA supports precise action modeling and consistent training, laying groundwork for scaling VLA to larger models and datasets.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：现有视觉-语言-动作（VLA）模型在动作解码阶段存在碎片化问题：主流方法要么以固定的从左到右顺序自回归生成动作，要么在主干网络之外附加独立的MLP或扩散头，导致信息路径断裂、训练需求特殊化，阻碍了统一、可扩展架构的构建。
- **整体含义**：本文旨在解决VLA中动作生成方式的不一致性，提出一种与视觉语言模型（VLM）原生兼容的离散扩散策略，从而简化训练流程、保留预训练先验知识，并为大规模VLA模型奠定基础。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将离散扩散模型引入VLA动作解码，对**离散化的动作块**进行建模，使扩散过程的自适应顺序解码与VLM的离散token接口自然兼容。
- **关键技术细节**：
  - **动作离散化**：将连续动作空间划分为离散动作块（action chunks），作为离散token处理。
  - **离散扩散过程**：在离散token空间上应用正向噪声添加和反向去噪，实现渐进式精炼（progressive refinement），而非固定顺序自回归。
  - **自适应解码顺序**：模型在去噪过程中先解决简单的动作元素，再处理困难的部分，提升解码效率。
  - **二次重掩码**（secondary re-masking）：在多个精炼轮次中对不确定的预测进行重新掩码并再次去噪，增强一致性和错误修正能力。
  - **统一Transformer架构**：将离散扩散解码器直接集成到VLM的Transformer主干中，无需额外头网络；支持并行解码，打破自回归瓶颈，减少函数评估次数。

## 3. 实验设计：数据集、基准、对比方法

- **数据集与场景**：
  - **LIBERO**：机器人操作基准（多个任务）。
  - **SimplerEnv-Fractal**：可视化匹配任务。
  - **SimplerEnv-Bridge**：整体性能评估。
- **基准（Benchmark）**：使用各数据集的标准成功率或视觉匹配率指标。
- **对比方法**：
  - 自回归解码（autoregressive）
  - MLP解码器
  - 连续扩散模型（continuous diffusion）
- 消融实验：评估自适应解码顺序、二次重掩码等组件的贡献。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅在摘要中提及方法“减少函数评估次数”，但无具体硬件配置。

## 5. 实验数量与充分性

- **实验数量**：主要在三个数据集上进行了主要对比实验，并包含消融研究（验证自适应解码顺序、二次重掩码等）。每组数据集均有数值结果。
- **充分性评估**：实验覆盖了多种VLA解码范式（自回归、MLP、连续扩散），对比基线全面；消融实验验证了关键设计。但数据集局限于模拟环境（LIBERO、SimplerEnv），缺乏真实机器人实验，且未提供统计显著性测试或多次运行标准差，公平性基本满足但客观性略有不足。

## 6. 主要结论与发现

- 离散扩散VLA在**LIBERO**上达到 **96.3%** 平均成功率，在**SimplerEnv-Fractal**上达到 **71.2%** 视觉匹配率，在**SimplerEnv-Bridge**上总体**54.2%**，均优于自回归、MLP解码器和连续扩散基线。
- 自适应解码顺序和二次重掩码显著提升了动作一致性和错误修正能力。
- 离散扩散与VLA的兼容为实现统一、可扩展的策略提供了新方向，支持更高效、更精确的动作建模。

## 7. 优点

- **方法创新**：将离散扩散无缝集成到VLM的离散token接口中，避免了额外的头部网络和复杂的训练流程。
- **效率优势**：支持并行解码，打破自回归瓶颈，减少函数评估次数。
- **性能领先**：在多个基准上取得SOTA结果，且训练更统一。
- **设计巧妙**：自适应解码顺序和二次重掩码机制有效提升了生成质量和鲁棒性。

## 8. 不足与局限

- **实验覆盖有限**：仅在模拟环境（LIBERO、SimplerEnv）中评估，缺乏真实机器人实验，泛化性和实际部署风险未知。
- **算力信息缺失**：未报告训练资源，难以评估方法的可复现性和经济成本。
- **未提供统计显著性**：没有多次运行的标准差或置信区间，结果可能存在偶然性。
- **离散化带来的精度损失**：将连续动作离散化可能引入量化误差，对于高精度任务（如精密操作）可能不足。
- **可扩展性论证不足**：虽然声称“为更大模型和数据集奠定基础”，但未提供模型规模或数据规模扩展的实验证据。

（完）
