---
layout: paper
title: "Energy Transformer"
permalink: /papers/energy-transformer/
description: "NeurIPS 2023. A transformer whose attention layers are designed to minimize an engineered energy function, bringing attention, energy-based models, and Dense Associative Memory into a single architecture."
keywords: "energy transformer, attention, energy-based models, Dense Associative Memory, modern Hopfield networks, image completion, graph anomaly detection, NeurIPS 2023"

authors:
  - name: Benjamin Hoover
    url: https://bhoov.com
    symbol: "*"
  - name: Yuchen Liang
    symbol: "*"
    url: https://scholar.google.com/citations?user=yVhSIBcAAAAJ&hl=en
  - name: Bao Pham
    url: /
    symbol: "*"
  - name: Rameswar Panda
    url: https://rpand002.github.io/
  - name: Hendrik Strobelt
    url: http://hendrik.strobelt.com
  - name: Duen Horng Chau
    url: https://faculty.cc.gatech.edu/~dchau/
  - name: Mohammed J. Zaki
    url: https://www.cs.rpi.edu/~zaki/
  - name: Dmitry Krotov
    url: https://dmitrykrotov.com

venue: "NeurIPS 2023"
note: "<sup>*</sup>Indicates Equal Contribution"

links:
  - name: Paper
    url: https://arxiv.org/pdf/2302.07253
    icon: fas fa-file-pdf
  - name: arXiv
    url: https://arxiv.org/abs/2302.07253
    icon: ai ai-arxiv
  - name: Code (JAX)
    url: https://github.com/bhoov/energy-transformer-jax
    icon: fab fa-github
  - name: Code (PyTorch)
    url: https://github.com/Lemon-cmd/energy-transformer-torch
    icon: fab fa-github
  - name: NeurIPS
    url: https://neurips.cc/virtual/2023/poster/71901
    icon: fas fa-chalkboard-user
  - name: Talk
    url: https://www.youtube.com/watch?v=5LXiQUsnHrI
    icon: fab fa-youtube

teaser: /assets/img/teaser_imgs/energy_transformer.png
teaser_caption: "A single Energy Transformer block is applied recurrently, and each pass updates the token representations by descending the engineered energy, so the forward pass is an optimization rather than a stack of distinct layers. The energy decreases monotonically until the tokens settle at a fixed point. The same block handles graphs (top) and image completion (bottom), where masked patches are filled in as the energy is minimized."

abstract: "Our work combines aspects of three promising paradigms in machine learning, namely, attention mechanism, energy-based models, and associative memory. Attention is the power-house driving modern deep learning successes, but it lacks clear theoretical foundations. Energy-based models allow a principled approach to discriminative and generative tasks, but the design of the energy functional is not straightforward. At the same time, Dense Associative Memory models or Modern Hopfield Networks have a well-established theoretical foundation, and allow an intuitive design of the energy function. We propose a novel architecture, called the Energy Transformer (or ET for short), that uses a sequence of attention layers that are purposely designed to minimize a specifically engineered energy function, which is responsible for representing the relationships between the tokens. In this work, we introduce the theoretical foundations of ET, explore its empirical capabilities using the image completion task, and obtain strong quantitative results on the graph anomaly detection and graph classification tasks."

bibtex: |
  @inproceedings{hoover2023energy,
    title={Energy Transformer},
    author={Hoover, Benjamin and Liang, Yuchen and Pham, Bao and Panda, Rameswar and Strobelt, Hendrik and Chau, Duen Horng and Zaki, Mohammed J and Krotov, Dmitry},
    booktitle={Advances in Neural Information Processing Systems (NeurIPS)},
    year={2023}
  }
---

<h2 class="title is-3">Highlights</h2>

- **Attention that descends an energy.** ET's attention layers are not designed first and explained later. They are derived as the gradient of a specifically engineered energy function, so a forward pass through the network is literally energy minimization.
- **One block, applied recurrently.** Rather than stacking distinct layers, ET applies a single block repeatedly, and the token representations follow the update $$\mathbf{x}^{t+1} = \mathbf{x}^{t} - \alpha \nabla_{\mathbf{g}} E^{t}$$ until the energy converges.
- **An interpretable system that converges.** Every update is the gradient of a scalar energy, so each module's contribution can be decoded back into the input space and inspected at any step. Because the energy is non-increasing along the trajectory, those updates shrink and the tokens settle at a fixed point, rather than merely stopping after a fixed depth.
- **A bridge between three traditions.** Attention supplies the empirical power, energy-based models supply the principled objective, and Dense Associative Memory supplies a well-founded recipe for designing the energy itself.
- **Works across modalities.** The same architecture completes masked image patches and delivers strong quantitative results on graph anomaly detection and graph classification.

<h2 class="title is-3" style="margin-top: 2.5rem;">Attention as the Gradient of an Energy</h2>

A single block holds two modules that read the same layer-normalized tokens. Energy Attention contributes a term that is low when queries align with keys, so attention here is not a hand-designed operation but a summand in the energy. The Hopfield Network contributes a second term that is low when tokens resemble the stored memory vectors, which is what makes the tokens behave like memories. Their sum, $$E = E^{\text{ATT}} + E^{\text{HN}}$$, is the entire objective, and the block updates the tokens by descending its gradient, either in continuous time or in discrete steps of size $$\alpha$$. Because both terms are differentiable scalars, the resulting implementation is short: the update is nothing more than automatic differentiation of the energy.

<figure class="image" style="margin: 2rem 0 0 0;">
  <img src="/assets/img/teaser_imgs/et_general.png"
       alt="The Energy Transformer block, its two energy terms, and the gradient descent update on their sum"
       style="border-radius: 10px;">
  <figcaption class="has-text-centered is-size-6" style="margin-top: 1rem; color: #4a4a4a;">
    <strong>Top.</strong> Layer-normalized tokens are read by two modules. Energy Attention makes queries align
    with keys in the latent space, and the Hopfield Network makes tokens look like memories. The Energy
    Transformer minimizes the sum of the two energies. <strong>Bottom.</strong> Each token descends the total
    energy gradient, either as a continuous flow or in discrete steps of size &alpha;. Since the energy is a
    differentiable scalar, an implementation is a few lines of autodiff.
  </figcaption>
</figure>

<h2 class="title is-3" style="margin-top: 2.5rem;">An Interpretable System That Converges</h2>

Because every update is the gradient of an energy, the contribution of each module can be decoded back into the input space and simply looked at. The figure follows one image completion across the descent, where at each step the token state receives a Hopfield Network update and an Energy Attention update, and each can be rendered on its own. Early in the descent the two updates are large and coarse as they rough in the masked patches. Late in the descent they have shrunk and the reconstruction has stopped changing, which is what convergence looks like from the outside: the energy has flattened and the tokens have settled at a fixed point.

<figure class="image" style="margin: 2rem 0 0 0;">
  <img src="/assets/img/teaser_imgs/interp_thru_time.png"
       alt="Hopfield Network and Energy Attention updates decoded into image space at an early and a late step of the descent"
       style="border-radius: 10px;">
  <figcaption class="has-text-centered is-size-6" style="margin-top: 1rem; color: #4a4a4a;">
    One image completion viewed across the descent. At every step the token state is updated by the sum of the
    Hopfield Network update and the Energy Attention update, and each contribution can be decoded back into
    image space on its own. At an early time <em>s</em> the updates are large and coarse; by a late time
    <em>t</em> they have shrunk and the reconstruction has stopped changing, as the tokens settle into a fixed
    point of the energy.
  </figcaption>
</figure>

<h2 class="title is-3" style="margin-top: 2.5rem;">Related Work</h2>

The Associative Memory foundations that ET is built on are surveyed in
[Modern Methods in Associative Memory](https://arxiv.org/abs/2507.06211), and the same energy-based
perspective is applied to generative models in
[Memorization to Generalization: Emergence of Diffusion Models from Associative Memory](https://arxiv.org/abs/2505.21777)
and [Language Diffusion Models are Associative Memories Capable of Retrieving Unseen Data](/papers/language-diffusion-associative-memory/).
For background, see my [blog post on energy-based modeling](/research/2025/12/15/ebm.html).
