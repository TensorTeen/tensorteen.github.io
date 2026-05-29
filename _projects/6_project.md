---
layout: page
title: Intelligent Tool Manipulation Systems
description: RE-GAINS & EnChAnT frameworks for enhanced LLM query responses
img: assets/img/projects/regains-enchant/project_cover.jpg
importance: 3
category: fun
related_publications: true
---

Large Language Models (LLMs) currently struggle with tool invocation and chaining, as they often hallucinate or miss essential steps in a sequence. To overcome these limitations and progress towards Artificial General Intelligence (AGI), it is crucial that LLMs efficiently perform logical and mathematical operations. 

This project {% cite girhepuje2024regains %} introduces **RE-GAINS** and **EnChAnT**, two novel frameworks that empower LLMs to tackle complex user queries by making API calls to external tools based on tool descriptions and argument lists. These systems analyze input queries, select appropriate tools, define arguments, and sequence the deployment of these tools without receiving actual results from individual intermediate calls.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/regains-enchant/regains_pipeline.jpg" title="RE-GAINS Pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/regains-enchant/enchant_pipeline.jpg" title="EnChAnT Pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: The RE-GAINS architecture, featuring tool and example retrievers feeding into an OpenAI system. Right: The EnChAnT pipeline, showcasing Jina Embeddings, task decomposition, and OpenChat 3.5 integrations alongside an LLM Format Enforcer.
</div>

### Dual-Pipeline Approach

Our work establishes two distinct pipelines tailored to specific objectives:

* **RE-GAINS (Retrieval Enhanced Generation via Actions INsights and States):** This pipeline is designed to maximize efficiency. It utilizes OpenAI models and embeddings coupled with a specialized prompt based on the Reasoning via Planning (RAP) framework. By retrieving relevant tools and examples, it accelerates reasoning and reduces input tokens, making it highly cost-effective.
* **EnChAnT (Enforced Creation of Actions and Thoughts):** Optimized for performance, this open-source solution leverages an LLM format enforcer, the OpenChat 3.5 model, and ToolBench's API Retriever. It uses a rigorous three-stage process: Tool Retrieval, Task Decomposition, and Recomposition.

### Mitigating Hallucinations with LLM Enforcers

In the realm of complex question answering, hallucination remains a prevalent challenge, particularly concerning tool manipulation. LLMs often generate nonexistent resources or hallucinate argument substitutions (e.g., mistakenly formatting array indices). 

To counter this, we integrated the Language Model Format Enforcer (LMFE), which filters the model's generated tokens at every time step. This forces the LLM to output valid JSON schemas and eliminates tool and argument hallucinations. For closed-source models like GPT-3.5, we developed a novel pipeline that passes the output into an open-source model acting as an "identity function" constrained by the enforcer.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/regains-enchant/llm_enforcer.jpg" title="LLM Enforcer Pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/regains-enchant/copilot_example.jpg" title="GitHub Copilot Example" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Our novel pipeline to apply LLM format enforcers to closed-source models like GPT 3.5 by routing outputs through an OpenChat 3.5 identity function. Right: An observation of GitHub Copilot successfully auto-completing a user query, demonstrating the real-world applicability of tool manipulation capabilities.
</div>

### Evaluation and Efficiency

We adopted a multi-faceted evaluation framework inspired by ControlLLM. Our models were assessed based on rigorous metrics:
* **Tool Selection:** Irrelevant Tool Inclusion Rate (IR), Necessary Tool Inclusion Rate (NR), and Missing Tool Rate (MR).
* **Argument Assignment:** Resource Hallucination Rate (HR).
* **Solution Evaluation:** BLEU Score and ROUGE-L F1 Score.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/regains-enchant/benchmark_results.jpg" title="Benchmark Table" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Benchmark results evaluating different implementations on our golden dataset. RE-GAINS paired with GPT-3.5-turbo demonstrates a high Necessary Tool Inclusion Rate (0.961) and incredibly low Resource Hallucination Rate (0.003).
</div>

Both RE-GAINS and EnChAnT proved to be incredibly low-cost—operating at approximately $0.01 per query—while demonstrating the scalable ability to invoke and chain externally described tools across various domains. Ultimately, these systems represent a significant step towards developing AI agents capable of effectively handling complex logical tasks with external assistance.
