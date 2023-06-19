---
title: "BitFit: Simple Parameter-efficient Fine-tuning for Transformer-based Masked Language-models"
tags:
    - peft
    - transformer
    - plm
authors:
    - Elad Ben-Zaken
    - Shauli Ravfogel
    - Yoav Goldberg
status: read
url: 
    - "[arxiv](https://arxiv.org/abs/2106.10199)"
    - "[paper](https://arxiv.org/abs/2106.10199.pdf)"
aliases:
    - "Ben Zaken et al., 2021"
published: "2021-06-15"
---
```dataview
table without ID url from "Mathematics/Machine Learning/Papers/BitFit.md"
```

## TL;DR
Do not fine-tune everything, only the bias terms. Good in low data regime, otherwise a standard fine-tuning is probably better.

In a [[Attention is all you need|transformer architecture]], the *query*, *key* and *value* encoders of the layer $l$ and head $m$ are calculated as
$$
\begin{align}
\mathbf{Q}^{m,l}(\mathbf{x}) &= \textcolor{lightblue}{\mathbf{W}_q^{m,l}}\mathbf{x} + \textcolor{violet}{\mathbf{b}_q^{m,l}} \\
\mathbf{K}^{m,l}(\mathbf{x}) &= \textcolor{lightblue}{\mathbf{W}_k^{m,l}}\mathbf{x} + \textcolor{violet}{\mathbf{b}_k^{m,l}} \\
\mathbf{V}^{m,l}(\mathbf{x}) &= \textcolor{lightblue}{\mathbf{W}_v^{m,l}}\mathbf{x} + \textcolor{violet}{\mathbf{b}_v^{m,l}} \\
\end{align}
$$
To learn a new task, only update the biases $\textcolor{violet}{\mathbf{b}}$ and keep the weights $\textcolor{lightblue}{\mathbf{W}}$ fixed. Same thing goes for the MLP and layer-norm LN.

$$
\begin{align}
\mathbf{h}_1^{l} &= \text{att}\left(
	\mathbf{Q}^{1,l}, \mathbf{K}^{1,l}, \mathbf{V}^{1,l},
	\ldots,   
	\mathbf{Q}^{m,l}, \mathbf{K}^{m,l}, \mathbf{V}^{m,l}
\right)\\

\mathbf{h}_2^{l} &= \text{Dropout}\left(
	\textcolor{lightblue}{\mathbf{W}_{m_1}^{l}}\mathbf{h}_1^l + \textcolor{violet}{\mathbf{b}_{m_1}^{l}} 
\right)\\

\mathbf{h}_3^{l} &= \textcolor{lightblue}{\mathbf{g}_{LN_1}^l} \odot \frac{(\mathbf{h}_2^{l} + \mathbf{x})-\mu}{\sigma} + \textcolor{violet}{\mathbf{b}_{LN_1}^{l}}\\

\mathbf{h}_4^{l} &= \text{GELU}\left(\textcolor{lightblue}{\mathbf{W}_{m_2}^{l}}\mathbf{h}_3^l + \textcolor{violet}{\mathbf{b}_{m_2}^{l}} \right)\\

\mathbf{h}_5^{l} &= \text{Dropout}\left(
	\textcolor{lightblue}{\mathbf{W}_{m_3}^{l}}\mathbf{h}_4^l + \textcolor{violet}{\mathbf{b}_{m_3}^{l}} 
\right)\\

\text{out}^{l} &= \textcolor{lightblue}{\mathbf{g}_{LN_2}^l} \odot \frac{(\mathbf{h}_5^{l} + \mathbf{h}_3^{l})-\mu}{\sigma} + \textcolor{violet}{\mathbf{b}_{LN_2}^{l}}\\
\end{align}
$$
No reason to think this could not apply to any other transfer learning objective.


> [!blank-container|left-small]
> ![[../Research/Images/BitFit/BitFit_fig1.png|300]]
> > *Figure 1*: Change in bias components (RTE task).

The change in bias vector, defined as 
$$
\frac{1}{dim(\mathbf{b})}\lVert\mathbf{b}_0 - \mathbf{b}_F\lVert_1
$$
that is, the average absolute change, across its dimensions, between the initial LM values $\mathbf{b}_0$ and its fine-tuned values $\mathbf{b}_0$ . The larget changes are in the top layers. 

> [!blank-container|right-small]
> ![[../Research/Images/BitFit/BitFit_fig2.png|300]]
> > *Figure 2*: Comparison of BitFit and Full-FT with $\text{BERT}_{\text{BASE}}$ exact match score on SQuAD validation set.


## Abstract

We introduce BitFit, a sparse-finetuning method where only the bias-terms of the model (or a subset of them) are being modified. We show that with small-to-medium training data, applying BitFit on pre-trained BERT models is competitive with (and sometimes better than) fine-tuning the entire model. For larger data, the method is competitive with other sparse fine-tuning methods. Besides their practical utility, these findings are relevant for the ques- tion of understanding the commonly-used pro- cess of finetuning: they support the hypoth- esis that finetuning is mainly about expos- ing knowledge induced by language-modeling training, rather than learning new task-specific linguistic knowledge.