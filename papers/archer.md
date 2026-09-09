---
layout: paper
title: "ARCHER: Amortized Cross-specimen Pose Estimation for Cryo-electron Microscopy"
year: 2026
permalink: /papers/archer/
description: "A reference-conditioned pose estimator for single-particle cryo-EM, trained once across 3,330 structures and applied zero-shot to specimens it has never seen."
keywords: "cryo-EM, cryo-electron microscopy, pose estimation, amortized inference, contrastive learning, SO(3), conformational heterogeneity, ARCHER"

authors:
  - name: Nhan D. Nguyen
  - name: Bao Pham
    url: /

links:
  - name: Paper
    url: https://arxiv.org/pdf/2608.22029v1
    icon: fas fa-file-pdf
  - name: arXiv
    url: https://arxiv.org/abs/2608.22029v1
    icon: ai ai-arxiv
  - name: Code
    url: https://github.com/ndnng/ARCHER
    icon: fab fa-github

teaser: /assets/img/teaser_imgs/archer_contrast.png
teaser_width: 80%
teaser_caption: "(A) Isosurfaces for five CESPED targets, one row each: the deposited reference, the ARCHER reconstruction, and that reconstruction after cryoFM restoration. (B) Experimental particles embedded by principal component analysis, coloured by dataset. The specimens occupy distinguishable regions, and the separation is structural rather than instrumental. (C) Cosines between restoration displacement vectors. A restorer moves every map in nearly the same direction, whichever estimator produced it."

abstract: "Single-particle cryo-electronic microscopy (cryo-EM) pose estimation is traditionally solved anew for each dataset, where iterative refinement is done from scratch while the estimator learns to store the molecule in its weights. In this work, we show that pose inference is a generalizable, specimen-agnostic operation when conditioned explicitly on a reference volume. We introduce ARCHER, an amortized contrastive classifier that models the pose posterior over a discrete rotation grid. Trained across a variety of protein structures, it operates zero-shot without retraining per structure. This transferability is grounded in Fourier-space information mechanics, where all specimen dependence is captured by the reference structure's power spectrum and spatial extent. ARCHER achieves a median angular error of 5.0° on 100 held-out test structures and 2.5° on experimental particles, matching dedicated estimators within 0.16 Å in 3D reconstruction. Crucially, downstream conformational signal is preserved. The leading conformational coordinate correlates at 0.97 with deposited benchmarks, faithfully reconstructing free-energy basins and mobile domains. These results overall demonstrate that cryo-EM pose estimation can be generalized across different structures."

bibtex: |
  @article{nguyen2026archer,
    title={ARCHER: Amortized cross-specimen pose estimation for cryo-electron microscopy},
    author={Nguyen, Nhan and Pham, Bao},
    journal={bioRxiv},
    pages={2026--08},
    year={2026},
    publisher={Cold Spring Harbor Laboratory}
  }
---

<h2 class="title is-3">Highlights</h2>

- **The reference is an argument, not a parameter.** Every pose estimator evaluates the posterior $$p(\mathbf{R} \mid y, V)$$. Existing methods absorb the volume $$V$$ into their weights, so every new specimen needs its own training run. Supply $$V$$ at inference instead, and what is left to learn is the match between a noisy projection and a candidate view under a known transfer function. That operation is set by the microscope, and is shared across specimens.
- **One set of weights, applied zero-shot.** Trained across $$3{,}330$$ structures, ARCHER reaches $$5.0^\circ$$ median angular error on $$100$$ held-out structures and $$2.5^\circ$$ on experimental particles, reconstructing within $$0.16$$ Å of estimators fit to the specimen itself.
- **A model fitted to one protein is at chance on the next.** A per-specimen estimator scores $$3.9^\circ$$ to $$8.8^\circ$$ on the CESPED target it was fitted to, and $$80^\circ$$ to $$135^\circ$$ on the others, which is chance level. Fitting those four separately cost $$90.5$$ single-GPU hours; ARCHER covers all four in one training run.
