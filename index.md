---
layout: single
title: "Howdy!"
permalink: /
author_profile: true
feature_row:
  - image_path: /assets/images/p1-card.png
    alt: "SleeperDiff framework diagram"
    title: "SleeperDiff"
    excerpt: "Adversarial images that look normal until a specific edit, like a blur, gets applied. Built on Stable Diffusion. Under blur it worked 72% of the time, under scaling almost never, and it transferred poorly to other models. The report says why. AI Security course, team of three. [Code on GitHub](https://github.com/gavingleinig/semantic-transformation-adv-attack)"
    url: /assets/reports/p1-sleeperdiff.pdf
    btn_label: "Report (PDF)"
    btn_class: "btn--primary"
  - image_path: /assets/images/p2-card.png
    alt: "Sparse-view Gaussian splatting comparison"
    title: "Wheat Fields from Four Photos"
    excerpt: "3D models of a wheat plot from only four camera angles, with a diffusion model I fine-tuned on the scene filling in what the cameras missed. PSNR went from 18.4 to 25.9 over the sparse baseline. Computer Vision course."
    url: /assets/reports/p2-wheat-results.pdf
    btn_label: "Write-up (PDF)"
    btn_class: "btn--primary"
  - image_path: /assets/images/p4-card.png
    alt: "Mean cluster metrics across taxonomic levels"
    title: "Faster Clustering with Approximate MSTs"
    excerpt: "Single-linkage clustering normally needs an exact minimum spanning tree, which means every pairwise distance. We implemented a subquadratic approximation in Julia and tested whether its trees are good enough to cluster with. On synthetic data it nearly matched the exact tree, on noisy genomic data it did better, and on recipes nothing worked. Algorithms course, written with Brendan Larson. [Code on GitHub](https://github.com/gavingleinig/MFC-Julia)"
    url: /assets/reports/p4-mst-clustering.pdf
    btn_label: "Report (PDF)"
    btn_class: "btn--primary"
  - image_path: /assets/images/p3-card.png
    alt: "RAG privacy policy classification dataflow"
    title: "RAG for Privacy Policies"
    excerpt: "Does handing GPT-3.5 the actual GDPR text help it classify privacy policies? Mostly no, but it cut the segments the model gave up on by about half. Written with Tristan Lanclos."
    url: /assets/reports/p3-rag-privacy.pdf
    btn_label: "Report (PDF)"
    btn_class: "btn--primary"
---

I'm Gavin. I finished my Master's in Computer Science at Texas A&M in May 2026, a year after my B.S. there.

Since graduating I've been doing independent consulting for a few small businesses in Texas. I sit with the owner or the bookkeeper, work out what is actually slowing them down, then build it: a warehouse that joins a restaurant's sales, payroll and arcade feeds into a daily scorecard, a pipeline that turns handwritten field test sheets into database records, a support inbox that sorts its own mail. I build mostly in Python and SQL, with agentic coding tools doing a lot of the typing. I'm now looking for a full-time engineering role on a team working on hard problems.

In school I gravitated toward computer vision and security, and especially where the two meet. Below are four graduate projects. The reports say what did not work as well as what did.

{% include feature_row %}

My [resume](/assets/reports/resume.pdf) has the rest. The best way to reach me is [email](mailto:gavingleinig@gmail.com).
