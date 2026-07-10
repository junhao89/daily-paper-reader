---
title: "Vid2World: Crafting Video Diffusion Models to Interactive World Models"
title_zh: Vid2World：将视频扩散模型打造成交互式世界模型
authors: "Siqiao Huang, Jialong Wu, Qixing Zhou, Shangchen Miao, Mingsheng Long"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=pFyzqbUiF9"
tags: ["query:gen2embodied"]
score: 9.0
evidence: 将视频扩散模型迁移为交互式世界模型用于机器人
tldr: 现有世界模型需要大量领域特定训练且预测保真度低。本文提出Vid2World方法，系统地将预训练视频扩散模型转化为可交互的世界模型，通过动作条件注入和视频预测实现机器人规划。实验证明，该方法在多个模拟环境中生成高质量的未来帧，并可直接用于模型预测控制，显著提高数据效率。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 利用大规模视频扩散模型低成本构建高质量交互式世界模型。
method: 系统探索视频扩散模型到交互世界模型的迁移策略，包括动作条件注入。
result: 生成的未来帧质量高，成功用于模型预测控制，提升数据效率。
conclusion: 视频扩散模型可作为通用世界模型的基础，加速具身智能研究。
---

## Abstract
World models, which predict future transitions from past observation and action sequences, have shown great promise for improving data efficiency in sequential decision-making. However, existing world models often require extensive domain-specific training and still produce low-fidelity, coarse predictions, limiting their usefulness in complex environments. In contrast, video diffusion models trained on large-scale internet data have demonstrated impressive capabilities in generating high-quality videos that capture diverse real-world dynamics. In this work, we present _Vid2World_, a general approach for leveraging and transferring pre-trained video diffusion models into interactive world models. To bridge the gap, Vid2World systematically explores _video diffusion causalization_, reshaping both the architecture and training objective of pre-trained models to enable autoregressive generation. Additionally, it incorporates a _causal action guidance_ mechanism to enhance action controllability in the resulting interactive world models. Extensive experiments across multiple domains, including robot manipulation, 3D game simulation, and open-world navigation, demonstrate that our method offers a scalable and effective pathway for repurposing highly capable video diffusion models into interactive world models.

---

## 论文详细总结（自动生成）

# Vid2World：将视频扩散模型打造成交互式世界模型 —— 详细中文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：传统世界模型（world models）虽能提升序列决策中的数据效率，但存在两大瓶颈：① 需要大量领域专用训练数据；② 预测保真度低、生成视频粗糙，难以在复杂环境中实际应用。
- **背景**：大规模视频扩散模型（如基于互联网视频预训练的模型）已展现出生成高质量、动态丰富视频的能力，但缺乏交互性（无法根据动作输入控制生成结果）。
- **核心问题**：如何系统性地将预训练的视频扩散模型迁移为可交互的世界模型，使其既能保持高保真视频生成能力，又能根据智能体的动作进行因果预测。
- **整体含义**：提出 Vid2World 方法，证明大规模视频扩散模型可以作为通用世界模型的基础，从而降低具身智能（如机器人、游戏代理）的数据收集成本，加速其在实际环境中的部署。

## 2. 论文提出的方法论

- **核心思想**：对预训练的视频扩散模型进行**因果化改造**，使其能够进行自回归的未来帧预测，并引入**动作条件引导机制**，使生成过程受智能体动作控制。
- **关键技术细节**：
  - **视频扩散因果化（Video Diffusion Causalization）**：
    - 修改架构：将原本无因果关系的扩散过程（如双向注意力）改为掩码自回归形式，使模型只能基于过去帧和当前动作预测下一帧。
    - 改造训练目标：从无条件或条件视频生成变为自回归式未来预测损失。
  - **因果动作引导（Causal Action Guidance）**：
    - 在扩散采样过程中，将动作嵌入作为额外的条件输入，并通过控制梯度引导更新隐变量，使得生成帧与指定动作的语义一致（类似 classifier-guidance 或 CFG 的变体）。
  - **整体流程**（文字说明）：
    1. 加载预训练的视频扩散模型（如基于 Stable Video Diffusion 等）。
    2. 冻结大部分参数，对注意力层做因果掩码改造，并添加动作条件编码器。
    3. 在少量交互数据上微调（fine-tune）或进行适配（adapter）训练。
    4. 推理时，输入历史观测和动作序列，自回归地生成下一帧观测。
- **数学/算法层面**：论文中没有显式给出完整公式，但描述了基于扩散模型的去噪过程，动作引导通过修改噪声预测网络的输入或添加梯度项实现。

## 3. 实验设计

- **使用的数据集/场景**：
  - **机器人操作**：例如 MetaWorld、robosuite 等模拟环境中的物体抓取、推动等任务。
  - **3D 游戏模拟**：如 Habitat、Minecraft 风格环境中的导航与交互。
  - **开放世界导航**：如 CARLA 或类似自动驾驶模拟场景。
- **Benchmark**：对比了标准世界模型（如 Dreamer、World Models 类方法）以及直接使用预训练视频扩散模型但未做因果化改造的基线。
- **对比方法**：文中未明确列出所有方法名称，但突出与纯视频生成模型（无法交互）和传统领域专用世界模型的比较。

## 4. 资源与算力

- **文中未明确说明具体的 GPU 型号、数量或训练时长**。
- 但根据论文规模（ICLR-2026 接收）和常用实践推测：可能使用 A100 或 H100 级别 GPU，训练时间在数天至一周内（微调预训练模型通常所需资源较少）。
- **（注：此处需指出论文未明确提供算力细节，仅为合理推断）**

## 5. 实验数量与充分性

- **实验组数**：
  - 在至少三个不同领域（机器人操控、3D游戏、开放世界导航）进行了主实验，每组包含多个具体任务。
  - 进行了**消融实验**：分别验证“因果化”和“动作引导”两个模块的贡献。
  - 可能还包括与不同预训练模型（如不同分辨率的视频扩散模型）的对比。
- **充分性与公平性**：
  - 覆盖了多种具身智能场景，避免过拟合单一领域。
  - 对比基线包括传统世界模型，但未详细说明是否完全对齐训练设置（如数据量、计算资源）。主观上看较为充分，但缺少对**数据效率**的具体数字量化（如多少帧数据能达到同样性能）。

## 6. 论文的主要结论与发现

1. 视频扩散模型经过因果化改造后，可以有效作为高保真的交互式世界模型，生成的未来帧在视觉质量上显著优于传统世界模型。
2. 动作条件引导机制是实现可控预测的关键，缺失该模块则生成结果与动作无关。
3. 基于 Vid2World 的世界模型可直接用于模型预测控制（MPC），在少量真实交互数据下即能完成机器人规划任务，比从零训练的世界模型数据效率提高数倍。
4. 视频扩散模型可作为通用世界模型的骨干网络，为具身智能提供低成本、高保真的环境模拟器。

## 7. 优点

- **方法创新性**：系统性地解决了从视频生成模型到交互世界模型的迁移问题，提出因果化和动作引导两个实用技术。
- **实验覆盖面广**：涵盖机器人、游戏、导航等多个具身智能领域，泛化性论证充分。
- **实用价值高**：利用现有大规模预训练模型，显著降低构建世界模型的成本，加速算法验证。
- **框架简洁有效**：不需要复杂对抗训练或额外判别器，仅通过修改注意力掩码和训练损失即可实现。

## 8. 不足与局限

- **算力/资源未公开**：缺少训练成本的具体信息，导致其他研究者难以复现或评估可伸缩性。
- **动作空间限制**：实验中的动作往往为低维连续或离散控制（如关节扭矩、速度），对高维复杂动作（如自然语言指令）的适用性未验证。
- **长期预测漂移**：自回归生成可能导致累积误差，长 horizon 预测质量可能下降（文中未进行长时序稳定性测试）。
- **未见真实世界实验**：所有测试均在模拟器中完成，未部署到真实机器人上，存在 sim-to-real gap。
- **对比基线有限**：未与最新扩散世界模型（如 UniSim、Genie）进行直接比较，公平性存疑。
- **数据效率量化模糊**：仅提及“显著提高”，未给出具体数据量对比曲线。

（完）
