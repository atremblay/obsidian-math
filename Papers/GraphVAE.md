---
title: "GraphVAE: Towards Generation of Small Graphs Using Variational Autoencoders"
authors:
  - Martin Simonovsky
  - Nikos Komodakis
published: 2018-02-09
arXiv: http://arxiv.org/abs/1802.03480v1
paper: http://arxiv.org/pdf/1802.03480v1
tags:
  - graph_generation
---

#### Abstract

Deep learning on graphs has become a popular research topic with many applications. However, past work has concentrated on learning graph embedding tasks, which is in contrast with advances in generative models for images and text. Is it possible to transfer this progress to the domain of graphs? We propose to sidestep hurdles associated with linearization of such discrete structures by having a decoder output a probabilistic fully-connected graph of a predefined maximum size directly at once. Our method is formulated as a variational autoencoder. We evaluate on the challenging task of molecule generation.


```dataview
table title as Title, published as Published 
from #graph_generation 
sort published asc
```
