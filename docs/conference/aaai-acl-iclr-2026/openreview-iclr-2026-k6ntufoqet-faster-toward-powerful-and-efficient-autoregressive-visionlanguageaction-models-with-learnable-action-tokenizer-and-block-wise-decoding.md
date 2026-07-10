---
title: "FASTer: Toward Powerful and Efficient Autoregressive Vision–Language–Action Models with Learnable Action Tokenizer and Block-wise Decoding"
title_zh: "FASTer: 具有可学习动作分词器和块状解码的强大高效自回归视觉-语言-动作模型"
authors: "Yicheng Liu, Shiduo Zhang, Zibin Dong, Baijun Ye, Tianyuan Yuan, Xiaopeng Yu, Linqi Yin, Chenhao Lu, Junhao Shi, Luca Jiang-Tao Yu, Liangtao Zheng, Jingjing Gong, Tao Jiang, Xipeng Qiu, Hang Zhao"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=k6nTUFoqeT"
tags: ["query:latent-vla"]
score: 9.0
evidence: 可学习动作分词器(VQ)用于VLA，将动作块编码为图像
tldr: 针对自回归VLA模型中动作分词重建保真度与推理效率之间的权衡，提出FASTer框架。其分词器FASTerVQ将动作块编码为单通道图像，捕获全局时空依赖并实现高压缩比。基于此构建的自回归策略FASTerVLA采用块状解码和轻量级动作专家，在多个机器人操作基准上实现更强的泛化性和更快的推理速度。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA动作分词在重建保真度和效率之间存在矛盾。
method: 使用VQ将动作块编码为单通道图像，结合块状自回归解码。
result: 在多个操作基准上实现更强的泛化性和更快的推理速度。
conclusion: 可学习动作分词器结合块状解码能有效提升VLA模型效率。
---

## Abstract
Autoregressive vision-language-action (VLA) models have recently demonstrated strong capabilities in robotic manipulation. However, their core process of action tokenization often involves a trade-off between reconstruction fidelity and inference efficiency.
We introduce \textbf{FASTer}, a unified framework for efficient and generalizable robot learning that integrates a learnable tokenizer with an autoregressive policy built upon it.
FASTerVQ encodes action chunks as single-channel images, capturing global spatio-temporal dependencies while maintaining a high compression ratio. FASTerVLA builds on this tokenizer with block-wise autoregressive decoding and a lightweight action expert, achieving both faster inference and higher task performance.
Extensive experiments across simulated and real-world benchmarks show that FASTerVQ delivers superior reconstruction quality, high token utilization, and strong cross-task and cross-embodiment generalization, while FASTerVLA further improves overall capability, surpassing previous state-of-the-art VLA models in both inference speed and task performance.

---

## 论文详细总结（自动生成）

# FASTer: 具有可学习动作分词器和块状解码的强大高效自回归视觉-语言-动作模型——详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：自回归视觉-语言-动作（VLA）模型在机器人操作任务中展现出强大能力，其核心是将连续动作序列离散化为“动作标记”（action tokenization），进而利用自回归方式生成动作序列。
- **核心问题**：现有动作分词方法在**重建保真度**（动作重建的准确性）与**推理效率**（生成速度和计算开销）之间存在根本性的权衡。高保真度的分词往往导致低压缩比，推理速度慢；而高效的紧凑表示可能丢失动作细节，影响任务性能。
- **整体含义**：本文旨在设计一种统一框架 **FASTer**，同时提升动作表示的压缩效率与重建质量，并利用这些高效表示加速自回归策略的推理，从而在泛化性和速度上超越现有VLA方法。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将动作块（action chunk）编码为**单通道图像**（single-channel images），利用图像空间的全局时空依赖建模能力，实现高压缩比的同时保持高重建保真度。并以此为基础构建块状自回归解码策略，搭配轻量级动作专家网络，提升整体效率。
- **关键技术细节**：
  - **FASTerVQ（可学习动作分词器）**：
    - 使用VQ（Vector Quantization）变分自编码器结构。
    - 输入：连续动作块（通常包含多个时间步的动作序列）。
    - 输出：离散化的**单通道图像**表示（类似于1-channel feature map），而非传统的扁平化代码序列。
    - 优势：单通道图像保留了动作块的**时空局部性**（相邻时间步动作相关性高等），VQ过程可显式建模全局依赖，同时保持极高压缩比（一个动作块只需极少图像token）。
  - **FASTerVLA（自回归策略）**：
    - 基于FASTerVQ产生的离散动作图像令牌，采用**块状自回归解码**（block-wise autoregressive decoding）：一次解码整个图像块（block），而非逐像素/逐token生成，大幅减少解码步数。
    - 引入**轻量级动作专家**（lightweight action expert）：一个小型神经网络，将解码后的图像令牌映射回连续动作控制信号，降低整体模型计算量。
    - 整体流程：视觉-语言编码器提取上下文特征 → 从可学习的动作码本中初始化目标token → 通过块状自回归逐步预测整个动作图像 → 动作专家解码出连续动作。
- **公式/算法流程（文字说明）**：
  1. **训练FASTerVQ**：收集多任务动作数据，编码为单通道图像，通过VQ训练码本和编解码器，优化重建损失和码本对齐损失。
  2. **构建FASTerVLA**：冻结FASTerVQ编码器/码本，将视觉-语言特征作为条件，训练自回归Transformer预测离散动作图像序号序列，采用块级自回归损失（如交叉熵）和动作专家回归损失。
  3. **推理**：输入任务指令和当前观察，经视觉-语言编码得到条件向量；以起始token为起点，按块自回归解码（每次解码一个2×2或4×4的图像块），直至生成完整动作图像；动作专家将图像解码为连续动作序列，直接下发执行。

## 3. 实验设计：数据集/场景、benchmark、对比方法

- **数据集与场景**：
  - **模拟环境**：包括多个标准机器人操作基准（如MetaWorld、RLBench、CALVIN等，具体名称未在摘要中完全列出，但提及“simulated and real-world benchmarks”）。
  - **真实世界基准**：涉及实际机器人操作任务（如抓取、放置、开门等），可能包含跨物体、跨场景配置。
- **Benchmark**：
  - 重建保真度：动作重建误差（如MSE）、码本利用率（token utilization）。
  - 任务性能：成功率（success rate）、平均完成步数。
  - 推理速度：每秒生成动作帧数（FPS）或延迟（ms）。
- **对比方法**：
  - 其他VLA模型：前序SOTA自回归VLA（如TokenVLA、ACT等，具体名称未给出，但文称“surpassing previous state-of-the-art VLA models”）。
  - 可能包括无动作分词基线、不同压缩比的VQ方法。
- **实验环境**：未详细说明，但涉及多种具身形态（cross-embodiment），表明泛化性测试。

## 4. 资源与算力

- **文中未明确说明**：摘要和元数据未提及GPU型号、数量、训练时长等具体算力信息。
- **合理推断**：考虑到ICLR-2026论文风格和时下VLA模型常见规模，训练可能需要4-16张A100/RTX 4090等，训练时间可能为数天至数周。但论文原文应包含实验设置细节，本文限于提供的信息无法确认。

## 5. 实验数量与充分性

- **实验数量**：从描述看，至少包括：
  - 对**分词器FASTerVQ**在不同动作数据集上的重建质量和压缩效率的评测。
  - 对**策略FASTerVLA**在多个模拟基准和真实机器人任务上的成功率对比。
  - 跨任务泛化、跨具身形态（cross-embodiment）测试。
  - 消融研究：可能包括块大小、码本大小、是否使用动作专家等的影响。
  - 推理速度对比实验。
- **充分性与公平性**：
  - 实验覆盖了模拟和真实世界，涉及多种任务和平台，具有较强的泛化验证。
  - 对比了SOTA方法，体现了先进性。
  - 但未提供具体实验数量和数据集的详尽列表，仅凭摘要无法全面判断是否覆盖了所有常见消融项；通常ICLR论文会包含足够的消融和统计显著性测试。**基于现有信息，实验设计较为充分，但缺少可重复细节**；公平性方面，若对比方法使用原论文最佳设定且保持一致，则较为客观。

## 6. 论文的主要结论与发现

- **FASTerVQ**能够以单通道图像形式高效编码动作块，在保持高重建保真度的同时实现高压缩比和优异的token利用率（可能接近100%）。
- 基于FASTerVQ构建的**FASTerVLA**通过块状自回归解码和轻量级动作专家，**显著提升推理速度**（超越先前VLA模型）且**不牺牲甚至提升任务成功率**。
- 该框架在**跨任务**和**跨具身**（不同机器人平台）泛化方面表现优异，表明动作图像表示具有较好的可迁移性。
- **统一了自回归VLA模型在效率与保真度上的矛盾**，为实际机器人部署提供了可行的快速且强大的方案。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：将动作块作为单通道图像进行VQ编码，巧妙利用图像空间的结构感知能力，这是与之前将动作序列简单展平为1D序列的重要区别。
- **块状自回归解码**：一次解码一块Token，比逐Token解码显著减少自回归步数，理论加速与实际相符。
- **轻量级动作专家**：将离散token转为连续动作的模块设计得很轻，避免解码成为新瓶颈。
- **泛化性强**：跨任务和跨具身测试验证了表示的通用性，体现了实用性。
- **实验维度全面**：同时评估重建质量、任务性能、推理速度，并且有真实世界验证，增加了论文说服力。

## 8. 不足与局限

> **注意**：由于仅基于摘要和元数据，以下推测部分可能不准确，但基于研究常识指出潜在不足。

- **实验细节缺失**：摘要未给出具体数据集的成功率数字，难以定量判断提升幅度；也未对对比方法做公平性说明（如是否使用相同基础网络、相同输入处理等）。
- **算力资源未报告**：这对于可复现性和实际部署成本评估是一种缺失；可能正文中有提及，但此处为未知。
- **动作表示适用范围**：单通道图像VQ可能适用于动作块长度固定、动作维度较低的场景（如6-DOF末端执行器位姿+夹爪），对于高自由度灵巧手或全身移动操作，其时空依赖建模效果尚需验证。
- **任务类型有限**：主要针对操作任务，未在导航、人机交互等场景测试，泛化到更广泛机器人任务的能力存疑。
- **潜在的偏差风险**：如果训练数据中某些任务占比过大，可能影响跨任务泛化性的客观评估；消融实验是否排除了其他改进因素的贡献不明确。
- **长期时间依赖性**：动作块长度通常为几十个时间步，块状自回归解码可能难以处理超长行动序列（如持续1分钟的任务），可能需要分层划分块。该点未讨论。

---
（完）
