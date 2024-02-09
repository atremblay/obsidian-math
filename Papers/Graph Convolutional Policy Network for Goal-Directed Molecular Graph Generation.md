---
title: Graph Convolutional Policy Network for Goal-Directed Molecular Graph Generation
authors:
  - Jiaxuan You
  - Bowen Liu
  - Rex Ying
  - Vijay Pande
  - Jure Leskovec
published: 2018-06-07
arXiv: http://arxiv.org/abs/1806.02473v3
paper: http://arxiv.org/pdf/1806.02473v3
tags:
  - graph_generation
---

#### Abstract

Generating novel graph structures that optimize given objectives while obeying some given underlying rules is fundamental for chemistry, biology and social science research. This is especially important in the task of molecular graph generation, whose goal is to discover novel molecules with desired properties such as drug-likeness and synthetic accessibility, while obeying physical laws such as chemical valency. However, designing models to find molecules that optimize desired properties while incorporating highly complex and non-differentiable rules remains to be a challenging task. Here we propose Graph Convolutional Policy Network (GCPN), a general graph convolutional network based model for goal-directed graph generation through reinforcement learning. The model is trained to optimize domain-specific rewards and adversarial loss through policy gradient, and acts in an environment that incorporates domain-specific rules. Experimental results show that GCPN can achieve 61% improvement on chemical property optimization over state-of-the-art baselines while resembling known molecules, and achieve 184% improvement on the constrained property optimization task.



```dataview
table title as Title, published as Published 
from #graph_generation 
sort published asc
```
