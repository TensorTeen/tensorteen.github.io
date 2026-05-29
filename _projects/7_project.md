---
layout: page
title: IndiCASA
description: A Dataset and Bias Evaluation Framework in LLMs Using Contrastive Embedding Similarity in the Indian Context
img: assets/img/projects/indicasa/indicasa_fig2.jpg
importance: 2
category: work
related_publications: true
---

As Large Language Models (LLMs) gain traction in high-stakes domains, evaluating them for embedded biases has become critical. However, existing bias evaluation frameworks are largely Western-centric and fail to capture the nuanced, complex sociolinguistic landscape of India, particularly its fluid and context-dependent caste hierarchies. 

To address this, we introduce **IndiCASA** (IndiBias-based Contextually Aligned Stereotypes and Anti-stereotypes), a novel dataset comprising 2,575 human-validated sentences that span five demographic axes: caste, gender, religion, disability, and socioeconomic status. Alongside the dataset, we propose a bias evaluation framework that utilizes a contrastively trained encoder to capture fine-grained biases through embedding similarity.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/indicasa/indicasa_fig1.jpg" title="Transformation of Embedding Space" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/indicasa/indicasa_fig2.jpg" title="Overall Research Workflow" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/indicasa/indicasa_fig3.jpg" title="Dataset Curation Workflow" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Transformation of the embedding space before and after tuning the encoder on the IndiCASA dataset. Middle: The overall research workflow encompassing Dataset Curation, Encoder Tuning, and Bias Evaluation. Right: The end-to-end Human-AI collaborative workflow utilized for generating the IndiCASA dataset.
</div>

### Methodology: Contrastive Learning for Contextual Bias

One of the primary challenges in evaluating bias is that language models often treat stereotypical and anti-stereotypical sentences as semantically similar due to lexical overlap (e.g., varying by only a single demographic identifier like "Brahmin" or "Dalit"). To overcome this, our framework trains a dedicated encoder using contrastive loss, specifically optimizing the embedding space to increase intra-class similarity while maximizing inter-class separation. 

By applying Normalized Temperature-scaled Cross Entropy (NT-Xent) loss, the encoder learns to distinguish socio-cultural bias signals from superficial lexical patterns. We then evaluate generated text against an ideal unbiased uniform distribution, calculating a Bias Score on a scale from 0 to 100.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/indicasa/indicasa_fig4.jpg" title="Validation Metric Comparison" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/indicasa/indicasa_fig5.jpg" title="t-SNE Embedding Clusters" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Comparison of Validation representation across different models and contrastive loss functions, demonstrating NT-Xent's superior performance. Right: Two-Component t-SNE plots showing highly dispersed embeddings in the vanilla model versus clear, well-defined clustering in the finetuned encoder for caste and religion biases.
</div>

### Key Findings

We evaluated several open-weight LLMs (including Llama-3.1, Gemma-2, Mistral, and DeepSeek) using our framework. Our findings revealed that:
* **Widespread Bias:** All evaluated models exhibit varying degrees of stereotypical bias across demographic categories.
* **Disability Bias is Persistent:** Across models, disability consistently registered the highest bias scores. 
* **Religion Bias is Lower:** Religion-related bias scores were generally the lowest, likely reflecting global debiasing efforts during model alignment.
* **Caste and Socioeconomic Parity:** Models exhibited similar levels of bias for both caste and socioeconomic axes, suggesting these categories are treated similarly in the embedding space.

Our contrastively fine-tuned encoder successfully developed a rich embedding space capable of mapping these complex demographic relationships, providing a much-needed benchmark for assessing the fairness of generative models in the Indian context.
