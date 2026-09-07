---
layout: paper
title: "Language Diffusion Models are Associative Memories Capable of Retrieving Unseen Data"
permalink: /papers/language-diffusion-associative-memory/
description: "EMNLP 2026 (Main). Uniform-based discrete diffusion models behave as associative memories, and conditional entropy is a practical probe for their memorization-to-generalization transition."
keywords: "language diffusion models, discrete diffusion, associative memory, Hopfield networks, memorization, generalization, conditional entropy, EMNLP 2026"

authors:
  - name: Bao Pham
    url: /
  - name: Mohammed J. Zaki
    url: https://www.cs.rpi.edu/~zaki/
  - name: Luca Ambrogioni
    url: https://www.ru.nl/en/people/ambrogioni-l
  - name: Dmitry Krotov
    url: https://dmitrykrotov.com
  - name: Matteo Negri
    url: https://sites.google.com/view/matteonegriphysics

venue: "EMNLP 2026 (Main Conference)"

links:
  - name: Paper
    url: https://arxiv.org/pdf/2604.26841
    icon: fas fa-file-pdf
  - name: arXiv
    url: https://arxiv.org/abs/2604.26841
    icon: ai ai-arxiv
  - name: Code
    url: https://github.com/Lemon-cmd/Associative-Memory-and-Language-Diffusion
    icon: fab fa-github

teaser: /assets/img/teaser_imgs/tweet_fig_1.png
teaser_caption: "Corrupted tokens are fed to a uniform-based discrete diffusion model, which is then asked to recover them. When the training set is small, training examples are recovered token-for-token (memorization). As the training set grows, the basins around training examples shrink while basins around unseen test examples expand, until both converge to the same recovery level (generalization)."

abstract: "When do language diffusion models memorize their training data, and how to quantitatively assess their true generative regime? We address these questions by showing that Uniform-based Discrete Diffusion Models (UDDMs) fundamentally behave as Associative Memories with emergent creative capabilities. The core idea of an AM is to reliably recover stored data points as memories by establishing distinct basins of attraction around them. Historically, models like Hopfield networks use an explicit energy function to guarantee these stable attractors. We broaden this perspective by leveraging the observation that energy is not strictly necessary, as basins of attraction can also be formed via conditional likelihood maximization. By evaluating token recovery of training and test examples, we identify in UDDMs a sharp memorization-to-generalization transition governed by the size of the training dataset: as it increases, basins around training examples shrink and basins around unseen test examples expand, until both later converge to the same level. Crucially, we can detect this transition using only the conditional entropy of predicted token sequences: memorization is characterized by vanishing conditional entropy, while in the generalization regime the conditional entropy of most tokens remains finite. Thus, conditional entropy offers a practical probe for the memorization-to-generalization transition in deployed models."

bibtex: |
  @article{pham2026language,
    title={Language Diffusion Models are Associative Memories Capable of Retrieving Unseen Data},
    author={Pham, Bao and Zaki, Mohammed J and Ambrogioni, Luca and Krotov, Dmitry and Negri, Matteo},
    journal={arXiv preprint arXiv:2604.26841},
    year={2026}
  }
---

<h2 class="title is-3">Highlights</h2>

- **Language diffusion models are associative memories.** Uniform-based discrete diffusion models (UDDMs) carve out basins of attraction around data points, recovering them from corrupted token sequences the way a Hopfield network recovers a stored pattern.
- **Energy is not required for attractors.** Stable attractors need not come from an explicit energy function; conditional likelihood maximization is enough to form basins of attraction.
- **A sharp memorization-to-generalization transition, controlled by dataset size.** As the training set grows, basins around training examples shrink and basins around *unseen test* examples expand, until training and test recovery converge — the model retrieves data it was never trained on.
- **Conditional entropy is a practical probe.** Memorization shows up as vanishing conditional entropy over predicted tokens, while generalization leaves the conditional entropy of most tokens finite. This needs no access to the training set, so it can be applied to deployed models.

<h2 class="title is-3" style="margin-top: 2.5rem;">No Need For Energy: The Product of Pseudo-likelihood</h2>

Classical Associative Memories, and even their modern variants, rely on well-defined and explicit energy functions to define their attractors. Specifically, Hopfield networks and Dense Associative Memories define a functional $$E_\theta(x)$$ whose local minima sit at the stored patterns. Because the retrieval dynamics descend $$E_\theta$$, and $$E_\theta$$ is bounded below and non-increasing along the trajectory, convergence to a fixed point is guaranteed. The energy is what certifies that the basins exist. However, in the discrete setting, we can no longer rely on energy functions because things are no longer continuously differentiable. In our paper, we show that in the simplest setting of pseudo-likelihood, involving spins, a simple model trained with this objective shows the same behaviour as a language diffusion model when it comes to token recovery.

<figure class="image" style="margin: 2rem 0 0 0;">
  <img src="/assets/img/teaser_imgs/energy_tweet.png"
       alt="Basins of attraction can be established via conditional likelihood maximization without relying on an explicit and well-defined energy function"
       style="border-radius: 10px;">
  <figcaption class="has-text-centered is-size-6" style="margin-top: 1rem; color: #4a4a4a;">
    Maximizing the conditional likelihood of a spin given the rest of the sequence yields a Hebbian
    update modulated by a margin-dependent penalty. Because stability no longer rests on descending an
    energy, the weight matrix need not be symmetric for stored patterns to remain stable — basins of
    attraction still form around the data points.
  </figcaption>
</figure>


<h2 class="title is-3" style="margin-top: 2.5rem;">Associative Memory Metric Aligns with Standard Metric</h2>

Token recovery is the metric native to Associative Memory. Corrupt a sequence, let the model settle, and count how much of the original comes back; what this measures is the width of the basin around that sequence. Perplexity is the metric the language modelling community already reports. The two come from different traditions and measure different things, yet they mark the same event. The peak in perplexity (top row, coloured dashed line) falls at the dataset size where the recovery curves part ways (bottom row, grey dashed line): training recovery leaves its plateau at 100% while test recovery begins to climb. The takeaway is that the Associative Memory picture is not an interpretation layered on top of language diffusion models, but something already visible in a quantity these models are routinely evaluated on.

<figure class="image" style="margin: 2rem 0 0 0;">
  <img src="/assets/img/teaser_imgs/perplexity_combined.png"
       alt="Alignment of Perplexity and Token Recovery"
       style="border-radius: 10px;">
  <figcaption class="has-text-centered is-size-6" style="margin-top: 1rem; color: #4a4a4a;">
    Alignment of Perplexity and Token Recovery Rate. During memorization, perplexity starts low and increases up to a point, where this point denotes the phase transition from memorization to generalization. As generalization is becoming dominant, perplexity decreases while the token recovery rate on training samples falls and the recovery rate of test samples increases.
  </figcaption>
</figure>

<h2 class="title is-3" style="margin-top: 2.5rem;">Conditional Entropy Reveals Stable Tokens and Unstable Tokens</h2>

Token recovery tells us whether a basin exists, but measuring it requires the original data, since we have to know what the model was supposed to produce. Conditional entropy asks something far cheaper: how peaked is the model's own predictive distribution over each token? As it turns out, the two questions have the same answer. Tokens that the model recovers sit at vanishing conditional entropy, the signature of a token pinned to the bottom of a basin, while the tokens it fails to recover spread across a broad band of finite entropy. Memorization is therefore visible in the entropy alone: a model that has stored its training data predicts that data with almost no uncertainty. Because this measurement needs nothing but the model's own predictions, it can be run on a deployed model whose training set we cannot inspect.

<figure class="image" style="margin: 2rem 0 0 0;">
  <img src="/assets/img/teaser_imgs/unrecovered_recovered_ce.png"
       alt="Token conditional entropy separates recovered from unrecovered tokens across model sizes"
       style="border-radius: 10px;">
  <figcaption class="has-text-centered is-size-6" style="margin-top: 1rem; color: #4a4a4a;">
    <strong>(A)</strong> A training example is perturbed (orange) and handed back to the model. Trained on a
    small dataset, the model recovers the original tokens exactly (red); trained on the large dataset, most of
    those same tokens are no longer recovered (blue), as the basin around this training example has shrunk.
    <strong>(B)</strong> Densities of token conditional entropy on the full training set, split by whether a
    token was recovered, for the Tiny (~24M), Small (~135M), and Medium (~384M) models. Recovered tokens pile
    up at vanishing conditional entropy, while unrecovered tokens spread across a broad band of finite entropy.
    Separating the two requires only the model's own predictions and no access to the training set, which is
    what makes conditional entropy a usable probe on deployed models.
  </figcaption>
</figure>

<h2 class="title is-3" style="margin-top: 2.5rem;">Related Work</h2>

This paper extends the memorization-to-generalization picture developed for continuous diffusion models in
[Memorization to Generalization: Emergence of Diffusion Models from Associative Memory](https://arxiv.org/abs/2505.21777)
to the discrete, token-level setting. For background on the associative memory tools used here, see
[Modern Methods in Associative Memory](https://arxiv.org/abs/2507.06211) and my
[blog post on energy-based modeling](/research/2025/12/15/ebm.html).
