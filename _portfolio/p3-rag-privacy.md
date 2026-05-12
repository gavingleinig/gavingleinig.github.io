---
title: "Privacy Policy Classification with Retrieval-Augmented LLMs"
excerpt: "Does grounding GPT-3.5 in GDPR text via RAG improve multi-label classification of OPP-115 privacy policies?"
header:
  overlay_image: /assets/images/p3-flowchart.png
  overlay_filter: 0.55
  teaser: /assets/images/p3-flowchart.png
date: 2024-12-01
show_date: false
tags:
  - LLM
  - RAG
  - NLP
---

<p>
  <a href="/assets/reports/p3-rag-privacy.pdf" class="btn btn--primary btn--large">Read the full project report (PDF)</a>
  <a href="https://colab.research.google.com/drive/1TL74hhu7vM1tHlSK7g5J2o_GiVdvRhDG?usp=sharing" class="btn btn--inverse">Colab notebook</a>
</p>

A graduate Data Analytics and Cyber Security project at Texas A&M, co-authored with Tristan Lanclos.

## Motivation

Privacy policies are long, jargon-heavy legal documents that virtually no one reads. The average policy takes 12 minutes to read, and a user reading every new policy they encounter in a year would spend over 200 hours doing so. Less than 1% of users click the privacy policy link at all. Yet GDPR and similar regulations require companies to disclose specific data-handling practices clearly. We wanted to know whether grounding an LLM in GDPR source text via Retrieval-Augmented Generation actually improves classification accuracy over the LLM alone, without the cost of fine-tuning.

## Implementation

A RAG pipeline over GPT-3.5-turbo at temperature 0.7. We embedded 406 GDPR paragraphs with MPNet and stored them in ChromaDB. For each policy segment, we retrieved the top-k most relevant GDPR paragraphs by cosine similarity and injected them into a five-block prompt (data, task, format instruction, RAG context, one-shot example). We evaluated on OPP-115 over 9 first-tier data-practice categories, after dropping three "Other" subcategories the LLM could not classify meaningfully.

![Retrieval-augmented LLM dataflow](/assets/images/p3-flowchart.png)

## Results

| Metric | GPT-3.5 baseline | GPT-3.5 + RAG |
|---|---|---|
| Micro avg (P / R / F1) | 0.72 / 0.72 / 0.72 | 0.69 / 0.69 / 0.69 |
| Macro avg (P / R / F1) | 0.74 / 0.72 / 0.72 | 0.68 / 0.70 / 0.67 |
| Weighted avg (P / R / F1) | **0.77 / 0.72 / 0.73** | 0.75 / 0.69 / 0.70 |

The headline was a negative result. RAG did not improve aggregate F1 and was marginally worse on macro and weighted averages. Two findings underneath the headline are more interesting. First, RAG enhanced precision on well-supported classes and held F1 there, while reduced performance came on underrepresented classes where retrieval struggled. Second, RAG roughly halved the number of segments the model failed to classify at all. The baseline threw away about twice as many segments as the RAG-augmented version.

![Distribution of paragraph retrieval frequencies](/assets/images/p3-histogram.png)

## What I Learned

* RAG is not free quality. Retrieval corpus size, chunking, retrieval diversity, and prompt format dominate the impact. Swapping the LLM is the last lever, not the first.
* A strong base model masks RAG gains on majority classes. GPT-3.5 already knows a lot about privacy concepts. The wins were on precision for supported classes and on fewer "I cannot classify this" failures, neither captured by a single F1 number.
* Retrieval distribution is a diagnostic. The histogram showed about 30% of retrievals concentrated on a handful of GDPR paragraphs, a clear pointer to MMR-style diversity reranking as the next experiment.
* Co-authoring a research-style writeup taught me how to frame a near-null result honestly.

## Tools

GPT-3.5-turbo, ChromaDB, MPNet, sentence-transformers, Jupyter.
