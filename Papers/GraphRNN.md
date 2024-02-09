---
title: "GraphRNN: Generating Realistic Graphs with Deep Auto-regressive Models"
authors:
  - Jiaxuan You
  - Rex Ying
  - Xiang Ren
  - William L. Hamilton
  - Jure Leskovec
published: 2018-02-24
arXiv: http://arxiv.org/abs/1802.08773v3
paper: http://arxiv.org/pdf/1802.08773v3
tags:
  - graph_generation
---

#### Abstract

Modeling and generating graphs is fundamental for studying networks in biology, engineering, and social sciences. However, modeling complex distributions over graphs and then efficiently sampling from these distributions is challenging due to the non-unique, high-dimensional nature of graphs and the complex, non-local dependencies that exist between edges in a given graph. Here we propose GraphRNN, a deep autoregressive model that addresses the above challenges and approximates any distribution of graphs with minimal assumptions about their structure. GraphRNN learns to generate graphs by training on a representative set of graphs and decomposes the graph generation process into a sequence of node and edge formations, conditioned on the graph structure generated so far.   In order to quantitatively evaluate the performance of GraphRNN, we introduce a benchmark suite of datasets, baselines and novel evaluation metrics based on Maximum Mean Discrepancy, which measure distances between sets of graphs. Our experiments show that GraphRNN significantly outperforms all baselines, learning to generate diverse graphs that match the structural characteristics of a target set, while also scaling to graphs 50 times larger than previous deep models.


```dataview
table title as Title, published as Published 
from #graph_generation 
sort published asc
```

