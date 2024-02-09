---
title: "QA-GNN: Reasoning with Language Models and Knowledge Graphs for Question
  Answering"
authors:
  - "Michihiro Yasunaga"
  - "Hongyu Ren"
  - "Antoine Bosselut"
  - "Percy Liang"
  - "Jure Leskovec"
published: "2021-04-13"
arXiv: "http://arxiv.org/abs/2104.06378v5"
paper: "http://arxiv.org/pdf/2104.06378v5"
---

#### Abstract

The problem of answering questions using knowledge from pre-trained language models (LMs) and knowledge graphs (KGs) presents two challenges: given a QA context (question and answer choice), methods need to 
1. identify relevant knowledge from large KGs, and 
2. perform joint reasoning over the QA context and KG. 

In this work, we propose a new model, QA-GNN, which addresses the above challenges through two key innovations: 
1. relevance scoring, where we use LMs to estimate the importance of KG nodes relative to the given QA context, and
2. joint reasoning, where we connect the QA context and KG to form a joint graph, and mutually update their representations through graph neural networks. 

We evaluate our model on QA benchmarks in the commonsense (CommonsenseQA, OpenBookQA) and biomedical (MedQA-USMLE) domains. QA-GNN outperforms existing LM and LM+KG models, and exhibits capabilities to perform interpretable and structured reasoning, e.g., correctly handling negation in questions.