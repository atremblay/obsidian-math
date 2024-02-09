---
title: "UnICORNN: A recurrent model for learning very long time dependencies"
authors:
  - "T. Konstantin Rusch"
  - "Siddhartha Mishra"
published: "2021-03-09"
arXiv: "http://arxiv.org/abs/2103.05487v2"
paper: "http://arxiv.org/pdf/2103.05487v2"
---

#### Abstract

The design of recurrent neural networks (RNNs) to accurately process sequential inputs with long-time dependencies is very challenging on account of the exploding and vanishing gradient problem. To overcome this, we propose a novel RNN architecture which is based on a structure preserving discretization of a Hamiltonian system of second-order ordinary differential equations that models networks of oscillators. The resulting RNN is fast, invertible (in time), memory efficient and we derive rigorous bounds on the hidden state gradients to prove the mitigation of the exploding and vanishing gradient problem. A suite of experiments are presented to demonstrate that the proposed RNN provides state of the art performance on a variety of learning tasks with (very) long-time dependencies.
		