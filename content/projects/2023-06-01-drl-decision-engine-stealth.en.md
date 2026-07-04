---
title: "High-Throughput Deep Reinforcement Learning (DRL) Decision Engine (Stealth Pilot)"
description: "Stealth pilot BDQ deep reinforcement learning engine for cloud resource supply and scheduling with GPU replay and preference alignment."
date: 2023-06-01T00:00:00Z
featured: true
weight: 10
sitemap:
  priority: 0.9
  changefreq: monthly
cover:
    image: "/assets/images/projects/drl-decision-engine-cover.svg"
    style: "diagram"
    alt: "Cloud decision platform DRL architecture with parallel macro/micro encoders, BDQ dual-head outputs, and GPU replay buffer"
    relative: false
---

Designed and developed a high-throughput **sequential decision engine** for a **cloud computing decision platform**, delivering real-time sequential decisions for resource provisioning and scheduling under high-throughput, high-frequency runtime workloads. This ongoing stealth pilot applies deep reinforcement learning at systems scale—bridging algorithm design, distributed GPU training, and safety-aware policy alignment.

**Key Technologies:** PyTorch · Branching Dueling Q-Network (BDQ) · HF Accelerate · DeepSpeed · Ray Train/Serve · DDP · Preference Alignment

---

## System Overview & Sequential Decision Loop

The engine formulates provisioning and scheduling on the platform as a **Markov Decision Process (MDP)**: a **macro information encoder** and a **micro information encoder** process the heterogeneous platform observability **in parallel**—the former aggregating platform level signals, the latter capturing fast local observations—before fusion into the Actor-Critic backbone. The primary head emits discrete control actions; a parallel **preference alignment auxiliary network** regularizes the shared policy representation for safety and preference constraints. Decisions run at millisecond-scale intervals, balancing throughput, tail latency, and resource utilization.

<div class="diagram-placeholder">
<span class="diagram-title">[Figure 1: High-Frequency DRL Decision Loop]</span>
<div class="diagram-svg-container diagram-svg-container--drl-loop">
<svg width="100%" height="100%" viewBox="0 0 725 162" role="img" aria-label="High-frequency DRL decision loop">
<defs>
<marker id="drl-f1-arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
<path d="M 0 0 L 10 5 L 0 10 z" fill="var(--diagram-accent)"/>
</marker>
<marker id="drl-f1-arrow-stable" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
<path d="M 0 0 L 10 5 L 0 10 z" fill="var(--diagram-stable)"/>
</marker>
</defs>
<g transform="scale(1.25)">
<g class="diagram-edges" aria-hidden="true">
<line x1="76" y1="58" x2="136" y2="37" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow)"/>
<line x1="76" y1="72" x2="136" y2="93" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow)"/>
<line x1="210" y1="37" x2="258" y2="52" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow)"/>
<line x1="210" y1="93" x2="258" y2="78" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow)"/>
<line x1="342" y1="48" x2="382" y2="37" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow)"/>
<line x1="342" y1="82" x2="382" y2="93" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#drl-f1-arrow-stable)"/>
<line x1="452" y1="37" x2="490" y2="58" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow)"/>
<line x1="452" y1="93" x2="490" y2="72" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#drl-f1-arrow-stable)"/>
</g>
<g class="interactive-group">
<rect x="8" y="40" width="80" height="48" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.5"/>
<text x="48" y="60" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Platform</text>
<text x="48" y="76" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Observability</text>
</g>
<g class="interactive-group">
<rect x="130" y="12" width="90" height="46" rx="8" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="175" y="31" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Macro Enc</text>
<text x="175" y="47" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">parallel</text>
</g>
<g class="interactive-group">
<rect x="130" y="70" width="90" height="46" rx="8" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="175" y="89" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Micro Enc</text>
<text x="175" y="105" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">parallel</text>
</g>
<g class="interactive-group">
<rect x="250" y="20" width="100" height="88" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="2"/>
<text x="300" y="50" fill="var(--diagram-accent)" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">BDQ Policy</text>
<text x="300" y="66" fill="currentColor" fill-opacity="0.75" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Actor-Critic</text>
<text x="300" y="82" fill="currentColor" fill-opacity="0.55" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">shared repr.</text>
</g>
<g class="interactive-group">
<rect x="376" y="12" width="86" height="46" rx="8" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="419" y="31" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Control</text>
<text x="419" y="47" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">primary</text>
</g>
<g class="interactive-group">
<rect x="376" y="70" width="86" height="46" rx="8" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3"/>
<text x="419" y="89" fill="var(--diagram-stable)" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Pref Align</text>
<text x="419" y="105" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">auxiliary</text>
</g>
<g class="interactive-group">
<rect x="484" y="40" width="94" height="48" rx="8" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.5"/>
<text x="531" y="60" fill="var(--diagram-stable)" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">GPU Replay</text>
<text x="531" y="76" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Buffer</text>
</g>
</g>
</svg>
</div>
<p class="diagram-caption"><strong>Decision Loop:</strong> Platform observability feeds macro and micro encoders in parallel; fused representations enter the BDQ backbone. The primary head emits control actions; a preference alignment auxiliary network runs in parallel on the shared representation; transitions land in a GPU-resident replay buffer for distributed off-policy updates.</p>
</div>

---

## Core Technical Contributions

### 1. Branching Dueling Q-Network (BDQ) Actor-Critic Engine
* **Parallel State Encoding**: A **macro information encoder** and a **micro information encoder** process the heterogeneous platform observability in parallel, extracting macro signals and fast micro features before fusion into the BDQ backbone.
* **Dual-Head Outputs**: The Actor-Critic **primary head** emits discrete control actions; a **preference alignment auxiliary network** runs in parallel on the shared policy representation to enforce safety and preference constraints during training.
* **Architecture**: Implemented an **Actor-Critic** stack with **Dueling Advantage** decomposition to stabilize Q-value estimation when action spaces branch across concurrent control dimensions (e.g., capacity tiers, priority weights, policy levels).
* **Branching Actions**: Used a **Branching Dueling Q-Network (BDQ)** formulation so the agent factorizes multi-dimensional control signals instead of flattening them into an exponentially large discrete space—preserving sample efficiency under sparse, delayed rewards typical of platform sequential decision problems.

### 2. Distributed Training at Scale
* **Multi-Node GPU Clusters**: Scaled training across multi-node GPU clusters using **Hugging Face Accelerate**, **Distributed Data Parallel (DDP)**, and **DeepSpeed (ZeRO-2)** to partition optimizer states and sustain high throughput on large replay batches.
* **Ray Train / Ray Serve**: Orchestrated experiment sweeps and serving prototypes with **Ray Train** and **Ray Serve**, separating offline policy improvement from low-latency online inference paths during pilot evaluation.

<div class="diagram-placeholder">
<span class="diagram-title">[Figure 2: Distributed Training Topology]</span>
<div class="diagram-svg-container diagram-svg-container--drl-train">
<svg width="100%" height="100%" viewBox="-55 0 723 185" role="img" aria-label="Distributed DRL training topology">
<defs>
<marker id="drl-f2-arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
<path d="M 0 0 L 10 5 L 0 10 z" fill="var(--diagram-accent)"/>
</marker>
</defs>
<g transform="scale(1.25) translate(-32.23, 0)">
<g class="diagram-edges" aria-hidden="true">
<line x1="90.4" y1="74" x2="118" y2="74" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f2-arrow)"/>
<line x1="249" y1="49" x2="345" y2="49" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2" stroke-dasharray="3 2"/>
<line x1="249" y1="89" x2="345" y2="89" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2" stroke-dasharray="3 2"/>
</g>
<rect x="118" y="8" width="372" height="122" rx="10" fill="none" stroke="currentColor" stroke-opacity="0.25" stroke-width="1.2" stroke-dasharray="5 3"/>
<text x="304" y="28" fill="currentColor" fill-opacity="0.7" text-anchor="middle" class="diagram-train-heading">Distributed Training Job (Ray Train)</text>
<line x1="290" y1="33" x2="290" y2="105" stroke="currentColor" stroke-opacity="0.15" stroke-width="1"/>
<text x="204" y="37" fill="currentColor" fill-opacity="0.55" text-anchor="middle" class="diagram-train-node">Node 1</text>
<text x="390" y="37" fill="currentColor" fill-opacity="0.55" text-anchor="middle" class="diagram-train-node">Node 2</text>
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
<text x="297" y="59" fill="currentColor" fill-opacity="0.5" font-size="7" text-anchor="middle" font-family="sans-serif">DDP sync</text>
<text x="297" y="99" fill="currentColor" fill-opacity="0.5" font-size="7" text-anchor="middle" font-family="sans-serif">DDP sync</text>
<text x="304" y="132" fill="var(--diagram-stable)" text-anchor="middle" class="diagram-train-stable">Each worker: Accelerate · DDP · DeepSpeed ZeRO-2</text>
</g>
</svg>
</div>
<p class="diagram-caption"><strong>Training stack (schematic):</strong> Ray Train schedules a multi-node job across GPU workers; within the same run, each worker executes PyTorch via Accelerate with DDP gradient synchronization and DeepSpeed ZeRO-2 optimizer-state sharding. Ray Serve (online inference) is a separate path and omitted here.</p>
</div>

### 3. GPU-Resident Replay Buffer
* **Zero-Copy Training Path**: Designed a **GPU-resident replay buffer** so experience tuples remain device-local across sampling and gradient steps—eliminating CPU→GPU copy overheads that otherwise dominate short-horizon, high-frequency DRL workloads.
* **Throughput Impact**: Keeping transitions on-device improved effective training throughput during pilot benchmarks where decision cadence and batch sampling rates approached HPC-style duty cycles.

### 4. RLHF-Style Preference Alignment Network
* **Safety Regularization**: Built an auxiliary **preference alignment network** (RLHF-style) to regularize shared policy representations against predefined **safety bounds**—penalizing action trajectories that violate latency SLOs, overload thresholds, or fairness constraints during exploration.
* **Human-in-the-Loop Ready**: The alignment module accepts ranked trajectory pairs from operator feedback, enabling iterative policy refinement without destabilizing the core BDQ critic during stealth pilot iterations.

<div class="diagram-placeholder">
<span class="diagram-title">[Figure 3: Dual-Head Multi-Objective Policy (Primary + Auxiliary)]</span>
<div class="diagram-svg-container diagram-svg-container--drl-pref">
<svg width="100%" height="100%" viewBox="0 0 880 278" role="img" aria-label="Dual-head policy: horizontal main branch, L-shaped auxiliary branch">
<defs>
<marker id="drl-f3-arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
<path d="M 0 0 L 10 5 L 0 10 z" fill="var(--diagram-accent)"/>
</marker>
<marker id="drl-f3-arrow-stable" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
<path d="M 0 0 L 10 5 L 0 10 z" fill="var(--diagram-stable)"/>
</marker>
</defs>
<g transform="translate(109, 4)">
<g transform="scale(1.25)">
<g class="diagram-edges" aria-hidden="true">
<line x1="98" y1="42" x2="118" y2="42" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f3-arrow)"/>
<line x1="230" y1="42" x2="250" y2="42" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f3-arrow)"/>
<line x1="324" y1="42" x2="344" y2="42" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f3-arrow)"/>
<path d="M 69 64 L 69 128 L 118 128" stroke="var(--diagram-stable)" stroke-width="1.4" stroke-dasharray="4 3" fill="none" marker-end="url(#drl-f3-arrow-stable)"/>
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
<line x1="284" y1="134" x2="344" y2="134" stroke="var(--diagram-stable)" stroke-width="1.5" marker-end="url(#drl-f3-arrow-stable)"/>
<line x1="222" y1="193" x2="222" y2="165" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#drl-f3-arrow-stable)"/>
<path d="M 388 156 L 388 206 L 39 206 L 39 64" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3" fill="none" marker-end="url(#drl-f3-arrow-stable)"/>
<circle cx="148" cy="116" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="148" cy="128" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="148" cy="140" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="148" cy="152" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="210" cy="122" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="210" cy="134" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="210" cy="146" r="4.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.2"/>
<circle cx="278" cy="134" r="5.5" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.3"/>
</g>
<text x="6" y="42" fill="var(--diagram-accent)" text-anchor="end" font-family="sans-serif" class="diagram-anno-label">Main branch →</text>
<text x="58" y="98" fill="var(--diagram-stable)" text-anchor="start" font-family="sans-serif" class="diagram-anno-label">Aux branch</text>
<rect x="118" y="74" width="206" height="14" rx="4" fill="none" stroke="currentColor" stroke-opacity="0.2" stroke-width="1" stroke-dasharray="4 3"/>
<text x="221" y="84" fill="currentColor" fill-opacity="0.55" text-anchor="middle" font-family="sans-serif" class="diagram-anno-label">Preference space · safety bounds</text>
<rect x="118" y="90" width="206" height="75" rx="8" fill="none" stroke="var(--diagram-stable)" stroke-opacity="0.55" stroke-width="1.2" stroke-dasharray="4 3"/>
<text x="221" y="104" fill="var(--diagram-stable)" text-anchor="middle" font-family="sans-serif" class="diagram-anno-label">Auxiliary · Pref Align (aux head)</text>
<g class="interactive-group">
<rect x="10" y="20" width="88" height="44" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.5"/>
<text x="54" y="38" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Shared Policy</text>
<text x="54" y="54" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">repr · BDQ</text>
</g>
<g class="interactive-group">
<rect x="118" y="20" width="112" height="44" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.5"/>
<text x="174" y="38" fill="var(--diagram-accent)" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Primary · Output</text>
<text x="174" y="54" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Net (main head)</text>
</g>
<g class="interactive-group">
<rect x="250" y="20" width="74" height="44" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.3"/>
<text x="287" y="46" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Control</text>
</g>
<g class="interactive-group">
<rect x="344" y="20" width="88" height="44" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.5"/>
<text x="388" y="46" fill="var(--diagram-accent)" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">RL objective</text>
</g>
<g class="interactive-group">
<rect x="344" y="112" width="88" height="44" rx="8" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.5"/>
<text x="388" y="128" fill="var(--diagram-stable)" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Pref reg.</text>
<text x="388" y="144" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">loss</text>
</g>
<g class="interactive-group">
<rect x="178" y="193" width="88" height="26" rx="6" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="222" y="205" fill="currentColor" text-anchor="middle" font-family="sans-serif" class="diagram-box-label">Traj. A ≻ B</text>
<text x="222" y="215" fill="currentColor" fill-opacity="0.65" text-anchor="middle" font-family="sans-serif" class="diagram-anno-label">human feedback</text>
</g>
<text x="222" y="186" fill="currentColor" fill-opacity="0.5" font-size="7" text-anchor="middle" font-family="sans-serif">grad backprop · aux head</text>
<text x="448" y="42" fill="var(--diagram-accent)" text-anchor="start" class="diagram-pref-stable">Primary RL optimization</text>
<text x="448" y="134" fill="var(--diagram-stable)" text-anchor="start" class="diagram-pref-stable">Penalize violating trajectories</text>
</g>
</g>
</svg>
</div>
<p class="diagram-caption"><strong>Dual-head multi-objective (schematic):</strong> The <strong>main branch</strong> flows horizontally left-to-right: shared BDQ representation → Primary output network → control actions → RL objective. The <strong>Auxiliary preference-alignment network drops down in an L-shaped path</strong> from the shared trunk (width aligned from Primary left edge to Control right edge) into a preference regularizer <strong>vertically aligned</strong> with the RL objective; ranked pairs (A ≻ B) inject into the aux head without destabilizing the core critic.</p>
</div>

---

## Engineering Takeaways

* **Systems + RL**: Demonstrated end-to-end ownership from MDP formulation and branching action design through distributed PyTorch training and serving prototypes.
