---
title: Learning Diffusion Policy from Primitive Skills for Robot Manipulation
title_zh: 从原始技能中学习扩散策略用于机器人操作
authors: "Zhihao Gu, Ming Yang, Difan Zou, Dong Xu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38889/42851"
tags: ["query:latent-vla"]
score: 7.0
evidence: 使用视觉语言模型提取离散原始技能表征用于扩散策略
tldr: 针对全局指令导致动作生成不对齐的问题，提出了技能条件扩散策略SDP，通过视觉语言模型提取可复用的离散原始技能表征，结合条件动作规划，提高了机器人操作的准确性和泛化能力。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38889/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 845, \"height\": 321, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38889/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 806, \"height\": 371, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38889/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1444, \"height\": 703, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38889/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1566, \"height\": 462, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38889/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1668, \"height\": 340, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38889/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1358, \"height\": 717, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38889/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 866, \"height\": 405, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38889/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 773, \"height\": 546, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38889/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 844, \"height\": 210, \"label\": \"Table\"}]"
motivation: 现有扩散策略依赖全局指令生成短期控制信号，导致动作生成不对齐。
method: 提出SDP框架，利用视觉语言模型从视觉中提取离散原始技能表征，并集成可解释技能学习与条件动作规划。
result: SDP在机器人操作任务上表现出更好的动作对齐和泛化性能。
conclusion: 利用离散原始技能作为中间表征能有效提升扩散策略在复杂操作任务中的表现。
---

## Abstract
Diffusion policies have recently shown great promise for generating actions in robotic manipulation. However, existing approaches often rely on global instructions to produce short-term control signals, which can result in misalignment in action generation. We conjecture that the primitive skills, referred to as fine-grained, short-horizon manipulations, such as "move up" and "open the gripper", provide a more intuitive and effective interface for robot learning. To bridge this gap, we propose SDP, a skill-conditioned diffusion policy that integrates interpretable skill learning with conditional action planning. SDP abstracts eight reusable primitive skills across tasks and employs a vision-language model to extract discrete representations from visual observations and language instructions. Based on the representations, a lightweight router network is designed to assign a desired primitive skill for each state, which helps construct a single-skill policy to generate skill-aligned actions. By decomposing complex tasks into a sequence of primitive skills and selecting a single-skill policy, the proposed SDP ensures skill-consistent behavior across diverse tasks.
Extensive experiments on two challenging simulation benchmarks and real-world robot deployments demonstrate that SDP consistently outperforms state-of-the-art methods, providing a new paradigm for skill-based robot learning with diffusion policies.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有扩散策略（Diffusion Policy）在机器人操作中通常直接将高层次的语言指令映射到短期动作序列，这种粗粒度的指令与细粒度的控制信号之间存在粒度错配，导致动作生成出现歧义或不对齐。例如，任务“把柠檬放进黄色平底锅”需要分解为“向下移动”、“闭合夹爪”、“向上移动”等更具体的操作，但全局指令无法提供这种细分指导。
- **研究动机**：作者提出原始技能（primitive skills，如“向上移动”、“打开夹爪”等）作为更直观、更有效的中间接口，可以自然地衔接高层规划和低层控制，从而提升扩散策略的准确性和可解释性。
- **整体含义**：该工作将可解释的原始技能学习与扩散策略条件动作规划相结合，提出一种新的技能条件扩散策略（SDP）范式，旨在通过分解复杂任务为一系列可复用的原始技能，达到更精确、更一致的行为生成。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 2.1 核心思想
- 将不同任务中的短期操作抽象为 **8 种可复用的原始技能**（roll, yaw, open gripper, move up, translate, close gripper, move down, rotate）。
- 利用视觉语言模型（VLM）从当前视觉观测和高层语言指令中提取离散表示，再通过轻量级路由器网络为每个状态分配最合适的原始技能。
- 构建**单技能扩散策略**：根据分配的技能动态参数化策略的 FFN 层，生成与技能对齐的动作序列。

### 2.2 关键技术细节
1. **原始技能定义与组合提示集成（CPE）**：
   - 定义技能集合 \( P = \{\text{roll}, \text{yaw}, \dots\} \)。
   - 设计统一文本模板 `"the robot arm is going to {skill}."`，利用冻结的 CLIP 文本编码器和 MLP 生成每个技能的提示嵌入 \( p_i \)。
   - 预计算并存储所有技能提示，避免推理时重复计算。

2. **视觉语言模型提取状态表示**：
   - 使用共享图像编码器处理静态相机和手腕相机图像，获得视觉嵌入；高层指令通过 tokenizer 和词嵌入层处理后获得文本嵌入。
   - 视觉与文本令牌拼接后送入 Transformer 编码器，得到联合表示 \( z_{vl} \)。
   - 对 \( z_{vl} \) 做 token 平均得到 \( z_{\text{avg}} \)，再经过 MLP 和 Softmax 产生 8 个技能的 logits，通过 top-1 选择最匹配的技能，并从预计算的嵌入中取出对应技能嵌入 \( z \)。

3. **单技能扩散策略学习**：
   - **先验注入**：时间步长、本体感觉通过 MLP 编码后，用改进的 AdaLN（每个层共享调制信号）注入；视觉语言信息通过 Cross-Attention 注入每个 Transformer 块。
   - **技能相关 FFN 层**：引入类似 LoRA 的额外 FFN 层，其权重矩阵 \( W_z^1, W_z^2 \) 由技能嵌入 \( z \) 通过 MLP 生成，与原始 FFN 层结合：  
     \[
     \text{FFN}(x) = W_z^2 \cdot \text{SwishGLU}(W_z^1 x) + \text{FFN}_{\text{ori}}(x)
     \]
     该设计显式建立了技能与特征提取的依赖关系，同时减少参数量。
   - **训练目标**：基于去噪评分匹配损失 \( L_{\text{SM}} \) 加上正交损失 \( L_{\text{Orth}} \)（降低技能提示间的余弦相似度），总损失 \( L = L_{\text{SM}} + \gamma L_{\text{Orth}} \)，\(\gamma=0.01\)。
   - 推理时使用 DDIM 采样，仅需 4 步去噪即可生成动作。

## 3. 实验设计：数据集、基准、对比方法

### 3.1 仿真基准
- **CALVIN**：包含 4 种场景（A/B/C/D），34 个不同任务，24,000 条带语言标注的演示。评估设置：
  - `ABCD→D`：在 A、B、C、D 上训练，D 上零样本测试。
  - `ABC→D`：仅用 A、B、C 训练，在 D 上零样本测试。
  - 评估指标：连续 1~5 个任务的完成率及平均连续完成任务长度（1000 条指令链）。
- **LIBERO**：包含四个任务套件（Spatial、Object、Goal、Long），每个套件 10 个任务，每个任务 50 条人类遥操作演示。评估：在目标套件上进行监督微调，报告成功率。

### 3.2 真实世界评估
- 使用 6-DoF Lebai 机器人，共 9 个任务，每任务 30 条轨迹，20 次试验平均成功率。
- **多任务学习**（6 个任务）：空间意识（拾取放入、打开微波炉放薯片）、工具使用（扫入簸箕、用勺搅拌）、语义理解（倒水、叠积木）。
- **视觉泛化**（3 个任务）：操作未见物体（苹果、香蕉）、复杂干扰物下的拾取放置。

### 3.3 对比方法
- **仿真**：DiffPolicy、RoboFlamingo、GR-1、MDT、MoDE、Octo、OpenVLA、UniVLA、MaIL、UniActions 等（部分由官方代码复现）。
- **真实**：MoDE、OpenVLA。

## 4. 资源与算力

论文明确提到：
- 仿真任务：在 4 块 A100 GPU 上微调 40 个 epoch，优化器 AdamW，学习率 1e-4，batch size 64。
- 图像分辨率：224×224（静态和腕部相机）。
- 去噪步数：4 步（Nd=4）。
- 模型参数量：1017M，FLOPS 74.5G，推理时间 45.1ms（相对 MoDE 的 30.5ms 略有增加）。
- 真实世界任务：仅使用静态相机图像，训练 200 个 epoch。

说明实验算力资源充足，且模型规模较大，但推理效率可接受。

## 5. 实验数量与充分性

- **仿真实验**：
  - CALVIN 上两种设置（ABCD→D 和 ABC→D）各报告 3 个种子平均结果，含 1~5 步任务完成率和平均长度。
  - LIBERO 上四个套件，每个套件报告成功率，含不同基线的多组对比。
- **真实实验**：9 个任务，每任务 20 次试验（共 180 次独立测试），报告平均成功率。
- **消融实验**（表 3）：
  - (a) 关键组件消融：基础DP → 加Cross-Attention → 加Prior Injection → 加Skill Abstraction → 加CPE，在 CALVIN 和 LIBERO-Long 上逐步提升效果。
  - (b) 技能条件策略对比：Addition、Concat、FiLM 与本文公式(4)对比，本文最优。
- **复杂度分析**（表 4）：对比参数量、FLOPs、推理时间。
- **可视化分析**（图 5）：展示了技能分配随时间的变化与对应观测，定性验证技能与动作的对齐。

实验设计较为完整，覆盖多任务、泛化、消融、可视化等方面，对比方法涵盖当前主流 VLA 和扩散策略，且消融实验有明确的增量分析，实验结论客观可信。

## 6. 论文的主要结论与发现

- SDP 在 CALVIN 的 `ABC→D` 设置中，连续完成 5 个任务的成功率比 MoDE 高 14.5%（76.9% vs 62.4%），平均完成任务长度从 3.92 提升至 4.49。
- 在 LIBERO 上，SDP 平均成功率 96.9%，超越所有对比方法（如 UniVLA 92.5%，MDT 76.1%），特别在 LIBERO-Long 上达 93.8%，而其他方法均低于 90%。
- 真实世界多任务学习和视觉泛化上，SDP 显著优于 MoDE 和 OpenVLA，尤其在干扰物存在时仍保持 65% 成功率（基线低于 20%）。
- 消融实验证明每个组件（Cross-Attention、Prior Injection、Skill Abstraction、CPE）均有增益，其中技能抽象和条件策略设计贡献最大。
- 可视化显示 SDP 无需显式监督就能学到与观测对齐的技能分配，展现了可解释性。

## 7. 优点

1. **创新性**：首次将可解释的原始技能显式引入扩散策略的条件生成，提出技能条件 FFN 层建立技能与动作的依赖，思路新颖。
2. **性能突出**：在两个仿真基准和真实场景中均取得大幅领先，特别是对长 horizon 任务和零样本泛化能力提升明显。
3. **可解释性**：技能分配可视化能够清晰地展示任务分解过程，且技能文本模板化，便于人类理解。
4. **实用性**：仅需 4 步去噪（优于常见的 10 步），推理时间仅增加约 14.6ms，参数量虽大但可通过预存储提示压缩开销。
5. **实验充分**：对比方法多、场景涵盖仿真和真实、包含消融、复杂度、可视化等多角度分析，结论可靠。

## 8. 不足与局限

1. **技能数量固定**：只定义了 8 种原始技能，对于更复杂或新类型操作可能不够灵活，需要手动扩展技能集。
2. **模型规模较大**：1017M 参数（相比 Diff-P-T 的 286M 和 MoDE 的 780M），FLOPS 也更高，对边缘部署不友好。
3. **泛化边界有限**：虽然对形状相似的未见物体（苹果）表现好，但遇到形状差异大的（香蕉）性能下降明显，说明技能分配可能仍受视觉特征相似度影响。
4. **技能分配依赖 VLM**：VLM 的输出质量会影响路由准确性，但论文未深入分析 VLM 产生错误分配的频率或鲁棒性。
5. **真实场景仅 9 个任务**：相对仿真基准覆盖的任务数较少（LIBERO 有 40 个任务，CALVIN 有 34 个），真实世界泛化能力评估尚显初步。
6. **未对比最新 VLA 方法**：比如 RT-2、π0 等，虽然提到了 OpenVLA 和 UniVLA，但未包含所有最新工作。
7. **超参数敏感性**：正交损失 γ 固定为 0.01，未报告该超参数对性能的影响。

（完）
