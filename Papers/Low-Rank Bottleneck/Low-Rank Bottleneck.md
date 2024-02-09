---
title: "Low-Rank Bottleneck in Multi-head Attention Models"
authors:
    - Srinadh Bhojanapalli
    - Chulhee Yun
    - Ankit Singh Rawat
    - Sashank J. Reddi
    - Sanjiv Kumar
url:
    - "[arxiv](https://arxiv.org/abs/2002.07028)"
    - "[paper](https://arxiv.org/pdf/2002.07028.pdf)"
published: "2020-02-17"
tags:
    - transformer
---

# Low-Rank Bottleneck in Multi-head Attention Models
###### Authors
<ul>
<li class="author">Srinadh Bhojanapalli</li>
<li class="separator author">|</li>
<li class="author">Chulhee Yun</li>
<li class="separator author">|</li>
<li class="author">Ankit Singh Rawat</li>
<li class="separator author">|</li>
<li class="author">Sashank J. Reddi</li>
<li class="separator author">|</li>
<li class="author">Sanjiv Kumar</li>
</ul>

```dataview
table without ID url from "Mathematics/Machine Learning/Papers/Low-Rank Bottleneck.md"
```

#### Abstract

Attention based Transformer architecture has enabled signiﬁcant advances in the ﬁeld of natural language processing. In addition to new pre-training techniques, recent improvements crucially rely on working with a relatively larger embedding dimension for tokens. Unfortunately, this leads to models that are prohibitively large to be employed in the downstream tasks. In this paper we identify one of the important factors contributing to the large embedding size requirement. In particular, our analysis highlights that the scaling between the number of heads and the size of each head in the current architecture gives rise to a low-rank bottleneck in attention heads, causing this limitation. We further validate this in our experiments. As a solution we propose to set the head size of an attention unit to input sequence length, and independent of the number of heads, resulting in multi-head attention layers with provably more expressive power. We empirically show that this allows us to train models with a relatively smaller embedding dimension and with better performance scaling.

## 1 Introduction

Attention based architectures, such as Transformers, have been effective for sequence modelling tasks such as machine translation [Gehring et al., 2017, Vaswani et al., 2017], question answering, sentence classiﬁcation [Radford et al., 2018, Devlin et al., 2018] and document generation [Liu et al., 2018]. These models have emerged as better alternatives to the recurrent models - RNNs [Sutskever et al., 2014], LSTMs [Hochreiter and Schmidhuber, 1997] and GRUs [Cho et al., 2014]. This is mainly due to their feed forward structure, which removes the sequential processing bottleneck for sequence data, making them easier to train compared to the recurrent models. Self attention models also have found applications in vision [Wang et al., 2018], adversarial networks [Zhang et al., 2018], reinforcement learning [Zambaldi et al., 2018, Li, 2017] and speech recognition [Chiu et al., 2018].

Recent advances in using the self attention models in natural language tasks have been made by ﬁrst using a language modeling task to pre-train the models and then ﬁne tuning the learned models on speciﬁc downstream tasks. Radford et al. [2018] and Devlin et al. [2018] used Transformers to pre-train a language model and showed that the ﬁne tuned model outperforms LSTMs on many natural language understanding and question answering tasks. For example, BERT [Devlin et al., 2018], a 24 layer transformer model, is shown to achieve the state of the art performance on several NLP tasks, including on the SQuAD dataset. These advances, in addition to novel pre-training tasks, relied on bigger models with a larger embedding size. BERT model uses an embedding size of 1024 [Devlin et al., 2018]; GPT-2 uses models with embedding size up to 1600 [Radford et al., 2019].

A single Transformer block consists of two key components: a multi-head self attention layer followed by a feed forward layer [Vaswani et al., 2017]. A single head in a multi-head attention layer, computes self attention between the tokens in the input sequence, which it then uses to compute a weighted average of embeddings for each token. Each head projects the data into a lower dimensional subspace, and computes the self attention in this subspace. This projection size for each head is commonly referred to as the head size.
 
 
8
# heads
336M
# params
90.89±0.15
SQuAD - F1
SQuAD - EM 84.1±0.34

MNLI

85±0.2

16
336M
90.61±0.14
83.75±0.27
84.5±0.4

32
336M
90.45±0.08
83.48±0.13
84.4±0.2

Table 1: Performance of BERTLARGE [Devlin et al., 2018], a 24 layer Transformer with an embedding size of 1024, suffers with the increasing number of heads after 8 heads.

To keep the number of parameters ﬁxed in the attention layer regardless of the number of heads, the prevalent heuristic is to scale the head size with 1/(number of heads). This heuristic was initially proposed in Vaswani et al. [2017] and has become a de facto standard heuristic in multi-head attention models [Radford et al., 2018, Devlin et al., 2018]. However, increasing the number of heads decreases the head size, decreasing the expressive power of individual heads. We prove that reducing the head size to a value below the input sequence length harms the representation power of each head (see Theorem 1). This is because a smaller head size introduces a rank constraint on the projection matrices in each head, and limits their representation power. We indeed notice this effect in practice: while the performance improves with increasing the number of heads in the beginning [Devlin et al., 2018], we notice a drop in the performance once the number of heads increases beyond a certain threshold, as seen in Table 1 and Fig. 1 (see also Table 4(A) in Vaswani et al. [2017]).


> [!blank-container]
> ![[Images/figure1.svg]]
> > **Figure 1**: Performance of Transformers trained with the prevalent head size heuristic (dp = d/h) (baseline) compared with the ﬁxed head size (dp = 32) on a language modeling task (LM1B) on the test set. We train baseline models with embedding sizes from 256 to 512. We train the ﬁxed head size models with a ﬁxed embedding size of 256 and a head size of 32, and vary the number of heads from 4 to 70, while matching the number of parameters. The plots clearly indicate that ﬁxing the head size allows us to train Transformers with a smaller embedding size (plot (b)), and with a better scaling of performance (plot (a)). Note that for perplexity lower values are better.
> 

In order to avoid hurting the performance, the existing models allow for multiple heads by increasing the embedding size, which in turn increases the head size. However, larger embedding size, in addition to increasing the number of parameters, makes it expensive to use the model and the learned embeddings in downstream tasks, as the downstream model sizes scale with the embedding size of the tokens. For example, the inference time and memory required in retrieval tasks typically increases linearly with the embedding size.

In this paper we propose setting the head size of attention units to input sequence length. While this is a simple hyper-parameter change in the Transformer architecture, we show that it is important to set this value appropriately to avoid the low-rank bottleneck (see Theorem 1), and to improve the representation power (see Theorem 2). This ﬁxed head size is also independent of both the number of heads and the embedding size of the model. This allows us to train models with a relatively smaller embedding size (hence fewer parameters) without affecting the head size. Another advantage of the ﬁxed head size is that unlike the standard setting which requires the number of heads to be a factor of the embedding size, we are free to set an arbitrary number of heads as required for the task.

Interestingly, we note that this simple yet novel approach of ﬁxing the head size in multi-head Transformers results in empirically superior performance. We evaluate Transformers trained with this ﬁxed head size on language modeling (LM1B dataset), natural language inference (MNLI dataset) and question answering tasks (SQuAD dataset). We show that ﬁxing the head size allows us to train Transformers with a better performance scaling and smaller embedding size. We show that with the ﬁxed head size Transformers trained with an embedding size of 512 can match the performance of the BERTLARGE[Devlin et al., 2018], a Transformer with an embedding size of 1024 (see Fig. 2). We further present experimental results evaluating the effect of different choices of the head size and the embedding size in Section 4.

Our contributions in this paper lie in identifying and rigorously proving the low rank bottleneck in multi-head attention models, and showing that ﬁxing the head size to input sequence length results in a strictly better model, both theoretically and empirically. The contributions of this paper are summarized below.

- We analyze the representation power of the multi-head self attention layer and prove the low-rank bottleneck the head size places on the attention units (Theorem 1).
- We propose to set the head size to input sequence length, and show that ﬁxing the head size strictly improves the expressive power of the multi-head attention layers compared to the standard heuristic for setting the head size (Theorem 2). This allows us to both increase the number of heads per layer and decrease the embedding size, without hurting the performance. We develop a novel construction based approach to prove this result, which can potentially be useful in analyzing other variants of the Transformer architecture. 
- We experimentally show that with a ﬁxed head size, Transformers can be trained with better performance scaling and a smaller embedding size on three standard NLP tasks.

### 1.1 Related Works

Given the signiﬁcance of self attention models, there has been work trying to both improve the performance and speed up the computation in Transformers. Ott et al. [2018] and You et al. [2019] reduce precision and use large batch training to reduce the training time of the attention models. Child et al. [2019] propose sparse self attention models to speed up the computation in the attention layer for long sequence data generation tasks. They show that these sparse attention models can be trained on tasks with sequence length greater than 10k without sacriﬁcing the accuracy. Dehghani et al. [2018] propose a depth recurrent Transformer network that reuses the parameters across layers. They show that this modiﬁcation makes the Transformer networks Turing complete even with ﬁnite precision weights. Yang et al. [2019] propose a new way to increase the effective sequence length that the Transformer attends to, by reusing the intermediate embeddings across sequences. They show that the modiﬁed architecture performs better on tasks that require computing context over longer sequence lengths. We note that most of these modiﬁcations rely on the multi-head self attention, the same building block of the Transformers. Our work is studying this basic multi-head attention layer, and suggesting a new way to set the head size, which can be easily applied along with any of the above architectural modiﬁcations.

Wu et al. [2019] propose to replace the self-attention layer with lightweight dynamic convolutions and show improved performance on machine translation and language modeling. Even though the resulting model has faster inference time, it still needs to use a large embedding size (1024), as big as the original attention models. We believe the techniques in this paper can be combined with these results to realize both smaller embedding size and faster inference time.

Sun et al. [2019] perform neural architecture search using evolutionary methods on sequence to sequence models and ﬁnd an evolved transformer architecture, which in addition to multi-head attention units, has convolution ﬁlter and gated linear units. Our proposed modiﬁcations stay closer to Transformers in spirit and can be used as seed units for this architecture search.

Yang et al. [2017] have studied the effect of rank constraint caused by the small projection sizes in computing the softmax loss. The situation in self attention layers is a bit different. While the expressive power of each head reduces with the decreasing head size, at the same time we are increasing the number of heads, which can potentially negate this and increase the overall capacity of the layer. As we show in Theorem 2, the prevalent head size heuristic indeed limits the expressive power of the multi-head attention layer.

Yun et al. [2019] studied the representation power of Transformers and showed that they are universal approximators of sequence to sequence functions. However they do not study the low rank bottleneck caused by the prevalent head size heuristic and its connection to the embedding size.

Voita et al. [2019], Michel et al. [2019] study the importance of different heads in an attention layer. They observe that, during inference, many of the heads in each layer can be pruned away with a little effect on the prediction. However, they still need multiple heads during the training.

Child et al. [2019], Correia et al. [2019] impose sparsity structure on the attention layer during training to improve both interpretability and performance. Fixing the head size will in fact make it easier to learn such sparsity patterns, as a low rank constraint does not allow a head to express all possible sparsity patterns. Combining these techniques can hence potentially enable training of sparse attention models with a smaller embedding size.

## 2 Transformer Architecture and Analysis

In this section, we present the Transformer architecture and analyze the representation power of the multi-head self attention, a key component of the Transformer block.

The input to a Transformer network is a sequence of n tokens. Typically, each token is converted into a token embedding of dimension d by an embedding layer. We let X ∈ Rd×n be the embedding matrix corresponding to the n tokens in the input sequence.

### 2.1 Single-Head Attention

The Transformer block is a combination of a self attention layer followed by a feed forward layer [Vaswani et al., 2017]. Both layers have a skip connection and use Layer Normalization (LN) [Ba et al., 2016]. In particular, for token embeddings X, the dot product attention is computed as follows.

Attention(X) = WvX · Softmax

(cid:20) (WkX)T (WqX)
√
dk

(cid:21)

= WvX · P.

(1)

Here Wq ∈ Rdq×d, Wk ∈ Rdk×d and Wv ∈ Rdv×d represent the projection matrices associated with the query, key and value respectively in an attention unit [Vaswani et al., 2017]. For a single-head attention unit, we have dq = dk = dv = d. In the dot-product attention (cf. (1)), P aims to capture the context of the input for a given token based on the remaining tokens in the input sequence. Subsequently, the output of the attention layer takes the following form.

LN (X + Wo · Attention(X)) ,

(2)

where LN(·) represents the layer-normalization operation. Given the attention module, as deﬁned in (1), it is natural to question its ability to represent arbitrary contexts P for a given input sequence X.

In the following result we establish that for a large enough projection size an attention unit can represent any data pair (X, P). We also show that the model cannot represent arbitrary context when d is smaller than n, creating a low-rank bottleneck.

Theorem 1 (Representation Theorem). If dq = dk = d ≥ n, then given any full column rank matrix X ∈ Rd×n and an arbitrary n × n positive column stochastic matrix P, there always exists d × d projection matrices Wq and Wk such that

Softmax

(cid:20) (WkX)T (WqX)
√
dk

(cid:21)

= P.

(3)

If dq = dk = d < n, there exist X and P such that (3) does not hold for all Wq and Wk.

This result shows that the projection dimension dq = dk = d needs to be larger than the sequence length n for the attention unit to be able to represent any desired context P. Even though this result describes a single example sequence case, it highlights a fundamental property of the model architecture that decreasing the projection size below a certain threshold introduces a bottleneck.

Proof of Theorem 1. d ≥ n case. To prove the ﬁrst part of the result, we present an explicit construction of Wk and Wq which allows us to generate P from X using the dot product attention. Since X has full column rank, there exists a left inverse X† = (XT X)−1XT ∈ Rn×d such that X†X = In. Let Wk = ˜WkX† and Wq = ˜WqX†. Then

XT WT

k WqX = XT (X†)T ˜WT

k

˜WqX†X = In · ˜WT
k

˜Wq · In
˜Wq = ˜Wkq.

= ˜WT
k

(4)

Now that the above choice of Wq and Wk has handled the dependence on X, we will choose a ˜Wkq depending on P and ﬁnish the construction. Below we express the Softmax operation on the query and key inner products. Note that the Softmax here is a columnwise operator computing the attention scores for each query. By using (4), we obtain that

Softmax

(cid:20) (WkX)T (WqX)
√
dk

(cid:21)

= Softmax

(cid:35)

(cid:34) ˜Wkq√

dk

= exp

(cid:33)

(cid:32) ˜Wkq√

dk

· D−1

˜Wkq

,

where D ˜Wkq

is an n × n diagonal matrix such that

(D ˜Wkq

)ii =

n
(cid:88)

j=1

exp

(cid:32)

( ˜Wkq)ji√
dk

(cid:33)

(cid:32)

(cid:32)

=

1T exp

( ˜Wkq)
√
dk

(cid:33)(cid:33)

.

i

4

Hence, we can establish the desired result by showing that there always exists a ˜Wkq that satisﬁes the following ﬁxed point equation.

exp

(cid:33)

(cid:32) ˜Wkq√

dk

= P · D ˜Wkq

.

Given P, to construct such a ˜Wkq, we pick an arbitrary positive diagonal matrix D0, and set

˜Wkq =

(cid:112)

dk · log (P · D0) .

(5)

(6)

Since P is a positive matrix, such a ˜Wkq always exists. Next, we verify that this construction indeed satisﬁes the ﬁxed point equation (cf. (5)). Note that

D ˜Wkq

= Diag

(cid:32)

(cid:32)

1T exp

(cid:33)(cid:33)

( ˜Wkq)
√
dk

= Diag (cid:0)1T P · D0

(cid:1) = D0.

(7)

The last equation follows from the fact that P is a column stochastic matrix. Now, using (6) and (7),

exp

(cid:33)

(cid:32) ˜Wkq√

dk

= P · D0 = P · D ˜Wkq

.

This completes the ﬁrst part of the proof.

d < n case. Consider the case of d = 1 and n = 2. Then X ∈ R1×2 and Wq and Wk ∈ R1×1. Let X = [1, 0]. Then

Softmax

(cid:20) (WkX)T (WqX)
√
dk

(cid:21)

= Softmax

(cid:20) [1, 0]T WT
k Wq[1, 0]
√
dk

(cid:21)

= Softmax

(cid:20)(cid:20)WkWq
0

(cid:21)(cid:21)
0
0

.

This matrix clearly cannot be used to generate P that have distinct elements in the second column, e.g., P =
(cid:21)
(cid:20)0.5 0.75
.
0.5 0.25

### 2.2 Multi-Head Attention

As discussed in Section 2.1, an attention unit updates the embedding of an input token based on a weighted average of the embeddings of all the tokens in the sequence, using the context P (cf. (1)). Vaswani et al. [2017] proposed Multi-Head attention mechanism that increases the representation power of an attention layer, where multiple attention units operate on different low dimensional projections of the input, with each attention unit being referred to as a head. This is followed by concatenation of the outputs from different heads. In particular, the computation inside a Multi-Head attention with h heads takes the following form:

head(X)i = Wi

vX · Softmax (cid:2)(Wi

kX)T (Wi

qX)/

√

(cid:3) ∈ R d

h ×n

d
h

MultiHead(X) = Concat[head(X)1, · · · , head(X)h] ∈ Rd×n.

The output of the Multi-head attention layer then becomes

Z = LN (X + Wo · MultiHead(X)) ,

(8)

h × d matrices. Therefore, each head projects the input onto a d

where Wo ∈ Rd×d. For a model with h heads, the query, key and value projection matrices {Wi v} are d h -dimensional subspace to compute the context, and keeps the number of parameters ﬁxed per layer. Using MultiHead has resulted in empirically better performance over the single head attention layer [Vaswani et al., 2017].

k} and {Wi

q}, {Wi

5

(a) LM1B

(b) LM1B

### 2.3 Low-Rank Bottleneck

While increasing the number of heads seemingly gives the model more expressive power, at the same time we are reducing the head size, which can decrease the expressive power. When the number of heads h is larger than d n , the attention unit inside each head projects onto a dimension smaller than n, creating a low-rank bottlenck and loses its ability to represent arbitrary context vectors (cf. Theorem 1). Interestingly, this is consistent with the empirical observation in Table 1 that increasing h beyond 8 results in performance degradation in BERTLARGE [Devlin et al., 2018]; note that d = 1024 and n = 128 for most of the pre-training phase of BERTLARGE.

Since the sequence length is ﬁxed from the data/task at hand, the only way to increase the number of heads without introducing the low-rank bottleneck is by increasing the embedding size d. This is a fundamental limitation of the currently dominant head size heuristic, that we need to increase the embedding size in order to support more heads.

Unfortunately, increasing the embedding size leads to higher computation and memory requirements to train and store the model. Further, since it is common to use learned embeddings from Transformer based models for downstream tasks [Devlin et al., 2018], larger embedding size increases the model size and computation required for all the downstream tasks as well.

## 3 Fixed Multi-Head Attention

In this section we propose to ﬁx the head size of the Transformer, which allows us to enjoy the advantage of higher expressive power of multiple heads without requiring the embedding size to be large. The key is to decouple the dependency between the projection size in a head and the embedding size of the model. The projection matrices now project onto subspaces of a ﬁxed dimension dp irrespective of the number of heads h. This approach where dp is independent of d and h leads to the following attention mechanism.

ﬁxedhead(X)i = Vi

vX · Softmax (cid:2)(Vi

kX)T (Vi

qX)/

√

(cid:3) ∈ Rdp×n

dp

FixedMultiHead(X) = Concat[ﬁxedhead(X)1, · · · , ﬁxedhead(X)h] ∈ Rdp·h×n.

Note that the projection matrices used here {Vi
output of this new multi-head attention layer takes the following form.

k} and {Vi

q}, {Vi

v} are dp × d matrices. With Vo ∈ Rd×h·dp , the

Z = LN (X + Vo · FixedMultiHead(X)) .

This modiﬁcation makes each attention head more similar to a hidden unit in a feed forward network or a ﬁlter in a convolutional network, and allows us to vary the number of heads without worrying about reducing the representation power per head. The downside is, unlike the standard MultiHead, the number of parameters per layer increases with the number of heads. However, this modiﬁcation allows us to train a model with a smaller embedding size without a low-rank bottleneck, ultimately allowing us to reduce the total number of parameters in the model.

### 3.1 MultiHead vs. FixedMultiHead Attention

Given a MultiHead layer, we can always represent it using a FixedMultiHead layer, whenever we have the head size dp ≥ d/h. While this shows that increasing the number of heads h beyond d/dp makes individual heads of the FixedMultiHead as expressive as the ones in the MultiHead, it is not obvious if FixedMultiHead is strictly more expressive. Can the FixedMultiHead layer represent functions that the standard MultiHead layer can not represent? In this subsection we show that indeed, in the multi-head regime, the FixedMultiHead layer is strictly better than the standard MultiHead layer in terms of expressive power.

Consider the standard multi-head attention units in (8).

fW(X) = Wo · MultiHead(X).

We denote the collection of all parameter matrices as W. Similarly, consider the function represented by the ﬁxed head size attention units:

gV(X) = Vo · FixedMultiHead(X).

Let V be the collection of all these parameter matrices. We deﬁne F and G to be the class of functions fW(·) and gV(·), respectively. As noted above, if dp ≥ d/h, we have F ⊂ G.

The following theorem shows that even for simple examples in G, functions in F fail to represent them; this already shows that F is a strict subset of G.

Theorem 2. Let n ≥ 2, d ≥ dp, and h > d/dp. Consider a FixedMultiHead attention layer gV(·) with parameters that satisfy the following conditions:


k)T Vi

q = U, for all i = 1, . . . , h, where U is a rank-dp matrix.

Vo ×




V1
v
...
Vh
v


 is full rank, and (Vi


Then, for any fW ∈ F, there exists X ∈ Rd×n such that fW(X) (cid:54)= gV(X).

Because (cid:107)fW(X) − gV(X)(cid:107) is a continuous function of X, existence of such an X implies that the integral of the norm of difference (i.e., approximation error) is strictly positive. We note that the assumptions on Vi q in the above Theorem are made to provide a simple and constructive proof; in fact, failure of MultiHead (F) to represent such simple attention layers suggests that the situation is likely worse for more complex functions.

k and Vi

Theorem 2 shows that the expressive power of the FixedMultiHead attention function class is strictly superior to the standard MultiHead attention function class. Hence the heuristic of reducing the head size with the number of heads is limiting the expressive power of MultiHead, whereas using the ﬁxed head size will increase the expressive power of the attention layers.

## 4 Experiments

The goal of this section is to show that setting the head size in a principled way leads to better performance than using the prevalent heuristic. We again note that while this is a simple hyper-parameter change to the Transformer, setting this to input sequence length as shown in our analysis, allows us to train better models with a smaller embedding size.


(a) SQuAD F1

(b) SQuAD EM

(c) MNLI





In this section we present our experiments on three standard NLP tasks, language modeling (LM1B), question answering (SQuAD), and sentence entailment (MNLI), to demonstrate: 1) Increasing the number of heads in Trans- formers beyond a certain point hurts the performance with the prevalent head size heuristic, but always helps with the ﬁxed head size attention layers; 2) Decoupling the head size from embedding size allows us to train models with a smaller embedding size; and 3) Setting the head size appropriately in the Transformers allows us to train models with a better performance scaling. We ﬁrst describe our experimental setup followed by our results and ablation studies on the proposed modiﬁcations.

### 4.1 Setup and Datasets

For the language modeling task we use the one billion word benchmark dataset (LM1B) [Chelba et al., 2013]. This dataset has around 30M training examples and around 300k examples in the test set. We use a sub-word tokenizer with



8
# heads
168M
# params
89.6±0.17
SQuAD - F1
SQuAD - EM 82.73±0.21
83.5±0.2

MNLI

12
193M
90.25±0.21
83.18±0.24
84.2±0.2

16
218M
90.43±0.14
83.59±0.06
83.9±0.2

32
319M
90.95±0.14
84.4±0.29
84.9±0.2

(A) Increasing number of heads

32
head size
130M
# params
88.53±0.06
SQuAD - F1
SQuAD - EM 81.19±0.21
82.5±0.1

MNLI

64
142M
89.51±0.15
82.41±0.32
83.4±0.3

128
168M
89.6±0.17
82.73±0.21
83.5±0.2

256
218M
90.33±0.23
83.36±0.48
83.9±0.2

(B) Increasing head size

Table 2: Ablation studies on SQuAD and MNLI: (A) 24 layer Transformer with a ﬁxed head size of 128 and 512 embedding size shows an improvement in the accuracy with the increasing number of heads. (B) The ﬁxed head size model with 512 embedding size and 8 heads shows an improvement in accuracy with the increasing head size. This shows that indeed head size is an important capacity controlling parameter in the self attention architecture.

32k vocab and cap the input to 256 sequence length. We train a 6 layer Transformer model with the ADAM optimizer using the tensor2tensor library [Vaswani et al., 2018]. The detailed experimental setting is presented in Section C.

Multi-Genre Natural Language Inference (MNLI) is a sentence level entailment task, designed to test natural language understanding [Williams et al., 2018]. Given a premise sentence and a hypothesis sentence, the goal is to predict whether hypothesis entails, contradicts or is neutral to the premise. We report the classiﬁcation accuracy for this task. Stanford Question Answering Dataset (SQuAD) is a question answering dataset, where given a paragraph and a question, the goal is to predict the sequence of words in the paragraph that constitute the answer to the question [Rajpurkar et al., 2016]. This is a harder word level task, compared to the sentence classiﬁcation task. We report both Exact Match (EM) and F1 scores for this task. All results in this section are reported on the Dev set, which has not been used in any experimental choices in this paper.

For these latter two tasks, we follow the two stage approach of ﬁrst pre-training on a language modeling task and then ﬁne-tuning the models on the task data. We follow the same experimental setup for both pre-training and ﬁne-tuning as BERT [Devlin et al., 2018], and use their codebase[^1]. We ﬁrst pre-train our models using the masked language model and the next sentence prediction objectives, and then ﬁne tune the pre-trained model for individual tasks [Devlin et al., 2018]. For pre-training we use English Wikipedia and BooksCorpus dataset [Zhu et al., 2015]. The input to the models is tokenized using the WordPiece representation with 30000 tokens in the vocabulary. We present the key experiment choices in Section C, and refer the reader to Devlin et al. [2018] for a complete description of the setup. Choice of the head size. Our proposed modiﬁcation introduces head size dp as a new model hyper-parameter. We choose head size to be 128 for our BERT experiments, as most of the pre-training is done with 128 sequence length data. While we have ablation studies (cf. Table 2(B)) showing bigger head size improves the performance, there is a tradeoff between increasing the head size vs number of heads vs layers. We found that having sufﬁciently large head size, e.g., matching the pre-training sequence length, is better than having a larger embedding size.

[^1]: https://github.com/google-research/bert

### 4.2 Results

For our ﬁrst set of experiments we want to see if Transformers trained with a ﬁxed head size and a smaller embedding size can match the performance of training with the standard head size heuristic but with a larger embedding size. As a baseline for the language modeling task, we train Transformers with the embedding size increasing from 256 to 512 with different number of heads. We train the ﬁxed head size models with a ﬁxed embedding size of 256 and a head size of 32, with an increasing number of heads from 4 to 70. We notice that Transformers with a ﬁxed head size and an embedding size of 256 have better performance than the baseline models with an embedding size of 512 (see Fig. 1). We repeat the similar experiment on the other two tasks, where for baseline we train BERTLARGE, a 24 layer, 16 head Transformer with the standard head size heuristic, with embedding sizes from 512 to 1024. We compare it with the ﬁxed head size model, with an embedding size of 512 and a head size of 128, with an increasing number of heads from 8 to 32. We again notice that the Transformers trained with a ﬁxed head size and 512 embedding size have better performance than the baseline, BERTLARGE (see Fig. 2).

> [!blank-container]
> ![[Images/figure2.svg]]
> > **Figure 2**: Comparison of 24 layer Transformer models trained with the prevalent head size heuristic BERTLARGE (baseline) vs. the ﬁxed head size model on the SQuAD and MNLI dev sets. We vary the embedding size of the baseline models from 512 to 1024. We train the ﬁxed head size models with a ﬁxed embedding size of 512 and a head size of 128, with a varying number of heads from 8 to 32, while matching the number of parameters. Fixing the head size allows us to train models with a smaller embedding size of 512 and with a better performance.



Note that simply trying to increase the head size of the Transformers by decreasing the number of heads does not improve the performance, as decreasing the number of heads reduces the expressive power of the model (see Fig. 4 in the Appendix). Hence, both the head size and the number of heads needs to be set high enough for better performance.

### 4.3 Ablation

Increasing heads. From Table 1 and Fig. 1a we can see that increasing the number of heads hurts the performance of the Transformer after a certain number. We repeat the same experiments with the ﬁxed head size Transformer, and present the results in Table 2(A) and Fig. 3a. The results show that the performance of the modiﬁed model improves monotonically as the number of heads increase. This is because the model capacity (a function of the head size) is no longer reduced with the increasing number of heads. Increasing head size. In Table 2(B) and Fig. 3b, we present comparisons between models with different head sizes. This shows that the gains in the performance of the ﬁxed head size models indeed come from adjusting the head size of the query, key and value layers in the attention unit. The table shows a clear trend of better performance with a larger head size, suggesting that it indeed is an important factor in the performance of the attention models.


> [!multi-column]
> > [!blank-container]
> > ![[Images/figure3a.svg]]
> > (a)
>   
> > [!blank-container]
> > ![[Images/figure3b.svg]]
> > (b)
> 
> > **Figure 3**: Ablation studies on LM1B: (a) We ﬁx the embedding size of all the models to 256 and vary the capacity of Transformers trained with the prevalent head size heuristic (baseline) by increasing the size of the feedforward layers. For the ﬁxed head size models we ﬁx the head size to 32, so 8 head ﬁxed head size model is the same as the 8 head baseline model. We notice that again with the standard heuristic increasing the number of heads beyond 16 hurts the performance, whereas with a ﬁxed head size increasing the number of heads monotonically improves the performance. (b) We show the effect of head size on the performance with different number of heads. Both plots clearly show the advantage in having an additional way to tune the capacity of Transformers with a ﬁxed embedding size.

## 5 Conclusion

In this paper we studied the representation power of the multi-head self attention models and proved the low-rank bottleneck that results from a small head size in the multi-head attention. We showed that the larger embedding size used in the current models is a consequence of this low-rank bottleneck in multi-head attention layers. We propose to instead use ﬁxed head size attention units, with the head size set to input sequence length, to avoid this bottleneck. We showed that it allows us to increase the number of heads without increasing the embedding size. As a consequence we are able to train Transformers with a smaller embedding size and fewer parameters, with better performance. In the future, it will be interesting to experiment with varying head sizes within an attention block and across layers. This requires further understanding of the role of each layer in computing the context, which is an interesting direction for
the future work.


## A Notation

Embedding size
Number of layers
Number of heads
Sequence length
Vocab size
Head size

d
l
h
n
v
dp

## B Proofs



Proof of Theorem 2. First let us rewrite the MultiHead and FixedMultiHead layers as follows. The MultiHead layer
can be rewritten as

fW(X) = Wo · MultiHead(X) =

h
(cid:88)

i=1

Wi

oWi

vX · Softmax (cid:2)(Wi

kX)T (Wi

qX)/

√

(cid:3) ,

d
h

where Wi
matrices as W.

o are d × d/h matrices and Wi

v, Wi

k, and Wi

q are d/h × d matrices. We denote the collection of all parameter

Similarly, rewrite the ﬁxed head size attention layer as

gV(X) = Vo · FixedMultiHead(X) =

h
(cid:88)

i=1

Vi

oVi

vX · Softmax (cid:2)(Vi

kX)T (Vi

qX)/

√

(cid:3) ,

dp

where Vi

o ∈ Rd×dp , and Vi

v, Vi

k, Vi

q ∈ Rdp×d. Let V be the collection of all these matrices.

The outline of the proof is basically a case analysis: we divide possible values of W into three categories, and show

in each case that there exists a X such that fW(X) (cid:54)= gV(X). Here are the three cases:

• Case 1: (cid:80)h

i=1 Wi

oWi

• Case 2: (cid:80)h

oWi
is not skew-symmetric.

i=1 Wi

v (cid:54)= (cid:80)h
v = (cid:80)h

i=1 Vi

i=1 Vi

oVi
v.
v, and there exists i ∈ {1, . . . , h} such that U/(cid:112)dp−(Wi

oVi

k)T (Wi

q)/(cid:112)d/h

• Case 3: (cid:80)h

i=1 Wi

oWi

v = (cid:80)h

i=1 Vi

oVi

v, and all U/(cid:112)dp − (Wi

k)T (Wi

q)/(cid:112)d/h are skew-symmetric.

In the ﬁrst case, we can choose any v such that ((cid:80)h

Case 1.
(cid:2)v v . . . v(cid:3). Then, note that for any column stochastic matrix P, we have XP = X. Therefore,

v − (cid:80)h

i=1 Wi

i=1 Vi

oWi

oVi

v)v (cid:54)= 0. Choose X = v1T =

h
(cid:88)

i=1

h
(cid:88)

i=1

=

Wi

oWi

vX · Softmax (cid:2)(Wi

kX)T (Wi

qX)/

√

d/h(cid:3) −

h
(cid:88)

i=1

Vi

oVi

vX · Softmax (cid:2)(Vi

kX)T (Vi

qX)/

√

(cid:3)

dp

Wi

oWi

vX −

h
(cid:88)

i=1

Vi

oVi

vX = (

h
(cid:88)

i=1

Wi

oWi

v −

h
(cid:88)

i=1

Vi

oVi

v)v1T (cid:54)= 0.

In cases where (cid:80)h

v is at most rank d/h, it follows that all columns in Wi

Case 2.
oWi
Wi
for any v (cid:54)= 0, {Wi
oWi
combination of d/h column vectors of Wi

vv, i = 1, . . . , h} is a set of linearly independent vectors, because each Wi

oWi
o that are linearly independent of other column vectors in Wj

v is full rank by assumption and each
o ∈ Rd×d/h must be linearly independent. Therefore,
vv is a linear
o, j (cid:54)= i.

i=1 Wi

oWi

i=1 Vi

oVi

i=1 Vi

oVi

v, since (cid:80)h

v = (cid:80)h

13

Now consider any v ∈ Rd, and X = veT

1 , where e1 = (1, 0, . . . , 0) ∈ Rn. Deﬁne φ(t) = exp(t)/(exp(t) + n − 1).

Then, we have

gV(X) =

h
(cid:88)

i=1

Vi

oVi

√
vX · Softmax (cid:2)XT UX/

(cid:3) =

dp

h
(cid:88)

i=1

Vi

oVi

vX · Softmax









vT Uv√
dp
0
...
0

0

. . .

0 . . .
...
. . .
0 . . .

(cid:32) h
(cid:88)

=

i=1

Vi

oVi
v

(cid:33) (cid:20)
φ

(cid:18)

(cid:19)

vT Uv√
dp

v v
n

. . .

(cid:21)

v
n

=

(cid:32) h
(cid:88)

i=1

Wi

oWi
v

(cid:33) (cid:20)
φ

(cid:18)

(cid:19)

vT Uv√
dp

v v
n

. . .

Similarly, we can calculate


0

0


...


0

(cid:21)

.

v
n

fW(X) =

=

h
(cid:88)

i=1

h
(cid:88)

i=1

Wi

oWi

vX · Softmax (cid:2)(Wi

kX)T (Wi

qX)/

√

d/h(cid:3)

Wi

oWi
v

(cid:20)

φ

(cid:18) vT (Wi
√

qv

k)T Wi
d/h

(cid:19)

v v
n

. . .

(cid:21)

.

v
n

Notice that all the columns of fW(X) and gV(X), from the second columns to the last ones, are the same. We now
compare the ﬁrst columns:

fW(X):,1 − gV(X):,1 =

(cid:32)

(cid:32)

φ

h
(cid:88)

i=1

vT (Wi

k)T Wi

qv

(cid:112)d/h

(cid:33)

(cid:32)

− φ

vT Uv
(cid:112)dp

(cid:33)(cid:33)

Wi

oWi

vv.

Recall that for any v (cid:54)= 0, Wi
k)T Wi
d/h

(cid:18) vT (Wi
√

all φ

− φ

vT Uv√
dp

qv

(cid:18)

(cid:19)

oWi
vv are linearly independent, so fW(X):,1 − gV(X):,1 = 0 if and only if
(cid:19)
are zero. However, since there exists i ∈ {1, . . . , h} such that U/(cid:112)dp −

(Wi

k)T (Wi

making φ

q)/(cid:112)d/h is not skew-symmetric, we can choose v to be one that satisﬁes
(cid:18) vT (Wi
√

(cid:54)= 0, therefore fW(X):,1 − gV(X):,1 (cid:54)= 0.

− φ

qv

(cid:18)

(cid:19)

(cid:19)

k)T Wi
d/h

vT Uv√
dp

vT (Wi
√

qv

k)T Wi
d/h

(cid:54)= vT Uv√
dp

, hence

Case 3. Now consider any X = (cid:2)v1 v2 0 . . . 0(cid:3), where v1 and v2 will be chosen later. Deﬁne φ1(t1, t2) =
exp(t1)/(exp(t1) + exp(t2) + n − 2), φ2(t1, t2) = exp(t2)/(exp(t1) + exp(t2) + n − 2). Then, we have

gV(X) =

h
(cid:88)

i=1

Vi

oVi

vX · Softmax












vT
1 Uv1√
dp
vT
2 Uv1√
dp
0
...
0

vT
1 Uv2√
dp
vT
2 Uv2√
dp
0
...
0

0

0

0
...
0

. . .

. . .

. . .
. . .
. . .












0

0

0
...
0

.

Therefore, the ﬁrst column of gV(X) can be written as

gV(X):,1 =

(cid:32) h
(cid:88)

i=1

(cid:33) (cid:34)

(cid:32)

Wi

oWi
v

φ1

vT
1 Uv1
(cid:112)dp

,

vT
2 Uv1
(cid:112)dp

(cid:33)

(cid:32)

v1 + φ2

vT
1 Uv1
(cid:112)dp

,

vT
2 Uv1
(cid:112)dp

(cid:33)

(cid:35)

v2

.

14

Similarly, the ﬁrst column of fW(X) is

fW(X):,1 =

(cid:34)

(cid:32)

Wi

oWi
v

φ1

h
(cid:88)

i=1

(cid:32)

φ2

1 (Wi
vT

qv1

vT
2 (Wi

k)T Wi
(cid:112)d/h
k)T Wi
(cid:112)d/h

,

,

qv1

qv1

k)T Wi
(cid:112)d/h
k)T Wi
(cid:112)d/h

(cid:33)

v1+

(cid:33)

(cid:35)

v2

.

1 (Wi
vT

qv1

2 (Wi
vT

Since U/(cid:112)dp − (W1
for all v1. Recall that U is rank-dp by assumption, so U/(cid:112)dp − (W1
(cid:18)

q)/(cid:112)d/h is skew-symmetric by assumption, we have vT

k)T (W1

k)T (W1

(cid:19)

v1 = 0
q)/(cid:112)d/h is at least rank dp − d/h ≥ 1,

(W1
k)T (W1
q )
√
d/h

U√
dp

−

1

(cid:18)

(cid:19)

so we can choose any v1 such that

U√
dp

−

k)T (W1
(W1
q )
√
d/h

v1 (cid:54)= 0.

k)T (W1
(W1
q )
√
d/h

v1 are nonzero, We can always choose ˜v2 such that ˜vT
2

(cid:19)

(cid:18)

U√
dp

v1 > 0 and

If both U√
dp
(cid:18) (W1
k)T (W1
q )
√
d/h

v1 and
(cid:19)

˜vT
2

v1 < 0. This means that if we choose v2 = α˜v2 and scale α → ∞,

(cid:33)

,

vT
2 Uv1
(cid:112)dp

→ 1,

(cid:32)

(cid:32)

(cid:32)

φ1

φ1

φ2

vT
1 Uv1
(cid:112)dp
1 (W1
vT

,

qv1

vT
2 Uv1
(cid:112)dp
k)T W1
(cid:112)d/h
k)T W1
(cid:112)d/h

qv1

(cid:33)

(cid:32)

→ 0, φ2

2 (W1
vT

k)T W1
(cid:112)d/h
k)T W1
(cid:112)d/h

1 (W1
vT

2 (W1
vT

vT
1 Uv1
(cid:112)dp
(cid:33)

qv1

qv1

(cid:33)

→ 0.

→

exp(vT
1 (W1

1 (W1
k)T W1

k)T W1

qv1/(cid:112)d/h)
qv1/(cid:112)d/h) + n − 2

exp(vT

,

Then, consider the difference fW(X):,1−gV(X):,1. Recall that for any v, W1
1}. This means that, to show fW(X):,1 − gV(X):,1 (cid:54)= 0, it sufﬁces to show that

oW1

vv is independent of {Wi

oWi

vv, i (cid:54)=

vT
1 (W1

qv1

vT
2 (W1

qv1

(cid:34)

(cid:32)

φ1

(cid:34)

(cid:32)

φ2

k)T W1
(cid:112)d/h
k)T W1
(cid:112)d/h

k)T W1
(cid:112)d/h
k)T W1
(cid:112)d/h

(cid:33)

(cid:33)

(cid:32)

(cid:32)

− φ1

− φ2

vT
1 Uv1
(cid:112)dp
vT
1 Uv1
(cid:112)dp

,

,

vT
2 Uv1
(cid:112)dp
vT
2 Uv1
(cid:112)dp

(cid:33) (cid:35)

W1

oW1

vv1+

(cid:33) (cid:35)

W1

oW1

vv2 (cid:54)= 0.

1 (W1
vT

qv1

2 (W1
vT

qv1

,

,

,

,

If we scale v2 = α˜v2 with large enough α, the second term will dominate the ﬁrst term and the ﬁrst term will never be
able to cancel the second one. Thus, by choosing large enough α > 0, we can make sure that the sum is nonzero.
v1 = 0), we can choose ˜v2 = U√
dp

v1
and use a similar scaling argument. By choosing large enough α > 0 and v2 = α˜v2, one can show that the difference
fW(X):,1 − gV(X):,1 is nonzero.

Even in case where one of U√
dp

k)T (W1
(W1
q )
√
d/h

k)T (W1
(W1
q )
√
d/h

v1 is zero (say

v1 and

## C Experimental settings

For our experiments with the language modeling (LM1B dataset), we train 6 layer Transformer models. We use a batch
size of 4096 and train for 250k steps. We use a learning rate of 0.1 with a linear warm up for the ﬁrst 10k steps. We
decay the learning rate with the square root of the number of steps. We train the baseline models, with the prevalent
head size heuristic, with the embedding dimension varying from 256 to 512. We ﬁx the width of the feed forward layer
in the Transformer to be 1024. In addition, we use weight decay of 0.01 and dropout with probability of 0.1 on all the
layers.

For our experiments with BERT, we follow the same experimental settings as in [Devlin et al., 2018]. We present
the key details here and refer the reader to [Devlin et al., 2018]. We train with a batch size of 1024 for 450k steps with
inputs of sequence length n = 128 followed by 50k steps with inputs of sequence length 512. In contrast the BERT
paper uses a batch size of 512, and does the pre-training for 900K steps with 128 sequence length inputs and 100k steps
with 512 sequence length inputs. We train using ADAM with a learning rate of 1e-4, and a linear warmup and decay
schedule as in BERT. We use 5k warmup steps for the ﬁrst stage, and a re-warmup of 3k steps for the second stage [You
et al., 2019]. Again, we use weight decay of 0.01 and dropout with probability of 0.1 on all the layers.

For the language modeling task, training is performed on 4 TPUv2 chips for a couple of hours. For BERT models
training is performed on 16 TPUv3 chips in the ﬁrst stage and 64 TPUv3 chips for the second stage. Pre-training with
this conﬁguration takes between 2 to 3 days. We did not attempt to ﬁnd the optimal hyper-parameters for the ﬁxed head
size architecture, and use the same hyper-parameters as used for training the BERT models.

## D Additional experimental results

Figure 4: Performance of the Transformers trained with the prevalent head size heuristic (baseline) compared with
the ﬁxed head size (dp) models for a language modeling task (LM1B) on the test set. Unlike Fig.1, we vary both the
embedding size and the number of heads of the baseline models to keep their head size ﬁxed to 32. We train the ﬁxed
head size models with a ﬁxed embedding size of 256 and a head size of 32, and vary the number of heads from 4 to
70, while matching the number of parameters. The plot again clearly indicates the advantage of the ﬁxed head size
models. The main issue with the baseline models is that ﬁxing the head size to 32 forces the number of heads to be
small when the embedding size is small. Reducing the number of heads below certain threshold hurts the performance
of the Transformer.


8
# heads
214M
# params
90.35±0.14
SQuAD - F1
SQuAD - EM 83.37±0.12
84.4±0.2

MNLI

12
252M
90.48±0.09
83.67±0.03
84.4±0.2

16
290M
90.92±0.14
84.16±0.35
84.7±0.1

20
327M
90.89±0.08
84.29±0.16
85.1±0.4

(A) Increasing number of heads

Table 3: (A): 24 layer Transformer trained with a ﬁxed head size of 128 and an embedding size of 768 shows an
improvement in the accuracy with the increasing number of heads.
