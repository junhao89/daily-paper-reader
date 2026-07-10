---
title: "World-In-World: World Models in a Closed-Loop World"
title_zh: 世界中的世界：闭环世界中的世界模型
authors: "Jiahan Zhang, Muqing Jiang, Nanru Dai, TaiMing Lu, Arda Uzunoglu, Shunchi Zhang, Yana Wei, Jiahao Wang, Vishal M. Patel, Paul Pu Liang, Daniel Khashabi, Cheng Peng, Rama Chellappa, Tianmin Shu, Alan Yuille, Yilun Du, Jieneng Chen"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=yDmb7xAfeb"
tags: ["query:wam-vla"]
score: 7.0
evidence: 在闭环环境中基准测试世界模型用于具身AI，与世界动作模型相关
tldr: 针对现有关环基准测试忽略了世界模型（WM）在具身任务中的实际效用，提出了World-In-World平台，在闭环环境中评估WM；提供统一的在线规划策略和标准化动作API，揭示了WM在决策中的潜力与局限。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有世界模型基准采用开环协议，忽略了其在实际具身任务中的效用。
method: 构建World-In-World平台，在闭环环境中通过统一在线规划策略和标准化动作API评估异构世界模型。
result: 揭示世界模型在具身任务中的有效性和局限性。
conclusion: 闭环评估是理解世界模型在机器人中实际效用的关键。
---

## Abstract
Generative world models (WMs) can now simulate worlds with striking visual realism, which naturally raises the question of whether they can endow embodied agents with predictive perception for decision making. Progress on this question has been limited by fragmented evaluation: most existing benchmarks adopt open-loop protocols that emphasize visual quality in isolation, leaving the core issue of embodied utility unresolved, i.e., *do WMs actually help agents succeed at embodied tasks?*
To address this gap, we introduce World-In-World, the first open platform that benchmarks WMs in a closed-loop setting that mirrors real agent-environment interactions. World-In-World provides a unified online planning strategy and a standardized action API, enabling heterogeneous WMs for decision making.
We curate four closed-loop environments that rigorously evaluate diverse WMs, prioritize task success as the primary metric, and move beyond the common focus on visual quality; we also present the first data scaling law for world models in embodied settings.
Our study uncovers three surprises: (1) visual quality alone does not guarantee task success—controllability matters more; (2) scaling post-training with action-observation data is more effective than upgrading the pretrained video generators; and (3) allocating more inference-time compute allows WMs to substantially improve closed-loop performance. By centering evaluation on closed-loop outcomes, World-In-World establishes a new benchmark for the systematic assessment of WMs.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：当前生成式世界模型（WM）能够产生高度逼真的视觉模拟，但现有基准大多采用开环（open-loop）评估协议，仅孤立地关注视觉质量，忽略了世界模型在具身智能体决策中的实际效用。核心问题在于：**世界模型是否真正帮助智能体成功完成具身任务？**
- **整体含义**：需要从“视觉质量”转向“任务成功率”，在闭环（closed-loop）环境中衡量世界模型的实用性，以弥合模型能力与真实应用之间的鸿沟。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：构建一个开放的闭环基准平台 **World-In-World**，模拟真实智能体-环境交互，将世界模型与在线规划策略结合，以任务成功率为首要评估指标。
- **关键技术细节**：
  - **统一在线规划策略**：提供标准化的规划算法（如模型预测控制 MPPI 或基于价值的方法），使得不同世界模型能在相同规划框架下进行比较。
  - **标准化动作 API**：定义统一的动作接口，允许异构世界模型（不同架构、不同预训练方式）接入平台进行决策。
  - **评估闭环性能**：每次执行中，智能体根据当前观测使用世界模型预测未来状态，选择最优动作，并在环境中执行，收集新的观测，循环直至任务结束。
- **算法流程（文字说明）**：智能体接收初始观测 → 世界模型基于历史轨迹预测多种未来可能性 → 规划器利用这些预测计算动作（例如通过滚动优化） → 执行动作 → 环境返回新观测和奖励 → 更新历史并重复，直到终止条件。

### 3. 实验设计：数据集/场景、基准、对比方法
- **测评场景**：包含 **4个闭环环境**（具体环境名称未在摘要中列出，推测为模拟机器人导航、操控等具身任务场景）。
- **基准（Benchmark）**：World-In-World 平台本身作为基准，提供统一的闭环评估流程。
- **对比方法**：未在摘要中详细列出，但隐含着比较不同世界模型（如不同预训练视频生成器、不同后训练策略）在闭环任务中的表现。
- **数据缩放定律**：首次揭示了世界模型在具身设定下的数据缩放定律（随训练数据量增加的性能变化）。

### 4. 资源与算力
- **未明确说明**：摘要及提取的文本中未提及使用的 GPU 型号、数量、训练时长等算力信息。因此无法总结具体的资源消耗，需指出这一缺失。

### 5. 实验数量与充分性
- **实验数量**：主要围绕四个闭环环境展开，并进行了数据缩放实验（不同规模的后训练数据）。此外还包含消融实验（如对比不同规划策略、不同世界模型组件）——但摘要中未详细列出具体组数。
- **充分性与公平性**：实验设计强调统一规划策略和标准化 API，确保不同模型在同一条件下评估，具有公平性。但仅覆盖四个环境，可能不足以完全泛化到所有具身任务。结论揭示了三个“惊喜”，表明实验设计有效发现了关键因素。

### 6. 论文的主要结论与发现
- **三大惊喜发现**：
  1. **视觉质量不能保证任务成功**：模型的可控性（controllability）比视觉逼真度更重要。
  2. **后训练数据缩放比升级视频生成器更有效**：使用动作-观测数据进行后训练（scaling post-training with action-observation data）对闭环性能的提升大于更换更先进的预训练视频生成器。
  3. **增加推理计算量可显著改善闭环表现**：在推理阶段分配更多计算资源（如更长的规划 horizon 或更多采样）能大幅提升任务成功率。
- **核心结论**：闭环评估是理解世界模型在机器人中实际效用的关键，任务成功率应取代视觉质量成为首要指标。

### 7. 优点：方法或实验设计上的亮点
- **首次提出闭环基准平台**：填补了现有开环基准的空白，直接关联具身智能体的决策效用。
- **统一评估框架**：通过标准化的规划策略和动作 API，消除了不同模型因规划方式差异导致的比较偏差。
- **强调任务成功率**：推动领域从“好看”转向“好用”，更具应用价值。
- **揭示数据缩放定律和推理计算策略**：为后续研究提供了实用指导（如资源分配应侧重后训练数据和推理计算而非模型预训练规模）。

### 8. 不足与局限
- **环境覆盖有限**：仅包含 4 个闭环环境，可能无法覆盖多种真实机器人任务（如操作复杂物体、多智能体协同等）。
- **缺乏算力报告**：未披露训练和评估所需的计算资源，限制了可复现性和成本评估。
- **世界模型异构性可能引入隐含偏差**：虽然统一了姿态 API，但不同世界模型的内部架构差异可能导致规划器对不同模型的适配程度不同，影响公平性。
- **未讨论泛化到真实物理世界**：所有测试均在模拟环境中进行，实体机器人上的迁移效果未知。
- **对比方法不够详尽**：摘要未列出具体对比的世界模型名称，削弱了结论的可复现性。

（完）
