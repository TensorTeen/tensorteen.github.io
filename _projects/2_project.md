---
layout: page
title: Training-Free Memory Layer for Robotics
description: Developed a **Training-Free Memory Conditioned Action Generation** framework. This non-parametric, retrieval-augmented system conditions a frozen VLA on historical expert trajectories, guiding action generation without requiring any fine-tuning or architectural changes.
img: assets/img/projects/fujitsu/Figure_1.jpg
importance: 2
category: work
---

Vision-Language-Action (VLA) models have fundamentally shifted the paradigm of robotic control, enabling generalist agents to follow natural language instructions. However, these foundation models struggle significantly with long-horizon tasks. Because they typically operate as reactive, first-order Markovian systems, they rely almost entirely on immediate sensory input and ignore historical context. This dynamic induces compounding errors over extended inference timesteps and leaves models vulnerable to perceptual aliasing—where visually identical pre- and post-grasp scenes cause unstable action predictions.

To solve this, we developed a **Training-Free Memory Conditioned Action Generation** framework. This non-parametric, retrieval-augmented system conditions a frozen VLA on historical expert trajectories, guiding action generation without requiring any fine-tuning or architectural changes.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/fujitsu/Figure_2.jpg" title="Figure 2: Effect of memory on long-horizon task" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/fujitsu/Figure_7.jpg" title="Figure 7: Callbacks to the memory (CALVIN)" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/fujitsu/Figure_8.jpg" title="Figure 8: Callbacks to the memory (LIBERO)" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 2 shows a standard VLA policy drifting early in a long-horizon task versus successful completion when augmented with our memory framework. Figures 7 and 8 highlight the frequent callbacks to memory during failure cases across CALVIN and LIBERO, demonstrating how the system steps in precisely when the base model fails.
</div>

---

### Methodology: How Training-Free Memory Works

Our framework bypasses the computational bottlenecks of traditional fine-tuning, such as catastrophic forgetting and exhaustive hyperparameter searches. Instead, it uses a plug-and-play memory module designed for fast, inference-time adaptation. 

* **Memory Construction:** We build an external memory bank by slicing expert training trajectories into sliding-window chunks. Each chunk consists of a vision-language model (VLM) embedding and a corresponding robot state embedding.
* **State-Centric Retrieval:** Because visual embeddings can lack fine-grained discriminative power during distribution shifts, we retrieve memories using purely proprioceptive state embeddings. 
* **High-Speed Indexing:** To ensure low latency, the system organizes these state embeddings using a Hierarchical Navigable Small World (HNSW) data structure. This allows for highly efficient, high-recall approximate nearest neighbor searches using the $L_2$ distance metric.
* **Selective Gating:** The memory is not always active. To preserve fast inference times on simple tasks, we employ an adaptive fallback strategy that triggers memory conditioning only when the base policy fails.

<div class="row justify-content-sm-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/fujitsu/Figure_3.jpg" title="Figure 3: The framework of our proposed Memory" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 3: The full pipeline framework. Expert demonstrations are encoded into VLM and state trajectories to build the HNSW index. During inference, if the agent struggles, the current state queries the HNSW index to retrieve relevant past VLM embeddings, which are concatenated to guide the frozen VLA action expert.
</div>

---

### Evaluation and Breakthrough Results

We rigorously evaluated our framework against state-of-the-art models (such as FlowerVLA and MoDE) across 5 datasets from the popular CALVIN and LIBERO benchmarks. 

* **Benchmark Dominance:** Our method achieved relative gains of up to 27% in task completion success rates over standard baselines. 
* **Long-Horizon Extension:** We extended the CALVIN benchmark to test extreme task horizons (6 to 10 sequential instructions). Under these highly complex conditions, our framework yielded up to 30% relative improvement margins.
* **Robustness to Corruption:** When deployed in simulated out-of-distribution (OOD) environments featuring visual corruptions like defocus, smudges, and sensor dead pixels, the memory-conditioned models mitigated performance degradation, showing average absolute gains of 10.7%.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/fujitsu/Figure_1.jpg" title="Figure 1: Comparison of VLA performance" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/fujitsu/Figure_11.jpg" title="Figure 11: Comparison of results over observation corruptions" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 1 details consistent performance gains over baseline VLAs across the CALVIN and LIBERO benchmarks. Figure 11 demonstrates the framework's robustness, showing significant performance retention even when the agent's camera observations are subjected to heavy visual corruptions like smudging or defocus.
</div>

### Real-World Deployment

To validate the framework's practical viability, we deployed it on a physical Cobot-Magic-AgileX robot equipped with a 6-DOF arm. The robot was tasked with a sequential, long-horizon objective: reaching a microwave, opening the door, and placing a bowl inside. 

Evaluated heavily under real-world OOD conditions (such as skewed microwave placement and handle occlusions), the memory-augmented system demonstrated remarkable resilience, realizing up to a 2X increase in success rates compared to the base policy.

<div class="row">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/fujitsu/Figure_5.jpg" title="Figure 5: Real World Qualitative comparison of task execution" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/fujitsu/Figure_4.jpg" title="Figure 4: Inference Time OOD variations" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 5 shows first-person and side-camera views of the robot successfully grasping the microwave handle and placing the bowl inside utilizing memory conditioning, overcoming the oscillations seen in the baseline. Figure 4 highlights the rigorous out-of-distribution variations tested, including skewed placements and cloth occlusions.
</div>
