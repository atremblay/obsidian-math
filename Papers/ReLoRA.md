---
title: "Stack More Layers Differently: High-Rank Training Through Low-Rank
  Updates"
authors:
  - "Vladislav Lialin"
  - "Namrata Shivagunde"
  - "Sherin Muckatira"
  - "Anna Rumshisky"
published: "2023-07-11"
arXiv: "http://arxiv.org/abs/2307.05695v3"
paper: "http://arxiv.org/pdf/2307.05695v3"
---

#### Abstract

Despite the dominance and effectiveness of scaling, resulting in large networks with hundreds of billions of parameters, the necessity to train overparametrized models remains poorly understood, and alternative approaches do not necessarily make it cheaper to train high-performance models. In this paper, we explore low-rank training techniques as an alternative approach to training large neural networks. We introduce a novel method called ReLoRA, which utilizes low-rank updates to train high-rank networks. We apply ReLoRA to pre-training transformer language models with up to 350M parameters and demonstrate comparable performance to regular neural network training. Furthermore, we observe that the efficiency of ReLoRA increases with model size, making it a promising approach for training multi-billion-parameter networks efficiently. Our findings shed light on the potential of low-rank training techniques and their implications for scaling laws.
		