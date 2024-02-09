---
title: "BloombergGPT: A Large Language Model for Finance"
authors:
  - "Shijie Wu"
  - "Ozan Irsoy"
  - "Steven Lu"
  - "Vadim Dabravolski"
  - "Mark Dredze"
  - "Sebastian Gehrmann"
  - "Prabhanjan Kambadur"
  - "David Rosenberg"
  - "Gideon Mann"
published: "2023-03-30"
arXiv: "http://arxiv.org/abs/2303.17564v2"
paper: "http://arxiv.org/pdf/2303.17564v2"
---

#### Abstract

The use of NLP in the realm of financial technology is broad and complex, with applications ranging from sentiment analysis and named entity recognition to question answering. Large Language Models (LLMs) have been shown to be effective on a variety of tasks; however, no LLM specialized for the financial domain has been reported in literature. In this work, we present BloombergGPT, a 50 billion parameter language model that is trained on a wide range of financial data. We construct a 363 billion token dataset based on Bloomberg's extensive data sources, perhaps the largest domain-specific dataset yet, augmented with 345 billion tokens from general purpose datasets. We validate BloombergGPT on standard LLM benchmarks, open financial benchmarks, and a suite of internal benchmarks that most accurately reflect our intended usage. Our mixed dataset training leads to a model that outperforms existing models on financial tasks by significant margins without sacrificing performance on general LLM benchmarks. Additionally, we explain our modeling choices, training process, and evaluation methodology. We release Training Chronicles (Appendix C) detailing our experience in training BloombergGPT.
		