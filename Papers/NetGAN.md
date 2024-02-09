---
title: "NetGAN: Generating Graphs via Random Walks"
authors:
  - Aleksandar Bojchevski
  - Oleksandr Shchur
  - Daniel Zügner
  - Stephan Günnemann
published: 2018-03-02
arXiv: http://arxiv.org/abs/1803.00816v2
paper: http://arxiv.org/pdf/1803.00816v2
tags:
  - graph_generation
---

#### Abstract

We propose NetGAN - the first implicit generative model for graphs able to mimic real-world networks. We pose the problem of graph generation as learning the distribution of biased random walks over the input graph. The proposed model is based on a stochastic neural network that generates discrete output samples and is trained using the Wasserstein GAN objective. NetGAN is able to produce graphs that exhibit well-known network patterns without explicitly specifying them in the model definition. At the same time, our model exhibits strong generalization properties, as highlighted by its competitive link prediction performance, despite not being trained specifically for this task. Being the first approach to combine both of these desirable properties, NetGAN opens exciting avenues for further research.



```dataview
table title as Title, published as Published 
from #graph_generation 
sort published asc
```