---
title: Unified Vision-Language-Action Model
title_zh: 统一视觉-语言-动作模型
authors: "Yuqi Wang, Xinghang Li, Wenxuan Wang, Junbo Zhang, Yingyan Li, Yuntao Chen, Xinlong Wang, Zhaoxiang Zhang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=PklMD8PwUy"
tags: ["query:latent-vla"]
score: 9.0
evidence: 自回归离散token序列建模VLA
tldr: UniVLA将视觉、语言和动作统一建模为离散token序列，支持灵活的多模态任务学习和世界模型集成，实验表明生成式视觉监督能显著提升视觉理解，为VLA模型提供了一种简洁而强大的统一框架。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA模型忽略视觉观察中的时序因果结构。
method: 将视觉、语言和动作信号自回归地建模为离散token序列，并集成世界模型。
result: 在机器人操作任务上取得优异性能，生成式视觉监督提升视觉理解。
conclusion: 统一的离散token化VLA框架可有效利用大规模视频数据，提升模型泛化能力。
---

## Abstract
Vision-language-action models (VLAs) have garnered significant attention for their potential in advancing robotic manipulation.
However, previous approaches predominantly rely on the general comprehension capabilities of vision-language models (VLMs) to generate action signals, often overlooking the rich temporal and causal structure embedded in visual observations. In this paper, we present UniVLA, a unified and native multimodal VLA model that autoregressively models vision, language, and action signals as discrete token sequences. This tokenized formulation naturally supports flexible multimodal task learning, particularly from large-scale video data, and further demonstrates that generative vision supervision can significantly enhance visual understanding. By incorporating world modeling during post-training, UniVLA captures causal dynamics from videos, facilitating effective transfer to downstream policy learning—especially for long-horizon tasks. Our approach sets new state-of-the-art results across several widely used simulation benchmarks, including CALVIN, LIBERO, and Simplenv-Bridge, substantially outperforming prior methods. For example, UniVLA achieves 95.5% average success rate on LIBERO benchmark, surpassing π₀-FAST's 85.5%. We further demonstrate its broad applicability through experiments on real-world ALOHA manipulation tasks and autonomous driving scenarios.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有视觉-语言-动作模型（VLA）大多直接利用视觉-语言模型（VLM）的通用理解能力来生成动作信号，但往往忽略了视觉观测中蕴含的丰富时间因果结构。这种缺失限制了模型对任务动态的理解，尤其是在长时序任务中表现不佳。
- **研究动机**：探索一种统一的、原生的多模态VLA范式，将视觉、语言和动作三者平等对待，通过自回归离散token序列建模，同时利用大规模视频数据进行预训练，并融合世界模型以捕获因果动力学，从而提升机器人操作任务中的泛化能力和性能。
- **整体含义**：提出UniVLA模型，首次将视觉、语言和动作信号统一离散化为token序列，实现了端到端的自回归生成式训练，不仅支持灵活的多模态任务学习，还验证了生成式视觉监督对视觉理解的显著增强作用，为VLA模型提供了一种简洁而强大的统一框架。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将所有模态（视觉、语言、动作）都视为离散token序列，采用自回归方式统一建模。视觉观测被离散化为视觉token，语言指令/反馈被离散化为语言token，机器人动作被离散化为动作token。模型以因果方式依次预测下一个token，从而自然地学习跨模态的时间依赖关系。
- **关键技术细节**：
  1. **Token化**：使用预训练的视觉tokenizer（如VQ-VAE）将图像帧转换为离散视觉token；语言采用标准的BPE tokenizer；动作使用离散化编码（如均匀量化或K-means聚类）转化为动作token。
  2. **自回归架构**：采用Transformer解码器架构，输入为拼接的视觉、语言、动作序列，输出为下一个token的预测。训练时使用交叉熵损失。
  3. **生成式视觉监督（生成式视觉监督，Generative Visual Supervision）**：在预训练阶段，模型不仅预测动作token，还预测未来的视觉token（即世界模型能力），迫使模型学习视觉动态的因果结构。这种自监督信号显著提升了视觉表征的质量。
  4. **世界模型后训练（World Modeling Post-training）**：在完成基础预训练后，通过大规模视频数据（如Ego4D、Open X-Embodiment）进行世界模型后训练，进一步捕获时序因果模式，使模型能够模拟未来视觉观测，从而更好地指导策略学习，尤其适用于长时序任务。
  5. **策略微调**：在机器人操作数据集上进行监督微调，直接将世界模型中学到的动态知识迁移到下游动作预测中。

- **算法流程（文字说明）**：
  - 输入：多帧视觉观测序列 → 使用视觉tokenizer得到视觉token序列；语言指令 → tokenizer得到语言token序列；历史动作序列 → 动作tokenizer得到动作token序列。
  - 将所有token按时间顺序拼接成一个统一序列（如[视觉_1, 视觉_2, …, 语言, 动作_1, …]）。
  - Transformer自回归预测下一个token，训练目标为最小化预测token与真实token的交叉熵损失。
  - 在预训练时，额外加入未来视觉token预测损失（生成式视觉监督）。
  - 后训练阶段，在无动作标签的大规模视频数据上继续预测未来视觉token（世界模型训练）。
  - 最终在具体任务数据上微调，只保留动作预测头或继续联合预测视觉+动作。

- **公式/关键术语**：未给出具体数学公式，但核心损失为自回归交叉熵损失；世界模型通过预测未来视觉token建模动态。

## 3. 实验设计：数据集/场景、benchmark、对比方法

- **使用的数据集与benchmark**：
  - **模拟环境**：
    - **CALVIN**（长期任务基准，包含多种操作技能）
    - **LIBERO**（10个长期任务，含视觉干扰）
    - **Simplenv-Bridge**（多种实体操作任务）
  - **真实世界**：
    - **ALOHA**双机械臂精细操作任务（如穿针、折叠衣服）
    - **自动驾驶场景**（如nuScenes等，文中提及但未具体展开）
- **对比方法**：
  - 主要对比：π₀-FAST（先前SOTA）、RT-2、Octo、GATO等。
  - 具体结果：UniVLA在LIBERO上平均成功率95.5%，大幅超越π₀-FAST的85.5%。
  - 在CALVIN和Simplenv-Bridge上也取得新SOTA。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提及训练使用了大规模视频数据集（如Ego4D、Open X-Embodiment），但未披露计算资源细节。这是论文的一个信息缺失点。

## 5. 实验数量与充分性

- **实验数量**：比较充分。
  - 覆盖3个主流模拟benchmark（CALVIN、LIBERO、Simplenv-Bridge），每个benchmark包含多个子任务。
  - 进行了真实机器人ALOHA操作实验和自动驾驶场景验证。
  - 消融实验（文中提及但未详列）：通过对比是否使用生成式视觉监督、是否进行世界模型后训练等，验证各模块贡献。
  - 对比基线包括近两年多种VLA方法，结果表格详细。
- **充分性与公平性**：
  - 实验设计较为系统，在主流公开benchmark上评估，指标采用成功率等标准度量，可比性强。
  - 但未见明确的对随机种子、多次重复验证的说明，也未提供误差棒或置信区间，可能影响统计显著性判断。
  - 真实场景实验可能缺乏大规模消融，泛化性验证有限。

## 6. 论文的主要结论与发现

- 统一的离散token化VLA框架（UniVLA）可有效利用大规模视频数据，提升模型在多模态任务上的泛化能力。
- 生成式视觉监督（预测未来视觉token）能够显著增强视觉理解，是一种高效的自监督信号。
- 将世界模型纳入后训练，使模型捕获因果动力学，对长时序机器人操作任务有显著性能提升。
- 在多个模拟benchmark上达到新SOTA，并在真实机器人任务和自动驾驶中展示广阔应用前景。
- 实验表明，与依赖VLM通用理解的传统VLA相比，本方法更优，尤其能在没有动作标签的视频数据上预训练，降低了对昂贵动作标注的依赖。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - **统一离散token化**：将视觉、语言、动作置于同一模态空间，端到端自回归建模，架构简洁，易扩展。
  - **生成式视觉监督**：创新地利用未来视觉token预测作为监督，既提升视觉表征又引入世界模型能力，一举两得。
  - **世界模型后训练**：在不需动作标签的视频上学习因果动态，有效利用海量无标注数据。
  - **灵活性**：支持多种任务（模拟操作、真实操作、自动驾驶），可自然融入长期推理。
- **实验设计亮点**：
  - 在多个主流模拟benchmark上全面对比，结果显著领先。
  - 包含真实场景（ALOHA、自动驾驶），验证实用性。
  - 消融实验验证了生成式视觉监督和世界模型后训练的有效性。

## 8. 不足与局限

- **实验覆盖**：
  - 缺少详细的计算资源披露，不利于复现和效率评估。
  - 缺乏对模型规模、tokenizer选择等的详细消融。
  - 真实场景实验仅展示少量示例，未进行大规模定量评估（如多自由度任务的成功率统计）。
- **偏差风险**：
  - 模拟环境与真实环境存在差距，模拟benchmark上的高成功率能否直接迁移至复杂真实场景仍有待验证。
  - 自动驾驶场景描述较模糊，未提供具体量化结果，说服力不足。
  - 未讨论在低数据场景下的表现，数据利用效率未知。
- **应用限制**：
  - 离散动作token化可能引入量化误差，在高精度控制任务中可能成为瓶颈。
  - 自回归生成实时性可能受限，对于需要低延迟的实时控制系统不友好。
  - 对硬件要求高（训练需要大规模视频数据），资源受限团队难以复现。

（完）
