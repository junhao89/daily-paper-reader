---
title: Stable Language Guidance for Vision–Language–Action Models
title_zh: 面向视觉-语言-动作模型的稳定语言引导
authors: "Zhihao Zhan, Yuhao Chen, Jiaying Zhou, Qinhan Lyu, Hao Liu, Keze Wang, Liang Lin, Guangrun Wang"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.190.pdf"
tags: ["query:latent-vla"]
score: 7.0
evidence: 提高VLA模型的语言鲁棒性
tldr: 视觉-语言-动作（VLA）模型对语言扰动极其敏感。本文识别出“模态坍缩”现象：视觉先验压制稀疏的语言信号，导致过拟合特定措辞而忽略语义。提出残差语义引导（RSS）概率框架，通过蒙特卡洛句法集成和残差动作引导，解耦物理能力与语义执行。实验证明RSS显著提升了VLA模型的语言泛化能力，在多个基准上优于现有方法。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 830, \"height\": 434, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1632, \"height\": 1017, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 802, \"height\": 570, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1656, \"height\": 342, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 831, \"height\": 438, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 968, \"height\": 628, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 963, \"height\": 636, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 131, \"height\": 130, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 86, \"height\": 89, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 131, \"height\": 129, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 88, \"height\": 89, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 131, \"height\": 131, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 87, \"height\": 89, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 132, \"height\": 131, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-015.webp\", \"caption\": \"\", \"page\": 0, \"index\": 15, \"width\": 972, \"height\": 598, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-016.webp\", \"caption\": \"\", \"page\": 0, \"index\": 16, \"width\": 131, \"height\": 128, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-017.webp\", \"caption\": \"\", \"page\": 0, \"index\": 17, \"width\": 87, \"height\": 90, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-018.webp\", \"caption\": \"\", \"page\": 0, \"index\": 18, \"width\": 131, \"height\": 127, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-019.webp\", \"caption\": \"\", \"page\": 0, \"index\": 19, \"width\": 130, \"height\": 129, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.190/fig-020.webp\", \"caption\": \"\", \"page\": 0, \"index\": 20, \"width\": 87, \"height\": 90, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1656, \"height\": 520, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1249, \"height\": 578, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1642, \"height\": 447, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 832, \"height\": 275, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1130, \"height\": 667, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1325, \"height\": 242, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1497, \"height\": 597, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1332, \"height\": 687, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1328, \"height\": 429, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1329, \"height\": 427, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 1329, \"height\": 428, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 1329, \"height\": 428, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 1329, \"height\": 1579, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 1576, \"height\": 708, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-015.webp\", \"caption\": \"\", \"page\": 0, \"index\": 15, \"width\": 1572, \"height\": 244, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.190/table-016.webp\", \"caption\": \"\", \"page\": 0, \"index\": 16, \"width\": 1575, \"height\": 244, \"label\": \"Table\"}]"
motivation: VLA模型对语言指令微小变化敏感，影响实际部署的可靠性。
method: 提出残差语义引导框架，通过分布扩展和残差动作解耦视觉与语言。
result: 在多个VLA基准上，RSS显著提升了对同义指令的鲁棒性。
conclusion: 解耦多模态交互是增强VLA语言鲁棒性的有效手段。
---

## Abstract
Vision-Language-Action (VLA) models have demonstrated impressive capabilities in generalized robotic control; however, they remain notoriously brittle to linguistic perturbations. We identify a critical "modality collapse” phenomenon where strong visual priors overwhelm sparse linguistic signals, causing agents to overfit to specific instruction phrasings while ignoring the underlying semantic intent. To address this, we propose Residual Semantic Steering (RSS), a probabilistic framework that disentangles physical affordance from semantic execution. RSS introduces two theoretical innovations: (1) Monte Carlo Syntactic Integration, which approximates the true semantic posterior via dense, LLM-driven distributional expansion, and (2) Residual Affordance Steering, a dual-stream decoding mechanism that explicitly isolates the causal influence of language by subtracting the visual affordance prior. Theoretical analysis suggests that RSS effectively maximizes the mutual information between action and intent while suppressing visual distractors. Empirical results across diverse manipulation benchmarks demonstrate that RSS achieves state-of-the-art robustness, maintaining performance even under adversarial linguistic perturbations.

---

## 论文详细总结（自动生成）

# 论文总结：Stable Language Guidance for Vision–Language–Action Models

## 1. 核心问题与研究动机
- **问题背景**：视觉-语言-动作（VLA）模型在通用机器人控制中表现出色，但**对语言指令的微小变化（如近义词替换、词序打乱、掩码等）极度脆弱**，导致实际部署不可靠。
- **核心现象**：作者识别出 **“模态坍缩”（modality collapse）**——强视觉先验（如物体位置、场景几何）压制稀疏的语言信号，使模型过度拟合训练指令的表面措辞，忽略背后的语义意图。
- **具体表现**：训练数据中句法分布稀疏（manifold sparsity），且视觉信号梯度占主导（prior dominance），导致模型产生“指令盲”（instruction blindness）或机械执行模式（rote pattern execution）。

## 2. 方法论：Residual Semantic Steering (RSS) 框架
- **核心思想**：解耦物理 affordance（视觉本能）与语义执行，通过两种机制增强语言鲁棒性：
  - **Monte Carlo Syntactic Integration (MCSI)**：针对训练指令稀疏性问题，利用大语言模型（Oracle Teacher，如Qwen2.5-VL）生成密集的句法邻域（同义改写、不同表述），优化期望语义损失 $\mathcal{L}_{\text{RSS}} = \mathbb{E} \left[ \frac{1}{K} \sum_{k=1}^K -\log \pi_\theta(a|o, l_k) \right]$，迫使模型对句法变化不变，近似真实语义后验 $p(a|o, z)$。
  - **Residual Affordance Steering (RAS)**：针对视觉先验主导，将标准的条件logits分解为**基础 affordance 分布** $s(a|o, \emptyset)$（仅依赖视觉信息）与**纯语义调制** $\Delta_{\text{sem}} = s(a|o, l) - s(a|o, \emptyset)$。最终策略为 $\tilde{\pi}(a|o, l) \propto \exp\left( s(a|o, \emptyset) + \gamma \cdot \Delta_{\text{sem}} \right)$，其中 $\gamma > 1$ 为引导系数。此举等效于抑制由视觉本能驱动的动作，放大语言因果信号。

- **理论分析**：通过线性近似证明RAS能将语言特征权重放大 $\gamma$ 倍，恢复语言模态在梯度中的秩，实现语义与视觉的正交化。

## 3. 实验设计
### 数据集与基准
- **主流基准**：**LIBERO**（包含Spatial、Object、Goal、Long四个子集），以及扩展的 **LIBERO-Plus**（含相机、语言、光照、背景、布局等扰动）。
- **扰动类型**：
  1. **Destructive Instruction Overwriting**：空白指令、通用指令（“do something”）、多词改写、随机词序、逐步掩码（mask 20%~80%）。
  2. **Obfuscated Instruction Reinterpretation**：多词替换（R0）、添加无关上下文（R1）、常识描述（R2）、推理链（R3）、混淆（R4，引入无关物体否定）。
  3. **Out-of-Distribution (OOD) Semantic Transfer**：保留物体但组合新任务，在少量步数（10/100/1000步）微调后评测。

### 对比方法
- **基线模型**：π0（Black et al., 2024）、π0.5（Intelligence et al., 2025），以及OpenVLA、Octo、Dita、SpatialVLA等代表性方法。
- **消融变体**：仅RAS、仅MCSI、RAS+MCSI组合。

## 4. 资源与算力
- **训练设置**：30,000步，batch size 32，学习率余弦衰减（warm-up 10,000步，峰值 5×10⁻⁵），EMA decay=0.999。
- **推理硬件**：单张 **NVIDIA RTX 3090** GPU进行模型评估与部署。
- **训练具体GPU配置未明确说明**（可能多卡？文中仅提及推理单卡），但整体算力需求中等。

## 5. 实验数量与充分性
- **丰富实验数量**：包含主表（表1~7）、附录中16张详细表格（表8~16），覆盖：
  - 三种扰动类型下 π0 和 π0.5 的完整结果（每个扰动类型含多种强度）。
  - 不同LLM教师（DeepSeek-R1、Qwen-3.5、InternVL3、LLaVA-OneVision）的泛化测试。
  - 超参数消融（引导系数γ、去噪步数）。
  - LIBERO-Plus 多维度评估（相机、语言、光照等7类扰动）。
- **公平性**：在相同训练/推理条件下对比，控制MCSI和RAS的添加；同时测试了不同VLM生成训练数据的稳健性。
- **充分性**：实验覆盖广、消融完整，且提供了详细的失败分析与定性结果，结论可信度高。

## 6. 主要结论与发现
- **RSS显著提升鲁棒性**：在破坏性指令改写下，RAS+MCSI平均提升π0基线约30%（从52.37%到82.22%），π0.5提升约11%（从75.90%到86.98%）。
- **MCSI主导语义泛化**：在混淆改写（R3/R4）和OOD场景中，MCSI贡献最大；RAS则在空白/通用指令下效果更强。
- **引导系数需适中**：γ过大（>2.0）会导致对损坏指令的过敏感；最佳值在1.25~1.5之间。
- **跨VLM教师泛化良好**：即使使用不同LLM生成训练邻域或测试改写，RSS性能变化小于2.5%。
- **RAS在模糊指令下保守**：对于“do something”等极端模糊指令，RAS会抑制动作，避免基于视觉本能错误执行（作者视为安全特性）。

## 7. 优点
- **理论清晰**：从模态坍缩问题出发，提出解耦视觉affordance与语义的框架，并给出线性近似证明。
- **方法新颖**：MCSI利用LLM扩展训练分布；RAS将CFG思想创新地用于偏差抑制（而非质量提升）。
- **实验扎实**：设计多种扰动类型，覆盖不同强度，消融充分，并验证了对不同VLM的鲁棒性。
- **代码开源**：提供GitHub仓库，可复现。

## 8. 不足与局限
- **模糊指令下的保守行为**：RAS在“do something”等提示下可能完全不执行动作，虽然安全但不利于人机交互。
- **OOD泛化仍有限**：在10步/100步微调下性能不高，1000步后才接近较好水平，表明对新组合任务的快速适应仍有挑战。
- **额外LLM开销**：MCSI需要LLM生成大量改写下，增加了训练时的计算和API成本。
- **未在真实机器人平台验证**：所有实验在仿真环境中进行，实际物理交互中的噪声和延迟可能影响效果。
- **基线选择范围有限**：主要对比π0和π0.5，未全面对比最新方法（如RDT-1B、E0等）。

（完）
