---
title: "JointGT: Graph-Text Joint Representation Learning for Text Generation
  from Knowledge Graphs"
authors:
  - "Pei Ke"
  - "Haozhe Ji"
  - "Yu Ran"
  - "Xin Cui"
  - "Liwei Wang"
  - "Linfeng Song"
  - "Xiaoyan Zhu"
  - "Minlie Huang"
published: "2021-06-19"
arXiv: "http://arxiv.org/abs/2106.10502v1"
paper: "http://arxiv.org/pdf/2106.10502v1"
---

#### Abstract

Existing pre-trained models for knowledge-graph-to-text (KG-to-text) generation simply fine-tune text-to-text pre-trained models such as BART or T5 on KG-to-text datasets, which largely ignore the graph structure during encoding and lack elaborate pre-training tasks to explicitly model graph-text alignments. To tackle these problems, we propose a graph-text joint representation learning model called JointGT. During encoding, we devise a structure-aware semantic aggregation module which is plugged into each Transformer layer to preserve the graph structure. Furthermore, we propose three new pre-training tasks to explicitly enhance the graph-text alignment including respective text / graph reconstruction, and graph-text alignment in the embedding space via Optimal Transport. Experiments show that JointGT obtains new state-of-the-art performance on various KG-to-text datasets.