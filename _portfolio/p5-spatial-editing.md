---
title: "Spatially and Relationally Aware Image Editing via a Multi-Model Pipeline"
excerpt: "Modular VLM-orchestrated pipeline that separates semantic reasoning, perception, spatial math, and pixel rendering."
header:
  overlay_image: /assets/images/p5-hero.jpg
  overlay_filter: 0.55
  teaser: /assets/images/p5-teaser.jpg
date: 2026-04-01
show_date: false
tags:
  - Computer Vision
  - VLM
  - Diffusion
  - Scene Graphs
---

<p>
  <a href="/assets/reports/p5-spatial-editing.pdf" class="btn btn--primary btn--large">Read the full project report (PDF)</a>
</p>

<!-- TODO: compile main.tex (CVPR template) and place the PDF at assets/reports/p5-spatial-editing.pdf -->

A graduate Computer Vision project at Texas A&M (CSCE 753), co-authored with Malvika Koushik, Vijay Murugan Appavu Sivaprakasam, and Dev Pankajbhai Goti.

## Motivation

End-to-end diffusion models struggle with precise spatial reasoning and relational editing. They rely on latent-space pattern matching rather than explicit 3D geometry, so they hallucinate new features or pixel-blend rather than physically manipulate objects. We wanted to know whether separating semantic reasoning from deterministic spatial math from pixel rendering, by routing through a "mixture of experts" pipeline, could deliver geometrically sound edits without global hallucinations.

## Implementation

A four-stage pipeline. **Stage 1: Intent Extraction.** A Vision-Language Model (Gemma 4 E4B) parses the user's natural-language edit instruction and the input image into structured JSON: task type, subject and reference objects with both short detector labels and detailed visual descriptions. **Stage 2: Cascading Grounding and Masking.** Grounding DINO proposes candidate bounding boxes from the short labels, CLIP filters them against the detailed descriptions, and SAM produces a pixel-perfect mask of the winner. **Stage 3: Spatial Reasoning Engine.** Depth Anything V2 generates a monocular depth map, which we use to compute realistic scale factors and 2D affine transforms for the requested relational operator. **Stage 4: Generative Execution.** LaMa erases the subject from its original location, the warped subject is composited with Poisson blending, and Stable Diffusion XL Inpainting refines only a thin edge mask. We evaluated three Stage 4 variants and converged on this hybrid approach.

![Stage 2: Grounding and masking](/assets/images/p5-stage2.jpg)

![Stage 3: Spatial reasoning](/assets/images/p5-stage3.jpg)

Evaluated end-to-end on a synthetic CLEVR-based benchmark of 150 images with structured ground-truth commands, plus qualitative tests on COCO.

## Results

| Stage | Metric | Value |
|---|---|---|
| Orchestrator | Intent accuracy | **99.3%** |
| Spatial experts | Grounding accuracy | 71.3% |
| Spatial reasoning | Avg pixel error | 51.8 px |
| Generative execution | Background SSIM | **0.976** |
| Generative execution | Background PSNR | 29.29 dB |
| Generative execution | Subject LPIPS | 0.162 |

The localized-editing approach successfully preserved structure: background SSIM of 0.976 against a global-editing baseline of 0.962. The hybrid Stage 4 modestly beat the SD-only variant on PSNR and SSIM. A separate ablation across Stable Diffusion versions found SD 1.5 matches or beats SDXL on every metric while being much cheaper, making it the practical choice. The big remaining bottleneck was grounding at 71.3%: DINO and CLIP struggle with full-image ambiguity, and future iterations should let the VLM pre-crop the search region.

![Qualitative successes](/assets/images/p5-successes.jpg)

## What I Learned

* Separating reasoning, perception, math, and rendering eliminates the global hallucinations that plague end-to-end diffusion, at the cost of constraining edits to operations the deterministic engine can express.
* The VLM as a "director" is more reliable than I expected. 99.3% intent accuracy on natural language to structured JSON means the bottleneck has moved downstream into perception.
* Bigger models are not always better. SD 1.5 beat SDXL Base on every metric and tied SDXL Inpainting at a fraction of the compute, a reminder that benchmark dominance does not always survive contact with a real pipeline.
* Coordinating a 4-person team across modular stages forced clean interfaces between components, which paid off when we swapped Stage 4 implementations three times.

## Tools

PyTorch, Gemma 4 E4B, Grounding DINO, CLIP, SAM, Depth Anything V2, Stable Diffusion XL Inpainting, LaMa, Poisson blending.
