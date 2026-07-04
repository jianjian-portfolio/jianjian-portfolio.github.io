---
title: "高吞吐深度强化学习（DRL）序贯决策引擎（Stealth Pilot）"
description: "Stealth Pilot 云计算 BDQ 深度强化学习序贯决策引擎：资源供给与调度，GPU replay 与偏好对齐。"
date: 2023-06-01T00:00:00Z
featured: true
weight: 10
sitemap:
  priority: 0.9
  changefreq: monthly
cover:
    image: "/assets/images/projects/drl-decision-engine-cover.svg"
    style: "diagram"
    alt: "云计算决策平台 DRL 架构：并行 Macro/Micro 编码、BDQ 双头输出与 GPU Replay Buffer"
    relative: false
---

设计并开发了一套高吞吐**序贯决策引擎**，面向**云计算决策平台**，在极端、高频的运行时负载下提供资源供给与调度相关的实时序贯决策。该 Stealth Pilot 自 2023 年 6 月持续至今，在系统规模上落地深度强化学习——贯通算法设计、分布式 GPU 训练与性能约束下的策略对齐。

**核心技术栈：** PyTorch · 分支对决 Q 网络（BDQ）· HF Accelerate · DeepSpeed · Ray Train/Serve · DDP · 偏好对齐（Preference Alignment）

---

## 系统概览与序贯决策闭环

引擎将平台上的供给与调度问题建模为**马尔可夫决策过程（MDP）**：**Macro 信息编码器**与 **Micro 信息编码器**并行处理多源平台观测（前者聚合平台级宏观信号，后者捕捉局部高频观测），融合后馈入 Actor-Critic 主策略；主头输出离散控制动作，并行的 **Preference Alignment 辅助网络**在共享策略表示上施加安全与偏好约束。决策在毫秒级间隔内完成，并平衡吞吐量、尾延迟与资源利用率。

<div class="diagram-placeholder">
<span class="diagram-title">[图 1: 高频 DRL 决策闭环]</span>
<div class="diagram-svg-container">
<svg width="100%" height="100%" viewBox="0 0 524 130" style="max-width: 524px;" role="img" aria-label="高频 DRL 决策闭环">
<defs>
<marker id="drl-f1-arrow-zh" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
<path d="M 0 0 L 10 5 L 0 10 z" fill="var(--diagram-accent)"/>
</marker>
</defs>
<g class="interactive-group">
<rect x="6" y="45" width="76" height="40" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.5"/>
<text x="44" y="62" fill="currentColor" font-size="9" font-weight="600" text-anchor="middle" font-family="sans-serif">平台观测</text>
<text x="44" y="76" fill="currentColor" fill-opacity="0.65" font-size="7" text-anchor="middle" font-family="sans-serif">Observability</text>
</g>
<g class="interactive-group">
<rect x="94" y="18" width="86" height="38" rx="8" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="137" y="34" fill="currentColor" font-size="9" font-weight="600" text-anchor="middle" font-family="sans-serif">Macro 编码</text>
<text x="137" y="48" fill="currentColor" fill-opacity="0.65" font-size="7" text-anchor="middle" font-family="sans-serif">并行</text>
</g>
<g class="interactive-group">
<rect x="94" y="74" width="86" height="38" rx="8" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="137" y="90" fill="currentColor" font-size="9" font-weight="600" text-anchor="middle" font-family="sans-serif">Micro 编码</text>
<text x="137" y="104" fill="currentColor" fill-opacity="0.65" font-size="7" text-anchor="middle" font-family="sans-serif">并行</text>
</g>
<g class="interactive-group">
<rect x="200" y="28" width="98" height="74" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="2"/>
<text x="249" y="54" fill="var(--diagram-accent)" font-size="10" font-weight="700" text-anchor="middle" font-family="sans-serif">BDQ 策略</text>
<text x="249" y="70" fill="currentColor" fill-opacity="0.75" font-size="8" text-anchor="middle" font-family="sans-serif">Actor-Critic</text>
<text x="249" y="84" fill="currentColor" fill-opacity="0.55" font-size="7" text-anchor="middle" font-family="sans-serif">共享表示</text>
</g>
<g class="interactive-group">
<rect x="316" y="18" width="82" height="38" rx="8" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="357" y="34" fill="currentColor" font-size="9" font-weight="600" text-anchor="middle" font-family="sans-serif">控制动作</text>
<text x="357" y="48" fill="currentColor" fill-opacity="0.65" font-size="7" text-anchor="middle" font-family="sans-serif">主头</text>
</g>
<g class="interactive-group">
<rect x="316" y="74" width="82" height="38" rx="8" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3"/>
<text x="357" y="90" fill="var(--diagram-stable)" font-size="8" font-weight="600" text-anchor="middle" font-family="sans-serif">偏好对齐</text>
<text x="357" y="104" fill="currentColor" fill-opacity="0.65" font-size="7" text-anchor="middle" font-family="sans-serif">辅助网络</text>
</g>
<g class="interactive-group">
<rect x="416" y="45" width="96" height="40" rx="8" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.5"/>
<text x="464" y="62" fill="var(--diagram-stable)" font-size="9" font-weight="600" text-anchor="middle" font-family="sans-serif">GPU Replay</text>
<text x="464" y="76" fill="currentColor" fill-opacity="0.65" font-size="7" text-anchor="middle" font-family="sans-serif">Buffer</text>
</g>
<line x1="82" y1="58" x2="94" y2="37" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow-zh)"/>
<line x1="82" y1="72" x2="94" y2="93" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow-zh)"/>
<line x1="180" y1="37" x2="200" y2="52" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow-zh)"/>
<line x1="180" y1="93" x2="200" y2="78" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow-zh)"/>
<line x1="298" y1="48" x2="316" y2="37" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow-zh)"/>
<line x1="298" y1="82" x2="316" y2="93" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#drl-f1-arrow-zh)"/>
<line x1="398" y1="37" x2="416" y2="58" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow-zh)"/>
<line x1="398" y1="93" x2="416" y2="72" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#drl-f1-arrow-zh)"/>
</svg>
</div>
<p class="diagram-caption"><strong>决策闭环：</strong> 平台观测并行经 Macro / Micro 信息编码器融合后输入 BDQ 主策略；主头输出控制动作，Preference Alignment 辅助网络并行挂载于共享表示；二者产生的转移写入 GPU 驻留 replay buffer，供分布式离策略更新使用。</p>
</div>

---

## 核心技术贡献

### 1. 基于 BDQ 的 Actor-Critic 引擎
* **并行状态编码**：**Macro 信息编码器**与 **Micro 信息编码器**并行处理多源平台观测，分别提取延时宏观与高频微观特征，融合为多时间尺度状态表示后馈入 BDQ 主策略。
* **双头输出**：Actor-Critic **主头**产生离散控制动作；**Preference Alignment 辅助网络**并行挂载于共享策略表示，在训练期施加安全与偏好约束。
* **架构**：实现带 **Dueling Advantage** 分解的 **Actor-Critic** 结构，在动作空间沿并发控制维度分支时稳定 Q 值估计（如容量档位、优先级权重、策略层级等离散组合）。
* **分支动作**：采用 **Branching Dueling Q-Network (BDQ)**，将多维控制信号因子化，避免展平为指数级离散空间——在平台序贯决策常见的稀疏、延迟奖励下保持样本效率。

### 2. 大规模分布式训练
* **多节点 GPU 集群**：通过 **Hugging Face Accelerate**、**分布式数据并行（DDP）** 与 **DeepSpeed（ZeRO-2）** 在多节点 GPU 上扩展训练，分区优化器状态以支撑大规模 replay batch。
* **Ray Train / Ray Serve**：以 **Ray Train** 与 **Ray Serve** 编排实验与服务原型，在 pilot 评估中将离线策略优化与低延迟在线推理路径分离。

<div class="diagram-placeholder">
<span class="diagram-title">[图 2: 分布式训练拓扑]</span>
<div class="diagram-svg-container">
<svg width="100%" height="100%" viewBox="0 0 420 130" style="max-width: 400px;" role="img" aria-label="分布式 DRL 训练拓扑">
<g class="interactive-group">
<rect x="20" y="45" width="90" height="40" rx="6" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.5"/>
<text x="65" y="69" fill="currentColor" font-size="9" font-weight="600" text-anchor="middle" font-family="sans-serif">Ray Head</text>
</g>
<g class="interactive-group">
<rect x="140" y="20" width="80" height="36" rx="6" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.5"/>
<text x="180" y="42" fill="currentColor" font-size="8" text-anchor="middle" font-family="sans-serif">GPU Worker</text>
</g>
<g class="interactive-group">
<rect x="140" y="74" width="80" height="36" rx="6" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.5"/>
<text x="180" y="96" fill="currentColor" font-size="8" text-anchor="middle" font-family="sans-serif">GPU Worker</text>
</g>
<g class="interactive-group">
<rect x="260" y="20" width="80" height="36" rx="6" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.5"/>
<text x="300" y="42" fill="currentColor" font-size="8" text-anchor="middle" font-family="sans-serif">GPU Worker</text>
</g>
<g class="interactive-group">
<rect x="260" y="74" width="80" height="36" rx="6" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.5"/>
<text x="300" y="96" fill="currentColor" font-size="8" text-anchor="middle" font-family="sans-serif">GPU Worker</text>
</g>
<g class="interactive-group">
<rect x="355" y="52" width="50" height="26" rx="4" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.5"/>
<text x="380" y="69" fill="var(--diagram-stable)" font-size="8" font-weight="600" text-anchor="middle" font-family="sans-serif">Accelerate</text>
</g>
<line x1="110" y1="65" x2="140" y2="38" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.5"/>
<line x1="110" y1="65" x2="140" y2="92" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.5"/>
<line x1="220" y1="38" x2="260" y2="38" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.5"/>
<line x1="220" y1="92" x2="260" y2="92" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.5"/>
<text x="360" y="38" fill="currentColor" fill-opacity="0.55" font-size="8" font-family="sans-serif">DDP</text>
<text x="360" y="92" fill="currentColor" fill-opacity="0.55" font-size="8" font-family="sans-serif">ZeRO-2</text>
</svg>
</div>
<p class="diagram-caption"><strong>训练栈：</strong> Ray 协调多 GPU Worker；Accelerate + DDP 同步梯度；DeepSpeed ZeRO-2 分区优化器状态，降低大批量 DRL 更新的显存压力。</p>
</div>

### 3. GPU 驻留 Replay Buffer
* **零拷贝训练路径**：设计 **GPU 驻留 replay buffer**，使 experience tuple 在采样与梯度步骤间保持 device-local——消除在短 horizon、高频 DRL 场景下常占主导的 CPU→GPU 拷贝开销。
* **吞吐收益**：在决策 cadence 与 batch 采样率接近 HPC 级占空比的 pilot benchmark 中，设备端驻留 transitions 提升了有效训练吞吐。

### 4. RLHF 风格偏好对齐头
* **安全正则**：构建辅助 **偏好对齐头**（RLHF 风格），在预定义**偏好空间**下正则化共享策略表示——在探索阶段惩罚违反偏好指标、过载阈值或公平性约束的动作轨迹。
* **Human-in-the-Loop**：对齐模块接受用户反馈的轨迹排序对，支持在 Stealth Pilot 迭代中稳定 refine 策略，而不破坏核心 BDQ critic。

---

## 工程总结

* **系统 + RL**：从 MDP 建模、分支动作设计到分布式 PyTorch 训练与服务原型，体现端到端 systems-AI 能力。
