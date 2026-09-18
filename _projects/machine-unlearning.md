---
layout: page
title: Machine Unlearning
description: Assessing deep learning unlearning techniques on ResNet-18 using a novel Likelihood-Ratio privacy metric.
img: assets/img/projects/unlearning-preview.png
importance: 2
category: coursework
github: https://github.com/igor-martinelli/ETH-Deep-Learning
---

### Overview

Machine Unlearning focuses on selectively inducing a trained deep learning model to "forget" specific subsets of its training data without requiring full retraining from scratch. Developed as part of the **Deep Learning** course at **ETH Zurich**, this project evaluates eight unlearning algorithms on a **ResNet-18** architecture across image classification (**CIFAR-10**) and age regression (**AgeDB**).

To ensure practical efficiency over full retraining, all unlearning algorithms were strictly constrained to consume **$\le 5\%$** of the total retraining compute time (maximum of 5 epochs).

---

### Custom Privacy Metric ($\hat{\epsilon}$)

Evaluating unlearning efficacy purely on accuracy drop is misleading, as low accuracy on the forget set does not guarantee statistical indistinguishability from a retrained model. We formulated an empirical privacy score ($\hat{\epsilon}$) grounded in group-level Differential Privacy and Likelihood-Ratio Membership Inference Attacks (MIA):

1. **Hinge Loss Statistics**: Extracted un-normalized output logits across $N = 80$ retrained models (trained on $D \setminus S$) vs. $N = 80$ unlearned models.
2. **Likelihood-Ratio Attack**: Fitted Gaussian distributions on the hinge-loss behaviors to calculate likelihood ratios for each sample.
3. **$\hat{\epsilon}$ Estimation**: Formulated the empirical privacy bound across Detection Error Tradeoff (DET) decision boundaries:

$$\hat{\epsilon} = \max \left| \log \frac{1 - \delta - \text{FPR}}{\text{FNR}}, \, \log \frac{1 - \delta - \text{FNR}}{\text{FPR}} \right|$$

Lower $\hat{\epsilon}$ values indicate higher statistical indistinguishability between an unlearned model and a model that was never exposed to the forget set.

<div class="row mt-3 mb-3 justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/unlearning-fig1.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Figure 1: Conceptual overlap of retrained vs. unlearned hinge loss distributions across decision boundaries to derive empirical FPR, FNR, and ε bounds." %}
    </div>
</div>

---

### Unlearning Methodology

We benchmarked eight unlearning strategies categorized into baseline, adaptation, pruning, and structural reset techniques:

* **Finetuning**: Fine-tunes the model on the retain set for 5 epochs using a low learning rate ($\eta = 0.001$).
* **Label Poisoning**: Assigns random labels to the forget set. Evaluated as **Poison** (1 epoch on forget set) and **Poison Full** (5 epochs on full dataset).
* **Two-Stage Hybrid**: Mitigates accuracy drops from poisoning by performing 1 epoch of label poisoning followed by 5 epochs of retain set fine-tuning.
* **Selective Pruning**: Overfits an auxiliary model on the forget set to 100% accuracy, identifies weights closest to the target model using a 0.30 quantile threshold, re-initializes them via Kaiming initialization, and fine-tunes on the retain set.
* **Pruning Last Layer**: Restricts selective weight re-initialization exclusively to the final classification layer.
* **Selective Pruning Complex (Novel)**: Overfits two auxiliary models—one on the forget set ($S$) and one on a random slice of the retain set ($D \setminus S$). Re-initializes only shared weights intrinsic to both distributions before fine-tuning on the retain set.
* **Activation Reset (Novel)**: Measures average convolutional kernel activations on forget vs. retain samples. Re-initializes kernels whose mean activation on the forget set exceeds 80% of retain set samples, along with the final classification head.

---

### Empirical Results & Comparison

Below are the experimental results on **CIFAR-10** ($\delta = 0.05$, $N=80$ models per evaluation):

| Method | Privacy Bound ($\hat{\epsilon}$) ↓ | Retain Acc ($D \setminus S$) | Forget Acc ($S$) | Test Acc |
| :--- | :---: | :---: | :---: | :---: |
| **Baseline (Retrained)** | **0.88** | 99.92% | 94.61% | 94.99% |
| **Pruning Complex (Ours)** | **2.20** | 99.99% | 91.10% | 94.31% |
| **Pruning Last Layer** | 2.30 | 99.99% | 94.43% | 95.04% |
| **Two-Stage Hybrid** | 2.83 | 96.09% | 96.23% | 94.58% |
| **Selective Pruning** | 3.22 | 98.21% | 97.47% | 92.60% |
| **Activation Reset (Ours)** | 3.58 | 99.99% | 93.96% | 94.76% |
| **Finetuning** | 3.74 | 99.99% | 99.51% | 94.58% |
| **Poison** | 4.16 | 99.72% | 99.55% | 94.21% |
| **Poison Full** | 4.20 | 99.99% | 87.41% | 94.16% |

* **Optimal Privacy ($\hat{\epsilon}$ Bound)**: **Pruning Complex** achieves the lowest empirical privacy bound ($\hat{\epsilon} = 2.20$) across all evaluated trade-off values of $\delta$ (as illustrated in Figure 2), bringing it closest to the baseline retrained model ($\hat{\epsilon} = 0.88$).
* **Preservation of Model Utility**: All methods successfully preserve test accuracy within a tight range of $92.60\%$ to $95.04\%$ and maintain near-perfect retain set accuracy ($96.09\%$–$99.99\%$).
* **The Accuracy–Privacy Disconnect**: A sharp drop in forget set accuracy does not indicate effective unlearning. For example, **Poison Full** depresses forget set accuracy to $87.41\%$, yet exhibits the worst privacy score ($\hat{\epsilon} = 4.20$) because Membership Inference Attacks can easily isolate the artificially induced loss distributions.
* **Targeted vs. Naïve Unlearning**: Methods relying on selective weight re-initialization (**Pruning Complex** and **Pruning Last Layer**) consistently outperform standard fine-tuning ($\hat{\epsilon} = 3.74$) and label poisoning ($\hat{\epsilon} = 4.16$) by targeting the specific parameters encoding the forget set distribution.

<div class="row mt-3 mb-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/unlearning-fig2.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Figure 2: Empirical privacy bounds (ε̂) evaluated across varying delta (δ) parameters on CIFAR-10." %}
    </div>
</div>

---

### Key Takeaways

1. **Superior Forget Quality**: **Pruning Complex** achieved the lowest privacy bound ($\hat{\epsilon} = 2.20$), significantly outperforming basic fine-tuning ($\hat{\epsilon} = 3.74$) while retaining high test utility ($94.31\%$).
2. **The Accuracy Paradox**: Forcing low accuracy on the forget set (e.g., **Poison Full** achieving 87.41% forget accuracy) resulted in the worst privacy protection ($\hat{\epsilon} = 4.20$). Membership inference attacks easily detect output distribution artifacts induced by poisoning.

<div class="row mt-3 mb-3">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/unlearning-prune-complex.png" class="img-fluid rounded z-depth-1" zoomable=true caption="<b>Pruning Complex</b>: High distribution overlap indicating true statistical unlearning." %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/unlearning-poison-full.png" class="img-fluid rounded z-depth-1" zoomable=true caption="<b>Poison Full</b>: Clear distribution separation revealing vulnerability to MIA." %}
    </div>
</div>

<ol start="3">
  <li><b>Cross-Domain Generalization</b>: Evaluating on regression tasks (<b>AgeDB</b>, MAE $6.73$) showed that poisoning methods alter logit distributions differently across classification vs. regression tasks, highlighting the need for task-specific unlearning metrics.</li>
</ol>

---

### Repository & Report

* **Repository**: [igor-martinelli/ETH-Deep-Learning](https://github.com/igor-martinelli/ETH-Deep-Learning)
* **PDF Report**: [Download Project Report (PDF)]({{ '/assets/pdf/unlearning-report.pdf' | relative_url }})