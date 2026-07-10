---
title: "Discrete Diffusion VLA: Bringing Discrete Diffusion to Action Decoding in Vision-Language-Action Policies"
title_zh: 离散扩散VLA：将离散扩散引入视觉-语言-动作策略的动作解码
authors: "Zhixuan Liang, Yizhuo Li, Tianshuo Yang, Chengyue Wu, Sitong Mao, Tian Nian, Liuao Pei, Shunbo Zhou, Xiaokang Yang, Jiangmiao Pang, Yao Mu, Ping Luo"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=YWeNCMxdhM"
tags: ["query:latent-vla"]
score: 9.0
evidence: 用于VLA动作解码的离散扩散方法
tldr: 针对现有VLA模型因分离动作头导致信息割裂的问题，提出离散扩散VLA，将离散扩散建模应用于离散化动作块，所有组件统一为transformer架构。该方法保留扩散逐步细化且兼容VLM离散令牌接口，实现自适应解码顺序。实验表明其具备扩展性和高效性，为VLA提供统一可扩展方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有VLA模型的动作解码方式导致架构碎片化，阻碍统一扩展。
method: 提出统一transformer策略，用离散扩散建模离散化动作块，与VLM的离散令牌接口自然兼容。
result: 实现自适应解码顺序，在仿真机器人任务上取得高效性能。
conclusion: 离散扩散VLA为VLA模型提供了一种统一、可扩展的架构范式。
---

## Abstract
Vision–Language–Action (VLA) models adapt large vision–language backbones to map images and instructions into robot actions. However, prevailing VLAs either generate actions autoregressively in a fixed left-to-right order or attach separate MLP or diffusion heads outside the backbone, leading to fragmented information pathways and specialized training requirements that hinder a unified, scalable architecture. We present Discrete Diffusion VLA, a unified-transformer policy that models discretized action chunks with discrete diffusion. The design retains diffusion's progressive refinement paradigm while remaining natively compatible with the discrete token interface of VLMs. Our method achieves an adaptive decoding order that resolves easy action elements before harder ones and uses secondary re-masking to revisit uncertain predictions across refinement rounds, which improves consistency and enables robust error correction. This unified decoder preserves pretrained vision-language priors, supports parallel decoding, breaks the autoregressive bottleneck, and reduces the number of function evaluations. Discrete Diffusion VLA achieves 96.3% avg. success rates on LIBERO, 71.2% visual matching on SimplerEnv-Fractal and 54.2% overall on SimplerEnv–Bridge, improving over autoregressive, MLP decoder and continuous diffusion baselines. These findings indicate that discrete-diffusion VLA supports precise action modeling and consistent training, laying groundwork for scaling VLA to larger models and datasets.

---

## 论文详细总结（自动生成）

# 离散扩散VLA：将离散扩散引入视觉-语言-动作策略的动作解码 —— 论文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

*   **研究动机**：现有的视觉-语言-动作（VLA）模型通常通过两种方式生成动作：一是采用固定的从左到右的自回归生成，二是在视觉-语言骨干网络之外附加专用的MLP或连续扩散解码头。这两种方式都会导致信息路径碎片化（action decoding模块与VLM主干分离），且需要专门的训练策略，阻碍了VLA架构的统一性和可扩展性。
*   **核心问题**：如何设计一种统一的、可扩展的动作解码机制，使之既能自然地融入VLM的离散令牌接口，又能保留扩散模型逐步细化的优势，同时避免自回归的固定顺序瓶颈。
*   **整体含义**：论文提出**离散扩散VLA**（Discrete Diffusion VLA），通过将离散扩散建模应用于离散化动作块，并采用统一的Transformer架构，解决了上述问题。该方法在多个仿真机器人基准上取得了优于自回归、MLP解码器和连续扩散基线的结果，为VLA模型向更大模型和数据集的规模化提供了可行范式。

## 2. 论文提出的方法论：核心思想、关键技术细节

*   **核心思想**：将机器人动作离散化为令牌序列（类似VLM中的离散令牌），然后使用离散扩散模型对动作令牌进行逐步细化生成。整个动作解码器与VLM骨干网络共享统一的Transformer架构，无需额外分离的解码头。
*   **关键技术细节**：
    *   **离散化动作块**：将连续的动作空间（如关节角度、末端执行器位姿）量化成离散令牌，每个动作对应一组离散令牌序列，使其与VLM的离散接口天然兼容。
    *   **离散扩散过程**：在训练时，从真实动作令牌开始逐渐加噪（正向过程）；在推理时，从纯噪声令牌开始逐步去噪（逆向过程），每一步预测更接近真实动作的令牌分布。
    *   **自适应解码顺序**：不同于固定从左到右的自回归生成，离散扩散允许模型在每一轮去噪步骤中自适应地决定哪些动作令牌先被确定（先解决“容易”的元素），哪些令牌保持不确定等待后续修正。
    *   **二次重掩码（Secondary Re-masking）**：在去噪过程中，模型会重新评估之前预测但不确定的动作令牌，通过再次掩码它们并重新修正，实现错误恢复和一致性的提升。
    *   **并行解码**：由于离散扩散过程可以同时对多个令牌进行预测（而非逐令牌顺序生成），实现了并行计算，减少了函数评估次数（NFE），提高了推理效率。

## 3. 实验设计：数据集、基准与对比方法

*   **数据集/场景**：
    *   **LIBERO**：多任务桌面操作仿真环境，评估成功率。
    *   **SimplerEnv-Fractal**：基于Fractal数据集的简化仿真环境，使用视觉匹配（visual matching）指标。
    *   **SimplerEnv-Bridge**：基于Bridge数据集的简化仿真环境，评估总体成功率。
*   **基准（Benchmark）**：每个数据集对应的标准评估指标（LIBERO使用平均成功率，SimplerEnv使用视觉匹配或总体成功率）。
*   **对比方法**：
    *   **自回归解码基线**：传统VLA中按固定顺序逐令牌生成动作。
    *   **MLP解码器基线**：在VLM骨干后附加独立的MLP层输出连续动作。
    *   **连续扩散基线**：使用连续扩散模型作为动作头，而非离散化。
*   **实验结果**：
    *   LIBERO：平均成功率 **96.3%**，优于所有基线。
    *   SimplerEnv-Fractal：视觉匹配 **71.2%**，优于基线。
    *   SimplerEnv-Bridge：总体成功率 **54.2%**，优于基线。

## 4. 资源与算力

*   **文中未明确说明**：论文摘要和元数据中未提及训练所使用的GPU型号、数量、训练时长等具体算力信息。因此无法总结资源消耗细节。这一点需要在未来工作中补充。

## 5. 实验数量与充分性

*   **实验数量**：论文在三个不同的仿真环境/数据集上进行了评估，对比了至少三种基线方法（自回归、MLP解码器、连续扩散）。但由于仅提供摘要级别的描述，未明确提及是否包含消融实验（如离散化粒度、扩散步数、重掩码策略的贡献等）。
*   **充分性与公平性**：
    *   **优点**：涵盖多个典型仿真任务，对比了当前主流的动作解码方式，结果明显优于基线，具有一定说服力。
    *   **不足**：
        *   缺乏消融实验的详细分析，无法判断各组件（自适应顺序、二次重掩码、离散化分辨率）的独立贡献。
        *   无真实机器人实验，泛化到真实世界物理环境的能力尚未验证。
        *   未说明随机性控制或多次重复实验的统计量（标准差等），难以判断结果的稳定性。
    *   **客观性**：实验设计合理，但在信息完整性和严谨性上尚有提升空间。

## 6. 论文的主要结论与发现

*   **主要结论**：离散扩散VLA通过统一Transformer架构将离散扩散建模用于动作解码，实现了高成功率、高视觉匹配度，并且比自回归、MLP解码器和连续扩散基线更优。该方法保留了预训练VLM的先验知识，支持并行解码和自适应顺序，减少了函数评估次数。
*   **关键发现**：
    *   离散化动作令牌与VLM离散接口自然兼容，无需额外适配。
    *   自适应解码顺序和二次重掩码机制有效提升了动作预测的一致性和纠错能力。
    *   该架构具备良好可扩展性，为VLA模型向更大模型和数据集扩展奠定了基础。

## 7. 优点：方法或实验设计上的亮点

*   **方法亮点**：
    *   **统一架构**：动作解码与VLM共享Transformer，避免了信息碎片化和分离训练的问题。
    *   **离散扩散的先天优势**：同时具备逐步细化和并行解码，且兼容离散令牌，无需重构VLM接口。
    *   **自适应解码顺序**：模仿人类先易后难的动作规划，提高了解码效率。
    *   **二次重掩码纠错**：在扩散迭代中回访修正，增强了鲁棒性。
*   **实验设计亮点**：
    *   在多个有代表性的仿真基准上对比，且涵盖不同评测指标（成功率、视觉匹配）。
    *   与几种主流的动作解码范式（自回归、MLP、连续扩散）直接对比，基线设置全面。

## 8. 不足与局限

*   **实验覆盖方面**：
    *   缺少真实机器人实验，方法在真实物理世界中的性能未知。
    *   未进行详细的消融研究，无法量化自适应顺序和二次重掩码的具体贡献。
    *   未报告多次实验的方差，结果的可靠性待进一步验证。
*   **方法局限性**：
    *   离散化动作的分辨率（量化粒度）对最终性能有直接影响，但文中未讨论最优选择或敏感度分析。
    *   扩散的步数与计算开销之间的权衡未提及，实际推理效率可能仍受限于扩散步骤数。
    *   对VLM骨干网的依赖较强，若骨干预训练未覆盖机器人领域知识，可能需要额外微调。
*   **应用限制**：
    *   目前仅在仿真环境验证，应用于真实机器人时可能面临延迟、噪声、动力学不匹配等问题。
    *   离散化动作令牌的序列长度可能随任务复杂度增加而增长，对长程任务的可扩展性尚不明确。

（完）
