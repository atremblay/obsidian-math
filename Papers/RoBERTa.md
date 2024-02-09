---
title: "RoBERTa: A Robustly Optimized BERT Pretraining Approach"
aliases:
    - RoBERTa
authors:
  - "Yinhan Liu"
  - "Myle Ott"
  - "Naman Goyal"
  - "Jingfei Du"
  - "Mandar Joshi"
  - "Danqi Chen"
  - "Omer Levy"
  - "Mike Lewis"
  - "Luke Zettlemoyer"
  - "Veselin Stoyanov"
published: "2019-07-26"
arXiv: "http://arxiv.org/abs/1907.11692v1"
paper: "http://arxiv.org/pdf/1907.11692v1"
---

#### Abstract

  Language model pretraining has led to significant performance gains but
careful comparison between different approaches is challenging. Training is
computationally expensive, often done on private datasets of different sizes,
and, as we will show, hyperparameter choices have significant impact on the
final results. We present a replication study of BERT pretraining (Devlin et
al., 2019) that carefully measures the impact of many key hyperparameters and
training data size. We find that BERT was significantly undertrained, and can
match or exceed the performance of every model published after it. Our best
model achieves state-of-the-art results on GLUE, RACE and SQuAD. These results
highlight the importance of previously overlooked design choices, and raise
questions about the source of recently reported improvements. We release our
models and code.

		