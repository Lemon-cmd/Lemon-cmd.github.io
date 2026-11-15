---
layout: paper
title: "Emergence of Diffusion Models from Associative Memory through Memorization and Generalization"
year: 2026
permalink: /papers/memorization-to-generalization/
description: "Nature Communications 2026. Diffusion models viewed as Dense Associative Memories, where the spurious states that appear between memorization and generalization turn out to be the first sign of generative capability."
keywords: "diffusion models, Dense Associative Memory, Hopfield networks, spurious states, memorization, generalization, energy landscape, Nature Communications"

authors:
  - name: Bao Pham
    url: /
    symbol: "*"
  - name: Gabriel Raya
    symbol: "*"
  - name: Matteo Negri
    url: https://sites.google.com/view/matteonegriphysics
  - name: Mohammed J. Zaki
    url: https://www.cs.rpi.edu/~zaki/
  - name: Luca Ambrogioni
    url: https://www.ru.nl/en/people/ambrogioni-l
  - name: Dmitry Krotov
    url: https://dmitrykrotov.com

venue: "Nature Communications, 2026"
note: "<sup>*</sup>Indicates Equal Contribution"

links:
  - name: Paper
    url: https://www.nature.com/articles/s41467-026-78727-y
    icon: fas fa-file-pdf
  - name: arXiv
    url: https://arxiv.org/abs/2505.21777
    icon: ai ai-arxiv
  - name: Code
    url: https://github.com/Lemon-cmd/Diffusion-Models-and-Associative-Memory
    icon: fab fa-github
  - name: Slides
    url: https://sites.google.com/view/aspen2026/talks-slides
    icon: fas fa-chalkboard-user
  - name: NeurIPS 2024
    url: https://neurips.cc/virtual/2024/107925
    icon: fas fa-video
  - name: ICLR 2025
    url: https://iclr.cc/virtual/2025/33414
    icon: fas fa-video

teaser: /assets/img/teaser_imgs/energy_teaser.png
teaser_caption: "Generation viewed as descent on a Dense Associative Memory energy landscape. With little training data every minimum sits on a training example, and retrieval returns that example (memorization). As the training set grows, new minima appear between data points, which is the spurious phase. With more data still, the landscape flattens into broad basins whose minima are novel samples rather than stored ones (generalization)."

abstract: "Dense Associative Memories (DenseAMs) are generalizations of Hopfield networks that store training data at local minima of the energy landscape. When the amount of training data exceeds the critical memory storage capacity, new local minima (different from the training data) emerge. These emergent local minima, known as spurious states, can hinder memory retrieval. We examine diffusion models (DMs) through the DenseAM lens, viewing their generative process as an attempt of a memory retrieval. In the small data regime, DMs create distinct attractors for each training sample, akin to DenseAMs below the critical capacity. As the training set size increases, DMs transition to generalization. We identify a critical intermediate phase, predicted by DenseAM theory, characterized by the emergence of spurious states. In generative modeling, these previously overlooked states are no longer negative artifacts but rather are the first signs of generative capabilities. Their existence is demonstrated across a wide range of architectures and datasets."

bibtex: |
  @article{pham2026emergence,
    title={Emergence of diffusion models from associative memory through memorization and generalization},
    author={Pham, Bao and Raya, Gabriel and Negri, Matteo and Zaki, Mohammed J. and Ambrogioni, Luca and Krotov, Dmitry},
    journal={Nature Communications},
    year={2026},
    doi={10.1038/s41467-026-78727-y},
    url={https://www.nature.com/articles/s41467-026-78727-y}
  }
---

<h2 class="title is-3">Highlights</h2>

- **Generation as memory retrieval.** A Dense Associative Memory stores training data at local minima of its energy landscape. Reading a diffusion model the same way makes its generative process an attempt at retrieval, so the geometry of the landscape, rather than the architecture, explains what the model returns.
- **Spurious states are not a failure mode.** Classical associative memory treats the minima that appear past the critical storage capacity as artifacts that corrupt retrieval. In a generative model they are the opposite: the intermediate phase they define is the first point at which the model produces something that is not a stored training example.
- **One transition, controlled by dataset size.** Below the critical capacity, each training sample gets its own attractor and the model memorizes. Above it, the spurious phase appears, and beyond that the landscape flattens into basins that correspond to generalization. The phase is predicted by DenseAM theory and observed across a wide range of architectures and datasets.

<h2 class="title is-3" style="margin-top: 2.5rem;">Related Work</h2>

The discrete, token-level counterpart of this picture is developed in
[Language Diffusion Models are Associative Memories Capable of Retrieving Unseen Data](/papers/language-diffusion-associative-memory/).
For the associative memory tools used here, see
[Modern Methods in Associative Memory](https://arxiv.org/abs/2507.06211) and
[Energy Transformer](/papers/energy-transformer/), or my
[blog post on energy-based modeling](/research/2025/12/15/ebm.html).
