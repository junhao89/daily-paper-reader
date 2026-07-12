<div class="dpr-home-notice-card">
  <h3 class="dpr-home-notice-title">🚀 Start Here</h3>
  <ul class="dpr-home-notice-list">
    <li><a href="#/tutorial/README">使用教程</a></li>
  </ul>
</div>

## 每次日报
- 最新运行日期：2026-07-12
- 运行时间：2026-07-12 21:40:17 UTC
- 运行状态：成功
- 本次总论文数：21
- 精读区：8
- 速读区：13

### 今日简报（AI）
今日共推荐21篇论文，精选精读8篇，重点聚焦轻量级机器人控制与世界模型领域。  
最值得关注的是 XS-VLA 利用粗粒度蒸馏与潜流匹配实现轻量机器人控制（10分），以及 Look Before You Leap 通过树搜索蒸馏用于冻结VLA模型的动作评估（9分）。  
建议优先精读这两篇高分论文，并留意速读中关于世界模型的 SiamJEPA 和 Mask2Real-WM，探索模型压缩与 Sim-to-Real 迁移的实用技巧。
- 详情：[/202607/12/README](/202607/12/README)

### 精读区论文标签
1. [XS-VLA: Coupling Coarse-grained Spatial Distillation with Latent Flow Matching for Lightweight Robotic Control](/202607/12/2607.04171v1-xs-vla-coupling-coarse-grained-spatial-distillation-with-latent-flow-matching-for-lightweight-robotic-control)  
   标签：评分：10.0/10、query:latent-vla
   evidence：潜在流匹配用于轻量级VLA机器人控制
2. [Look Before You Leap: Distilling Tree Search into Action Evaluation for Frozen VLA Models](/202607/12/2607.03751v1-look-before-you-leap-distilling-tree-search-into-action-evaluation-for-frozen-vla-models)  
   标签：评分：9.0/10、query:latent-vla
   evidence：通过将树搜索蒸馏到动作评估中提升冻结VLA模型
3. [High-Fidelity One-Step Generative Visuomotor Policy via Recursive Correction, Frequency Consistency, and Contrastive Flow Matching](/202607/12/2607.03865v1-high-fidelity-one-step-generative-visuomotor-policy-via-recursive-correction-frequency-consistency-and-contrastive-flow-matching)  
   标签：评分：9.0/10、query:gen2embodied
   evidence：用于机器人控制的一步流匹配视觉运动策略
4. [Worldscape-MoE: A Unified Mixture-of-Experts World Model for Scalable Heterogeneous Action Control](/202607/12/2607.03964v1-worldscape-moe-a-unified-mixture-of-experts-world-model-for-scalable-heterogeneous-action-control)  
   标签：评分：9.0/10、query:wam-vla
   evidence：提出统一的混合专家世界模型，支持异构动作控制
5. [Optimal Transport Q-Learning for Flow Policy Steering and Acceleration](/202607/12/2607.06262v1-optimal-transport-q-learning-for-flow-policy-steering-and-acceleration)  
   标签：评分：9.0/10、query:gen2embodied
   evidence：使用强化学习微调流策略，直接相关于流匹配和VLA模型
6. [Lift3D-VLA: Lifting VLA Models to 3D Geometry and Dynamics-Aware Manipulation](/202607/12/2607.06564v1-lift3d-vla-lifting-vla-models-to-3d-geometry-and-dynamics-aware-manipulation)  
   标签：评分：9.0/10、query:latent-vla
   evidence：用于操作的3D点云推理VLA
7. [Pelican-VLA 0.5: Attending Before Acting Benefits Generalization](/202607/12/2607.06655v2-pelican-vla-05-attending-before-acting-benefits-generalization)  
   标签：评分：9.0/10、query:latent-vla
   evidence：统一VLA模型，融合视觉语言理解、未来帧生成和动作预测
8. [WCog-VLA: A Dual-Level World-Cognitive Vision-Language-Action Model for End-to-End Autonomous Driving](/202607/12/2607.08375v1-wcog-vla-a-dual-level-world-cognitive-vision-language-action-model-for-end-to-end-autonomous-driving)  
   标签：评分：9.0/10、query:latent-vla
   evidence：提出WCog-VLA，一种具有双层次世界认知的视觉-语言-动作模型用于自动驾驶

### 速读区论文标签
1. [SiamJEPA: On the Role of Siamese Student Encoders in JEPA](/202607/12/2607.04044v1-siamjepa-on-the-role-of-siamese-student-encoders-in-jepa)  
   标签：评分：8.0/10、query:latent-vla
   evidence：研究JEPA中的孪生编码器，直接相关于联合嵌入预测架构
2. [Learning Task-Sufficient World Models by Synergizing Agentic Exploration and Structured Modeling](/202607/12/2607.04409v1-learning-task-sufficient-world-models-by-synergizing-agentic-exploration-and-structured-modeling)  
   标签：评分：8.0/10、query:wam-vla
   evidence：学习任务充分的潜在世界模型用于决策
3. [Mask2Real-WM: Segmentation Masks as a Sim-to-Real Bridge for Controllable Dexterous World Models](/202607/12/2607.04546v1-mask2real-wm-segmentation-masks-as-a-sim-to-real-bridge-for-controllable-dexterous-world-models)  
   标签：评分：8.0/10、query:wam-vla
   evidence：使用分割掩码和视频扩散的动作条件世界模型用于灵巧操作
4. [KAM-WM: Kinematic Affordance Maps from Latent World Models for Robot Manipulation](/202607/12/2607.04652v1-kam-wm-kinematic-affordance-maps-from-latent-world-models-for-robot-manipulation)  
   标签：评分：8.0/10、query:latent-vla
   evidence：潜在世界模型用于运动学可操作图提取
5. [UNIVERSE: Unified Video Action Models for Autonomous Driving with Flexible Mask-Modulated Modality Generation](/202607/12/2607.05133v1-universe-unified-video-action-models-for-autonomous-driving-with-flexible-mask-modulated-modality-generation)  
   标签：评分：8.0/10、query:wam-vla
   evidence：提出统一视频-动作模型作为自动驾驶世界动作模型
6. [DynaVieW: Schema-Guided World Modeling for Understanding Hierarchical Visual Dynamics](/202607/12/2607.04112v1-dynaview-schema-guided-world-modeling-for-understanding-hierarchical-visual-dynamics)  
   标签：评分：7.0/10、query:wam-vla
   evidence：模式引导的世界模型用于视觉动态，与基于视频的世界模型相关
7. [Spatial Attention: Adapting Execution Horizons for Diffusion Policies via Observation Sensitivity](/202607/12/2607.04739v1-spatial-attention-adapting-execution-horizons-for-diffusion-policies-via-observation-sensitivity)  
   标签：评分：7.0/10、query:gen2embodied
   evidence：利用观测敏感性自适应调整扩散策略的执行视野
8. [PRISM: Personalized Robotic Dataset Generation via Image-based Scene and Motion Synthesis](/202607/12/2607.04880v1-prism-personalized-robotic-dataset-generation-via-image-based-scene-and-motion-synthesis)  
   标签：评分：7.0/10、query:latent-vla
   evidence：为VLA策略生成个性化数据集
9. [Cortex: A Bidirectionally Aligned Embodied Agent Framework for Long-horizon Manipulation](/202607/12/2607.05377v1-cortex-a-bidirectionally-aligned-embodied-agent-framework-for-long-horizon-manipulation)  
   标签：评分：7.0/10、query:latent-vla
   evidence：结合层次化规划和技能原语的VLA框架
10. [Mask-based Predictive Representations for Reinforcement Learning](/202607/12/2607.04153v1-mask-based-predictive-representations-for-reinforcement-learning)  
   标签：评分：6.0/10、query:latent-vla
   evidence：基于掩码预测的自监督学习用于控制任务的表示学习
11. [Aura: Consistent Multi-Subject Video Generation via VLM-Grounded Semantic Alignment](/202607/12/2607.04311v1-aura-consistent-multi-subject-video-generation-via-vlm-grounded-semantic-alignment)  
   标签：评分：6.0/10、query:gen2embodied
   evidence：使用VLM的一致多主体视频生成框架
12. [Last-Meter Precision Navigation for UAVs: A Diffusion-Refined Aerial Visual Servoing Approach](/202607/12/2607.04352v1-last-meter-precision-navigation-for-uavs-a-diffusion-refined-aerial-visual-servoing-approach)  
   标签：评分：6.0/10、query:gen2embodied
   evidence：基于扩散模型的视觉伺服精化方法，适用于机器人控制
13. [RoboDojo: A Unified Sim-and-Real Benchmark for Comprehensive Evaluation of Generalist Robot Manipulation Policies](/202607/12/2607.04434v1-robodojo-a-unified-sim-and-real-benchmark-for-comprehensive-evaluation-of-generalist-robot-manipulation-policies)  
   标签：评分：6.0/10、query:wam-vla
   evidence：统一的仿真-实机基准，用于评估包括VLA和WAM的通用机器人操作策略


<div class="dpr-home-promo-card">
  <h3 class="dpr-home-promo-title">💬 社区与支持</h3>
  <ul class="dpr-home-promo-list">
    <li>欢迎 Star / Fork / Issue / PR</li>
    <li>QQ群：583867967（欢迎交流，已有：1151人）</li>
  </ul>
</div>
