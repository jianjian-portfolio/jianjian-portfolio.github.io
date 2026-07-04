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
<div class="diagram-svg-container">
<svg width="100%" height="100%" viewBox="0 0 524 130" style="max-width: 524px;" role="img" aria-label="High-frequency DRL decision loop">
<defs>
<marker id="drl-f1-arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
<path d="M 0 0 L 10 5 L 0 10 z" fill="var(--diagram-accent)"/>
</marker>
</defs>
<g class="interactive-group">
<rect x="6" y="45" width="76" height="40" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="1.5"/>
<text x="44" y="62" fill="currentColor" font-size="9" font-weight="600" text-anchor="middle" font-family="sans-serif">Platform</text>
<text x="44" y="76" fill="currentColor" fill-opacity="0.65" font-size="7" text-anchor="middle" font-family="sans-serif">Observability</text>
</g>
<g class="interactive-group">
<rect x="94" y="18" width="86" height="38" rx="8" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="137" y="34" fill="currentColor" font-size="9" font-weight="600" text-anchor="middle" font-family="sans-serif">Macro Enc</text>
<text x="137" y="48" fill="currentColor" fill-opacity="0.65" font-size="7" text-anchor="middle" font-family="sans-serif">parallel</text>
</g>
<g class="interactive-group">
<rect x="94" y="74" width="86" height="38" rx="8" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="137" y="90" fill="currentColor" font-size="9" font-weight="600" text-anchor="middle" font-family="sans-serif">Micro Enc</text>
<text x="137" y="104" fill="currentColor" fill-opacity="0.65" font-size="7" text-anchor="middle" font-family="sans-serif">parallel</text>
</g>
<g class="interactive-group">
<rect x="200" y="28" width="98" height="74" rx="8" fill="var(--entry)" stroke="var(--diagram-accent)" stroke-width="2"/>
<text x="249" y="54" fill="var(--diagram-accent)" font-size="10" font-weight="700" text-anchor="middle" font-family="sans-serif">BDQ Policy</text>
<text x="249" y="70" fill="currentColor" fill-opacity="0.75" font-size="8" text-anchor="middle" font-family="sans-serif">Actor-Critic</text>
<text x="249" y="84" fill="currentColor" fill-opacity="0.55" font-size="7" text-anchor="middle" font-family="sans-serif">shared repr.</text>
</g>
<g class="interactive-group">
<rect x="316" y="18" width="82" height="38" rx="8" fill="var(--entry)" stroke="currentColor" stroke-opacity="0.35" stroke-width="1.2"/>
<text x="357" y="34" fill="currentColor" font-size="9" font-weight="600" text-anchor="middle" font-family="sans-serif">Control</text>
<text x="357" y="48" fill="currentColor" fill-opacity="0.65" font-size="7" text-anchor="middle" font-family="sans-serif">primary</text>
</g>
<g class="interactive-group">
<rect x="316" y="74" width="82" height="38" rx="8" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3"/>
<text x="357" y="90" fill="var(--diagram-stable)" font-size="8" font-weight="600" text-anchor="middle" font-family="sans-serif">Pref Align</text>
<text x="357" y="104" fill="currentColor" fill-opacity="0.65" font-size="7" text-anchor="middle" font-family="sans-serif">auxiliary</text>
</g>
<g class="interactive-group">
<rect x="416" y="45" width="96" height="40" rx="8" fill="var(--entry)" stroke="var(--diagram-stable)" stroke-width="1.5"/>
<text x="464" y="62" fill="var(--diagram-stable)" font-size="9" font-weight="600" text-anchor="middle" font-family="sans-serif">GPU Replay</text>
<text x="464" y="76" fill="currentColor" fill-opacity="0.65" font-size="7" text-anchor="middle" font-family="sans-serif">Buffer</text>
</g>
<line x1="82" y1="58" x2="94" y2="37" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow)"/>
<line x1="82" y1="72" x2="94" y2="93" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow)"/>
<line x1="180" y1="37" x2="200" y2="52" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow)"/>
<line x1="180" y1="93" x2="200" y2="78" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow)"/>
<line x1="298" y1="48" x2="316" y2="37" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow)"/>
<line x1="298" y1="82" x2="316" y2="93" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#drl-f1-arrow)"/>
<line x1="398" y1="37" x2="416" y2="58" stroke="var(--diagram-accent)" stroke-width="1.5" marker-end="url(#drl-f1-arrow)"/>
<line x1="398" y1="93" x2="416" y2="72" stroke="var(--diagram-stable)" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#drl-f1-arrow)"/>
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
<div class="diagram-svg-container">
<svg width="100%" height="100%" viewBox="0 0 420 130" style="max-width: 400px;" role="img" aria-label="Distributed DRL training topology">
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
<p class="diagram-caption"><strong>Training Stack:</strong> Ray coordinates multi-GPU workers; Accelerate + DDP synchronize gradients; DeepSpeed ZeRO-2 partitions optimizer states to reduce memory pressure during large-batch DRL updates.</p>
</div>

### 3. GPU-Resident Replay Buffer
* **Zero-Copy Training Path**: Designed a **GPU-resident replay buffer** so experience tuples remain device-local across sampling and gradient steps—eliminating CPU→GPU copy overheads that otherwise dominate short-horizon, high-frequency DRL workloads.
* **Throughput Impact**: Keeping transitions on-device improved effective training throughput during pilot benchmarks where decision cadence and batch sampling rates approached HPC-style duty cycles.

### 4. RLHF-Style Preference Alignment Head
* **Safety Regularization**: Built an auxiliary **preference alignment head** (RLHF-style) to regularize shared policy representations against predefined **safety bounds**—penalizing action trajectories that violate latency SLOs, overload thresholds, or fairness constraints during exploration.
* **Human-in-the-Loop Ready**: The alignment module accepts ranked trajectory pairs from operator feedback, enabling iterative policy refinement without destabilizing the core BDQ critic during stealth pilot iterations.

---

## Engineering Takeaways

* **Systems + RL**: Demonstrated end-to-end ownership from MDP formulation and branching action design through distributed PyTorch training and serving prototypes.
