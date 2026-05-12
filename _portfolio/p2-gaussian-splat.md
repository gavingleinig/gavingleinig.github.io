---
title: "Sparse-View Wheat Field Reconstruction with Gaussian Splatting"
excerpt: "Filling sparse 3D Gaussian Splatting reconstructions with a scene-tuned ControlNet + LoRA generative prior."
header:
  overlay_image: /assets/images/p2-hero.png
  overlay_filter: 0.5
  teaser: /assets/images/p2-teaser.png
date: 2024-12-01
show_date: false
tags:
  - 3D Reconstruction
  - Gaussian Splatting
  - ControlNet
  - LoRA
---

<p>
  <a href="/assets/reports/p2-gaussian-splat-slides.pdf" class="btn btn--primary btn--large">Read the presentation slides (PDF)</a>
  <a href="https://github.com/gavingleinig/CSCE625Project" class="btn btn--inverse">Code on GitHub</a>
</p>

A graduate Computer Vision project at Texas A&M (CSCE 625).

## Motivation

Precision agriculture and plant phenotyping need detailed 3D models of wheat, but field cameras only capture a handful of viewpoints per plant. 3D Gaussian Splatting is fast and high-quality but degrades badly with sparse views, producing holes, floaters, and missing geometry on the far side of the plant. We wanted to know whether a 2D generative prior could plug those gaps without forcing a return to dense capture.

## Implementation

A three-stage pipeline. First, we train a sparse 3DGS model from only 4 viewpoints. Second, we fine-tune Stable Diffusion v1.5 with ControlNet and LoRA on the scene's own renders, conditioning on depth and normals so the diffusion model learns this plant rather than wheat in general. Third, we sample novel views from the repair model and feed them back into 3DGS training, iteratively fixing artifacts and filling occlusions. Baselines were pure sparse 3DGS (4 views) and dense 3DGS (30 views).

![Model overview](/assets/images/p2-overview.png)

## Results

| Model | SSIM | PSNR | LPIPS |
|---|---|---|---|
| Sparse 3DGS (4 views) | 0.395 | / | / |
| **Sparse + Refinement (4 views)** | **0.774** | **25.9** | **0.271** |
| Dense 3DGS (30 views) | matched | matched | matched |

The refinement step roughly doubles SSIM over sparse-only and matches the dense baseline using about 13% of the views.

![Broad comparison](/assets/images/p2-hero.png)

## What I Learned

* Generative priors do real work in low-data regimes, but only when the prior is fine-tuned to the scene. A vanilla SD checkpoint hallucinates plausible wheat that does not match the real plant. The ControlNet and LoRA fine-tune is what makes the geometry align.
* Plumbing beats modeling. Most of the engineering went into data flow between 3DGS and the diffusion model: view sampling, masking, conditioning signals, training loop interleaving.
* Evaluation choices shape the story. SSIM and PSNR rewarded our method strongly; LPIPS told a more cautious story. Reporting all three forced an honest narrative.

## Tools

PyTorch, 3D Gaussian Splatting, Stable Diffusion v1.5, ControlNet, LoRA.
