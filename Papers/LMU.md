---
title: "Parallelizing Legendre Memory Unit Training"
authors:
  - "Narsimha Chilkuri"
  - "Chris Eliasmith"
published: "2021-02-22"
arXiv: "http://arxiv.org/abs/2102.11417v2"
paper: "http://arxiv.org/pdf/2102.11417v2"
---

#### Abstract

Recently, a new recurrent neural network (RNN) named the Legendre Memory Unit (LMU) was proposed and shown to achieve state-of-the-art performance on several benchmark datasets. Here we leverage the linear time-invariant (LTI) memory component of the LMU to construct a simplified variant that can be parallelized during training (and yet executed as an RNN during inference), thus overcoming a well known limitation of training RNNs on GPUs. We show that this reformulation that aids parallelizing, which can be applied generally to any deep network whose recurrent components are linear, makes training up to 200 times faster. Second, to validate its utility, we compare its performance against the original LMU and a variety of published LSTM and transformer networks on seven benchmarks, ranging from psMNIST to sentiment analysis to machine translation. We demonstrate that our models exhibit superior performance on all datasets, often using fewer parameters. For instance, our LMU sets a new state-of-the-art result on psMNIST, and uses half the parameters while outperforming DistilBERT and LSTM models on IMDB sentiment analysis.
		