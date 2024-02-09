---
title: "BioGPT: Generative Pre-trained Transformer for Biomedical Text
  Generation and Mining"
authors:
  - "Renqian Luo"
  - "Liai Sun"
  - "Yingce Xia"
  - "Tao Qin"
  - "Sheng Zhang"
  - "Hoifung Poon"
  - "Tie-Yan Liu"
published: "2022-10-19"
arXiv: "http://arxiv.org/abs/2210.10341v3"
paper: "http://arxiv.org/pdf/2210.10341v3"
---

#### Abstract

Pre-trained language models have attracted increasing attention in the biomedical domain, inspired by their great success in the general natural language domain. Among the two main branches of pre-trained language models in the general language domain, i.e., BERT (and its variants) and GPT (and its variants), the first one has been extensively studied in the biomedical domain, such as BioBERT and PubMedBERT. While they have achieved great success on a variety of discriminative downstream biomedical tasks, the lack of generation ability constrains their application scope. In this paper, we propose BioGPT, a domain-specific generative Transformer language model pre-trained on large scale biomedical literature. We evaluate BioGPT on six biomedical NLP tasks and demonstrate that our model outperforms previous models on most tasks. Especially, we get 44.98%, 38.42% and 40.76% F1 score on BC5CDR, KD-DTI and DDI end-to-end relation extraction tasks respectively, and 78.2% accuracy on PubMedQA, creating a new record. Our case study on text generation further demonstrates the advantage of BioGPT on biomedical literature to generate fluent descriptions for biomedical terms. Code is available at https://github.com/microsoft/BioGPT.
		