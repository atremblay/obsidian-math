---
title: "D-VAE: A Variational Autoencoder for Directed Acyclic Graphs"
authors:
  - Muhan Zhang
  - Shali Jiang
  - Zhicheng Cui
  - Roman Garnett
  - Yixin Chen
published: 2019-04-24
arXiv: http://arxiv.org/abs/1904.11088v4
paper: http://arxiv.org/pdf/1904.11088v4
tags:
  - graph_generation
---

#### Abstract

Graph structured data are abundant in the real world. Among different graph types, directed acyclic graphs (DAGs) are of particular interest to machine learning researchers, as many machine learning models are realized as computations on DAGs, including neural networks and Bayesian networks. In this paper, we study deep generative models for DAGs, and propose a novel DAG variational autoencoder (D-VAE). To encode DAGs into the latent space, we leverage graph neural networks. We propose an asynchronous message passing scheme that allows encoding the computations on DAGs, rather than using existing simultaneous message passing schemes to encode local graph structures. We demonstrate the effectiveness of our proposed DVAE through two tasks: neural architecture search and Bayesian network structure learning. Experiments show that our model not only generates novel and valid DAGs, but also produces a smooth latent space that facilitates searching for DAGs with better performance through Bayesian optimization.



```dataview
table title as Title, published as Published 
from #graph_generation 
sort published asc
```