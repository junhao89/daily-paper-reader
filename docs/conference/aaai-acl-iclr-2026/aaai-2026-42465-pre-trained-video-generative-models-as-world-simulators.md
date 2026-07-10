---
title: Pre-Trained Video Generative Models as World Simulators
title_zh: 预训练视频生成模型作为世界模拟器
authors: "Haoran He, Yang Zhang, Liang Lin, Zhongwen Xu, Ling Pan"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/42465/46426"
tags: ["query:gen2embodied"]
score: 9.0
evidence: 将视频生成模型转化为可操作的世界模拟器
tldr: 视频生成模型在互联网数据上预训练后能生成逼真视频，但缺乏交互性。本文提出动态世界模拟（DWS）方法，通过轻量级动作条件模块将预训练视频生成模型转化为可执行指定动作轨迹的交互式世界模拟器。该方法无需复杂视觉细节，实现了动作与视觉变化的对齐，为机器人策略提供高质量模拟环境。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-42465/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 778, \"height\": 634, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-42465/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 753, \"height\": 512, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-42465/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 849, \"height\": 414, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-42465/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 880, \"height\": 682, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-42465/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 780, \"height\": 659, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-42465/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1727, \"height\": 458, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-42465/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 878, \"height\": 506, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-42465/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 873, \"height\": 115, \"label\": \"Table\"}]"
motivation: 解决预训练视频生成模型只能基于静态提示生成视频、无法建模交互动态场景的局限。
method: 提出动态世界模拟（DWS），通过轻量通用动作条件模块将视频生成模型转化为可执行指定动作轨迹的交互式世界模拟器。
result: 方法实现了动作与视觉变化精确对齐，在多个任务上验证了生成高质量交互视频的能力。
conclusion: 该工作为利用视频生成模型构建机器人世界模型提供了有效途径。
---

## Abstract
Video generative models pre-trained on large-scale internet datasets have achieved remarkable success, excelling at producing realistic synthetic videos. However, they often generate clips based on static prompts (e.g., text or images), limiting their ability to model interactive and dynamic scenarios. In this paper, we propose Dynamic World Simulation (DWS), a novel approach to transform pre-trained video generative models into controllable world simulators capable of executing specified action trajectories. To achieve precise alignment between conditioned actions and generated visual changes, we introduce a lightweight, universal action-conditioned module that seamlessly integrates into any existing model. Instead of focusing on complex visual details, we demonstrate that consistent dynamic transition modeling is the key to building powerful world simulators. Building upon this insight, we further introduce a motion-reinforced loss that enhances action controllability by compelling the model to capture dynamic changes more effectively. Experiments demonstrate that DWS can be versatilely applied to both diffusion and autoregressive transformer models, achieving significant improvements in generating action-controllable, dynamically consistent videos across games and robotics domains. Moreover, to facilitate the applications of the learned world simulator in downstream tasks such as model-based reinforcement learning, we propose prioritized imagination to improve sample efficiency, demonstrating competitive performance compared with state-of-the-art methods.

---

## 论文详细总结（自动生成）

# 预训练视频生成模型作为世界模拟器——论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：现有视频生成模型（如Open-Sora、Wan2.1、iVideoGPT）虽能生成高质量、时间一致性强的视频，但均基于静态提示（文本或初始帧）一次性生成，缺乏**帧级交互性**和**帧间动态建模**能力，无法模拟智能体与环境间的交互场景（如游戏、机器人控制）。
- **背景**：构建“世界模拟器”（world simulator）需实现动作条件化视频预测，即根据智能体动作生成后续视觉帧。现有方法（如Genie、GameNGen）需从零训练大规模模型，计算资源消耗巨大（Genie用256 TPUv5p训练125k步），且微调方法（如AViD、GameFactory）高度依赖特定架构，难以迁移到快速演进的视频生成模型上。
- **核心目标**：提出一种**架构无关、轻量、高效**的框架，将任意预训练视频生成模型转化为可执行指定动作轨迹的交互式世界模拟器，从而利用大规模互联网视频中蕴含的物理常识和动态规则。

## 2. 方法论：核心思想、关键技术细节

### 2.1 总体框架：Dynamic World Simulation (DWS)

- **核心思想**：在预训练视频生成模型基础上，仅增加轻量模块和特殊损失函数，即可实现精确的帧级动作控制。不追求复杂视觉细节，而是聚焦于**一致的动态过渡建模**。
- **三个关键技术**：
  1. **动作条件化模块（Action-Conditioned Module）**
     - **目的**：实现每个动作与其对应帧的精确对齐。
     - **设计**：在每个Transformer块中引入由两个线性层组成的轻量模块。通过缩放（scale）和偏移（shift）操作调制帧嵌入 \( x_i \)：
       \[
       x_i = x_i + \text{FFN}(\text{LayerNorm}(x_i) \times (1 + \alpha_i) + \beta_i)
       \]
       其中 \(\alpha_i, \beta_i\) 从对应动作嵌入 \( c_i \) 回归得到。
     - **动作表示**：离散动作空间用语言模板描述（如“向左移动”），利用预训练文本编码器获得嵌入；连续动作空间用可训练的线性嵌入器。
  2. **运动增强损失（Motion-Reinforced Loss）**
     - **动机**：传统损失（MSE或交叉熵）对所有像素一视同仁，但动态变化像素（对应运动物体）对世界模拟至关重要，静态背景贡献小。
     - **方法**：计算相邻帧像素差，经softmax归一化得到权重 \(\omega\) ，对每个像素加权：
       \[
       \omega_{i+1} = c \cdot \text{softmax}(|x_{i+1} - x_i|), \quad \mathcal{L}_{\text{motion}} = \mathcal{L}_{\text{prev}} \cdot \omega
       \]
       其中 \(c\) 为超参数，调节强化强度。初始帧 \(\omega_0 = 1\)。
     - **效果**：模型更关注运动区域，提升动作诱导变化的预测能力。
  3. **优先想象（Prioritized Imagination）**
     - **问题**：在离线想象（model-based RL）中，均匀采样初始观测导致学习效率低。
     - **方案**：维护一个缓冲区，按TD损失幅度（代表学习潜力）对初始观测进行优先级排序，高潜力状态被更多采样。

### 2.2 与下游模型基强化学习（MBRL）的结合

- 除视频预测外，增加奖励预测模型（线性层+自注意力/交叉注意力）。
- 基于MBPO框架，用PPO算法训练actor-critic，合成轨迹由DWS世界模型生成。

## 3. 实验设计

### 3.1 数据集与场景

| 数据集 | 类型 | 动作空间 | 任务 |
|--------|------|----------|------|
| BAIR Robot Pushing | 机器人操作 | 连续 | 预测15帧视频 |
| Procgen (Coinrun, Ninja) | 游戏平台 | 离散 | 行动条件视频预测 + MBRL |
| Atari (Breakout, Battle Zone) | 街机游戏 | 离散 | 行动条件视频预测 + MBRL |

### 3.2 基模与对比方法

- **基模**：Open-Sora（扩散）、Wan2.1（流）、iVideoGPT（自回归Transformer）。
- **视频预测对比**：VideoGPT、MaskViT、FitVid、MCVD、MAGVIT、Open-Sora基准、Wan2.1基准、iVideoGPT基准。
- **MBRL对比**：Dreamerv3、PPO、PPG（仅Procgen）。
- **离线RL对比**：CQL、IQL（无/有世界模型增强数据）。

### 3.3 评估指标

- **视频质量**：FVD（↓）、PSNR（↑）、SSIM（↑）、LPIPS（↓）。
- **RL性能**：平均累积回报（±标准差），5个随机种子。

## 4. 资源与算力

- **未明确说明**：论文未报告DWS微调所需的具体GPU型号、数量及训练时长。
- **相关参考**：在相关工作部分提到，Genie和GameNGen需极高算力（如Genie: 256 TPUv5p × 125k步），暗示本文方法通过轻量微调显著降低资源需求，但未量化。

## 5. 实验数量与充分性

- **视频预测**：在BAIR上全面定量（4个指标×3种基模），在Atari和Procgen上也有结果（见附录）；定性图例涵盖两个域。
- **MBRL在线**：2个benchmark（Atari + Procgen），共4个环境，与Dreamerv3、PPO等SOTA对比，均有5个种子。
- **离线RL**：2个环境，2种算法（CQL、IQL），3个种子，有/无世界模型增强。
- **消融实验**：在Procgen上验证优先想象（图6b），在BAIR上验证运动增强损失和动作模块（图3b）。
- **充分性评价**：实验设计较全面，覆盖质量、控制性、下游应用，消融充分；但未涉及长视频（>15帧）、高分辨率、3D复杂环境等，泛化性验证有限。

## 6. 主要结论与发现

1. **DWS显著提升动作条件视频预测质量**：在BAIR上，Open-Sora+DWS将FVD降低11.7%，LPIPS降低27.9%；Wan2.1+DWS将FVD从29.6降至24.1；iVideoGPT+DWS在所有指标上均有提升。
2. **架构无关性**：方法同时适用于扩散/流/自回归三种主流架构，且均能改善。
3. **世界模拟器有效支持MBRL**：在Atari Breakout上，DWS+PPO达到Dreamerv3的约7倍回报；在Procgen上优于PPO和PPG。
4. **优先想象提升样本效率**：消融证实该机制带来显著性能增益。
5. **离线数据增强有效**：用世界模型生成数据扩充离线数据集，可提升CQL和IQL性能约0.3~0.6回报。

## 7. 优点

1. **轻量高效**：动作条件模块仅2个线性层，微调成本极低，免去从零训练的昂贵开销。
2. **架构通用**：可无缝集成到任何注意力块，不依赖特定模型设计，便于利用快速发展的视频生成模型。
3. **洞察深刻**：明确指出“动态过渡建模比视觉细节更重要”，并针对性设计运动增强损失，概念新颖、实现简单。
4. **下游应用完整**：不仅限于生成，还给出MBRL和离线RL的完整方案，优先想象具有实用价值。
5. **实验充分**：多数据集、多基模、多任务、多指标，消融齐全，且代码开源（附录提及）。

## 8. 不足与局限

1. **时空分辨率受限**：论文承认目前局限于**中等时空分辨率**（如15帧），长序列或高清视频的生成质量和延迟尚未验证。
2. **推理速度与质量的权衡**：未明确报告FPS或延迟，但对实时交互应用（如游戏）可能仍不满足要求。
3. **领域泛化有限**：仅在机器人推箱和2D平台游戏上测试，未在更复杂3D环境（如Habitat、MuJoCo）或开放性任务（如Minecraft）中评估。
4. **动作表示依赖先验**：离散动作需手工设计语言模板，可能无法覆盖所有游戏动作语义；连续动作嵌入器训练数据量未讨论。
5. **安全与公平性未涉及**：作为世界模拟器可能被滥用（如生成有害内容），论文未讨论任何伦理或偏见风险。
6. **缺少对预训练模型原始能力的深入分析**：未定量衡量DWS是否保持原模型的泛化能力，或是否引入灾难性遗忘。

（完）
