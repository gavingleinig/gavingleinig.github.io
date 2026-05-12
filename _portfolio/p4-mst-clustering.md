---
title: "Subquadratic MST Approximations for Single-Linkage Clustering"
excerpt: "Evaluating the Metric Forest Completion approximation against exact MST and k-centering for hierarchical clustering."
header:
  overlay_image: /assets/images/p4-hero.png
  overlay_filter: 0.55
  teaser: /assets/images/p4-teaser.png
date: 2026-05-01
show_date: false
tags:
  - Algorithms
  - Clustering
  - Approximation
---

<p>
  <a href="/assets/reports/p4-mst-clustering.pdf" class="btn btn--primary btn--large">Read the full project report (PDF)</a>
</p>

<!-- TODO: compile final-project-spring-2026.tex and place the PDF at assets/reports/p4-mst-clustering.pdf -->

A graduate Algorithms project at Texas A&M (CSCE 689 Special Topics in Algorithms), co-authored with Brendan Larson.

## Motivation

Single-linkage clustering is functionally equivalent to finding a Minimum Spanning Tree on a complete metric graph. Computing an exact metric MST requires every pairwise distance and runs in O(n^2 log n) time, which is impractical for massive datasets. The Metric Forest Completion (MFC) framework offers a subquadratic approximation with a 2-gamma theoretical guarantee, but its clustering quality (not just spanning tree weight) had not been thoroughly evaluated. We tested whether the MFC approximation is actually viable for downstream clustering tasks.

## Implementation

We benchmarked MFC against exact MST, k-centering, and naive spanning tree baselines on three datasets: synthetic Gaussian mixtures (varying dimensionality d and cluster count g), the Cooking dataset (39,774 recipes with Jaccard distance), and the GreenGenes microbial dataset (30,000 sequences with taxonomic ground truth at 5 levels from Phylum to Genus). We evaluated each method with Adjusted Rand Index and Normalized Mutual Information, sweeping the spanning-tree cut threshold to find the best achievable clustering at each cluster count.

![Mean cluster metrics across taxonomic levels](/assets/images/p4-hero.png)

## Results

On synthetic Gaussians, the MFC approximation tracks the exact MST closely on ARI and NMI across a wide range of cluster counts and dimensions. On the GreenGenes dataset, MFC actually outperformed the exact MST on ARI at Phylum (0.4265 vs 0.4199), Family (0.6375 vs 0.5925), and Genus (0.6974 vs 0.6568) levels. On the Cooking dataset, single-linkage clustering performed poorly across the board (ARI essentially zero), indicating the dataset is unsuited to the method, not the approximation.

Runtime was the headline win. On Cooking (N=39,774), MFC ran in **203 seconds versus 1,861 seconds** for the exact MST: a 9x speedup with identical clustering quality.

![ARI vs Sigma on synthetic Gaussians](/assets/images/p4-ari-synthetic.png)

## What I Learned

* Approximation guarantees on the cost function (spanning tree weight) do not automatically transfer to downstream task quality. We had to measure clustering metrics directly.
* Single-linkage clustering is sensitive to whether the underlying data has compact, well-separated clusters. The Cooking dataset failure was a property of the data, not the algorithm.
* k-centering quietly wins on several real-world tasks (Class, Order, Genus). It's a good baseline that algorithm papers should not skip.
* Working with a co-author on an algorithms-style paper sharpened how I read and reproduce results from a referenced framework.

## Tools

Python, NumPy, scikit-learn, custom MST and k-centering implementations.
