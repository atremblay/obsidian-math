---
title: "CDLM: Cross-Document Language Modeling"
aliases:
  - CDLM
authors:
  - "Avi Caciularu"
  - "Arman Cohan"
  - "Iz Beltagy"
  - "Matthew E. Peters"
  - "Arie Cattan"
  - "Ido Dagan"
published: "2021-01-02"
arXiv: "http://arxiv.org/abs/2101.00406v2"
paper: "http://arxiv.org/pdf/2101.00406v2"
---

#### Abstract

We introduce a new pretraining approach geared for multi-document language modeling, incorporating two key ideas into the masked language modeling self-supervised objective. First, instead of considering documents in isolation, we pretrain over sets of multiple related documents, encouraging the model to learn cross-document relationships. Second, we improve over recent long-range transformers by introducing dynamic global attention that has access to the entire input to predict masked tokens. We release CDLM (Cross-Document Language Model), a new general language model for multi-document setting that can be easily applied to downstream tasks. Our extensive analysis shows that both ideas are essential for the success of CDLM, and work in synergy to set new state-of-the-art results for several multi-text tasks. Code and models are available at https://github.com/aviclu/CDLM.
		