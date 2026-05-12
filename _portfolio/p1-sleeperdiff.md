---
title: "SleeperDiff: Transform-Dependent Adversarial Attacks"
excerpt: "Diffusion-based adversarial inputs that activate only after a specific image transform is applied."
header:
  overlay_image: /assets/images/p1-hero.png
  overlay_filter: 0.5
  teaser: /assets/images/p1-teaser.png
date: 2025-12-01
show_date: false
tags:
  - AI Security
  - Diffusion Models
  - PyTorch
---

<p>
  <a href="/assets/reports/p1-sleeperdiff.pdf" class="btn btn--primary btn--large">Read the full project report (PDF)</a>
</p>

A graduate AI Security project at Texas A&M, co-authored with Murugan and Nadakatti.

## Motivation

Most adversarial attacks on vision models fire on every viewing of the image. That makes them easy to spot and implausible in the physical world. We designed an attack that stays hidden under normal conditions and only triggers when a specific transform is applied, such as blurring, gamma correction, or scaling. That asymmetry matches realistic threat models, where automated pipelines apply known transformations but a human auditor reviews the input first.

## Implementation

We built a dual-objective optimization framework on Stable Diffusion v1.5, combining DDIM inversion with latent-space adversarial search. The loss balances three goals: benign preservation under the identity transform, adversarial activation under the target transform, and robustness across transform parameters via Expectation-over-Transformation. Surrogate classifier was Inception-v3, evaluated on the ImageNet-Compatible 100-image subset.

![Proposed optimization framework](/assets/images/p1-hero.png)

## Results

| Transform | Attack success | Benign accuracy |
|---|---|---|
| Gaussian blur | **72%** | 100% |
| Gamma correction | 59% | 100% |
| Scaling | low | 100% |

Perceptual quality reached LPIPS 0.132, competitive with DiffAttack at 0.126. Black-box transfer across surrogate classifiers was modest. Scaling proved hard to attack in latent space, suggesting our loss formulation favors photometric transforms.

![Blurring attack results](/assets/images/p1-blurring.png)

## What I Learned

* Most of the work was reading. Reproducing DiffAttack and DDIM inversion took longer than the novel loss work.
* Expectation-over-Transformation framing matters more than loss shape. Once you average over a transform distribution, the rest of the pipeline mostly takes care of itself.
* Latent-space attacks generalize to photometric perturbations but fight against geometry. Scaling shifts the latent grid in ways the optimizer cannot easily compensate for.
* Working on a 3-person research-style writeup taught me how to scope a contribution and frame an honest results section.

## Tools

PyTorch, Stable Diffusion v1.5, DDIM inversion, Inception-v3.
