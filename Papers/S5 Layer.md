---
title: "Simplified State Space Layers for Sequence Modeling"
authors:
  - "Jimmy T. H. Smith"
  - "Andrew Warrington"
  - "Scott W. Linderman"
published: "2022-08-09"
arXiv: "http://arxiv.org/abs/2208.04933v3"
paper: "http://arxiv.org/pdf/2208.04933v3"
---

#### Abstract

Models using structured state space sequence (S4) layers have achieved state-of-the-art performance on long-range sequence modeling tasks. An S4 layer combines linear state space models (SSMs), the HiPPO framework, and deep learning to achieve high performance. We build on the design of the S4 layer and introduce a new state space layer, the S5 layer. Whereas an S4 layer uses many independent single-input, single-output SSMs, the S5 layer uses one multi-input, multi-output SSM. We establish a connection between S5 and S4, and use this to develop the initialization and parameterization used by the S5 model. The result is a state space layer that can leverage efficient and widely implemented parallel scans, allowing S5 to match the computational efficiency of S4, while also achieving state-of-the-art performance on several long-range sequence modeling tasks. S5 averages 87.4% on the long range arena benchmark, and 98.5% on the most difficult Path-X task.
		