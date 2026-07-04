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
<div class="diagram-svg-container diagram-svg-container--drl-loop">
<svg width="100%" height="100%" viewBox="0 0 725 162" role="img" aria-label="高频 DRL 决策闭环">
<defs>
<marker id="drl-f1-arrow-zh" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
<path d="M 0 0 L 10 5 L 0 10 z" fill="var(--diagram-accent)"/>
</marker>
<marker id="drl-f1-arrow-stable-zh" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
<path d="M 0 0 L 10 5 L 0 10 z" fill="var(--diagram-stable)"/>
</marker>
</defs>
<g transform="scale(1.25)">
<g class="diagram-edges" aria-hidden="true">
<line x1="76" y1="58" x2="136" y2="37" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow-zh)"/>
<line x1="76" y1="72" x2="136" y2="93" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow-zh)"/>
<line x1="210" y1="37" x2="258" y2="52" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow-zh)"/>
<line x1="210" y1="93" x2="258" y2="78" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow-zh)"/>
<line x1="342" y1="48" x2="382" y2="37" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow-zh)"/>
<line x1="342" y1="82" x2="382" y2="93" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#drl-f1-arrow-stable-zh)"/>
<line x1="452" y1="37" x2="490" y2="58" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow-zh)"/>
<line x1="452" y1="93" x2="490" y2="72" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#drl-f1-arrow-stable-zh)"/>
</g>
<g class="interactive-group">
<rect x="8" y="40" width="80" height="48" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.5"/>
<text x="48" y="60" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">平台观测</text>
<text x="48" y="76" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Observability</text>
</g>
<g class="interactive-group">
<rect x="130" y="12" width="90" height="46" rx="8" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="175" y="31" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Macro 编码</text>
<text x="175" y="47" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">并行</text>
</g>
<g class="interactive-group">
<rect x="130" y="70" width="90" height="46" rx="8" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="175" y="89" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Micro 编码</text>
<text x="175" y="105" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">并行</text>
</g>
<g class="interactive-group">
<rect x="250" y="20" width="100" height="88" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="2"/>
<text x="300" y="50" fill="var(--diagram-accent)" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">BDQ 策略</text>
<text x="300" y="66" fill="currentColor" fill-opacity="0.75" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Actor-Critic</text>
<text x="300" y="82" fill="currentColor" fill-opacity="0.55" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">共享表示</text>
</g>
<g class="interactive-group">
<rect x="376" y="12" width="86" height="46" rx="8" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="419" y="31" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">控制动作</text>
<text x="419" y="47" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">主头</text>
</g>
<g class="interactive-group">
<rect x="376" y="70" width="86" height="46" rx="8" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3"/>
<text x="419" y="89" fill="var(--diagram-stable)" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">偏好对齐</text>
<text x="419" y="105" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">辅助网络</text>
</g>
<g class="interactive-group">
<rect x="484" y="40" width="94" height="48" rx="8" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.5"/>
<text x="531" y="60" fill="var(--diagram-stable)" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">GPU Replay</text>
<text x="531" y="76" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Buffer</text>
</g>
</g>
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
<div class="diagram-svg-container diagram-svg-container--drl-train">
<svg width="100%" height="100%" viewBox="-55 0 723 185" role="img" aria-label="分布式 DRL 训练拓扑">
<defs>
<marker id="drl-f2-arrow-zh" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
<path d="M 0 0 L 10 5 L 0 10 z" fill="var(--diagram-accent)"/>
</marker>
</defs>
<g transform="scale(1.25) translate(-32.23, 0)">
<g class="diagram-edges" aria-hidden="true">
<line x1="90.4" y1="74" x2="118" y2="74" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f2-arrow-zh)"/>
<line x1="249" y1="49" x2="345" y2="49" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2" stroke-dasharray="3 2"/>
<line x1="249" y1="89" x2="345" y2="89" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2" stroke-dasharray="3 2"/>
</g>
<rect x="118" y="8" width="372" height="122" rx="10" fill="none" stroke="currentColor" stroke-opacity="0.25" stroke-width="1.2" stroke-dasharray="5 3"/>
<text x="304" y="28" fill="currentColor" fill-opacity="0.7" text-anchor="middle" class="diagram-train-heading">分布式训练任务（Ray Train）</text>
<line x1="290" y1="33" x2="290" y2="105" stroke="currentColor" stroke-opacity="0.15" stroke-width="1"/>
<text x="204" y="37" fill="currentColor" fill-opacity="0.55" text-anchor="middle" class="diagram-train-node">节点 1</text>
<text x="390" y="37" fill="currentColor" fill-opacity="0.55" text-anchor="middle" class="diagram-train-node">节点 2</text>
<g class="interactive-group">
<rect x="-7.6" y="52" width="98" height="40" rx="6" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.5"/>
<text x="41.4" y="76" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Ray Head</text>
</g>
<g class="interactive-group">
<rect x="159" y="41" width="90" height="36" rx="6" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="204" y="63" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">GPU Worker</text>
</g>
<g class="interactive-group">
<rect x="159" y="83" width="90" height="36" rx="6" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="204" y="103" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">GPU Worker</text>
</g>
<g class="interactive-group">
<rect x="345" y="41" width="90" height="36" rx="6" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="390" y="63" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">GPU Worker</text>
</g>
<g class="interactive-group">
<rect x="345" y="83" width="90" height="36" rx="6" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="390" y="103" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">GPU Worker</text>
</g>
<text x="297" y="59" fill="currentColor" fill-opacity="0.5" font-size="7" text-anchor="middle" font-family="sans-serif">DDP 同步</text>
<text x="297" y="99" fill="currentColor" fill-opacity="0.5" font-size="7" text-anchor="middle" font-family="sans-serif">DDP 同步</text>
<text x="304" y="132" fill="var(--diagram-stable)" text-anchor="middle" class="diagram-train-stable">各 Worker：Accelerate · DDP · DeepSpeed ZeRO-2</text>
</g>
</svg>
</div>
<p class="diagram-caption"><strong>训练栈（示意）：</strong> Ray Train 调度跨节点的 GPU Worker；在同一训练任务内，各 Worker 通过 Accelerate 运行 PyTorch，以 DDP 同步梯度，并以 DeepSpeed ZeRO-2 分片优化器状态。Ray Serve（在线推理）为独立路径，此处未画出。</p>
</div>

### 3. GPU 驻留 Replay Buffer
* **零拷贝训练路径**：设计 **GPU 驻留 replay buffer**，使 experience tuple 在采样与梯度步骤间保持 device-local——消除在短 horizon、高频 DRL 场景下常占主导的 CPU→GPU 拷贝开销。
* **吞吐收益**：在决策 cadence 与 batch 采样率接近 HPC 级占空比的 pilot benchmark 中，设备端驻留 transitions 提升了有效训练吞吐。

### 4. RLHF 风格偏好对齐网络
* **安全正则**：构建辅助 **偏好对齐网络**（RLHF 风格），在预定义**偏好空间**下正则化共享策略表示——在探索阶段惩罚违反偏好指标、过载阈值或公平性约束的动作轨迹。
* **Human-in-the-Loop**：对齐模块接受用户反馈的轨迹排序对，支持在 Stealth Pilot 迭代中稳定 refine 策略，而不破坏核心 BDQ critic。

<div class="diagram-placeholder">
<span class="diagram-title">[图 3: 双头多目标策略网络（主输出 + 偏好辅助）]</span>
<div class="diagram-svg-container diagram-svg-container--drl-pref">
<svg width="100%" height="100%" viewBox="0 0 880 278" role="img" aria-label="双头多目标策略网络：主分支水平排列，辅助分支 L 形折线展开">
<defs>
<marker id="drl-f3-arrow-zh" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
<path d="M 0 0 L 10 5 L 0 10 z" fill="var(--diagram-accent)"/>
</marker>
<marker id="drl-f3-arrow-stable-zh" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
<path d="M 0 0 L 10 5 L 0 10 z" fill="var(--diagram-stable)"/>
</marker>
</defs>
<g transform="translate(109, 4)">
<g transform="scale(1.25)">
<g class="diagram-edges" aria-hidden="true">
<line x1="98" y1="42" x2="118" y2="42" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f3-arrow-zh)"/>
<line x1="230" y1="42" x2="250" y2="42" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f3-arrow-zh)"/>
<line x1="324" y1="42" x2="344" y2="42" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f3-arrow-zh)"/>
<path d="M 69 64 L 69 128 L 118 128" stroke="var(--diagram-stable)" stroke-width="1.4" stroke-dasharray="4 3" fill="none" marker-end="url(#drl-f3-arrow-stable-zh)"/>
<line x1="148" y1="116" x2="210" y2="122" stroke="var(--diagram-stable)" stroke-opacity="0.38" stroke-width="1.1"/>
<line x1="148" y1="116" x2="210" y2="134" stroke="var(--diagram-stable)" stroke-opacity="0.38" stroke-width="1.1"/>
<line x1="148" y1="116" x2="210" y2="146" stroke="var(--diagram-stable)" stroke-opacity="0.38" stroke-width="1.1"/>
<line x1="148" y1="128" x2="210" y2="122" stroke="var(--diagram-stable)" stroke-opacity="0.38" stroke-width="1.1"/>
<line x1="148" y1="128" x2="210" y2="134" stroke="var(--diagram-stable)" stroke-opacity="0.38" stroke-width="1.1"/>
<line x1="148" y1="128" x2="210" y2="146" stroke="var(--diagram-stable)" stroke-opacity="0.38" stroke-width="1.1"/>
<line x1="148" y1="140" x2="210" y2="122" stroke="var(--diagram-stable)" stroke-opacity="0.38" stroke-width="1.1"/>
<line x1="148" y1="140" x2="210" y2="134" stroke="var(--diagram-stable)" stroke-opacity="0.38" stroke-width="1.1"/>
<line x1="148" y1="140" x2="210" y2="146" stroke="var(--diagram-stable)" stroke-opacity="0.38" stroke-width="1.1"/>
<line x1="148" y1="152" x2="210" y2="122" stroke="var(--diagram-stable)" stroke-opacity="0.38" stroke-width="1.1"/>
<line x1="148" y1="152" x2="210" y2="134" stroke="var(--diagram-stable)" stroke-opacity="0.38" stroke-width="1.1"/>
<line x1="148" y1="152" x2="210" y2="146" stroke="var(--diagram-stable)" stroke-opacity="0.38" stroke-width="1.1"/>
<line x1="210" y1="122" x2="278" y2="134" stroke="var(--diagram-stable)" stroke-opacity="0.45" stroke-width="1.1"/>
<line x1="210" y1="134" x2="278" y2="134" stroke="var(--diagram-stable)" stroke-opacity="0.45" stroke-width="1.1"/>
<line x1="210" y1="146" x2="278" y2="134" stroke="var(--diagram-stable)" stroke-opacity="0.45" stroke-width="1.1"/>
<line x1="284" y1="134" x2="344" y2="134" stroke="var(--diagram-stable)" stroke-width="1.5" marker-end="url(#drl-f3-arrow-stable-zh)"/>
<line x1="222" y1="193" x2="222" y2="165" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#drl-f3-arrow-stable-zh)"/>
<path d="M 388 156 L 388 206 L 39 206 L 39 64" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3" fill="none" marker-end="url(#drl-f3-arrow-stable-zh)"/>
<circle cx="148" cy="116" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="148" cy="128" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="148" cy="140" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="148" cy="152" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="210" cy="122" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="210" cy="134" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="210" cy="146" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="278" cy="134" r="5.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.3"/>
</g>
<text x="6" y="42" fill="var(--diagram-accent)" text-anchor="end" font-family="sans-serif" class="diagram-anno-label">主分支 →</text>
<text x="58" y="98" fill="var(--diagram-stable)" text-anchor="start" font-family="sans-serif" class="diagram-anno-label">辅助分支</text>
<rect x="118" y="74" width="206" height="14" rx="4" fill="none" stroke="currentColor" stroke-opacity="0.2" stroke-width="1" stroke-dasharray="4 3"/>
<text x="221" y="84" fill="currentColor" fill-opacity="0.55" text-anchor="middle" font-family="sans-serif" class="diagram-anno-label">偏好空间 · 安全边界</text>
<rect x="118" y="90" width="206" height="75" rx="8" fill="none" stroke="var(--diagram-stable)" stroke-opacity="0.55" stroke-width="1.2" stroke-dasharray="4 3"/>
<text x="221" y="104" fill="var(--diagram-stable)" text-anchor="middle" font-family="sans-serif" class="diagram-anno-label">Auxiliary · 偏好对齐（辅助头）</text>
<g class="interactive-group">
<rect x="10" y="20" width="88" height="44" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.5"/>
<text x="54" y="38" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">共享策略</text>
<text x="54" y="54" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">表示 · BDQ</text>
</g>
<g class="interactive-group">
<rect x="118" y="20" width="112" height="44" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.5"/>
<text x="174" y="38" fill="var(--diagram-accent)" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Primary · 输出</text>
<text x="174" y="54" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">网络（主头）</text>
</g>
<g class="interactive-group">
<rect x="250" y="20" width="74" height="44" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.3"/>
<text x="287" y="46" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">控制动作</text>
</g>
<g class="interactive-group">
<rect x="344" y="20" width="88" height="44" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.5"/>
<text x="388" y="46" fill="var(--diagram-accent)" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">RL 目标</text>
</g>
<g class="interactive-group">
<rect x="344" y="112" width="88" height="44" rx="8" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.5"/>
<text x="388" y="128" fill="var(--diagram-stable)" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">偏好正则</text>
<text x="388" y="144" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">损失</text>
</g>
<g class="interactive-group">
<rect x="178" y="193" width="88" height="26" rx="6" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="222" y="205" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">轨迹 A ≻ B</text>
<text x="222" y="215" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-anno-label">人工反馈</text>
</g>
<text x="222" y="186" fill="currentColor" fill-opacity="0.5" font-size="7" text-anchor="middle" font-family="sans-serif">梯度回传 · 辅助头</text>
<text x="448" y="42" fill="var(--diagram-accent)" text-anchor="start" class="diagram-pref-stable">主 RL 优化</text>
<text x="448" y="134" fill="var(--diagram-stable)" text-anchor="start" class="diagram-pref-stable">探索期惩罚违规轨迹</text>
</g>
</g>
</svg>
</div>
<p class="diagram-caption"><strong>双头多目标（示意）：</strong> 主分支水平展开：共享 BDQ 表示 → Primary 输出网络 → 控制动作 → RL 目标。Auxiliary 偏好对齐网络自共享表示<strong>以 L 形折线向下接入</strong>（宽度对齐 Primary 左缘至控制动作右缘），经与 RL 目标<strong>垂直对齐</strong>的偏好正则损失回传梯度；人工排序对（A ≻ B）自下方注入辅助头，不破坏核心 critic。</p>
</div>

---

## 工程总结

* **系统 + RL**：从 MDP 建模、分支动作设计到分布式 PyTorch 训练与服务原型，体现端到端 systems-AI 能力。
