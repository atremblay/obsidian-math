---
title: DAG-Recurrent Neural Networks For Scene Labeling
authors:
  - Bing Shuai
  - Zhen Zuo
  - Gang Wang
  - Bing Wang
published: 2015-09-02
arXiv: http://arxiv.org/abs/1509.00552v2
paper: http://arxiv.org/pdf/1509.00552v2
tags:
  - graph_generation
---

#### Abstract

In image labeling, local representations for image units are usually generated from their surrounding image patches, thus long-range contextual information is not effectively encoded. In this paper, we introduce recurrent neural networks (RNNs) to address this issue. Specifically, directed acyclic graph RNNs (DAG-RNNs) are proposed to process DAG-structured images, which enables the network to model long-range semantic dependencies among image units. Our DAG-RNNs are capable of tremendously enhancing the discriminative power of local representations, which significantly benefits the local classification. Meanwhile, we propose a novel class weighting function that attends to rare classes, which phenomenally boosts the recognition accuracy for non-frequent classes. Integrating with convolution and deconvolution layers, our DAG-RNNs achieve new state-of-the-art results on the challenging SiftFlow, CamVid and Barcelona benchmarks.



```dataview
table title as Title, published as Published 
from #graph_generation 
sort published asc
```
