---
layout: page
title: Machine Unlearning & Privacy
description: Assessing deep learning unlearning techniques on ResNet-18 using a novel Likelihood-Ratio privacy metric.
img: assets/img/projects/unlearning-preview.png
importance: 3
category: work
github: https://github.com/igor-martinelli/ETH-Deep-Learning
---

### Overview

Machine Unlearning focuses on selectively inducing a trained deep learning model to "forget" specific subsets of its training data without requiring full retraining from scratch[cite: 1]. Developed as part of the **Deep Learning** course at **ETH Zurich**, this project explores unlearning effectiveness across image classification (CIFAR-10) and age regression (AgeDB) using a ResNet-18 architecture[cite: 1].

All unlearning algorithms were strictly constrained to consume **$\le 5\%$** of the compute time required for full model retraining (max 5 epochs)[cite: 1].

---

### Novel Unlearning Methods

In addition to standard finetuning, label poisoning, and selective pruning baselines, we introduced two novel unlearning techniques[cite: 1]:

* **Pruning Complex**: Overfits two separate auxiliary models—one on the forget set and one on a slice of the retain set—to identify weights intrinsic to both distributions[cite: 1]. These shared weights are re-initialized using Kaiming initialization before fine-tuning on the retain set[cite: 1].
* **Activation Reset**: Tracks kernel activation maps across convolutional layers to detect units that activate significantly higher ($>80\%$) on forget set samples compared to retain set samples[cite: 1]. Identified kernels and the final classification head are re-initialized and fine-tuned[cite: 1].

---

### Custom Privacy Metric ($\hat{\epsilon}$)

To evaluate forget quality beyond surface-level classification accuracy, we formulated an empirical metric ($\hat{\epsilon}$) rooted in group-level Differential Privacy and Likelihood-Ratio Membership Inference Attacks (MIA)[cite: 1]:

* **Hinge Loss Statistics**: Computed un-normalized output logit distributions across $N = 80$ models trained on retain sets versus unlearned models[cite: 1].
* **Likelihood-Ratio Attack**: Fitted Gaussian distributions on the loss behavior to calculate likelihood ratios between unlearned and retrained states[cite: 1].
* **$\hat{\epsilon}$ Evaluation**: Estimated the empirical privacy bound from False Positive (FPR) and False Negative Rates (FNR) across Detection Error Tradeoff (DET) decision boundaries[cite: 1]. Lower $\hat{\epsilon}$ values indicate higher statistical indistinguishability between an unlearned model and a model never trained on the data[cite: 1].

---

### Key Empirical Findings

* **Top Performance**: **Pruning Complex** achieved the best overall forget quality ($\hat{\epsilon} = 2.20$) on CIFAR-10, significantly outperforming standard finetuning ($\hat{\epsilon} = 3.74$) while maintaining 99.99% retain accuracy and 94.31% test accuracy[cite: 1].
* **Accuracy vs. Forgetting**: Forcing near 0% accuracy on the forget set (e.g., via aggressive poisoning) does **not** guarantee true unlearning, as membership inference attacks can still recover underlying distribution artifacts[cite: 1].

---

### Repository & Report

* **Repository**: [igor-martinelli/ETH-Deep-Learning](https://github.com/igor-martinelli/ETH-Deep-Learning)

* **PDF Download**: [Download Report (PDF)]({{ '/assets/pdf/Machine_Unlearning_Report.pdf' | relative_url }})[cite: 1]

<object data="{{ '/assets/pdf/Machine_Unlearning_Report.pdf' | relative_url }}" type="application/pdf" width="100%" height="800px" class="rounded z-depth-1 mt-3">
    <p>Your browser does not support inline PDF viewing. You can <a href="{{ '/assets/pdf/Machine_Unlearning_Report.pdf' | relative_url }}">click here to download the report</a>.</p>
</object>