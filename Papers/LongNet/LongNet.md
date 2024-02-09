---
title: "LongNet: Scaling Transformers to 1B Tokens"
authors:
    - Jiayu Ding
    - Shuming Ma
    - Li Dong
    - Xingxing Zhang
    - Shaohan Huang
    - Wenhui Wang
    - Furu Wei
url:
    - "[paper](https://arxiv.org/pdf/2307.02486.pdf)"
    - "[arXiv](https://arxiv.org/abs/2307.02486)"
published: "2023-07-05"
---

# LongNet: Scaling Transformers to 1B Tokens
###### Authors
<ul>
<li class="author">Jiayu Ding</li>
<li class="separator author">|</li>
<li class="author">Shuming Ma</li>
<li class="separator author">|</li>
<li class="author">Li Dong</li>
<li class="separator author">|</li>
<li class="author">Xingxing Zhang</li>
<li class="separator author">|</li>
<li class="author">Shaohan Huang</li>
<li class="separator author">|</li>
<li class="author">Wenhui Wang</li>
<li class="separator author">|</li>
<li class="author">Furu Wei</li>
</ul>

```dataview
table without ID url from "Mathematics/Machine Learning/Papers/LongNet/LongNet.md"
```


#### Abstract

Scaling sequence length has become a critical demand in the era of large language models. However, existing methods struggle with either computational complexity or model expressivity, rendering the maximum sequence length restricted. In this work, we introduce LONGNET, a Transformer variant that can scale sequence length to more than 1 billion tokens, without sacrificing the performance on shorter sequences. Specifically, we propose dilated attention, which expands the attentive field exponentially as the distance grows. LONGNET has significant advantages:
1. it has a linear computation complexity and a logarithm dependency between tokens; 
2. it can be served as a distributed trainer for extremely long sequences; 
3. its dilated attention is a drop-in replacement for standard attention, which can be seamlessly integrated with the existing Transformer-based optimization. Experiments results demonstrate that LONGNET yields strong performance on both long-sequence modeling and general language tasks. Our work opens up new possibilities for modeling very long sequences, e.g., treating a whole corpus or even the entire Internet as a sequence. Code is available at https://aka.ms/LongNet.



> [!blank-container]
> ![[figure_1.svg]]
> > **Figure 1**: Trend of Transformer sequence lengths over time.
> 

## 1. Introduction

Recent years have witnessed a trend toward scaling neural networks. The depth is primarily scaled up for exponential expressivity, producing many powerful deep networks. Then, the sparse MoE models and model parallelism approaches efficiently enlarge the hidden dimension. Sequence length, as the last atomic dimension of the neural network, is desirable to be unlimited. Breaking the limitation of sequence length introduces significant advantages. First, it provides large memory and receptive field for models, which is practical for them to interact with human and the world. Second, a longer context contains more complex causality and reasoning paths that models can exploit in training data. In contrast, short dependency has more spurious correlations, which is harmful to generalization. Third, it enables to explore the limits of in-context learning, which has the potential to be a paradigm shift for many-shot learning, as an extremely long context may help the models alleviate catastrophic forgetting.

The major challenge of scaling up sequence length is striking the right balance between the computational complexity and the model expressivity. RNN-style models are primarily implemented to increase the length. However, its sequential nature limits the parallelization during training, which is essential in long-sequence modeling. More recently, <mark style="background: #ADCCFFA6;">state space models</mark> are appealing to sequence modeling. It can operate as a CNN during training, and transform to an efficient RNN at test time. While they perform well at long-range benchmarks, their performance on regular lengths is not as good as Transformers, limited mainly by the model expressivity.

Another strand of scaling the sequence length is to decrease the complexity of Transformers, i.e., the quadratic complexity of self-attention. Implementing sliding windows or convolution modules over the attention is a straightforward way to make the complexity nearly linear. Nevertheless, this sacrifices the ability to recall the early tokens, forgetting the prompts at the very beginning of the sequence. Sparse attention reduces the computation by sparsifying the attention matrix, preserving the possibility of recalling long-distant information. For example, obtains $\mathcal{O}(N \sqrt{N} d)$ time complexity with a fixed sparse pattern. Besides the heuristic patterns, the learnable patterns prove to be useful for sparse attention. There are also some other efficient Transformer-based variants, including low-rank attention, kernel-based methods, downsampling approaches, recurrent models, and retrieval-based methods. Yet, none has been scaled to 1 billion tokens (see Figure 1).

> [!blank-container]
> | Method | Computation Complexity |
> | :--- | :---: |
> | Recurrent | $\mathcal{O}\left(N d^{2}\right)$ |
> | Vanilla Attention | $\mathcal{O}\left(N^{2} d\right)$ |
> | Sparse Attention | $\mathcal{O}(N \sqrt{N} d)$ |
> | **Dilated Attention (This Work)** | $\mathcal{O}(N d)$ |
> 
> > **Table 1**: Comparison of computation complexity among different methods. $N$ is the sequence length and $d$ is the hidden dimension.
> 

In this work, we successfully scale the sequence length to 1 billion tokens. Our solution is LONGNET, which replaces the attention of vanilla Transformers with a novel component named dilated attention. The general design principle is - *attention allocation decreases exponentially as the distance between tokens grows*. We prove that it obtains a linear computation complexity and a logarithm dependency between tokens. This deals with the contradiction between limited attention resources and the accessibility to every token. In the implementation, LONGNET can be transformed into a dense Transformer, which seamlessly supports the off-the-shelf optimization for Transformers (e.g., kernel fusion, quantization, and distributed training). Taking advantage of the linear complexity, LONGNET can parallelize the training across nodes, breaking the constraint of both computation and memory with a distributed algorithm. This allows us to efficiently scale up the sequence length to 1B tokens with nearly constant runtime (see Figure 5), while vanilla Transformer suffers from quadratic complexity.

## 2. LONGNET

### 2.1. Preliminary

The core of Transformers is self-attention, which maps a query and a set of keys and values to output. Given the inputs $Q, K, V \in \mathbb{R}^{N \times d}$, it computes the outputs $O$ with

$$
O=\operatorname{softmax}\left(Q K^{T}\right) V \tag{1}
$$

Self-attention struggles with long sequences, due to its quadratic dependency on the sequence length. One query would attend to all keys and values, leading to computational inefficiencies.

Sparse attention alleviates this issue by restricting the query's access to a subset of keys and values. The key of sparse attention is the sparse attention pattern $S \in\{0,1\}^{N \times N}$, which determines specific keys and values that the query $Q$ can attend to.

$$
O=\operatorname{softmax}\left(Q K^{T} \odot \mathbb{1}_{S}\right) V \tag{2}
$$

For example, the fixed pattern of sparse Transformer is composed of a local pattern and a strided pattern. The sequence is divided into blocks of length $l$. The local pattern allows one query to attend to tokens within the same block, while strided pattern allows one query to attend to the last $c$ tokens of each block. Formally, the local pattern $S_{i}^{(1)}=\{j \mid\lfloor j / l\rfloor=\lfloor i / l\rfloor\}$, and the strided pattern $S_{i}^{(2)}=\{j \mid j \bmod l \in\{t, t+1, \ldots, l\}\}$.

### 2.2. Dilated Attention

Figure 2 illustrates the overview of dilated attention. Dilated attention splits the input $(Q, K, V)$ into segments $\left\{\left(\widetilde{Q}_{i}, \widetilde{K}_{i}, \widetilde{V}_{i}\right)\right\}^{\frac{N}{w}}$ equally with a segment length $w$. Each segment is then sparsified along the sequence dimension by selecting the rows with an interval $r$. The computation can be written as:

$$
\begin{align}
\widetilde{Q}_{i} & =\left[Q_{i w}, Q_{i w+r}, Q_{i w+2 r}, \ldots, Q_{(i+1) w-1}\right] \tag{3} \\
\widetilde{K}_{i} & =\left[K_{i w}, K_{i w+r}, K_{i w+2 r}, \ldots, K_{(i+1) w-1}\right] \tag{4} \\
\widetilde{V}_{i} & =\left[V_{i w}, V_{i w+r}, V_{i w+2 r}, \ldots, V_{(i+1) w-1}\right] \tag{5}
\end{align}
$$

The sparsified segments $\left\{\left(\widetilde{Q}_{i}, \widetilde{K}_{i}, \widetilde{V}_{i}\right)\right\}^{\frac{N}{w}}$ are fed into the attention in parallel, after which are scattered and concatenated as the output $O$ :

$$
\begin{gather}
\widetilde{O}_{i}=\operatorname{softmax}\left(\widetilde{Q}_{i} \widetilde{K}_{i}^{T}\right) \widetilde{V}_{i} \tag{6} \\
\hat{O}_{i}=\left\{\widetilde{O}_{i, j}|j \bmod r=0 ; 0| j \bmod r \neq 0\right\}\tag{7} \\
O=\left[\hat{O}_{0}, \hat{O}_{1}, \ldots, \hat{O}_{\frac{N}{w}-1}\right] \tag{8}
\end{gather}
$$

In the implementation, the dilated attention can be transformed into dense attention between a gathering operation over the input $(Q, K, V)$ and a scattering operation over the output $\widetilde{O}_{i}$, so it can directly reuse any optimization for vanilla attention (e.g., flash attention). Dilated attention can significantly reduce the computation cost by a factor of $\frac{N}{w} r^{2}$ over the vanilla attention.

> [!blank-container]
> 
> ![[figure_2.svg]]
> > **Figure 2**: Building blocks of dilated attention used in LONGNET. It consists of a series of attention patterns for modeling both short-range and long-range dependency. The number of attention patterns can be extended according to the sequence length.

$$
\begin{gather}
O=\left.\sum_{i=1}^{k} \alpha_{i} O\right|_{r_{i}, w_{i}} \tag{9}\\
\alpha_{i}=\frac{s_{i}}{\sum_{j} s_{j}} \tag{10}
\end{gather}
$$

In practice, the segment size $w$ trades the globality of attention for efficiency, while the dilation with a size $r$ reduces the computation cost by approximating the attention matrix. To capture both long-range and short-range information efficiently, we implement a mixture of dilated attentions with different segment sizes and dilation rates $\left\{r_{i}, w_{i}\right\}^{k}$ : where $s_{i}$ denotes the denominator of the attention softmax for $\left.O\right|_{r_{i}, w_{i}}$. Note that the computations for $\left\{\left.O\right|_{r_{i}, w_{i}}\right\}^{k}$ are in parallel because there is no computation dependency among them. Experiments show that dynamic weights calculated by the denominator of the attention softmax are better than learnable fixed weights. For a query attends to keys in different dilated attentions, our method to mix dilated attentions is equivalent to gather keys in different parts and calculate softmax together.

Intuitively, the local attention should be precisely computed, while the global attention can be approximate. Therefore, we set a larger $w_{i}$ with a bigger $r_{i}$. Moreover, we gradually increase the $w_{i}$ for each attention until it reaches the maximum length $N$ or the number of attention patterns $k$ :

$$
\begin{gather}
w=\left\{w_{0}, w_{1}, w_{2}, \ldots, N\right\}^{k} \quad\left(w_{i}<w_{i+1}<N\right) \tag{11} \\
r=\left\{1, r_{1}, r_{2}, \ldots, r_{k}\right\}^{k} \quad\left(1<r_{i}<r_{i+1}\right) \tag{12}
\end{gather}
$$

In practice, we set $w$ and $r$ to geometric sequences for an exponential attentive field.

### 2.3. Multi-Head Dilated Attention

> [!blank-container]
> 
> ![[figure_3.svg]]
> > **Figure 3**: Dilated attention with multiple heads. The attention patterns differ among heads by shifting the position successively.

As shown in Figure 3, we differ in the computation among different heads by sparsifying different parts of the query-key-value pairs. Specifically, for the $j$-th head, we have an offset $s_{j}=j \bmod r$ when selecting the $(Q, K, V)$ :

$$
\begin{gather}
\widetilde{Q}_{i}=\left[Q_{i w+s_{j}}, Q_{i w+s_{j}+r}, Q_{i w+s_{j}+2 r}, \ldots, Q_{(i+1) w+s_{j}-1}\right] \tag{13} \\
\widetilde{K}_{i}=\left[K_{i w+s_{j}}, K_{i w+s_{j}+r}, K_{i w+s_{j}+2 r}, \ldots, K_{(i+1) w+s_{j}-1}\right] \tag{14} \\
\widetilde{V}_{i}=\left[V_{i w+s_{j}}, V_{i w+s_{j}+r}, V_{i w+s_{j}+2 r}, \ldots, V_{(i+1) w+s_{j}-1}\right] \tag{15}
\end{gather}
$$

Following the vanilla multi-head attention, the outputs of different heads are concatenated into a final output. The rest of the computation remains the same as the single-head counterpart in Section 2.2.

### 2.4. Computational Complexity and Token Dependency

Given dilated attention with a segment size and dilation rate of $(r, w)$, each query-key-value pair is sparsified from $(Q, K, V) \in \mathbb{R}^{N \times d}$ to $(Q, K, V) \in \mathbb{R}^{\frac{w}{r} \times d}$, so the flops of the attention computation are estimated as:

$$
F L O P s=\frac{2 N}{w}\left(\frac{w}{r}\right)^{2} d=\frac{2 N w d}{r^{2}} \tag{16}
$$

We further extend it to dilated attention with multiple segment sizes and dilation rates. The flops can be written as:

$$
F L O P s=2 N d \sum_{i=1}^{k} \frac{w_{i}}{r_{i}^{2}} \tag{17}
$$

With the segment sizes and dilation rates in Equation (11) and Equation (12), the flops are given by

$$
\text { FLOPs }=2 w_{0} N d \sum_{i=0}^{k-1} \frac{1}{\alpha^{i}} \leq \frac{2 \alpha}{\alpha-1} w_{0} N d \quad(\alpha>1) \tag{18}
$$

where $w_{0}$ is a predefined constant and $\alpha$ is the common ratio for geometric sequences $w$ and $r$. Therefore, the computation complexity of dilated attention is approximate to $\mathcal{O}(N d)$.

Moreover, the information of each tokens can be propagated to a maximum distance of $D$ :

$$
D=\sum_{i=0}^{l-1} w_{i}=w_{0} \sum_{i=0}^{l-1} \alpha^{i} \approx \frac{w_{0}}{\alpha-1} \alpha^{l} \tag{19}
$$
 
where $l$ is the length of the propagated path. Therefore, the maximum path length of a sequence with $N$ tokens can be estimated as:

$$
L \approx \log _{\alpha} \frac{N(\alpha-1)}{w_{0}} \quad(\alpha>1) \tag{20}
$$

This proves that the token dependency is approximate to $\mathcal{O}(\log N)$.

## 3 LONGNET as a Distributed Trainer: Scaling up to 1B Tokens

> [!blank-container|right-medium]
> 
> ![[figure_4.svg]]
> > **Figure 4**: Distributed training of LONGNET on two GPU devices. It parallelizes the training by partitioning the sequence dimension. The computation and communication costs are nearly constant as the number of devices grows.


Although the computation complexity of dilated attention has been greatly reduced to $\mathcal{O}(N d)$, it is infeasible to scale the sequence length to the million level on a single GPU device due to the computation and memory constraints. There are some distributed training algorithms for large-scale model training, such as model parallelism, sequence parallelism, and pipeline parallelism. However, they are insufficient for LONGNET especially when the sequence dimension is extremely large.

### 3.1 Distributed Algorithm

We take advantage of the linear computation complexity of LONGNET for the distributed training of sequence dimension. Without loss of generality, Figure 4 presents our distributed algorithm on two GPUs, which can be further scaled to an arbitrary number of devices. We start by splitting the input sequence along the sequence dimension. Each sequence is put on one device separately:

$$
X=\left[X_{1}, X_{2}\right] \tag{21}
$$

Then, they are projected into queries, keys, and values on the two devices:

$$
\left[Q_{1}, K_{1}, V_{1}\right]=\left[W_{Q}, W_{K}, W_{V}\right] X_{1}, \quad\left[Q_{2}, K_{2}, V_{2}\right]=\left[W_{Q}, W_{K}, W_{V}\right] X_{2} \tag{22}
$$

For the segment length $w_{i} \leq l$ (where $l$ is the sequence length on the local device), we compute the attention locally with Equation (3) to Equation (8). For the segment length $w_{i}>l$, the keys and values


> [!blank-container|left-large]
> ![[figure_5.svg]]
> Figure 5: Runtime of our dilated attention and vanilla attention. Both are equipped with FlashAttention.
> 

are distributed across different devices. Therefore, we collect the key-value pairs before computing the attention. We use Equation (3) to Equation (5) to sparsify the $\{Q, K, V\}$ into $\{\widetilde{Q}, \widetilde{K}, \widetilde{V}\}$. An all-gather operation is implemented to collect the key-value pairs:

$$
\widetilde{K}=\left[\widetilde{K_{1}}, \widetilde{K_{2}}\right], \quad \widetilde{V}=\left[\widetilde{V_{1}}, \widetilde{V_{2}}\right] \tag{23}
$$

Note that the all-gather operation in the backward becomes a reduce-scatter operation. Different from vanilla attention, both sizes of $\widetilde{K}_{i}$ and $\widetilde{V}_{i}$ are independent of the sequence length $N$, making the communication cost constant.

Finally, we compute the cross-attention with the local queries $\widetilde{Q_{i}}$ and the global key-value pairs $\{\widetilde{K}, \widetilde{V}\}$. The formulation is written as:

$$
\widetilde{O_{1}}=\operatorname{softmax}\left(\widetilde{Q_{1}} \widetilde{K}^{T}\right) \widetilde{V}, \quad \widetilde{O_{2}}=\operatorname{softmax}\left(\widetilde{Q_{2}} \widetilde{K}^{T}\right) \widetilde{V} \tag{24}
$$

The concatenation of the outputs across different devices becomes the final attention output:

$$
\widetilde{O}=\left[\widetilde{O_{1}}, \widetilde{O_{2}}\right] \tag{25}
$$

The distributed algorithm described above is orthogonal to other parallelisms, including data parallelism which partitions the batch dimension, model parallelism which partitions the hidden dimension, and pipeline parallelism which partitions the layers.

### 3.2 Scaling up to 1B Tokens

We verify the feasibility of scaling to 1B tokens with the modern distributed systems. Starting from 8K, we gradually scale the sequence length until the limit of GPU memory. We reduce the batch size accordingly to keep the number of tokens per batch at 1 billion. Each model of different sequence lengths has up to 3 segment lengths, which are 2,048, the number of tokens per device, and the sequence length. We compute the average speed in the forward propagation for 10 different runs.

Figure 5 reports the runtime of vanilla attention and our dilated attention. Both of them are implemented with FlashAttention Kernel for saving memory and improving speed. It shows that dilated attention can successfully scale up the sequence length with almost constant latency. By partitioning the sequence dimension, it can leverage the distributed systems to scale the sequence length to 1 billion tokens. In contrast, vanilla attention suffers from the quadratic dependency on the sequence length. Its latency dramatically increases as the length grows. Moreover, there is no distributed algorithm for vanilla attention to break sequence length limitation. This proves the advantage of the linear complexity as well as the distributed algorithm for LONGNET.

## 4 Experiments on Language Modeling

### 4.1 Setup

We implement LONGNET on language modeling. The backbone architecture is MAGNETO with XPOS relative position encoding, except that we replace the standard attention with our dilated attention. We use the base-size configuration of MAGNETO, which has a hidden dimension of 768, 12 attention heads, and 12 decoder layers. We pre-train the model with The Stack dataset, a source code collection in over 300 programming languages. The data is preprocessed with the tiktoken tokenizer[^2] with `cl100k_base` encoding. The models are trained with a batch size of $0.5 \mathrm{M}$ tokens for $300 \mathrm{~K}$ steps. More details regarding the hyperparameters can be found in the [[#A Hyperparameters|appendix]]. All experiments are conducted based on the torchscale codebase.

[^2]: https://github.com/openai/tiktoken

### 4.2 Results

We compare LONGNET with both vanilla Transformer and sparse Transformers. The differences among the architectures are the attention layers, while the others remain the same. We scale the sequence length of these models from $2 \mathrm{~K}$ to $32 \mathrm{~K}$, while reducing the batch size to keep the number of tokens per batch constant. For LONGNET, we use segment lengths of $w=\{2048,4096,8192,16384,32768\}$, and the dilated ratios are $r=\{1,2,4,6,12\}$. We implement the fixed pattern for sparse attention as in with multiple heads attending to distinct subblocks. The block size is set to 2048. We adjust their sparse ratios to match the computation flops with LONGNET so that the comparison is fair. The attention layers in vanilla Transformers are dense and fully connected, so the computation cost is much higher. Due to the computation constraints, we only scale it up to $32 \mathrm{~K}$ sequence length. All of our implementations of attention variants are based on FlashAttention[^3] for training efficiency. We customize the flash attention kernels for both sparse attention and dilated attention.

[^3]: https://github.com/HazyResearch/flash-attention/tree/main


> [!blank-container]
> | Model              | Length | Batch |      | Github |       |
> |:------------------ |:------:|:-----:|:----:|:------:|:-----:|
> |                    |        |       |  2K  |   8K   |  32K  |
> | Transformer        |   2K   |  256  | 4.24 |  5.07  | 11.29 |
> | Sparse Transformer |   8K   |  64   | 4.39 |  3.35  | 8.79  |
> | LongNet            |   8K   |  64   | 4.23 |  3.24  | 3.36  |
> | Sparse Transformer |  16K   |  32   | 4.85 |  3.73  | 19.77 |
> | LongNet            |  16K   |  32   | 4.27 |  3.26  | 3.36  |
> | Sparse Transformer |  32K   |  16   | 5.15 |  4.00  | 3.64  |
> | LongNet            |  32K   |  16   | 4.37 |  3.33  | 3.01  |
> 
> 
> > **Table 2**: Perplexity of language models for LongNet and the baselines.

Table 2 summarizes the results of these models on the Stack dataset. We use perplexity as the evaluation metric. The models are tested with different sequence lengths, ranging from $2 \mathrm{~K}$ to $32 \mathrm{~K}$. When the input is longer than the maximum length that the models support, we implement blockwise causal attention (BCA), a state-of-the-art extrapolation method for language model inference. Besides, we remove the absolute position encoding. Primarily, the results demonstrate that increasing the sequence length during training generally leads to a better language model. Secondly, the extrapolation of sequence length in inference does not apply to the case when the length is much larger than the model supports. Finally, LONGNET consistently outperforms the baseline models, proving its effectiveness in language modeling.

### 4.3 Scaling Curves of Sequence Length

> [!blank-container|right-large]
> ![[figure_6.svg]]
> > **Figure 6**: Test perplexity of LONGNET and dense Transformers using different sequence lengths during training. LONGNET outperforms dense Transformers with a lower perplexity and a significantly smaller amount of computation.


Previous work has shown that language models follow some scaling laws by increasing parameters or training tokens. We are interested in the performance of language models when the context length is scaled up during training. We test the losses with inputs of a mixture of different lengths, from $1 \mathrm{~K}$ to $32 \mathrm{~K}$. We use blockwise causal attention during inference to improve the generalization of sequence lengths.

Figure 6 plots the scaling curves of sequence length for both vanilla Transformers and LONGNET. We estimate the amount of compute by calculating the total flops of matrix multiplication. The results show that both vanilla Transformers and LONGNET benefit from a larger context length during training. However, LONGNET can scale up the context length more efficiently, achieving a lower test loss with a smaller amount of computing. This demonstrates the advantage of longer training input over extrapolation. In conclusion, our experiments show that LONGNET is a more efficient way to scale up the context length in language models. This is because LONGNET can learn longer-range dependencies more effectively.

### 4.4 Scaling up Model Size

An important property of large language models is that the loss scales as a power law with compute. To verify whether LONGNET still follows the similar scaling law, we train a series of models with different model sizes, from 125 million to 2.7 billion parameters. The $2.7 \mathrm{~B}$ model is trained with 300B tokens, while the rest digest about 40B tokens. Figure 7(a) plots the scaling curve of LONGNET regarding the compute. We compute the perplexity on the same test set. The amount of compute is estimated by calculating the total flops of matrix multiplication during training. It proves that LONGNET can still follow the power law. This implies that the dense Transformer is not a prerequisite for scaling the language models. Additionally, the scalability and the efficiency are both obtained by LONGNET.

> [!blank-container]
> ![[figure_7.svg]]
> > **Figure 7**: Left: Test loss of LonGNET with an increasing model size. The scaling curve follows a similar law to the vanilla Transformers. Right: Test loss of LONGNET using different context windows. A longer context window yields better language modeling.
> 


### 4.5 Long Context Prompting

Prompting is an essential method to guide and provide additional information to the language models. We conduct experiments to verify whether LONGNET can benefit from a longer context window for prompting. Specifically, we reserve a piece of prefixes as the prompt and test the perplexity of its suffixes. We gradually scale the length of the prompt from $2 \mathrm{~K}$ to $32 \mathrm{~K}$. For a fair comparison, we keep the suffixes the same, while increasing the length of the prefixes to the maximum lengths of the models. Figure 7(b) reports the results on the test set. It shows that the test loss of LONGNET gradually decreases as the context window grows. This demonstrates the superiority of LONGNET in fully leveraging the long context to improve the language model.

## 5 Conclusion and Future Work

We present LONGNET, a Transformer variant that can scale the sequence length to 1 billion tokens and beyond, with no loss in shorter sequences. The core of LONGNET is dilated attention, which reduces the computation complexity from quadratic to linear. LONGNET can be served as a distributed trainer that parallelizes the training of a sequence across multiple GPU devices. Experiments show that LONGNET has superior performance over the strong baselines on modeling both long and short sequences. In the future, we will extend LONGNET to support more tasks, e.g., multimodal large language modeling, BEiT pretraining, and genomic data modeling.

Acknowledgement We would like to acknowledge Yuqing Xia and Jilong Xue for the early exploration of the flash attention kernel.

## A Hyperparameters

> [!blank-container]
> | Hyperparameters | Value |
> | :--- | :---: |
> | Layers | 12 |
> | Hidden size | 768 |
> | FFN size | 3072 |
> | Heads | 12 |
> | Learning rate | $6 \mathrm{e}-4$ |
> | LR scheduler | Polynomial decay |
> | Warm-up steps | 750 |
> | Tokens per batch | $500 \mathrm{~K}$ |
> | Adam $\beta$ | $(0.9,0.98)$ |
> | Training steps | $300 \mathrm{~K}$ |
> | Gradient clipping | 2.0 |
> | Dropout | 0.0 |
> | Weight decay | 0.01 |
> 
> > **Table 3**: Hyperparamters used for the models in Table 2.

> [!blank-container]
> 
> | Parameters | Layers | Hidden | Heads | Learning Rate | Batch Size |
> | :---: | :---: | :---: | :---: | :---: | :---: |
> | $125 \mathrm{M}$ | 12 | 768 | 12 | $6 \mathrm{e}-4$ | $500 \mathrm{~K}$ |
> | $350 \mathrm{M}$ | 24 | 1024 | 16 | $6 \mathrm{e}-4$ | $500 \mathrm{~K}$ |
> | $760 \mathrm{M}$ | 24 | 1536 | 16 | $6 \mathrm{e}-4$ | $500 \mathrm{~K}$ |
> | $2.7 \mathrm{~B}$ | 32 | 2560 | 32 | $2 \mathrm{e}-4$ | $4 \mathrm{M}$ |
> 
> > **Table 4**: Hyperparamters used for the experiments in Figure 7(a).

