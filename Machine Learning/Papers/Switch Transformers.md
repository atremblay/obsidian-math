---
title: "Switch Transformers - Scaling to Trillion Parameter Models with Simple and Efficient Sparsity"
url:
    - "[paper](https://arxiv.org/pdf/2101.03961.pdf)"
    - "[arxiv](https://arxiv.org/abs/2101.03961)"
authors:
    - William Fedus
    - Barret Zoph
    - Noam Shazeer
published: "2021-01-11"
tags:
    - transformer
    - lm
status: "to read"
---


#### Abstract

In deep learning, models typically reuse the same parameters for all inputs. Mixture of Experts (MoE) defies this and instead selects different parameters for each incoming example. The result is a sparsely-activated model -- with outrageous numbers of parameters -- but a constant computational cost. However, despite several notable successes of MoE, widespread adoption has been hindered by complexity, communication costs and training instability -- we address these with the Switch Transformer. We simplify the MoE routing algorithm and design intuitive improved models with reduced communication and computational costs. Our proposed training techniques help wrangle the instabilities and we show large sparse models may be trained, for the first time, with lower precision (bfloat16) formats. We design models based off T5-Base and T5-Large to obtain up to 7x increases in pre-training speed with the same computational resources. These improvements extend into multilingual settings where we measure gains over the mT5-Base version across all 101 languages. Finally, we advance the current scale of language models by pre-training up to trillion parameter models on the "Colossal Clean Crawled Corpus" and achieve a 4x speedup over the T5-XXL model.
