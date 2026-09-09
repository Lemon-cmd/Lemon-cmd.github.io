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
  @inproceedings{HooverET23,
    author = {Hoover, Benjamin and Liang, Yuchen and Pham, Bao and Panda, Rameswar and Strobelt Hendrik and Chau, Duen Horng and Zaki, Mohammed and Krotov, Dmitry},
    booktitle = {Advances in Neural Information Processing Systems},
    doi = {10.52202/075280-1197},
    editor = {A. Oh and T. Naumann and A. Globerson and K. Saenko and M. Hardt and S. Levine},
    pages = {27532--27559},
    publisher = {Curran Associates, Inc.},
    title = {Energy Transformer},
    url = {https://proceedings.neurips.cc/paper_files/paper/2023/file/57a9b97477b67936298489e3c1417b0a-Paper-Conference.pdf},
    volume = {36},
    year = {2023}
  }
---

<h2 class="title is-3">Highlights</h2>

- **Attention that descends an energy.** ET's attention layers are not designed first and explained later. They are derived as the gradient of a specifically engineered energy function, so a forward pass through the network is literally energy minimization.
- **One block, applied recurrently.** Rather than stacking distinct layers, ET applies a single block repeatedly, and the token representations follow the update $$\mathbf{x}^{t+1} = \mathbf{x}^{t} - \alpha \nabla_{\mathbf{g}} E^{t}$$ until the energy converges.
- **An interpretable system that converges.** Every update is the gradient of a scalar energy, so each module's contribution can be decoded back into the input space and inspected at any step. Because the energy is non-increasing along the trajectory, those updates shrink and the tokens settle at a fixed point, rather than merely stopping after a fixed depth.
- **A bridge between three traditions.** Attention supplies the empirical power, energy-based models supply the principled objective, and Dense Associative Memory supplies a well-founded recipe for designing the energy itself.
- **Works across modalities.** The same architecture completes masked image patches and delivers strong quantitative results on graph anomaly detection and graph classification.

<h2 class="title is-3" style="margin-top: 2.5rem;">Attention as the Gradient of an Energy</h2>

A single block holds two modules that read the same layer-normalized tokens. Energy Attention contributes a term that is low when queries align with keys, so attention here is not a hand-designed operation but a sum in the energy across all attention heads and positions. The Hopfield Network contributes a second term that is low when tokens resemble the stored memory vectors, which is what makes the tokens behave like memories. Their sum, $$E = E^{\text{ATT}} + E^{\text{HN}}$$, is the entire objective, and the block updates the tokens by descending its gradient, either in continuous time or in discrete steps of size $$\alpha$$. Because both terms are differentiable scalars, the resulting implementation is short: the update is nothing more than automatic differentiation of the energy.

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


<h3 class="title is-3" style="margin-top: 2rem;">Node Anomaly Detection</h3>

<div class="table-container">
<table class="table is-narrow is-bordered is-fullwidth" style="font-size: 0.82rem; white-space: nowrap;">
<thead><tr><th></th><th>Dataset</th><th>Split</th><th>GraphConsis</th><th>CAREGNN</th><th>PC-GNN</th><th>BWGNN</th><th>MLP</th><th>GT</th><th style="background-color: rgba(50,115,220,0.07);">ET (Ours)</th></tr></thead>
<tbody>
<tr><th rowspan="8" style="vertical-align: middle; text-align: center;">Macro-F1</th><td rowspan="2" style="vertical-align: middle;">Yelp</td><td>1%</td><td>56.8<span style="color:#8a8a8a; font-size:0.85em;">±2.8</span></td><td>62.1<span style="color:#8a8a8a; font-size:0.85em;">±1.3</span></td><td>59.8<span style="color:#8a8a8a; font-size:0.85em;">±1.4</span></td><td>61.1<span style="color:#8a8a8a; font-size:0.85em;">±0.4</span></td><td>53.9<span style="color:#8a8a8a; font-size:0.85em;">±0.2</span></td><td>61.7<span style="color:#8a8a8a; font-size:0.85em;">±0.4</span></td><td style="background-color: rgba(50,115,220,0.07);"><strong>63.0</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.6</span></td></tr>
<tr><td>40%</td><td>58.7<span style="color:#8a8a8a; font-size:0.85em;">±2.0</span></td><td>63.3<span style="color:#8a8a8a; font-size:0.85em;">±0.9</span></td><td>63.0<span style="color:#8a8a8a; font-size:0.85em;">±2.3</span></td><td>71.0<span style="color:#8a8a8a; font-size:0.85em;">±0.9</span></td><td>57.5<span style="color:#8a8a8a; font-size:0.85em;">±0.8</span></td><td>68.7<span style="color:#8a8a8a; font-size:0.85em;">±0.4</span></td><td style="background-color: rgba(50,115,220,0.07);"><strong>71.5</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.1</span></td></tr>
<tr><td rowspan="2" style="vertical-align: middle;">Amazon</td><td>1%</td><td>68.5<span style="color:#8a8a8a; font-size:0.85em;">±3.4</span></td><td>68.7<span style="color:#8a8a8a; font-size:0.85em;">±1.6</span></td><td>79.8<span style="color:#8a8a8a; font-size:0.85em;">±5.6</span></td><td><strong>90.9</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.7</span></td><td>74.6<span style="color:#8a8a8a; font-size:0.85em;">±1.2</span></td><td>88.6<span style="color:#8a8a8a; font-size:0.85em;">±0.5</span></td><td style="background-color: rgba(50,115,220,0.07);">89.3<span style="color:#8a8a8a; font-size:0.85em;">±0.7</span></td></tr>
<tr><td>40%</td><td>75.1<span style="color:#8a8a8a; font-size:0.85em;">±3.2</span></td><td>86.3<span style="color:#8a8a8a; font-size:0.85em;">±1.7</span></td><td>89.5<span style="color:#8a8a8a; font-size:0.85em;">±0.7</span></td><td>92.2<span style="color:#8a8a8a; font-size:0.85em;">±0.4</span></td><td>79.1<span style="color:#8a8a8a; font-size:0.85em;">±1.2</span></td><td>91.7<span style="color:#8a8a8a; font-size:0.85em;">±0.8</span></td><td style="background-color: rgba(50,115,220,0.07);"><strong>92.8</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.3</span></td></tr>
<tr><td rowspan="2" style="vertical-align: middle;">T-Finance</td><td>1%</td><td>71.7</td><td>73.3</td><td>62.0</td><td>84.8</td><td>61.0</td><td>81.5</td><td style="background-color: rgba(50,115,220,0.07);"><strong>85.1</strong><span style="color:#8a8a8a; font-size:0.85em;">±1.0</span></td></tr>
<tr><td>40%</td><td>73.4</td><td>77.5</td><td>63.1</td><td>86.8</td><td>70.5</td><td>83.6</td><td style="background-color: rgba(50,115,220,0.07);"><strong>88.2</strong><span style="color:#8a8a8a; font-size:0.85em;">±1.0</span></td></tr>
<tr><td rowspan="2" style="vertical-align: middle;">T-Social</td><td>1%</td><td>52.4</td><td>55.8</td><td>51.1</td><td>75.9</td><td>50.0</td><td>64.3</td><td style="background-color: rgba(50,115,220,0.07);"><strong>79.1</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.7</span></td></tr>
<tr><td>40%</td><td>56.5</td><td>56.2</td><td>52.1</td><td><strong>83.9</strong></td><td>50.3</td><td>68.2</td><td style="background-color: rgba(50,115,220,0.07);">83.5<span style="color:#8a8a8a; font-size:0.85em;">±0.4</span></td></tr>
<tr><th rowspan="8" style="vertical-align: middle; text-align: center;">AUC</th><td rowspan="2" style="vertical-align: middle;">Yelp</td><td>1%</td><td>66.4<span style="color:#8a8a8a; font-size:0.85em;">±3.4</span></td><td>75.0<span style="color:#8a8a8a; font-size:0.85em;">±3.8</span></td><td><strong>75.4</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.9</span></td><td>72.0<span style="color:#8a8a8a; font-size:0.85em;">±0.5</span></td><td>59.8<span style="color:#8a8a8a; font-size:0.85em;">±0.4</span></td><td>72.5<span style="color:#8a8a8a; font-size:0.85em;">±0.6</span></td><td style="background-color: rgba(50,115,220,0.07);">73.2<span style="color:#8a8a8a; font-size:0.85em;">±0.8</span></td></tr>
<tr><td>40%</td><td>69.8<span style="color:#8a8a8a; font-size:0.85em;">±3.0</span></td><td>76.1<span style="color:#8a8a8a; font-size:0.85em;">±2.9</span></td><td>79.8<span style="color:#8a8a8a; font-size:0.85em;">±0.1</span></td><td>84.0<span style="color:#8a8a8a; font-size:0.85em;">±0.9</span></td><td>66.5<span style="color:#8a8a8a; font-size:0.85em;">±1.0</span></td><td>81.9<span style="color:#8a8a8a; font-size:0.85em;">±0.5</span></td><td style="background-color: rgba(50,115,220,0.07);"><strong>84.9</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.3</span></td></tr>
<tr><td rowspan="2" style="vertical-align: middle;">Amazon</td><td>1%</td><td>74.1<span style="color:#8a8a8a; font-size:0.85em;">±3.5</span></td><td>88.6<span style="color:#8a8a8a; font-size:0.85em;">±3.5</span></td><td>90.4<span style="color:#8a8a8a; font-size:0.85em;">±2.0</span></td><td>89.4<span style="color:#8a8a8a; font-size:0.85em;">±0.3</span></td><td>83.6<span style="color:#8a8a8a; font-size:0.85em;">±1.7</span></td><td>89.0<span style="color:#8a8a8a; font-size:0.85em;">±1.2</span></td><td style="background-color: rgba(50,115,220,0.07);"><strong>91.9</strong><span style="color:#8a8a8a; font-size:0.85em;">±1.0</span></td></tr>
<tr><td>40%</td><td>87.4<span style="color:#8a8a8a; font-size:0.85em;">±3.3</span></td><td>90.5<span style="color:#8a8a8a; font-size:0.85em;">±1.6</span></td><td>95.8<span style="color:#8a8a8a; font-size:0.85em;">±0.1</span></td><td><strong>98.0</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.4</span></td><td>89.8<span style="color:#8a8a8a; font-size:0.85em;">±1.0</span></td><td>95.4<span style="color:#8a8a8a; font-size:0.85em;">±0.6</span></td><td style="background-color: rgba(50,115,220,0.07);">97.3<span style="color:#8a8a8a; font-size:0.85em;">±0.4</span></td></tr>
<tr><td rowspan="2" style="vertical-align: middle;">T-Finance</td><td>1%</td><td>90.2</td><td>90.5</td><td>90.7</td><td>91.1</td><td>82.9</td><td>90.0</td><td style="background-color: rgba(50,115,220,0.07);"><strong>92.8</strong><span style="color:#8a8a8a; font-size:0.85em;">±1.1</span></td></tr>
<tr><td>40%</td><td>91.4</td><td>92.1</td><td>91.2</td><td>94.3</td><td>87.1</td><td>88.2</td><td style="background-color: rgba(50,115,220,0.07);"><strong>95.0</strong><span style="color:#8a8a8a; font-size:0.85em;">±3.0</span></td></tr>
<tr><td rowspan="2" style="vertical-align: middle;">T-Social</td><td>1%</td><td>65.2</td><td>71.2</td><td>59.8</td><td>88.0</td><td>56.3</td><td>81.4</td><td style="background-color: rgba(50,115,220,0.07);"><strong>91.9</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.6</span></td></tr>
<tr><td>40%</td><td>71.2</td><td>71.8</td><td>68.4</td><td><strong>95.2</strong></td><td>56.9</td><td>82.5</td><td style="background-color: rgba(50,115,220,0.07);">93.9<span style="color:#8a8a8a; font-size:0.85em;">±0.2</span></td></tr>
</tbody></table></div>
<p class="is-size-6" style="color:#4a4a4a; margin-top:-0.5rem;">Mean and standard deviation over 5 runs with different train/dev/test splits, following the protocol of Tang et al. Standard deviations are shown only where prior work reports them. <strong>Bold</strong> marks the best result in each row.</p>
<p class="is-size-7" style="color:#7a7a7a; margin-top:-0.75rem;"><strong>Baselines.</strong> GraphConsis (Liu et al., SIGIR 2020) &middot; CARE-GNN (Dou et al., CIKM 2020) &middot; PC-GNN (Liu et al., WWW 2021) &middot; BWGNN (<a href="https://arxiv.org/abs/2205.15508" target="_blank">Tang et al., 2022</a>) &middot; GT (<a href="https://arxiv.org/abs/2012.09699" target="_blank">Dwivedi &amp; Bresson, 2020</a>). MLP is a plain multilayer perceptron with no graph structure.</p>

<h3 class="title is-3" style="margin-top: 2.5rem;">Graph Classification</h3>

<div class="table-container">
<table class="table is-narrow is-bordered is-fullwidth" style="font-size: 0.82rem; white-space: nowrap;">
<thead><tr><th>Method</th><th>PROTEINS</th><th>NCI1</th><th>NCI109</th><th>DD</th><th>ENZYMES</th><th>MUTAG</th><th>MUTAGENICITY</th><th>FRANKENSTEIN</th></tr></thead>
<tbody>
<tr><th>WKPI (kmeans)</th><td>78.5<span style="color:#8a8a8a; font-size:0.85em;">±0.4</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;6.4</span></td><td style="background-color: #ededed;"><strong>87.5</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.5</span></td><td>85.9<span style="color:#8a8a8a; font-size:0.85em;">±0.4</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;1.5</span></td><td>82.0<span style="color:#8a8a8a; font-size:0.85em;">±0.5</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;13.7</span></td><td style="color:#b5b5b5;">&ndash;</td><td>85.8<span style="color:#8a8a8a; font-size:0.85em;">±2.5</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;14.2</span></td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td></tr>
<tr><th>WKPI (kcenters)</th><td>75.2<span style="color:#8a8a8a; font-size:0.85em;">±0.4</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;9.7</span></td><td>84.5<span style="color:#8a8a8a; font-size:0.85em;">±0.5</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;3.0</span></td><td style="background-color: #ededed;"><strong>87.4</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.3</span></td><td>80.3<span style="color:#8a8a8a; font-size:0.85em;">±0.4</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;15.4</span></td><td style="color:#b5b5b5;">&ndash;</td><td>88.3<span style="color:#8a8a8a; font-size:0.85em;">±2.6</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;11.7</span></td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td></tr>
<tr><th>Spec-GN</th><td style="color:#b5b5b5;">&ndash;</td><td>84.8<span style="color:#8a8a8a; font-size:0.85em;">±1.6</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;2.7</span></td><td>83.6<span style="color:#8a8a8a; font-size:0.85em;">±0.8</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;3.8</span></td><td style="color:#b5b5b5;">&ndash;</td><td>72.5<span style="color:#8a8a8a; font-size:0.85em;">±5.8</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;5.9</span></td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td></tr>
<tr><th>Norm-GN</th><td style="color:#b5b5b5;">&ndash;</td><td>84.9<span style="color:#8a8a8a; font-size:0.85em;">±1.7</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;2.6</span></td><td>83.5<span style="color:#8a8a8a; font-size:0.85em;">±1.3</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;3.9</span></td><td style="color:#b5b5b5;">&ndash;</td><td>73.3<span style="color:#8a8a8a; font-size:0.85em;">±8.0</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;5.1</span></td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td></tr>
<tr><th>GWL-WL</th><td>75.8<span style="color:#8a8a8a; font-size:0.85em;">±0.6</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;9.1</span></td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td>71.3<span style="color:#8a8a8a; font-size:0.85em;">±1.1</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;7.1</span></td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td style="background-color: #ededed;"><strong>78.9</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.3</span></td></tr>
<tr><th>HGP-SL</th><td style="background-color: #ededed;"><strong>84.9</strong><span style="color:#8a8a8a; font-size:0.85em;">±1.6</span></td><td>78.5<span style="color:#8a8a8a; font-size:0.85em;">±0.8</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;9.1</span></td><td>80.7<span style="color:#8a8a8a; font-size:0.85em;">±1.2</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;6.7</span></td><td>81.0<span style="color:#8a8a8a; font-size:0.85em;">±1.3</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;14.7</span></td><td>68.8<span style="color:#8a8a8a; font-size:0.85em;">±2.1</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;9.6</span></td><td style="color:#b5b5b5;">&ndash;</td><td style="background-color: #ededed;"><strong>82.2</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.6</span></td><td style="color:#b5b5b5;">&ndash;</td></tr>
<tr><th>DSGCN</th><td>77.3<span style="color:#8a8a8a; font-size:0.85em;">±0.4</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;7.6</span></td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td style="background-color: #ededed;"><strong>78.4</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.6</span></td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td></tr>
<tr><th>U2GNN</th><td>80.0<span style="color:#8a8a8a; font-size:0.85em;">±3.2</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;4.9</span></td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td style="background-color: #ededed;"><strong>95.7</strong><span style="color:#8a8a8a; font-size:0.85em;">±1.9</span></td><td style="color:#b5b5b5;">&ndash;</td><td>88.5<span style="color:#8a8a8a; font-size:0.85em;">±7.1</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;11.5</span></td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td></tr>
<tr><th>NDP</th><td>73.4<span style="color:#8a8a8a; font-size:0.85em;">±3.1</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;11.5</span></td><td>74.2<span style="color:#8a8a8a; font-size:0.85em;">±1.7</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;13.3</span></td><td style="color:#b5b5b5;">&ndash;</td><td>72.8<span style="color:#8a8a8a; font-size:0.85em;">±5.4</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;22.9</span></td><td>44.5<span style="color:#8a8a8a; font-size:0.85em;">±7.4</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;34.9</span></td><td>87.9<span style="color:#8a8a8a; font-size:0.85em;">±5.7</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;12.1</span></td><td>77.9<span style="color:#8a8a8a; font-size:0.85em;">±1.4</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;4.3</span></td><td style="color:#b5b5b5;">&ndash;</td></tr>
<tr><th>ASAP</th><td>74.2<span style="color:#8a8a8a; font-size:0.85em;">±0.8</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;10.7</span></td><td>71.5<span style="color:#8a8a8a; font-size:0.85em;">±0.4</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;16.0</span></td><td>70.1<span style="color:#8a8a8a; font-size:0.85em;">±0.6</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;17.3</span></td><td>76.9<span style="color:#8a8a8a; font-size:0.85em;">±0.7</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;18.8</span></td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td>66.3<span style="color:#8a8a8a; font-size:0.85em;">±0.5</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;12.6</span></td></tr>
<tr><th>EvoG</th><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td><td>55.7<br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;22.7</span></td><td style="background-color: #ededed;"><strong>100.0</strong></td><td style="color:#b5b5b5;">&ndash;</td><td style="color:#b5b5b5;">&ndash;</td></tr>
<tr style="background-color: rgba(50,115,220,0.07);"><th>ET (Ours)</th><td style="background-color: rgba(50,115,220,0.07);"><strong>90.3</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.7</span><br><span style="color:#2160c4; font-size:0.8em; white-space:nowrap;">&#9650;&thinsp;5.4</span></td><td style="background-color: rgba(50,115,220,0.07);"><strong>90.1</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.1</span><br><span style="color:#2160c4; font-size:0.8em; white-space:nowrap;">&#9650;&thinsp;2.6</span></td><td style="background-color: rgba(50,115,220,0.07);"><strong>90.5</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.1</span><br><span style="color:#2160c4; font-size:0.8em; white-space:nowrap;">&#9650;&thinsp;3.1</span></td><td style="background-color: rgba(50,115,220,0.07);"><strong>95.9</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.8</span><br><span style="color:#2160c4; font-size:0.8em; white-space:nowrap;">&#9650;&thinsp;0.2</span></td><td style="background-color: rgba(50,115,220,0.07);"><strong>99.8</strong><br><span style="color:#2160c4; font-size:0.8em; white-space:nowrap;">&#9650;&thinsp;21.4</span></td><td style="background-color: rgba(50,115,220,0.07);"><strong>96.6</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.2</span><br><span style="color:#c0392b; font-size:0.8em; white-space:nowrap;">&#9660;&thinsp;3.4</span></td><td style="background-color: rgba(50,115,220,0.07);"><strong>98.7</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.1</span><br><span style="color:#2160c4; font-size:0.8em; white-space:nowrap;">&#9650;&thinsp;16.5</span></td><td style="background-color: rgba(50,115,220,0.07);"><strong>99.8</strong><span style="color:#8a8a8a; font-size:0.85em;">±0.1</span><br><span style="color:#2160c4; font-size:0.8em; white-space:nowrap;">&#9650;&thinsp;20.9</span></td></tr>
</tbody></table></div>
<p class="is-size-6" style="color:#4a4a4a; margin-top:-0.5rem;">Mean and standard deviation from 100 runs of 10-fold cross validation, following the TUDataset protocol. Entries unavailable in prior work are marked &ndash;. The <span style="background-color:#ededed; padding:0 4px;">shaded</span> cell in each column is the strongest published baseline for that dataset, and every other entry carries its difference against it: <span style="color:#2160c4;">&#9650;</span> above, <span style="color:#c0392b;">&#9660;</span> below.</p>
<p class="is-size-7" style="color:#7a7a7a; margin-top:-0.75rem;"><strong>Baselines.</strong> WKPI (<a href="https://proceedings.neurips.cc/paper/2019/file/12780ea688a71dabc284b064add459a4-Paper.pdf" target="_blank">Zhao &amp; Wang, NeurIPS 2019</a>) &middot; Spec-GN / Norm-GN (<a href="https://proceedings.mlr.press/v162/yang22n.html" target="_blank">Yang et al., ICML 2022</a>) &middot; GWL-WL (Orsini et al., IJCAI 2015) &middot; HGP-SL (<a href="https://doi.org/10.1109/TKDE.2021.3090664" target="_blank">Zhang et al., TKDE 2021</a>) &middot; DSGCN (<a href="https://arxiv.org/abs/2003.11702" target="_blank">Balcilar et al., 2020</a>) &middot; U2GNN (<a href="https://doi.org/10.1145/3487553.3524258" target="_blank">Nguyen et al., WWW 2022</a>) &middot; NDP (Bianchi et al., TNNLS 2020) &middot; ASAP (<a href="https://arxiv.org/abs/1911.07979" target="_blank">Ranjan et al., 2019</a>) &middot; EvoG (<a href="https://doi.org/10.1109/WNYIPW.2019.8923110" target="_blank">Domingue et al., WNYISPW 2019</a>).</p>

<h2 class="title is-3" style="margin-top: 2.5rem;">Related Work</h2>

The Associative Memory foundations that ET is built on are surveyed in
[Modern Methods in Associative Memory](https://arxiv.org/abs/2507.06211), and the same energy-based
perspective is applied to generative models in
[Memorization to Generalization: Emergence of Diffusion Models from Associative Memory](https://arxiv.org/abs/2505.21777)
and [Language Diffusion Models are Associative Memories Capable of Retrieving Unseen Data](/papers/language-diffusion-associative-memory/).
For background, see my [blog post on energy-based modeling](/research/2025/12/15/ebm.html).
