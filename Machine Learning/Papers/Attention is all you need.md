---
title: Attention is all you need
tags:
    - lm
    - model
authors:
    - Ashish Vaswani
    - Llion Jones
    - Noam Shazeer
    - Niki Parmar
    - Jakob Uszkoreit
    - Aidan N. Gomez
    - Łukasz Kaiser
    - Illia Polosukhin
status: read
aliases:
    - "Vaswani et al., 2017"
published: "2017-12-06"
url:
    - "[paper](https://arxiv.org/pdf/1706.03762.pdf)"
    - "[arxiv](https://arxiv.org/abs/1706.03762)"
---

# Attention is all you need
###### Authors
<ul>
<li class="author">Ashish Vaswani</li>
<li class="separator author">|</li>
<li class="author">Llion Jones</li>
<li class="separator author">|</li>
<li class="author">Noam Shazeer</li>
<li class="separator author">|</li>
<li class="author">Niki Parmar</li>
<li class="separator author">|</li>
<li class="author">Jakob Uszkoreit</li>
<li class="separator author">|</li>
<li class="author">Aidan N. Gomez</li>
<li class="separator author">|</li>
<li class="author">Łukasz Kaiser</li>
<li class="separator author">|</li>
<li class="author">Illia Polosukhin</li>
</ul>

```dataview
table without ID url from "Mathematics/Machine Learning/Papers/Attention is all you need.md"
```

- [ ] Need to explain implementation details in PyTorch. The `tgt_mask` versus `tgt_key_padding_mask` is a bit confusing.

##### TL;DR
1.  Introduction of the Transformer: The paper "Attention is All You Need" presents the Transformer, a novel neural network architecture designed to address sequence-to-sequence tasks with better efficiency and scalability compared to traditional RNNs and LSTMs.
2.  Self-attention mechanism: The Transformer leverages self-attention, allowing it to process input sequences in parallel rather than sequentially, improving computational efficiency and enabling the model to capture long-range dependencies.
3.  Encoder-decoder structure: The Transformer maintains an encoder-decoder structure, with the encoder learning to map input sequences to continuous representations, and the decoder generating output sequences from these representations.
4.  Multi-head attention: The Transformer utilizes multi-head attention, which splits the attention mechanism into multiple parallel "heads" that learn different patterns, enabling the model to capture diverse information from the input sequence.
5.  Positional encoding: To account for the lack of sequential processing, the Transformer incorporates positional encodings into its input, allowing the model to learn and use positional information in the input sequence.
6.  Layer normalization and residual connections: The Transformer employs layer normalization and residual connections to improve training stability and help the model learn complex patterns.
7.  Successful results: The paper demonstrates the Transformer's success in various natural language processing tasks, such as machine translation and parsing, outperforming existing state-of-the-art models while requiring less computational resources.
8.  Future impact: The Transformer architecture has since become the foundation for many breakthroughs in NLP, including models like BERT, GPT, and T5, revolutionizing the field and enabling a wide range of applications.

## Abstract

The dominant sequence transduction models are based on complex recurrent or convolutional neural networks in an encoder-decoder configuration. The best performing models also connect the encoder and decoder through an attention mechanism. We propose a new simple network architecture, the Transformer, based solely on attention mechanisms, dispensing with recurrence and convolutions entirely. Experiments on two machine translation tasks show these models to be superior in quality while being more parallelizable and requiring significantly less time to train. Our model achieves 28.4 BLEU on the WMT 2014 English-to-German translation task, improving over the existing best results, including ensembles by over 2 BLEU. On the WMT 2014 English-to-French translation task, our model establishes a new single-model state-of-the-art BLEU score of 41.8 after training for 3.5 days on eight GPUs, a small fraction of the training costs of the best models from the literature. We show that the Transformer generalizes well to other tasks by applying it successfully to English constituency parsing both with large and limited training data.

## 1 Introduction

Recurrent neural networks, long short-term memory [^13] and gated recurrent [^7] neural networks in particular, have been firmly established as state of the art approaches in sequence modeling and transduction problems such as language modeling and machine translation [^35][^2][^5]. Numerous efforts have since continued to push the boundaries of recurrent language models and encoder-decoder architectures [^38][^24][^15].

Recurrent models typically factor computation along the symbol positions of the input and output sequences. Aligning the positions to steps in computation time, they generate a sequence of hidden states $h_t$, as a function of the previous hidden state $h_{t-1}$ and the input for position $t$. This inherently sequential nature precludes parallelization within training examples, which becomes critical at longer sequence lengths, as memory constraints limit batching across examples. Recent work has achieved significant improvements in computational efficiency through factorization tricks [^21] and conditional computation [^32], while also improving model performance in case of the latter. The fundamental constraint of sequential computation, however, remains.

Attention mechanisms have become an integral part of compelling sequence modeling and transduction models in various tasks, allowing modeling of dependencies without regard to their distance in the input or output sequences [^2] [^19]. In all but a few cases [^27], however, such attention mechanisms are used in conjunction with a recurrent network.

In this work we propose the Transformer, a model architecture eschewing recurrence and instead relying entirely on an attention mechanism to draw global dependencies between input and output. The Transformer allows for significantly more parallelization and can reach a new state of the art in translation quality after being trained for as little as twelve hours on eight P100 GPUs.

## 2 Background

The goal of reducing sequential computation also forms the foundation of the Extended Neural GPU [^16], ByteNet [^18] and ConvS2S [^9], all of which use convolutional neural networks as basic building block, computing hidden representations in parallel for all input and output positions. In these models, the number of operations required to relate signals from two arbitrary input or output positions grows in the distance between positions, linearly for ConvS2S and logarithmically for ByteNet. This makes it more difficult to learn dependencies between distant positions [^12]. In the Transformer this is reduced to a constant number of operations, albeit at the cost of reduced effective resolution due to averaging attention-weighted positions, an effect we counteract with Multi-Head Attention as described in [[#3.2 Attention|section 3.2]].

Self-attention, sometimes called intra-attention is an attention mechanism relating different positions of a single sequence in order to compute a representation of the sequence. Self-attention has been used successfully in a variety of tasks including reading comprehension, abstractive summarization, textual entailment and learning task-independent sentence representations [^4][^27][^28][^22].

End-to-end memory networks are based on a recurrent attention mechanism instead of sequence- aligned recurrence and have been shown to perform well on simple-language question answering and language modeling tasks [^34].

To the best of our knowledge, however, the Transformer is the first transduction model relying entirely on self-attention to compute representations of its input and output without using sequence- aligned RNNs or convolution. In the following sections, we will describe the Transformer, motivate self-attention and discuss its advantages over models such as [^17] [^18] and [^9].

## 3 Model 
Most competitive neural sequence transduction models have an encoder-decoder structure [^5][^2][^35]. Here, the encoder maps an input sequence of symbol representations $(x_1,...,x_n)$ to a sequence of continuous representations $z = (z_1,...,z_n)$. Given $z$, the decoder then generates an output sequence $(y_1, ..., y_m)$ of symbols one element at a time. At each step the model is auto-regressive [^10], consuming the previously generated symbols as additional input when generating the next.

The Transformer follows this overall architecture using stacked self-attention and point-wise, fully connected layers for both the encoder and decoder, shown in the left and right halves of Figure 1, respectively.

### 3.1 Encoder and Decoder Stacks
> [!blank-container|right-small]
> ![[../Research/Images/Attention is all you need/Figure 1.png|400]]
> 
> *Figure 1*: The Transformer - model architecture.

**Encoder**: The encoder is composed of a stack of $N = 6$ identical layers. Each layer has two sub-layers. The first is a multi-head self-attention mechanism, and the second is a simple, position-wise fully connected feed-forward network. We employ a residual connection [^11] around each of the two sub-layers, followed by layer normalization[^1]. That is, the output of each sub-layer is `LayerNorm(x + Sublayer(x))`, where `Sublayer(x)` is the function implemented by the sub-layer itself. To facilitate these residual connections, all sub-layers in the model, as well as the embedding layers, produce outputs of dimension $d_{model} = 512$.

**Decoder**: The decoder is also composed of a stack of $N = 6$ identical layers. In addition to the two sub-layers in each encoder layer, the decoder inserts a third sub-layer, which performs multi-head attention over the output of the encoder stack. Similar to the encoder, we employ residual connections around each of the sub-layers, followed by layer normalization. We also modify the self-attention sub-layer in the decoder stack to prevent positions from attending to subsequent positions. This masking, combined with the fact that the output embeddings are offset by one position, ensures that the predictions for position $i$ can depend only on the known outputs at positions less than $i$.

### 3.2 Attention
> [!blank-container|right-small]
> ![[../Research/Images/Attention is all you need/Figure 2.1.png|350]]
> *Figure 2.1*: Scaled Dot-Product Attention. 
> ![[../Research/Images/Attention is all you need/Figure 2.2.png|350]]
> *Figure 2.2*: Multi-Head Attention consists of several attention layers running in parallel.

^8d337e

An attention function can be described as mapping a query and a set of key-value pairs to an output, where the query, keys, values, and output are all vectors. The output is computed as a weighted sum of the values, where the weight assigned to each value is computed by a compatibility function of the query with the corresponding key.

#### 3.2.1 Scaled Dot-Product Attention

We call our particular attention _Scaled Dot-Product Attention_ ([[#^8d337e|Figure 2.1]]). The input consists of queries and keys of dimension $d_k$, and values of dimension $d_v$. We compute the dot products of the query with all keys, divide each by $\sqrt{d_k}$, and apply a softmax function to obtain the weights on the values.

In practice, we compute the attention function on a set of queries simultaneously, packed together into a matrix $Q$. The keys and values are also packed together into matrices $K$ and $V$. We compute the matrix of outputs as:

$$
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

The two most commonly used attention functions are additive attention [^2], and dot-product (multiplicative) attention. Dot-product attention is identical to our algorithm, except for the scaling factor of $\sqrt{d_k}$. Additive attention computes the compatibility function using a feed-forward network with a single hidden layer. While the two are similar in theoretical complexity, dot-product attention is much faster and more space-efficient in practice, since it can be implemented using highly optimized matrix multiplication code.

While for small values of $d_k$ the two mechanisms perform similarly, additive attention outperforms dot product attention without scaling for larger values of $d_k$[^3]. We suspect that for large values of $d_k$, the dot products grow large in magnitude, pushing the softmax function into regions where it has extremely small gradients[^a]. To counteract this effect, we scale the dot products by $\frac{1}{\sqrt{d_k}}$.

[^a]: To illustrate why the dot products get large, assume that the components of $q$ and $k$ are independent random variables with mean $0$ and variance $1$. Then their dot product, $q \cdot k = \sum_{i=1}^{d_k} q_i k_i$, has mean $0$ and variance $d_k$.

#### 3.2.2 Multi-Head Attention

Instead of performing a single attention function with $d_{\text{model}}$-dimensional keys, values, and queries, we found it beneficial to linearly project the queries, keys, and values $h$ times with different, learned linear projections to $d_k$, $d_k$, and $d_v$ dimensions, respectively. On each of these projected versions of queries, keys, and values, we then perform the attention function in parallel, yielding $d_v$-dimensional output values. These are concatenated and once again projected, resulting in the final values, as depicted in [[#^8d337e|Figure 2]].

Multi-head attention allows the model to jointly attend to information from different representation subspaces at different positions. With a single attention head, averaging inhibits this.

$$
\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h)W^O
$$

where

$$
\text{head}_i = \text{Attention}(QW_i^Q, KW_i^K, VW_i^V)
$$

Where the projections are parameter matrices $W_i^Q \in \mathbb{R}^{d_{\text{model}} \times d_k}$, $W_i^K \in \mathbb{R}^{d_{\text{model}} \times d_k}$, $W_i^V \in \mathbb{R}^{d_{\text{model}} \times d_v}$, and $W^O \in \mathbb{R}^{h d_v \times d_{\text{model}}}$.

In this work, we employ $h = 8$ parallel attention layers, or heads. For each of these, we use $d_k = d_v = \frac{d_{\text{model}}}{h} = 64$. Due to the reduced dimension of each head, the total computational cost is similar to that of single-head attention with full dimensionality.

#### 3.2.3 Applications of Attention in our Model

The Transformer uses multi-head attention in three different ways:
1. **Encoder-decoder attention**: The queries come from the previous decoder layer, and the memory keys and values come from the output of the encoder. This allows every position in the decoder to attend over all positions in the input sequence. This mimics the typical encoder-decoder attention mechanisms in sequence-to-sequence models such as ([^38] [^2] [^9]).

2. **Encoder self-attention**: The encoder contains self-attention layers. In a self-attention layer all of the keys, values, and queries come from the same place, in this case, the output of the previous layer in the encoder. Each position in the encoder can attend to all positions in the previous layer of the encoder.

3. **Decoder self-attention**: Similarly, self-attention layers in the decoder allow each position in the decoder to attend to all positions in the decoder up to and including that position. We need to prevent leftward information flow in the decoder to preserve the auto-regressive property. We implement this inside of scaled dot-product attention by masking out (setting to $- \infty$) all values in the input of the softmax which correspond to illegal connections. See [[#^8d337e|Figure 2]].

### 3.3 Position-wise Feed-Forward Networks

In addition to attention sub-layers, each of the layers in our encoder and decoder contains a fully connected feed-forward network, which is applied to each position separately and identically. This consists of two linear transformations with a ReLU activation in between.
$$
\mathrm{FFN}(x)=\max(0, xW_{1}+b_{1})W_{2}+b_{2}
$$
While the linear transformations are the same across different positions, they use different parameters from layer to layer. Another way of describing this is as two convolutions with kernel size 1. The dimensionality of input and output is $d_{\text{model}}=512$, and the inner-layer has dimensionality $d_{ff}=2048$.

### 3.4 Embeddings and Softmax

Similarly to other sequence transduction models, we use learned embeddings to convert the input tokens and output tokens to vectors of dimension $d_{\text{model}}$. We also use the usual learned linear transformation and softmax function to convert the decoder output to predicted next-token probabilities. In our model, we share the same weight matrix between the two embedding layers and the pre-softmax linear transformation, similar to (press2016using, ). In the embedding layers, we multiply those weights by $\sqrt{d_{\text{model}}}$.

### 3.5 Positional Encoding

Since our model contains no recurrence and no convolution, in order for the model to make use of the order of the sequence, we must inject some information about the relative or absolute position of the tokens in the sequence. To this end, we add "positional encodings" to the input embeddings at the bottoms of the encoder and decoder stacks. The positional encodings have the same dimension $d_{\text{model}}$ as the embeddings, so that the two can be summed. There are many choices of positional encodings, learned and fixed (JonasFaceNet2017, ).

In this work, we use sine and cosine functions of different frequencies:

$$PE_{(pos,2i)}=sin(pos/10000^{2i/d_{\text{model}}})$$

$$PE_{(pos,2i+1)}=cos(pos/10000^{2i/d_{\text{model}}})$$

where $pos$ is the position and $i$ is the dimension. That is, each dimension of the positional encoding corresponds to a sinusoid. The wavelengths form a geometric progression from $2\pi$ to $10000 \cdot 2\pi$. We chose this function because we hypothesized it would allow the model to easily learn to attend by relative positions, since for any fixed offset $k$, $PE_{pos+k}$ can be represented as a linear function of $PE_{pos}$.

We also experimented with using learned positional embeddings (JonasFaceNet2017, ) instead, and found that the two versions produced nearly identical results (see [[#^b3d051|Table 3]] row (E)). We chose the sinusoidal version because it may allow the model to extrapolate to sequence lengths longer than the ones encountered during training.

## 4 Why Self-Attention

In this section we compare various aspects of self-attention layers to the recurrent and convolutional layers commonly used for mapping one variable-length sequence of symbol representations $(x_1,…,x_n)$ to another sequence of equal length $(z_1,…,z_n)$, with $x_i,z_i\in\mathbb{R}^d$, such as a hidden layer in a typical sequence transduction encoder or decoder. Motivating our use of self-attention we consider three desiderata.

One is the total computational complexity per layer. Another is the amount of computation that can be parallelized, as measured by the minimum number of sequential operations required.

The third is the path length between long-range dependencies in the network. Learning long-range dependencies is a key challenge in many sequence transduction tasks. One key factor affecting the ability to learn such dependencies is the length of the paths forward and backward signals have to traverse in the network. The shorter these paths between any combination of positions in the input and output sequences, the easier it is to learn long-range dependencies (hochreiter2001gradient, ). Hence we also compare the maximum path length between any two input and output positions in networks composed of the different layer types.

<table>
    <tr>
        <th>Operation</th>
        <th>Computational Complexity</th>
        <th>Parallelism</th>
        <th>Time Complexity</th>
    </tr>
    <tr>
        <td>Self-Attention</td>
        <td class="math display">O(n^2 \cdot d)</td>
        <td class="math display">O(1)</td>
        <td class="math display">O(1)</td>
    </tr>
    <tr>
        <td>Recurrent</td>
        <td class="math display">O(n \cdot d^2)</td>
        <td class="math display">O(n)</td>
        <td class="math display">O(n)</td>
    </tr>
    <tr>
        <td>Convolutional</td>
        <td class="math display">O(k \cdot n \cdot d^2)</td>
        <td class="math display">O(1)</td>
        <td class="math display">O(\log_{k}(n))</td>
    </tr>
    <tr>
        <td>Self-Attention (restricted)</td>
        <td class="math display">O(r \cdot n \cdot d)</td>
        <td class="math display">O(1)</td>
        <td class="math display">O(\frac{n}{r})</td>
    </tr>
</table>

> *Table 1*: Maximum path lengths, per-layer complexity and minimum number of sequential operations for different layer types. $n$ is the sequence length, $d$ is the representation dimension, $k$ is the kernel size of convolutions and $r$ the size of the neighborhood in restricted self-attention.

As noted in Table 1, a self-attention layer connects all positions with a constant number of sequentially executed operations, whereas a recurrent layer requires $O(n)$ sequential operations. In terms of computational complexity, self-attention layers are faster than recurrent layers when the sequence length $n$ is smaller than the representation dimensionality $d$, which is most often the case with sentence representations used by state-of-the-art models in machine translations, such as word-piece (wu2016google, ) and byte-pair (sennrich2015neural, ) representations. To improve computational performance for tasks involving very long sequences, self-attention could be restricted to considering only a neighborhood of size $r$ in the input sequence centered around the respective output position. This would increase the maximum path length to $O(n/r)$. We plan to investigate this approach further in future work.

A single convolutional layer with kernel width $k<n$ does not connect all pairs of input and output positions. Doing so requires a stack of $O(n/k)$ convolutional layers in the case of contiguous kernels, or $O(log_k(n))$ in the case of dilated convolutions (NalBytenet2017, ), increasing the length of the longest paths between any two positions in the network. Convolutional layers are generally more expensive than recurrent layers, by a factor of $k$. Separable convolutions (xception2016, ), however, decrease the complexity considerably, to $O(k\cdot n\cdot d+n\cdot d^2)$. Even with $k=n$, however, the complexity of a separable convolution is equal to the combination of a self-attention layer and a point-wise feed-forward layer, the approach we take in our model.

As side benefit, self-attention could yield more interpretable models. We inspect attention distributions from our models and present and discuss examples in the appendix. Not only do individual attention heads clearly learn to perform different tasks, many appear to exhibit behavior related to the syntactic and semantic structure of the sentences.

## 5 Training

This section describes the training regime for our models.
### 5.1 Training Data and Batching

We trained on the standard WMT 2014 English-German dataset consisting of about 4.5 million sentence pairs. Sentences were encoded using byte-pair encoding ([DBLP:journals/corr/BritzGLL17,](https://htmltomd.com/#bib.bib3) ), which has a shared source-target vocabulary of about 37000 tokens. For English-French, we used the significantly larger WMT 2014 English-French dataset consisting of 36M sentences and split tokens into a 32000 word-piece vocabulary ([wu2016google,](https://htmltomd.com/#bib.bib38) ). Sentence pairs were batched together by approximate sequence length. Each training batch contained a set of sentence pairs containing approximately 25000 source tokens and 25000 target tokens.

### 5.2 Hardware and Schedule

We trained our models on one machine with 8 NVIDIA P100 GPUs. For our base models using the hyperparameters described throughout the paper, each training step took about 0.4 seconds. We trained the base models for a total of 100,000 steps or 12 hours. For our big models,(described on the bottom line of [[#^b3d051|table 3]]), step time was 1.0 seconds. The big models were trained for 300,000 steps (3.5 days).

### 5.3 Optimizer

We used the Adam optimizer ([Kingma & Ba, 2014](https://arxiv.org/abs/1412.6980)) with $\beta_1=0.9$, $\beta_2=0.98$ and $\epsilon=10^{-9}$. We varied the learning rate over the course of training, according to the formula:

$$
\text{lrate} = d_{\text{model}}^{-0.5} \cdot \min(\text{step\_num}^{-0.5}, \text{step\_num} \cdot \text{warmup\_steps}^{-1.5})
$$

(3)

This corresponds to increasing the learning rate linearly for the first $\text{warmup\_steps}$ training steps, and decreasing it thereafter proportionally to the inverse square root of the step number. We used $\text{warmup\_steps}=4000$.

### 5.4 Regularization

We employ three types of regularization during training:

**Residual Dropout**
We apply dropout ([Srivastava et al., 2014](http://jmlr.org/papers/v15/srivastava14a.html)) to the output of each sub-layer, before it is added to the sub-layer input and normalized. In addition, we apply dropout to the sums of the embeddings and the positional encodings in both the encoder and decoder stacks. For the base model, we use a rate of $P_{drop} = 0.1$.

<table>
    <tr>
        <th>Model</th>
        <th colspan = "2">BLUE</th>
        <th colspan = "2">Training Cost (FLOPs)</th>
    </tr>
    <tr>
        <td></td>
        <td>EN-DE</td>
        <td>EN-FR</td>
        <td>EN-DE</td>
        <td>EN-FR</td>
    </tr>
    <tr>
        <td>ByteNet[^8]</td>
        <td>23.75</td>
        <td></td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>Deep-Att + PosUnk</td>
        <td>39.2</td>
        <td></td>
        <td  class="math display">1.0\cdot 10^{20}</td>
        <td class="math display">1.0\cdot 10^{20}</td>
    </tr>
    <tr>
        <td>GNMT + RL</td>
        <td>24.6</td>
        <td>39.92</td>
        <td class="math display">2.3\cdot 10^{19}</td>
        <td class="math display">1.4\cdot 10^{20}</td>
    </tr>
    <tr>
        <td>ConvS2S</td>
        <td>25.16</td>
        <td>40.46</td>
        <td class="math display">9.6\cdot 10^{18}</td>
        <td class="math display">1.5\cdot 10^{20}</td>
    </tr>
    <tr>
        <td>MoE</td>
        <td>26.03</td>
        <td>40.56</td>
        <td class="math display">2.0\cdot 10^{19}</td>
        <td class="math display">1.2\cdot 10^{20}</td>
    </tr>
    <tr>
        <td>Deep-Att + PosUnk Ensemble</td>
        <td>40.4</td>
        <td></td>
        <td class="math display">8.0\cdot 10^{20}</td>
        <td></td>
    </tr>
    <tr>
        <td>GNMT + RL Ensemble</td>
        <td>26.30</td>
        <td>41.16</td>
        <td class="math display">1.8\cdot 10^{20}</td>
        <td class="math display">1.1\cdot 10^{21}</td>
    </tr>
    <tr>
        <td>ConvS2S Ensemble</td>
        <td>26.36</td>
        <td>41.29</td>
        <td class="math display">7.7\cdot 10^{19}</td>
        <td class="math display">1.2\cdot 10^{21}</td>
    </tr>
    <tr>
        <td>Transformer (base model)</td>
        <td>27.3</td>
        <td>38.1</td>
        <td class="math display">\boldsymbol{3.3\cdot 10^{18}}</td>
        <td class="math display">\boldsymbol{3.3\cdot 10^{18}}</td>
    </tr>
    <tr>
        <td>Transformer (big)</td>
        <td>28.4</td>
        <td>41.8</td>
        <td class="math display">2.3\cdot 10^{19}</td>
        <td class="math display">2.3\cdot 10^{19}</td>
    </tr>
</table>

> *Table 2*: The Transformer achieves better BLEU scores than previous state-of-the-art models on the English-to-German and English-to-French newstest2014 tests at a fraction of the training cost.

^eb5a1d

**Label Smoothing** 
During training, we employed label smoothing of value $\epsilon_{ls} = 0.1$ ([Szegedy et al., 2015](https://arxiv.org/abs/1512.00567)). This hurts perplexity, as the model learns to be more unsure, but improves accuracy and BLEU score.

## 6 Results
### 6.1 Machine Translation

On the WMT 2014 English-to-German translation task, the big transformer model (Transformer (big) in [[#^eb5a1d|Table 2]]) outperforms the best previously reported models (including ensembles) by more than $2.0$ BLEU, establishing a new state-of-the-art BLEU score of $28.4$. The configuration of this model is listed in the bottom line of [[#^b3d051|Table 3]]. Training took $3.5$ days on $8$ P100 GPUs. Even our base model surpasses all previously published models and ensembles, at a fraction of the training cost of any of the competitive models.

On the WMT 2014 English-to-French translation task, our big model achieves a BLEU score of $41.0$, outperforming all of the previously published single models, at less than $\frac{1}{4}$ the training cost of the previous state-of-the-art model. The Transformer (big) model trained for English-to-French used dropout rate $P_{drop} = 0.1$, instead of $0.3$.

For the base models, we used a single model obtained by averaging the last 5 checkpoints, which were written at 10-minute intervals. For the big models, we averaged the last 20 checkpoints. We used beam search with a beam size of $4$ and length penalty $\alpha = 0.6$ ([Wu et al., 2016](https://arxiv.org/abs/1609.08144)). These hyperparameters were chosen after experimentation on the development set. We set the maximum output length during inference to input length + $50$, but terminate early when possible ([Wu et al., 2016](https://arxiv.org/abs/1609.08144)).

[[#^eb5a1d|Table 2]] summarizes our results and compares our translation quality and training costs to other model architectures from the literature. We estimate the number of floating point operations used to train a model by multiplying the training time, the number of GPUs used, and an estimate of the sustained single-precision floating-point capacity of each GPU 2[^b].

[^b]: We used values of $2.8,3.7,6.0$ and 9.5 TFLOPS for K80, K40, M40 and P100, respectively. 

### 6.2 Model Variations

<table>
    <tr>
        <th>Model Variant</th>
        <td>N</td>
        <td class="math display">d_{\text{model}}</td>
        <td class="math display">d_{\text{ff}}</td>
        <td class="math display">h</td>
        <td class="math display">d_{k}</td>
        <td class="math display">d_{v}</td>
        <td class="math display">P_{drop}</td>
        <td class="math display">\epsilon_{ls}</td>
        <td>train steps</td>
        <td>PPL (dev)</td>
        <td>BLEU (dev)</td>
        <td>params <span  class="math display">\times 10^{6}</span></td>
    </tr>
    <tr>
        <th>base</th>
        <td>6</td>
        <td>512</td>
        <td>2048</td>
        <td>8</td>
        <td>64</td>
        <td>64</td>
        <td>0.1</td>
        <td>0.1</td>
        <td>100K</td>
        <td>4.92</td>
        <td>25.8</td>
        <td>65</td>
    </tr>
    <tr>
        <th rowspan=4>(A)</th>
        <td></td>
        <td></td>
        <td></td>
        <td>1</td>
        <td>512</td>
        <td>512</td>
        <td></td>
        <td></td>
        <td></td>
        <td>5.29</td>
        <td>24.9</td>
        <td>4</td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td></td>
        <td>4</td>
        <td>128</td>
        <td>128</td>
        <td></td>
        <td></td>
        <td></td>
        <td>5.00</td>
        <td>25.5</td>
        <td>16</td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td></td>
        <td>16</td>
        <td>32</td>
        <td>32</td>
        <td></td>
        <td></td>
        <td></td>
        <td>4.91</td>
        <td>25.8</td>
        <td>32</td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td></td>
        <td>32</td>
        <td>16</td>
        <td>16</td>
        <td></td>
        <td></td>
        <td></td>
        <td>5.01</td>
        <td>25.4</td>
        <td>16</td>
    </tr>
    <tr>
        <th rowspan=2>(B)</th>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>16</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>5.16</td>
        <td>25.1</td>
        <td>58</td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>32</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>5.01</td>
        <td>25.4</td>
        <td>60</td>
    </tr>
    <tr>
        <th rowspan=7>(C)</th>
        <td>2</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>6.11</td>
        <td>23.7</td>
        <td>36</td>
    </tr>
    <tr>
        <td>4</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>5.19</td>
        <td>25.3</td>
        <td>50</td>
    </tr>
    <tr>
        <td>8</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>4.88</td>
        <td>25.5</td>
        <td>80</td>
    </tr>
    <tr>
        <td></td>
        <td>256</td>
        <td></td>
        <td></td>
        <td>32</td>
        <td>32</td>
        <td></td>
        <td></td>
        <td></td>
        <td>5.75</td>
        <td>24.5</td>
        <td>28</td>
    </tr>
    <tr>
        <td></td>
        <td>1024</td>
        <td></td>
        <td></td>
        <td>128</td>
        <td>128</td>
        <td></td>
        <td></td>
        <td></td>
        <td>4.66</td>
        <td>26.0</td>
        <td>168</td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td>1024</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>5.12</td>
        <td>25.4</td>
        <td>53</td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td>4096</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>4.75</td>
        <td>26.2</td>
        <td>90</td>
    </tr>
    <tr>
        <th rowspan=4>(D)</th>
        <td></td>
        <td>256</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>0.0</td>
        <td></td>
        <td></td>
        <td>5.77</td>
        <td>24.6</td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>1024</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>0.2</td>
        <td></td>
        <td></td>
        <td>4.95</td>
        <td>25.5</td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td>1024</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>0.0</td>
        <td></td>
        <td>4.67</td>
        <td>25.3</td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td>4096</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>0.2</td>
        <td></td>
        <td>5.47</td>
        <td>25.7</td>
        <td></td>
    </tr>
    <tr>
        <th>(E)</th>
        <td colspan=9>positional embedding instead of sinusoids</td>
        <td>4.92</td>
        <td>25.7</td>
        <td></td>
    </tr>
    <tr>
        <th>big</th>
        <td>6</td>
        <td>1024</td>
        <td>4096</td>
        <td>16</td>
        <td></td>
        <td></td>
        <td>0.3</td>
        <td></td>
        <td>300K</td>
        <td><b>4.33</b></td>
        <td><b>26.4</b></td>
        <td>213</td>
    </tr>
</table>


>*Table 3*: Variations on the Transformer architecture. Unlisted values are identical to those of the base model. All metrics are on the English-to-German translation development set, newstest2013. Listed perplexities are per-wordpiece, according to our byte-pair encoding, and should not be compared to per-word perplexities.

^b3d051


In [[#^b3d051|Table 3]] rows (A), we vary the number of attention heads and the attention key and value dimensions, keeping the amount of computation constant, as described in [[#3.2.2 Multi-Head Attention|Section 3.2.2]]. While single-head attention is 0.9 BLEU worse than the best setting, quality also drops off with too many heads.

In [[#^b3d051|Table 3]] rows (B), we observe that reducing the attention key size dk hurts model quality. This suggests that determining compatibility is not easy and that a more sophisticated compatibility function than dot product may be beneficial. We further observe in rows (C) and (D) that, as expected, bigger models are better, and dropout is very helpful in avoiding over-fitting. In row (E) we replace our sinusoidal positional encoding with learned positional embeddings ([JonasFaceNet2017,](https://ar5iv.labs.arxiv.org/html/1706.03762#bib.bib9) ), and observe nearly identical results to the base model.

### 6.3 English Constituency Parsing

To evaluate if the Transformer can generalize to other tasks, we performed experiments on English constituency parsing. This task presents specific challenges: the output is subject to strong structural constraints and is significantly longer than the input. Furthermore, RNN sequence-to-sequence models have not been able to attain state-of-the-art results in small-data regimes [[37]](https://arxiv.org/abs/1412.6980).

We trained a 4-layer transformer with $d_{model} = 1024$ on the Wall Street Journal (WSJ) portion of the Penn Treebank [[25]](https://www.aclweb.org/anthology/J93-2004/), about 40K training sentences. We also trained it in a semi-supervised setting, using the larger high-confidence and BerkleyParser corpora from with approximately 17M sentences [[37]](https://arxiv.org/abs/1412.6980). We used a vocabulary of 16K tokens for the WSJ only setting and a vocabulary of 32K tokens for the semi-supervised setting.

We performed only a small number of experiments to select the dropout, both attention and residual ([[#5.4 Regularization|section 5.4]]), learning rates and beam size on the Section 22 development set, all other parameters remained unchanged from the English-to-German base translation model. During inference, we increased the maximum output length to input length + 300. We used a beam size of 21 and $\alpha = 0.3$ for both WSJ only and the semi-supervised setting.

Our results in Table 4 show that despite the lack of task-specific tuning, our model performs surprisingly well, yielding better results than all previously reported models with the exception of the Recurrent Neural Network Grammar [[8]](https://arxiv.org/abs/1602.07776).

In contrast to RNN sequence-to-sequence models [^37], the Transformer outperforms the Berkeley-Parser [^29] even when training only on the WSJ training set of 40K sentences.

## 7 Conclusion

In this work, we presented the Transformer, the first sequence transduction model based entirely on attention, replacing the recurrent layers most commonly used in encoder-decoder architectures with multi-headed self-attention. For translation tasks, the Transformer can be trained significantly faster than architectures based on recurrent or convolutional layers. On both WMT 2014 English-to-German and WMT 2014 English-to-French translation tasks, we achieve a new state of the art. In the former task our best model outperforms even all previously reported ensembles.

We are excited about the future of attention-based models and plan to apply them to other tasks. We plan to extend the Transformer to problems involving input and output modalities other than text and to investigate local, restricted attention mechanisms to efficiently handle large inputs and outputs such as images, audio and video. Making generation less sequential is another research goals of ours.

The code we used to train and evaluate our models is available at [https://github.com/tensorflow/tensor2tensor](https://github.com/tensorflow/tensor2tensor).

**Acknowledgements** We are grateful to Nal Kalchbrenner and Stephan Gouws for their fruitful comments, corrections and inspiration.


[^1]: Jimmy Lei Ba, Jamie Ryan Kiros, and Geoffrey E Hinton. Layer normalization. arXiv preprint arXiv:1607.06450, 2016. Available at: [https://arxiv.org/abs/1607.06450](https://arxiv.org/abs/1607.06450) 

[^2]: Dzmitry Bahdanau, Kyunghyun Cho, and Yoshua Bengio. Neural machine translation by jointly learning to align and translate. CoRR, abs/1409.0473, 2014. Available at: [https://arxiv.org/abs/1409.0473](https://arxiv.org/abs/1409.0473) 

[^3]: Denny Britz, Anna Goldie, Minh-Thang Luong, and Quoc V. Le. Massive exploration of neural machine translation architectures. CoRR, abs/1703.03906, 2017. Available at: [https://arxiv.org/abs/1703.03906](https://arxiv.org/abs/1703.03906) 

[^4]: Jianpeng Cheng, Li Dong, and Mirella Lapata. Long short-term memory-networks for machine reading. arXiv preprint arXiv:1601.06733, 2016. Available at: [https://arxiv.org/abs/1601.06733](https://arxiv.org/abs/1601.06733) 

[^5]: Kyunghyun Cho, Bart van Merrienboer, Caglar Gulcehre, Fethi Bougares, Holger Schwenk, and Yoshua Bengio. Learning phrase representations using rnn encoder-decoder for statistical machine translation. CoRR, abs/1406.1078, 2014. Available at: [https://arxiv.org/abs/1406.1078](https://arxiv.org/abs/1406.1078)

[^6]: Francois Chollet. Xception: Deep learning with depthwise separable convolutions. arXiv preprint arXiv:1610.02357, 2016. URL: [https://arxiv.org/abs/1610.02357](https://arxiv.org/abs/1610.02357)

[^7]: Junyoung Chung, Çaglar Gülçehre, Kyunghyun Cho, and Yoshua Bengio. Empirical evaluation of gated recurrent neural networks on sequence modeling. CoRR, abs/1412.3555, 2014. URL: [https://arxiv.org/abs/1412.3555](https://arxiv.org/abs/1412.3555)

[^8]: Chris Dyer, Adhiguna Kuncoro, Miguel Ballesteros, and Noah A. Smith. Recurrent neural network grammars. In Proc. of NAACL, 2016. URL: [https://aclanthology.org/N16-1024/](https://aclanthology.org/N16-1024/)

[^9]: Jonas Gehring, Michael Auli, David Grangier, Denis Yarats, and Yann N. Dauphin. Convolu- tional sequence to sequence learning. arXiv preprint arXiv:1705.03122v2, 2017. URL: [https://arxiv.org/abs/1705.03122v2](https://arxiv.org/abs/1705.03122v2)

[^10]: Alex Graves. Generating sequences with recurrent neural networks. arXiv preprint arXiv:1308.0850, 2013. URL: [https://arxiv.org/abs/1308.0850](https://arxiv.org/abs/1308.0850)

[^11]: Kaiming He, Xiangyu Zhang, Shaoqing Ren, and Jian Sun. Deep residual learning for im- age recognition. In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition, pages 770–778, 2016. URL: [https://openaccess.thecvf.com/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html](https://openaccess.thecvf.com/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html)

[^12]: Sepp Hochreiter, Yoshua Bengio, Paolo Frasconi, and Jürgen Schmidhuber. Gradient flow in recurrent nets: the difficulty of learning long-term dependencies, 2001. URL: [https://ieeexplore.ieee.org/document/6795961/](https://ieeexplore.ieee.org/document/6795961/)

[^13]: Sepp Hochreiter and Jürgen Schmidhuber. Long short-term memory. Neural computation, 9(8):1735–1780, 1997. URL: [https://direct.mit.edu/neco/article/9/8/1735/6127/Long-Short-Term-Memory](https://direct.mit.edu/neco/article/9/8/1735/6127/Long-Short-Term-Memory)

[^14]: Zhongqiang Huang and Mary Harper. Self-training PCFG grammars with latent annotations across languages. In Proceedings of the 2009 Conference on Empirical Methods in Natural Language Processing, pages 832–841. ACL, August 2009. URL: [https://aclanthology.org/D09-1087/](https://aclanthology.org/D09-1087/)

[^15]: Rafal Jozefowicz, Oriol Vinyals, Mike Schuster, Noam Shazeer, and Yonghui Wu. Exploring the limits of language modeling. arXiv preprint arXiv:1602.02410, 2016. URL: [https://arxiv.org/abs/1602.02410](https://arxiv.org/abs/1602.02410)

[^16] Łukasz Kaiser and Samy Bengio. Can active memory replace attention? In Advances in Neural Information Processing Systems, (NIPS), 2016. URL: [https://papers.nips.cc/paper/2016/hash/94c3753f3aeaf5b5a6b7d6b74f6bdf57-Abstract.html](https://papers.nips.cc/paper/2016/hash/94c3753f3aeaf5b5a6b7d6b74f6bdf57-Abstract.html)

[^17] Łukasz Kaiser and Ilya Sutskever. Neural GPUs learn algorithms. In International Conference on Learning Representations (ICLR), 2016. URL: [https://openreview.net/forum?id=Bygq-Hceg](https://openreview.net/forum?id=Bygq-Hceg)

[^18]: Nal Kalchbrenner, Lasse Espeholt, Karen Simonyan, Aaron van den Oord, Alex Graves, and Ko- ray Kavukcuoglu. Neural machine translation in linear time. arXiv preprint arXiv:1610.10099v2, 2017. URL: [https://arxiv.org/abs/1610.10099](https://arxiv.org/abs/1610.10099)

[^19]: Yoon Kim, Carl Denton, Luong Hoang, and Alexander M. Rush. Structured attention networks. In International Conference on Learning Representations, 2017. URL: [https://openreview.net/forum?id=HJFsFzE6tW](https://openreview.net/forum?id=HJFsFzE6tW)

[^20] Diederik Kingma and Jimmy Ba. Adam: A method for stochastic optimization. In ICLR, 2015. URL: [https://arxiv.org/abs/1412.6980](https://arxiv.org/abs/1412.6980)

[^21] Oleksii Kuchaiev and Boris Ginsburg. Factorization tricks for LSTM networks. arXiv preprint arXiv:1703.10722, 2017. URL: [https://arxiv.org/abs/1703.10722](https://arxiv.org/abs/1703.10722)

[^22] Zhouhan Lin, Minwei Feng, Cicero Nogueira dos Santos, Mo Yu, Bing Xiang, Bowen Zhou, and Yoshua Bengio. A structured self-attentive sentence embedding. arXiv preprint arXiv:1703.03130, 2017. URL: [https://arxiv.org/abs/1703.03130](https://arxiv.org/abs/1703.03130)

[^23] Minh-Thang Luong, Quoc V. Le, Ilya Sutskever, Oriol Vinyals, and Lukasz Kaiser. Multi-task sequence to sequence learning. arXiv preprint arXiv:1511.06114, 2015. URL: [https://arxiv.org/abs/1511.06114](https://arxiv.org/abs/1511.06114)

[^24] Minh-Thang Luong, Hieu Pham, and Christopher D Manning. Effective approaches to attention- based neural machine translation. arXiv preprint arXiv:1508.04025, 2015. URL: [https://arxiv.org/abs/1508.04025](https://arxiv.org/abs/1508.04025)

[^25] Mitchell P Marcus, Mary Ann Marcinkiewicz, and Beatrice Santorini. Building a large annotated corpus of English: The Penn Treebank. Computational linguistics, 19(2):313–330, 1993. URL: [https://www.aclweb.org/anthology/J93-2004/](https://www.aclweb.org/anthology/J93-2004/)

[^26] David McClosky, Eugene Charniak, and Mark Johnson. Effective self-training for parsing. In Proceedings of the Human Language Technology Conference of the NAACL, Main Conference, pages 152–159. ACL, June 2006. URL: [https://www.aclweb.org/anthology/N06-1020/](https://www.aclweb.org/anthology/N06-1020/)

[^27] Ankur Parikh, Oscar Täckström, Dipanjan Das, and Jakob Uszkoreit. A decomposable attention model. In Empirical Methods in Natural Language Processing, 2016. Available at: [https://www.aclweb.org/anthology/D16-1244/](https://www.aclweb.org/anthology/D16-1244/)

[^28] Romain Paulus, Caiming Xiong, and Richard Socher. A deep reinforced model for abstractive summarization. arXiv preprint arXiv:1705.04304, 2017. Available at: [https://arxiv.org/abs/1705.04304](https://arxiv.org/abs/1705.04304)

[^29] Slav Petrov, Leon Barrett, Romain Thibaux, and Dan Klein. Learning accurate, compact, and interpretable tree annotation. In Proceedings of the 21st International Conference on Computational Linguistics and 44th Annual Meeting of the ACL, pages 433–440. ACL, July 2006. Available at: [https://www.aclweb.org/anthology/P06-1055/](https://www.aclweb.org/anthology/P06-1055/)

[^30] Ofir Press and Lior Wolf. Using the output embedding to improve language models. arXiv preprint arXiv:1608.05859, 2016. Available at: [https://arxiv.org/abs/1608.05859](https://arxiv.org/abs/1608.05859)

[^31] Rico Sennrich, Barry Haddow, and Alexandra Birch. Neural machine translation of rare words with subword units. arXiv preprint arXiv:1508.07909, 2015. Available at: [https://arxiv.org/abs/1508.07909](https://arxiv.org/abs/1508.07909)

[^32] Noam Shazeer, Azalia Mirhoseini, Krzysztof Maziarz, Andy Davis, Quoc Le, Geoffrey Hinton, and Jeff Dean. Outrageously large neural networks: The sparsely-gated mixture-of-experts layer. arXiv preprint arXiv:1701.06538, 2017. Available at: [https://arxiv.org/abs/1701.06538](https://arxiv.org/abs/1701.06538)

[^33] Nitish Srivastava, Geoffrey E Hinton, Alex Krizhevsky, Ilya Sutskever, and Ruslan Salakhutdi- nov. Dropout: a simple way to prevent neural networks from overfitting. Journal of Machine Learning Research, 15(1):1929–1958, 2014. Available at: [https://jmlr.org/papers/v15/srivastava14a.html](https://jmlr.org/papers/v15/srivastava14a.html)

[^34] Sukhbaatar, Sainbayar, Szlam, Arthur, Weston, Jason, and Fergus, Rob. "End-to-end Memory Networks." In _Advances in Neural Information Processing Systems 28_, edited by C. Cortes, N. D. Lawrence, D. D. Lee, M. Sugiyama, and R. Garnett, 2440-2448. Curran Associates, Inc., 2015. [https://proceedings.neurips.cc/paper/2015/file/99aa4738c5b05f2dcd6e91966686aca9-Paper.pdf](https://proceedings.neurips.cc/paper/2015/file/99aa4738c5b05f2dcd6e91966686aca9-Paper.pdf)

[^35] Sutskever, Ilya, Vinyals, Oriol, and Le, Quoc VV. "Sequence to Sequence Learning with Neural Networks." In _Advances in Neural Information Processing Systems_, 3104-3112, 2014. [https://papers.nips.cc/paper/2014/file/a14ac55a4f27472c5d894ec1c3c743d2-Paper.pdf](https://papers.nips.cc/paper/2014/file/a14ac55a4f27472c5d894ec1c3c743d2-Paper.pdf)

[^36] Szegedy, Christian, Vanhoucke, Vincent, Ioffe, Sergey, Shlens, Jonathon, and Wojna, Zbigniew. "Rethinking the Inception Architecture for Computer Vision." _CoRR_, abs/1512.00567, 2015. [https://arxiv.org/abs/1512.00567](https://arxiv.org/abs/1512.00567)

[^37] Vinyals, Oriol, Kaiser, Lukasz, Koo, Terry, Petrov, Slav, Sutskever, Ilya, and Hinton, Geoffrey. "Grammar as a Foreign Language." In _Advances in Neural Information Processing Systems_, 2015. [https://papers.nips.cc/paper/2015/file/8d8a8e8e9fadaeb3f0efe65c3b8a381b-Paper.pdf](https://papers.nips.cc/paper/2015/file/8d8a8e8e9fadaeb3f0efe65c3b8a381b-Paper.pdf)

[^38]: Wu, Yonghui, Schuster, Mike, Chen, Zhifeng, Le, Quoc V, Norouzi, Mohammad, Macherey, Wolfgang, Krikun, Maxim, Cao, Yuan, Gao, Qin, Macherey, Klaus, et al. "Google’s Neural Machine Translation System: Bridging the Gap Between Human and Machine Translation." _arXiv preprint arXiv:1609.08144_, 2016. [https://arxiv.org/abs/1609.08144](https://arxiv.org/abs/1609.08144)

[^39] Zhou, Jie, Cao, Ying, Wang, Xuguang, Li, Peng, and Xu, Wei. "Deep Recurrent Models with Fast-forward Connections for Neural Machine Translation." _CoRR_, abs/1606.04199, 2016. [https://arxiv.org/abs/1606.04199](https://arxiv.org/abs/1606.04199)

[^40] Zhu, Muhua, Zhang, Yue, Chen, Wenliang, Zhang, Min, and Zhu, Jingbo. "Fast and Accurate Shift-Reduce Constituent Parsing." In _Proceedings of the 51st Annual Meeting of the ACL (Volume 1: Long Papers)_, 434-443. ACL, August 2013. [https://www.aclweb.org/anthology/P13-1043.pdf](https://www.aclweb.org/anthology/P13-1043.pdf)