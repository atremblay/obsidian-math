---
title: "BERT Pre-training of Deep Bidirectional Transformers for Language Understanding"
tags:
    - model
    - LM
status: read
aliases:
    - "Devlin et al., 2019"
authors:
    - Jacob Devlin 
    - "Ming-Wei Chang" 
    - Kenton Lee 
    - Kristina Toutanova
url:
    - "[paper](https://arxiv.org/pdf/1810.04805)"
    - "[arxiv](https://arxiv.org/abs/1810.04805)"
published: "2018-10-11"
---

# BERT Pre-training of Deep Bidirectional Transformers for Language Understanding
###### Authors
<ul>
<li class="author">Jacob Devlin</li>
<li class="separator author">|</li>
<li class="author">Ming-Wei Chang</li>
<li class="separator author">|</li>
<li class="author">Kenton Lee</li>
<li class="separator author">|</li>
<li class="author">Kristina Toutanova</li>
</ul>

```dataview
table without ID url from "Mathematics/Machine Learning/Papers/BERT.md"
```

###### Abstract

We introduce a new language representation model called BERT, which stands for Bidirectional Encoder Representations from Transformers. Unlike recent language representation models, BERT is designed to pre-train deep bidirectional representations from unlabeled text by jointly conditioning on both left and right context in all layers. As a result, the pre-trained BERT model can be fine-tuned with just one additional output layer to create state-of-the-art models for a wide range of tasks, such as question answering and language inference, without substantial task-specific architecture modifications.   
BERT is conceptually simple and empirically powerful. It obtains new state-of-the-art results on eleven natural language processing tasks, including pushing the GLUE score to 80.5% (7.7% point absolute improvement), MultiNLI accuracy to 86.7% (4.6% absolute improvement), SQuAD v1.1 question answering Test F1 to 93.2 (1.5 point absolute improvement) and SQuAD v2.0 Test F1 to 83.1 (5.1 point absolute improvement).

Surprisingly simple. Implementation is another story, but the overall paper is quite accessible. It helps to know [[GLUE|GLUE]].

### Input 

One vector for each of these are summed up:
- Words are broken down in tokens. A basic embedding layer for size `|tokens|`
- Each token is either part of the first or second sequence, $E_A$ and $E_B$.
- Each token has a position in the whole input. $E_1$ to $E_n$. These are a bit fancy. No need to cover them here.

These vectors are the inputs going into the model. Obviously they must be of the same size if we want to add them up.

> [!blank-container]
> ![[BERT input.png]]


### Pre-training
Pre-training is done with two different tasks. 

![[BERT pre-training.png|450]]

#### Next Sentence Prediction (NSP)

Predict if the sequence B should follow sequence A. Easy to get from a standard text. The `[CLS]` vector is used in combination with a fully connected layer. This is a simply binary classification.

#### Masked Language Model (MLM)

Random tokens are either replaced by a special token `[MASK]` 70% of the time, replaced by another random token 15% of the time or kept as is 15% of the time.

The corresponding token embedding at the end is fed to a fully connected with an output size of `|tokens|`. Loss is a standard softmax.


### Fine-tuning

This is the most important image. There is a unified architecture for the input. Wether it's a pair of sequences or just one sequence. 

A pair of sequence is simply separated by the special token `[SEP]` as well as the end of the last sequence. So `[CLS]<sequence 1>[SEP]<sequence 2>[SEP]`. Depending on the task, either the `[CLS]` vector at the end is used (for simple classification) or the token embeddings are used for tasks such as identifying which part of a text contains the answer to a question (QA task) or token classification such as NER.

These vectors are fed to a tiny head. Something really light weight. Then the whole thing, LM included, is trained on the specific task.

> While the LM is used by all task, this is NOT a multitask model. It's a different model for each tasks.

![[BERT fine-tune.png|525]]

#### NER
No CRF is used. Every token output are independant. Or rather, there are not transition matrix from one token to the next. Since the transformer attends to everything, we can say that the tokens are modeled jointly, but the hidden states (entites) are not modeled jointly. 
No idea why they don't use CRF. Maybe to stay as agnostic to the task as possible.

#### SQuAD v1.1

A question and text passage are provided as input (with segment embedding 0 and 1). Predict the start and end position in the text passage (segment 1) by doing a dot product between a start and end vectors $S, E \in \mathbb{R}^H$ with every token embedding coming out of the model.

During training, the probability of a word being the start word is

$$P_i = \frac{\exp{\langle S, T_i \rangle}}{\sum_j \exp{\langle S, T_j \rangle}}$$

Similarly, the probability of a word being the end word is 

$$P_i = \frac{\exp{\langle E, T_i \rangle}}{\sum_j \exp{\langle E, T_j \rangle}}$$

During inference, we need to worry about the the best combination of $S$ and $E$.
$$\text{span}=max_{j \ge i} \left( \left \langle S, T_i \right \rangle + \left \langle E, T_j \right \rangle \right)$$

Note that this assumes that the answer is in the text and that there is only one span.


#### SQuAD v2.0

Similar as SQuAD v1.1, but it's possible that the answer is not in the text.

$$\text{span}_{null}=\max_{j \ge i}\left( \left \langle S, [CLS] \right \rangle + \left \langle E, [CLS] \right \rangle \right)$$

A non-null answer is returned when $\text{span} > \text{span}_{null} + \tau$ where $\tau$ is a threshold selected on the valid set to maximize the F1 score.

> Unclear exactly on what the F1-score is calculated. $S$ and the right place, Yes-No. Same for $E$?

### Ablation studies

![[BERT ablation results.png|400]]


### References

[[Attention is all you need]]