---
title: "RWKV: Reinventing RNNs for the Transformer Era"
url:
    - "[arxiv](https://arxiv.org/abs/2305.13048)"
    - "[paper](https://arxiv.org/pdf/2305.13048.pdf)"
    - "[website](https://www.rwkv.com)"
tags:
    - lm
authors:
    - Bo Peng
    - Eric Alcaide
    - Quentin Anthony
    - Alon Albalak
    - Samuel Arcadinho
    - Huanqi Cao
    - Xin Cheng
    - Michael Chung
    - Matteo Grella
    - Kranthi Kiran GV
    - Xuzheng He
    - Haowen Hou
    - Przemysław Kazienko
    - Jan Kocon ́ 
    - Jiaming Kong 
    - Bartłomiej Koptyra 
    - Hayden Lau 
    - Krishna Sri Ipsit Mantri 
    - Ferdinand Mom
    - Atsushi Saito
    - Xiangru Tang
    - Bolun Wang
    - Johan S. Wind
    - Stanisław Woźniak 
    - Ruichong Zhang
    - Zhenyuan Zhang
    - Qihang Zhao
    - Peng Zhou
    - Jian Zhu
    - Rui-Jie Zhu
published: "2023-05-22"
status: "reading"
---
# RWKV: Reinventing RNNs for the Transformer Era
###### Authors
<ul>
<li class="author">Bo Peng</li>
<li class="separator author">|</li>
<li class="author">Eric Alcaide</li>
<li class="separator author">|</li>
<li class="author">Quentin Anthony</li>
<li class="separator author">|</li>
<li class="author">Alon Albalak</li>
<li class="separator author">|</li>
<li class="author">Samuel Arcadinho</li>
<li class="separator author">|</li>
<li class="author">Huanqi Cao</li>
<li class="separator author">|</li>
<li class="author">Xin Cheng</li>
<li class="separator author">|</li>
<li class="author">Michael Chung</li>
<li class="separator author">|</li>
<li class="author">Matteo Grella</li>
<li class="separator author">|</li>
<li class="author">Kranthi Kiran GV</li>
<li class="separator author">|</li>
<li class="author">Xuzheng He</li>
<li class="separator author">|</li>
<li class="author">Haowen Hou</li>
<li class="separator author">|</li>
<li class="author">Przemysław Kazienko</li>
<li class="separator author">|</li>
<li class="author">Jan Kocon ́</li>
<li class="separator author">|</li>
<li class="author">Jiaming Kong</li>
<li class="separator author">|</li>
<li class="author">Bartłomiej Koptyra</li>
<li class="separator author">|</li>
<li class="author">Hayden Lau</li>
<li class="separator author">|</li>
<li class="author">Krishna Sri Ipsit Mantri</li>
<li class="separator author">|</li>
<li class="author">Ferdinand Mom</li>
<li class="separator author">|</li>
<li class="author">Atsushi Saito</li>
<li class="separator author">|</li>
<li class="author">Xiangru Tang</li>
<li class="separator author">|</li>
<li class="author">Bolun Wang</li>
<li class="separator author">|</li>
<li class="author">Johan S. Wind</li>
<li class="separator author">|</li>
<li class="author">Stanisław Woźniak</li>
<li class="separator author">|</li>
<li class="author">Ruichong Zhang</li>
<li class="separator author">|</li>
<li class="author">Zhenyuan Zhang</li>
<li class="separator author">|</li>
<li class="author">Qihang Zhao</li>
<li class="separator author">|</li>
<li class="author">Peng Zhou</li>
<li class="separator author">|</li>
<li class="author">Jian Zhu</li>
<li class="separator author">|</li>
<li class="author">Rui-Jie Zhu</li>
</ul>

```dataview
table without ID url from "Mathematics/Machine Learning/Papers/RWKV/RWKV.md"
```

## Abstract  
Transformers have revolutionized almost all natural language processing (NLP) tasks but suffer from memory and computational complexity that scales quadratically with sequence length. In contrast, recurrent neural networks (RNNs) exhibit linear scaling in memory and computational requirements but struggle to match the same performance as Transformers due to limitations in parallelization and scalability. We propose a novel model architecture, Receptance Weighted Key Value (RWKV), that combines the efﬁcient parallelizable training of Transformers with the efﬁcient inference of RNNs. Our approach leverages a linear attention mechanism and allows us to formulate the model as either a Transformer or an RNN, which parallelizes computations during training and maintains constant computational and memory complexity during inference, leading to the ﬁrst non-transformer architecture to be scaled to tens of billions of parameters. Our experiments reveal that RWKV performs on par with similarly sized Transformers, suggesting that future work can leverage this architecture to create more efﬁcient models. This work presents a signiﬁcant step towards reconciling the trade-offs between computational efﬁciency and model performance in sequence processing tasks.[^1]

[^1]: Code at: https://github.com/BlinkDL/RWKV-LM

## 1  Introduction  
Deep learning techniques have made signiﬁcant strides in artiﬁcial intelligence, playing a pivotal role in various scientiﬁc and industrial applications. These applications often involve complex sequential data processing tasks that include natural language understanding, conversational AI, time-series analysis, and even indirect modalities that can be reframed as sequences, such as images and graphs (Brown et al., 2020; Ismail Fawaz et al., 2019; Wu et al., 2020; Albalak et al., 2022). Predominant among these techniques are RNNs, convolutional neural networks (CNNs), and the Transformer models ([[../Attention is all you need| Vaswani et al., 2017]]).  

Each of these has distinct drawbacks that restrict their efﬁciency in certain scenarios. RNNs suffer from the vanishing gradient problem, making them difﬁcult to train for long sequences. Additionally, they cannot be parallelized in the time dimension during training, which restricts their scalability (Hochreiter, 1998; Le and Zuidema, 2016). CNNs, on the other hand, are only adept at capturing local patterns, which limits their capacity to deal with long-range dependencies, crucial to many sequence processing tasks (Bai et al., 2018). 

Transformer models emerged as a powerful alternative due to their ability to handle both local and long-range dependencies and their capability for parallelized training (Tay et al., 2022). Recent models such as GPT-3 (Brown et al., 2020), ChatGPT (OpenAI, 2022; Koco´n et al., 2023), GPT-4 (OpenAI, 2023), [[../LLaMA]] (Touvron et al., 2023), and Chinchilla (Hoffmann et al., 2022) exemplify the capability of this architecture, pushing the frontiers of what’s possible in NLP. Despite these signiﬁcant advancements, the self-attention mechanism inherent to Transformers poses unique challenges, primarily due to its quadratic complexity. This complexity renders the architecture computationally expensive and memory-intensive for tasks involving long input sequences or in resource-constrained situations. These limitations have spurred a wealth of research aiming to improve the scaling properties of Transformers, often at the expense of some of the properties that make it so effective (Wang et al., 2020; Zaheer et al., 2020; Dao et al., 2022a).  

> [!blank-container|left-medium]
> <table>
>     <tr>
>         <th>Model</th>
>         <th>Time</th>
>         <th>Space</th>
>     </tr>
>     <tr>
>         <td>Transformer</td>
>         <td class="math display">O(T^2d)</td>
>         <td class="math display">O(T^2+Td)</td>
>     </tr>
>     <tr>
>         <td>Reformer</td>
>         <td class="math display">O(T\log Td)</td>
>         <td class="math display">O(T\log T + Td)</td>
>     </tr>
>     <tr>
>         <td>Linear Transformers</td>
>         <td class="math display">O(Td^2)</td>
>         <td class="math display">O(Td + d^2)</td>
>     </tr>
>     <tr>
>         <td>Performer</td>
>         <td class="math display">O(T^2d)</td>
>         <td class="math display">O(Td\log d + d^2 \log d)</td>
>     </tr>
>     <tr>
>         <td>AFT-full</td>
>         <td class="math display">O(T^2d)</td>
>         <td class="math display">O(Td)</td>
>     </tr>
>     <tr>
>         <td>MEGA</td>
>         <td class="math display">O(cTd)</td>
>         <td class="math display">O(cTd)</td>
>     </tr>
>     <tr>
>         <td>RWKV (ours)</td>
>         <td class="math display">O(T d)</td>
>         <td class="math display">O(d)</td>
>     </tr>
> </table>
> 
> > **Table 1**: Complexity comparison with different Transformers: Reformer (Kitaev et al., 2020), Linear Transformer (Katharopoulos et al., 2020), Performer (Choro- manski et al., 2020), AFT (Zhai et al., 2021), MEGA (Ma et al., 2023). Here $T$ denotes the sequence length, $d$ the feature dimension, and $c$ is MEGA’s chunk size of quadratic attention.


To tackle these challenges, we introduce the Receptance Weighted Key Value (RWKV) model, a novel architecture that effectively combines the strengths of RNNs and Transformers while circumventing key drawbacks. RWKV is carefully designed to alleviate the memory bottleneck and quadratic scaling associated with Transformers (Katharopoulos et al., 2020) with a more efﬁcient linear scaling, while still preserving the rich, expressive properties that make the Transformer a dominant architecture in the ﬁeld.

One of the deﬁning characteristics of RWKV is its ability to offer parallelized training and robust scalability, similar to Transformers. Moreover, we have reformulated the attention mechanism in RWKV to introduce a variant of linear attention, eschewing the traditional dot-product token interaction in favor of more effective channel-directed attention. This approach contrasts signiﬁcantly with the traditional Transformer architecture, where speciﬁc token interactions predominantly drive attention. The implementation of linear attention in RWKV is carried out without approximation, which offers a considerable improvement in efﬁciency and enhances the scalability, see Table 1. 

The overarching motivation behind developing RWKV is to bridge the gap between computational efﬁciency and expressive capacity in neural network architectures. It offers a promising and viable solution for handling tasks involving large-scale models with billions of parameters, exhibiting competitive performance at a fraction of the computational cost. Our experimental results suggest that RWKV could be a valuable tool for addressing the ongoing challenges in scaling and deploying AI models across various domains, particularly those involving sequential data processing. Thus, RWKV paves the way for the next generation of more sustainable and computationally efﬁcient AI models for sequence processing tasks.  

Our contributions in this paper are as follows:  
- We introduce the RWKV network architecture, which combines the advantages of RNNs and Transformers while mitigating their known limitations. 
- We propose a new attention mechanism reformulation that results in linear attention, eschewing the quadratic complexity associated with standard Transformer models. 
- We conduct a comprehensive series of experiments on benchmark datasets to showcase the performance, efﬁciency and scaling of RWKV in managing tasks involving large-scale models and long-range dependencies. 
- We release pretrained model ranging in size from 169 million to 14 billion parameters trained on the Pile (Gao et al., 2020).[^2]

[^2]: https://huggingface.co/RWKV 

## 2 Related Work 

Recently, a number of techniques have been proposed to address the limitations of transformers.  

**Optimizing Attention Mechanism** 
Many transformer variants (“x-formers”) have been introduced to reduce the complexity of transformers (Tay et al., 2022), including sparse attention (Beltagy et al., 2020; Kitaev et al., 2020; Guo et al., 2022), approximating the full attention matrix (Wang et al., 2020; Ma et al., 2021; Choromanski et al., 2020), combining chunked attention with gating (Ma et al., 2023) and other efﬁcient methods (Katharopoulos et al., 2020; Jaegle et al., 2021).  
Some recent works like FlashAttention (Dao et al., 2022a) and others (Rabe and Staats, 2022; Jang et al., 2019) share similarities with RWKV’s chunked computation scheme. Despite being memory-efﬁcient, their time complexity remains quadratic or contains chunk size as a hidden factor. In contrast, RWKV achieves better space and time complexity during inference by formulating a linear attention as an RNN. 

**Attention Free Models** 
Another line of research replaces the attention mechanism with other modules to scale to long sequences. MLP-Mixer and others (Tolstikhin et al., 2021; Liu et al., 2021) proposed the replacement of attention by MultiLayer Perceptrons (MLPs) in computer vision tasks. The Attention Free Transformer (AFT) (Zhai et al., 2021) replaces dot-product self-attention with a computationally efﬁcient alternative which can be seen as a multi-head attention where each feature dimension corresponds to a head. Inspired by AFT, RWKV takes a similar approach but modiﬁes the interaction weights for simplicity such that it can be transformed into an RNN. In parallel, RNNstyle (Hochreiter and Schmidhuber, 1997; Chung et al., 2014) recursive components have also been modiﬁed to increase context length, such as the Recurrent Memory Transformer (Bulatov et al., 2022, 2023) and Linear Recurrent Units (Orvieto et al., 2023). State space models (SSM) like S4 (Gu et al., 2022) and its variants (Dao et al., 2022b; Poli et al., 2023) are also proposed.  

Notably, Quasi-Recurrent neural network (QRNN) (Bradbury et al., 2017) uses both convolutional layers and recurrent pooling functions across timesteps and channels. While QRNN utilizes convolutional ﬁlters with ﬁxed sizes, RWKV employs a time-mixing module as an attention mechanism with time-decaying factors. Different from the element-wise pooling in QRNN, RWKV includes a parametrized channel-mixing module (see the green blocks in Fig.1c) that is parallelizable.  

## 3 Background  
Here we brieﬂy review the fundamentals of RNNs and Transformers.  

### 3.1  Recurrent Neural Networks (RNNs)  


Popular RNN architectures such as LSTM (Hochreiter and Schmidhuber, 1997) and GRU (Chung et al., 2014) are characterized by the following formulation (shown for LSTM, others can be reasoned similarly):

$$
\begin{align*}
f_t & = \sigma_g(W_f x_t + U_f h_{t-1} + b_f), \tag{1} \\
i_t & = \sigma_g(W_i x_t + U_i h_{t-1} + b_i), \tag{2} \\
o_t & = \sigma_g(W_o x_t + U_o h_{t-1} + b_o), \tag{3} \\
\tilde{c}_t & = \sigma_c(W_c x_t + U_c h_{t-1} + b_c), \tag{4} \\
c_t & = f_t \odot c_{t-1} + i_t \odot \tilde{c}_t, \tag{5} \\
h_t & = o_t \odot \sigma_h(c_t) \tag{6}
\end{align*}
$$

The data ﬂow of RNNs is shown in Fig. 1a. Although RNNs can be factored into two linear blocks ($W$ and $U$) and an RNN-speciﬁc block (1)–(6), as noted by Bradbury et al. (2017), the data dependency relying on previous time steps prohibits parallelizing these typical RNNs.  

> [!blank-container]
> ![[RWKV_figure1.svg]]
> > **Figure 1**: Computation structure of the RWKV in comparison to QRNN and RNN (Vanilla, LSTM, GRU, etc) architectures. Color codes: orange indicates time-mixing, convolutions or matrix multiplications, and the contin- uous block indicates that these computations can proceed simultaneously; blue signifies parameterless functions that operate concurrently along the channel or feature dimension (element-wise). Green indicates channel-mixing.

### 3.2 Transformers and AFT  

Introduced by [[../Attention is all you need|Vaswani et al., 2017]], Transformers are a class of neural networks that have become the dominant architecture for several NLP tasks. Instead of operating on sequences step-by-step like RNNs, Transformers rely on attention mechanisms to capture relationships between all input and all output tokens:

$$
Attn(Q, K, V ) = \text{softmax}(QK^{T})V \tag{7}
$$

where the multi-headness and scaling factor $\frac{1}{\sqrt{d_k}}$ are omitted for convenience. The core $QK^{T}$ multiplication is an ensemble of pairwise attention scores between each token in a sequence, which can be decomposed as vector operations:

$$
Attn(Q, K, V )_{t} = \frac{\sum_{i=1}^{T} e^{q^{T}_{t}k_{i}}v_{i}} { \sum_{i=1}^{T} e^{q^{T}_{t}k_{i}}} \tag{8}
$$

> [!NOTE]-
> This is the attention for the $t^{th}$ query vector,  not all of them. Changing things around a bit makes it simpler to read
> $$
> Attn(Q, K, V )_{t} = \sum_{i=1}^{T} \textcolor{orange}{\frac{ e^{q^{T}_{t}k_{i}}} { \sum_{i=1}^{T} e^{q^{T}_{t}k_{i}}}}v_i \tag{8}
> $$
> The part in orange is the softmax, the attention to the value $v_i$ for the query $q_t$. The outer summation is the weighted average of all the values $\mathbf{v}$. 

In AFT (Zhai et al., 2021), this is alternately formulated as:

$$
Attn^{+}(W, K, V )_{t} = \frac{\sum_{i=1}^{t} e^{w_{t,i}+k_{i}}v_{i}}{ \sum_{i=1}^{t} e^{w_{t,i}+k_{i}}} \tag{9}
$$

where $\{w_{t,i}\} \in \mathbb{R}^{T \times T}$ is the learned pair-wise position biases, and each $w_{t,i}$ is a scalar. 

Inspired by AFT, we let each $w_{t,i}$ in RWKV be a channel-wise time decay vector multiplied by the relative position, traced backwards from current time as it decays:

$$w_{t,i} = -(t - i)w \tag{10}$$


where $w \in (\mathbb{R}_{\ge 0})^{d}$, with $d$ the number of channels. We require $w$ to be non-negative to ensure that $e^{w_{t,i}} \leq 1$ and the per-channel weights decay backwards in time.

## 4 The Receptance Weighted Key Value  (RWKV) Model  


> [!blank-container|right-medium]
> ![[RWKV_figure2.svg]]
> > **Figure 2**: RWKV block elements (left) and RWKV residual block with a final head for language modeling (right) architectures.

The RWKV architecture derives its name from the four primary model elements used in the timemixing and channel-mixing blocks:
- R: **Receptance** vector acting as the acceptance of past information.
- W : **Weight** is the positional weight decay  vector. A trainable model parameter.
- K: **Key** is a vector analogous to K in traditional attention.
- V : **Value** is a vector analogous to V in traditional attention. 

Interactions between the main elements for every timestep are multiplicative, as illustrated in Fig. 2  
<br><br><br><br><br><br>
### 4.1 High-Level Summary  


> [!blank-container|left-medium]
> ![[RWKV_figure3.svg]]
> > **Figure 3**: RWKV architecture for language modelling.

The RWKV architecture is comprised of a series of stacked residual blocks, each formed by a time-mixing and a channel-mixing sub-blocks with recurrent structures.  

The recurrence is formulated both as a linear interpolation ($\mu_{\xi},\xi \in \{r,k,v\}$) between the current input $x_t$ and the input at the previous time step $x_{t-1}$ (a technique we refer to as time-shift mixing or token shift, indicated by the diagonal lines in Fig. 3), which can be adjusted independently for every linear projection of the input embedding (e.g., $R$, $K$, $V$ in time-mixing, and $R$, $K$ in channel-mixing), and as the time-dependent update of the $W$ $K$ $V$ which is formalized in equation 14. The $W$ $K$ $V$ computation is similar to AFT (Zhai et al., 2021), but $W$ is now a channel-wise vector multiplied by relative position rather than a pairwise matrix in AFT. We also introduce a vector $U$ for separately attending to the current token in order to compensate for potential degeneration of $W$ (see [[#G Model Behavior Visualization|Appendix G]] for more details).  

The time-mixing block is given by:

$$
\begin{align}
r_t &= W_r \cdot (\mu_r x_t + (1 - \mu_r) x_{t-1}) \tag{11} \\
k_t &= W_k \cdot (\mu_k x_t + (1 - \mu_k) x_{t-1}) \tag{12} \\
v_t &= W_v \cdot (\mu_v x_t + (1 - \mu_v) x_{t-1}) \tag{13} \\
wkv_t &= \frac{\sum_{i=1}^{t-1} e^{-(t-1-i)w+k_i}v_i + e^{u+k_t}v_t}{\sum_{i=1}^{t-1} e^{-(t-1-i)w+k_i} + e^{u+k_t}} \tag{14} \\
o_t &= W_o \cdot (\sigma(r_t) \odot wkv_t) \tag{15} 
\end{align}
$$

where the $WKV$ computation, $wkv_t$, plays the role of $Attn(Q, K, V)$ in Transformers without incurring a quadratic cost as interactions are between scalars. Intuitively, as time $t$ increases, the vector $o_t$ is dependent on a long history, represented by the summation of an increasing number of terms. For the target position $t$, RWKV performs a weighted summation in the positional interval of $[1, t]$, and then multiplies with the receptance $\sigma(r)$. Therefore, interactions are multiplicative inside a given timestep and summed over different timesteps.  
Further, the channel-mixing block is given by:

$$
\begin{align}
r_t &= W_r \cdot (\mu_r x_t + (1 - \mu_r) x_{t-1}) \tag{16} \\
k_t &= W_k \cdot (\mu_k x_t + (1 - \mu_k) x_{t-1}) \tag{17} \\
o_t &= \sigma(r_t) \odot (W_v \cdot \max(k_t, 0)^2) \tag{18}
\end{align}
$$

where we adopt squared ReLU activation (So et al., 2021). Note that in both time-mixing and channel-mixing, by taking the sigmoid of the receptance, we're intuitively using it as a "forget gate" to eliminate unnecessary historical information.

### 4.2 Transformer-like Parallelization  

RWKV can be efficiently parallelized in what we call a time-parallel mode, reminiscent of Transformers. The time complexity of processing a batch of sequences in a single layer is $O(BTd^2)$, which mainly consists of matrix multiplications $W_{\xi}$, $\xi \in \{r, k, v, o\}$ (assuming $B$ sequences, $T$ maximum tokens and $d$ channels). Meanwhile, updating attention scores $wkv_t$ requires a serial scan (see Appendix B for more detail) and has complexity $O(BTd)$. 
The matrix multiplications can be parallelized akin to $W_{\xi}$, $\xi \in \{Q, K, V, O\}$ in typical Transformers. 

The element-wise $W$ $K$ $V$ computation is time-dependent, but can be readily parallelized along the other two dimensions (Lei et al., 2018)[^3]. Additionally, token shift is implemented as a simple offset in the temporal dimension at each block using PyTorch (Paszke et al., 2019) library as 

```python
nn.ZeroPad2d((0,0,1,-1))
```

[^3]: If the sequence is very long, more sophisticated methods such as Martin and Cundy (2017) that parallelize over sequence length could be used.

### 4.3 RNN-like Sequential Decoding  

It is common in recurrent networks to use output at state $t$ as input at state $t + 1$. This is especially evident in the autoregressive decoding inference of a language model, requiring each token to be computed before fed into the next step, making it possible for `RWKV` to take advantage of its RNN-like structure, referred to as time-sequential mode. In such circumstances, `RWKV` can be conveniently formulated recursively for decoding during inference, as shown in [[#B. Time-Mixing Block as an RNN Cell|Appendix B]], which leverages the advantage that each output token is dependent only on the latest state, which is of constant size, irrespective of the sequence length.  It then behaves as an `RNN` decoder, yielding constant speed and memory footprint with respect to the sequence length, enabling the processing of longer sequences more efﬁciently. In contrast, self-attention typically requires a $KV$ cache growing linearly with respect to the sequence length, resulting in degraded efﬁciency and increasing memory footprint and time as the sequence grows longer.

### 4.4 Software Implementation  
RWKV is originally implemented using the Pytorch Deep Learning Library (Paszke et al., 2019) and a custom CUDA kernel for the W KV computation explained in 4.7. Although RWKV is a general recurrent network, its current implementation focuses in the task of language modeling (RWKV-LM). The model architecture is comprised of an embedding layer, for which we follow the setup described in Section 4.7 and several identical residual blocks applied sequentially as seen in Fig. 2 and 3 following the principles outlined in [[#4.6 Harnessing Temporal Structure for Sequential Data Processing|Section 4.6]]. After the last block, a simple output projection head composed by a LayerNorm (Ba et al., 2016) and a linear projection is used to obtain the logits to be used in the next-token prediction task and calculate the cross entropy loss during training. Both the embeddings generated after the last residual block and the logits could also be used later for downstream NLP tasks. Training is performed in time-parallel mode ([[#4.2 Transformer-like Parallelization|Section 4.2]]) while autoregressive inference and a potential chat interface[^4] leverage the time-sequential mode ([[#4.3 RNN-like Sequential Decoding|Section 4.3]]).

[^4]: https://github.com/BlinkDL/ChatRWKV
### 4.5 Gradient Stability and Layer Stacking

The RWKV architecture has been designed as a fusion of both Transformers and RNNs, offering the advantage of stable gradients and deeper architectures of Transformers compared to traditional RNNs while being efﬁcient in inference.
Previous work has sought to tackle the problem of gradient stability in RNNs with a variety of techniques including using non-saturated activation functions (Chandar et al., 2019), gating mechanism (Gu et al., 2019), gradient clipping (Pascanu et al., 2012), and adding constraints (Kanai et al., 2017; Miller and Hardt, 2018). While these techniques have seen little success, RWKV avoids the problem inherently by utilizing softmax in conjunction with RNN-style updates.
The RWKV model features a single-step process for updating attention-like scores, which includes a time-dependent softmax operation that helps numerical stability and guards against vanishing gradients (for rigorous proof, see [[#F Gradient Stability in RWKV|Appendix F]]). Intuitively, this operation ensures the gradient is propagated along the most relevant path. Layer normalization (Ba et al., 2016) is another key aspect of the architecture which enhances the training dynamics of deep neural networks by stabilizing gradients, addressing both vanishing and exploding gradient issues.
These design elements not only contribute to the RWKV architecture’s stability and learning capabilities but enable the stacking of multiple layers in a manner that surpasses the capabilities of any existing RNN. In doing so, the model is able to capture more complex patterns across various levels of abstraction (see also [[#G Model Behavior Visualization|Appendix G]]).


### 4.6 Harnessing Temporal Structure for  Sequential Data Processing

RWKV captures and propagates sequential information through the combination of three mechanisms: recurrence, time decay and token shift.

The **recurrence** in the time-mixing block of RWKV is the basis for the model’s capacity to capture intricate relationships between sequence elements and to propagate locality information through time.

The **time decay** mechanism ($e^{−w}$ and $e^u$ in equation 14), maintains sensitivity to the positional relationship between sequence elements. By gradually diminishing the inﬂuence of past information  over time, the model preserves a sense of temporal locality and progression, which is essential for sequential processing. This treatment of positional information in sequential data exhibits similarities to the Attention with Linear Biases (ALiBi) model (Press et al., 2022), where the linear biases facilitate input length extrapolation. In this context, the RWKV architecture can be perceived as a trainable version of ALiBi, seamlessly incorporating positional information without the necessity for explicit encoding. It can also be seen as an extension of the gated convolution introduced in Zhai et al. (2021) to the full sequence length until a given step.

The **token shift** or **time-shift mixing**, or (diagonal arrows in Figure 3), also contributes to the model’s adaptation to sequential data. By linearly interpolating between the current input and the previous time step input, the model naturally aggregates and gates information in the input channels. The overall structure of time-shift mixing bears resemblance to the causal convolution with no dilations in WaveNet (van den Oord et al., 2016), which is a classical architecture used for forecasting time series data.


### 4.7 Additional Optimizations 
###### Custom Kernels
To address inefﬁciencies in the W KV computation due to the sequential nature of the task when using standard deep learning frameworks, we implement a custom CUDA kernel so as to launch a single compute kernel in training accelerators. All other parts of the model are matrix multiplications and point-wise operations that can already be efﬁciently parallelized.

###### FFN with R gate
Prior research (Tolstikhin et al., 2021; Liu et al., 2021; Yu et al., 2022) suggests that self-attention may not be as essential in Transformer-based vision tasks as previously thought. Although it provided us with some insights, replacing self-attention entirely in natural language tasks could be too drastic. In our study, we partially dismantle the attention mechanism by replacing the ﬁxed $QKV$ formula with $KV$ and introducing a new time-decaying factor $W$ . This approach enables us to incorporate token and channelmixing components akin to MLP-mixer (Tolstikhin et al., 2021) and a gating unit R similar to gMLP (Liu et al., 2021), which enhance the performance of our RWKV model.

###### Small Init Embedding 
During the initial stage of training a transformer model ([[../Attention is all you need|Vaswani et al., 2017]]), we observe that the embedding matrix undergoes slow changes, which pose a challenge for the model to deviate from its initial noisy embed- ding state. To mitigate this issue, we propose an approach that involves initializing the embedding matrix with small values and subsequently applying an additional LayerNorm operation. By implementing this technique, we accelerate and stabilize the training process, enabling the training of deep architectures with post-LN components. The effectiveness of this approach is demonstrated in Figure 8, where it is shown to facilitate improved convergence by allowing the model to quickly transition away from the initially small embedding. This is achieved through small changes following a single step, which in turn lead to substantial alterations in directions and subsequently significant changes after the LayerNorm operation.

###### Custom Initialization 
Building on principles from previous works (He et al., 2016; Jumper et al., 2021), we initialize parameters to values as similar as possible to an identity mapping while breaking symmetry so there is a clean information path. Most weights are initialized to zero. No biases are used for linear layers. Specific formulas are given in [[#D Parameter initializations|Appendix D]]. We find the choice of initialization to be significant in convergence speed and quality (see [[#E Small Init Embedding|Appendix E]]).


## 5 Evaluations

In this section, we focus on evaluating to answer the following questions:
- **RQ1**:  Is RWKV competitive against quadratic transformer architectures with equal number of parameters and training tokens?
- **RQ2**: When increasing the number of parameters, does RWKV remain competitive against quadratic transformer architectures?
- **RQ3**: Does increasing parameters of RWKV yield better language modeling loss, when RWKV models are trained for context lengths that most open-sourced quadratic transformers cannot efﬁciently process?  


> [!blank-container]
> ![[RWKV_figure4.svg]]
> > **Figure 4**: Zero-Shot Performance: The horizontal axis is a number of parameters and the vertical axis is accuracy.

> [!blank-container|right-medium]
> ![[RWKV_figure5.svg]]
> > **Figure 5**: Increasing context length contributes to lower test loss on the Pile (Gao et al., 2020).

Addressing **RQ1** and **RQ2**, from Fig. 4, we can see that RWKV is very competitive on six benchmarks (Winogrande, PIQA, ARC-C, ARC-E, LAMBADA, and SciQ) against major open source quadratic complexity transformer models: Pythia (Biderman et al., 2023), OPT (Zhang et al., 2022) and BLOOM (Scao et al., 2022). RWKV even out- performs Pythia and GPT-Neo (Black et al., 2022) in four tasks: PIQA, OBQA, ARC-E, and COPA (See details in Appendix H). For **RQ3**, Fig. 5 shows that increasing context length leads to lower test loss on the Pile, an indication that RWKV can make effective use of long contextual information.




## 6  Inference Experiments

> [!blank-container|right-medium]
> ![[RWKV_figure6.svg]]
> > **Figure 6**: Cumulative time during text generation for different LLMs.

We benchmark inference requirements according to size and family. Specifically, we evaluate text generation speed and memory requirements on a typical compute platforms including CPU (x86) and GPU (NVIDIA A100 80GB). For all our ex- periments we use float32 precision. We include all model parameters in parameter count, including both embedding and non-embedding layers. Performance under different quantization setups is left to further work. See Appendix I for more results. 

Additionally, we carried out comparative studies on RWKV-4 and ChatGPT / GPT-4, see Appendix J. They revealed that RWKV-4 is very sensitive to prompt engineering. When the prompts were ad- justed from the ones used for GPT to more suitable for RWKV, the F1-measure performance increased even from 44.2% to 74.8%.

## 7 Future Work

There are several promising directions for future work on the RWKV architecture:
- Increasing model expressivity with enhanced time-decay formulations and exploring initial model states while maintaining efﬁciency.
- Further improving RWKV computational efficiency by applying parallel scan in the wkvt step to reduce the computational cost to $O(B \log(T )d)$.
- Investigating the application of RWKV to encoder-decoder architectures and potential replacement of cross-attention mechanism. This could have applicability seq2seq or multimodal settings, enhancing efficiency both in training and inference.
- Leveraging RWKV’s state (or context) for interpretability, predictability in sequence data and safety. Manipulating the hidden state could also guide behavior and allow greater customizability through prompt tuning.
- Exploring fine-tuned models in specific settings for enhanced interaction with humans (Ouyang et al., 2022). Particularly interesting would be the performance under different datasets and specific use cases.
- Adapting parameter-efficient fine-tuning methods such as [[../LoRA]]  and characterizing behavior under different quantization schemes for the proposed architecture


## 8 Conclusions

We introduced RWKV, a new approach to RNN models exploiting the potential of time-based mixing components. RWKV introduces several key strategies which allow it to capture locality and long-range dependencies, while addressing limitations of current architectures by: 
1. replacing the quadratic $QK$ attention by a scalar formulation with linear cost, 
2. reformulating recurrence and sequential inductive biases to unlock efficient training parallelization and efficient inference, and 
3. enhancing training dynamics using custom initializations.

We benchmark the proposed architecture in a wide variety of NLP tasks and show comparable performance to SoTA with reduced cost. Further experiments on expressivity, interpretability, and scaling showcase the model capabilities and draw parallels in behavior between RWKV and other LLMs.

RWKV opens a new door to scalable and efficient architectures to model complex relationships in sequential data. While many alternatives to Transformers have been proposed with similar claims, ours is the first to back up those claims with pretrained models with tens of billions of parameters.

## 9 Limitations

While our proposed RWKV model has demonstrated promising results regarding training and memory efficiency during inference, some limitations should be acknowledged and addressed in future work. First, the linear attention of RWKV leads to significant efficiency gains but still, it may also limit the model’s performance on tasks that require recalling minutiae information over very long contexts. This is due to the funneling of information through a single vector representation over many time steps, compared with the full information maintained by the quadratic attention of standard Transformers. In other words, the model’s recurrent architecture inherently limits its ability to “look back” at previous tokens, as opposed to traditional self-attention mechanisms. While learned time decay helps prevent the loss of information, it is mechanistically limited compared to full self-attention.

Another limitation of this work is the increased importance of prompt engineering in comparison to standard Transformer models. The linear attention mechanism used in RWKV limits the information from the prompt that will be carried over to the model’s continuation. As a result, carefully designed prompts may be even more crucial for the model to perform well on tasks.


## B. Time-Mixing Block as an RNN Cell

> [!blank-container|right-medium]
> ![[RWKV_figure7.svg]]
> > **Figure 7**: RWKV time-mixing block formulated as an RNN cell. Color codes: yellow (μ) denotes the token shift, red (1) denotes the denominator, blue (2) denotes the numerator, pink (3) denotes the fraction computa- tions in 14. h denotes the numerator-denominator tuple (a, b).

As stated in 4.3, the RWKV time-mixing block can be formulated as an RNN, as the W KV computation can be written in such a recursive form:

The initial conditions are:

$$
\begin{align}
a_0, b_0 &= 0 \tag{19}
\end{align}
$$

The computation equations are:

$$
\begin{align}
w_{kv}^t &= \frac{a_{t-1} + e^{u+kt}v_t}{b_{t-1} + e^{u+kt}} \tag{20}\\
a_t &= e^{-w}a_{t-1} + e^{kt}v_t \tag{21}\\
b_t &= e^{-w}b_{t-1} + e^{kt} \tag{22}
\end{align}
$$

The dataflow of the RNN-like time-mixing is shown in Fig. 7, where the hidden states $h$ is the numerator-denominator tuple $(a, b)$.

To avoid overflow in calculating $e^{kt}$, a numerical trick is used in the official implementation. Note

$$
\begin{align}
a_1 & = e^{-wa_0} + e^{k_0v_0} = e^{k_0v_0}, \tag{23} \\
b_1 & = e^{-wb_0} + e^{k_0} = e^{k_0}, \tag{24}
\end{align}
$$

and we set $a'_1 = v_0$, $b'_1 = 1$, $p_0 = k_0$, where $p_{t-1}$ stores the shared exponents of $a_t$ and $b_t$. Now the above recursion can be converted into a numerical safe version, for each time step $t > 1$:

$$
\begin{align}
q & := \max(p_{t−1}, u + k_t), \tag{25} \\
a^*_t & = e^{p_{t−1}−q}a'_{t-1} + e^{u+k_t−q}v_t, \tag{26} \\
b^*_t & = e^{p_{t−1}−q}b'_{t-1} + e^{u+k_t−q}, \tag{27} \\
wk_vt & = \frac{a^*_t}{b^*_t}. \tag{28}
\end{align}
$$

The update to $a'_t$, $b'_t$ and their shared exponent are also carried out in similar fashion:

$$
\begin{align}
q & := \max(p_{t−1} − w, k_t), \tag{29} \\
a'_t & = e^{p_{t−1}−w−q}a'_{t-1} + e^{k_t−q}v_t, \tag{30} \\
b'_t & = e^{p_{t−1}−w−q}b'_{t-1} + e^{k_t−q}, \tag{31} \\
p_t & = q. \tag{32}
\end{align}
$$


## C Parameter and FLOP Count for the RWKV Models

The following section provides an overview of the different RWKV model architectures along with their respective parameter and FLOP counts in Table 2.

> [!blank-container|right-medium]
> <table>
>     <thead>
>         <tr>
>             <th>Name</th>
>             <th>Layers</th>
>             <th>Model Dimension</th>
>             <th>Parameters</th>
>             <th>FLOPs per token</th>
>         </tr>
>     </thead>
>     <tbody>
>         <tr>
>             <td class="math display">169 M</td>
>             <td class="math display">12</td>
>             <td class="math display">768</td>
>             <td class="math display">1.693 \times 10^{8}</td>
>             <td class="math display">2.613 \times 10^{8}</td>
>         </tr>
>         <tr>
>             <td class="math display">430 M</td>
>             <td class="math display">24</td>
>             <td class="math display">1024</td>
>             <td class="math display">4.304 \times 10^{8}</td>
>             <td class="math display">7.573 \times 10^{8}</td>
>         </tr>
>         <tr>
>             <td class="math display">1.5 B</td>
>             <td class="math display">24</td>
>             <td class="math display">2048</td>
>             <td class="math display">1.515 \times 10^{9}</td>
>             <td class="math display">2.823 \times 10^{9}</td>
>         </tr>
>         <tr>
>             <td class="math display">3 B</td>
>             <td class="math display">32</td>
>             <td class="math display">2560</td>
>             <td class="math display">2.985 \times 10^{9}</td>
>             <td class="math display">5.710 \times 10^{9}</td>
>         </tr>
>         <tr>
>             <td class="math display">7 B</td>
>             <td class="math display">32</td>
>             <td class="math display">4096</td>
>             <td class="math display">7.393 \times 10^{9}</td>
>             <td class="math display">1.437 \times 10^{10}</td>
>         </tr>
>         <tr>
>             <td class="math display">14 B</td>
>             <td class="math display">40</td>
>             <td class="math display">5120</td>
>             <td class="math display">1.415 \times 10^{10}</td>
>             <td class="math display">2.778 \times 10^{10}</td>
>         </tr>
>     </tbody>
> </table>
> Table 2: RWKV model architectures and associated FLOP counts


The number of parameters for each model is computed using the formula: $\text{{\#parameters}} = 2VD + 13D^2L + D(11L + 4)$ where $V = 50277$ is the vocabulary size, $D$ represents the Model Dimension, and $L$ corresponds to the number of layers.

FLOPs is for a forward pass for one token. It was calculated as $6(VD + 13D^2L)$, which is the twice (add and multiply) the number of parameters in linear layers. The backwards pass FLOPs can be approximated as twice that of the forward pass. So $12 \times 10^3$ the total is $6(VD + 13D^2L)$ per token for training (3x forward FLOPs). It is noteworthy that FLOPs are independent of the context length, unlike regular transformers. The FLOP approximations in this paper are in line with the methodology used by Kaplan et al. (2020).

Alternative approximations for FLOPs include doubling the parameters which yields similar results within 2% for 14B and a 30% discrepancy for 169M variant. Another approximation is based on the number of non-embedding parameters multiplied by 2. This gives $2(VD + 13D^2L + D(11L + 4))$ resulting in 1.6% more FLOPs for 14B model and 8% more FLOPs for 169M model.

## D Parameter initializations

We describe the specific parameter initializations below and motivate the design choices. Parameters belonging to residual blocks are often adjusted by layer depth and total number of layers. Let $N$ denote the vocabulary size, $s$ denote the embedding dimension, $d$ denote the hidden size (we use $d = 4s$), $L$ the number of layers, $l$ the layer index (from 0 to $L-1$), we use the following initializations:

- Embeddings are initialized to $U(\pm1e-4)$ as explained in 4.7.
- All LayerNorm weights start from 1 and biases from 0.
- For the channel-mixing blocks (11), $\mu_{ki}$ and $\mu_{ri}$ are initialized to $\left(\frac{i}{s}\right)^{1-l/L}$.
- For the time-mixing blocks (16), initializations are $\mu_{vi} = \left(\frac{i}{s}\right)^{1-l/L}$ and $\mu_{ri} = 0.5\left(\frac{i}{s}\right)^{1-l/L}$.
- $w_i$ (14), also known as "time decay," is initialized to $\textstyle -5 + 8\cdot\left(\frac{i}{d-1}\right)^{0.7+1.3l/(L-1)}$. Intuitively, it is the discount factor applied to previous tokens over time.
- $u_i$ (14), also known as "bonus," is set to $0.5\left(\left(\left(i+1\right) \mod 3\right) - 1\right) + \log 0.3$. It is the special weighting applied to the current token in equation 14. The alternating zigzag pattern initially creates subtle variations in the tensor elements, which are intended to help the model treat different dimensions of the embedding distinctively.
- $W_o$ (15) (time-mixing) and $W_v$ (channel-mixing) are initialized to $\textstyle \mathcal{N}\left(0, \sqrt{\frac d s}=2\right)$.
- All $W_r$, $W_k$, $W_v$ weights are initialized to 0 so the model can start learning from the beginning without noisy signals.
- All LayerNorm weights start from 1 and biases from 0.

## E Small Init Embedding

This section presents experimental validation of small initialization embedding. The experimental setup is as follows. In the baseline configuration, the parameters are initialized using a normal distribution with a mean of 0.0 and a standard deviation of 0.02, which is a commonly used initialization method in models like BERT and GPT. On the other hand, in the small initialization of the embedding (small init emb) experiment, the parameters are initialized using a uniform distribution with a range of 1e-4, which is slightly different from RWKV where a normal distribution with a standard deviation of 1e-4 is used. However, this difference is negligible and does not affect our conclusions. The experiments were conducted with a batch size of 400.

## F Gradient Stability in RWKV

In this section, we present a mathematical description of the gradient stability property in RWKV, focusing specifically on the time-mixing block. By gradient stability, we mean that if the inputs $x_t$ are bounded and the model parameters are fixed, then the gradients with respect to $W_k$ and $W_v$ are uniformly bounded for all $T$ (thus not exploding). Consequently, we can control the amount each $x_t$ contributes to the gradient at $T$ in a naturally decaying fashion by the weight decay mechanism $w$ (thus not vanishing unless desired).

First, we make the simplification that there are no token shifts; this will not affect the final conclusion. In this scenario, $w_{kv}^T$ can be written as

$$
w_{kv}^T = \frac{1}{T} \sum_{t=1}^{T} K_e^t v_t = \frac{1}{T} \sum_{t=1}^{T} K_e^t \tag{1}
$$

where

$$
v_t = W_v x_t, \quad \frac{\partial v_t}{\partial W_v} = x_t \tag{2}
$$

and

$$
K_e^t = e^{W_k x_t + w_T^t}, \quad \frac{\partial K_e^t}{\partial W_k} = x_t K_e^t \tag{3}
$$

$S(\cdot)$ and $E(\cdot)$ are shorthand for denoting sums and averages over weights $K_e^t$.

The loss function at position $T$ can be written as

$$
L_T = l(f(w_{kv}^T), y_T) \tag{4}
$$

Because $w_{kv}^T$ relates to $(W_k)_{i,j}$ and $(W_v)_{i,j}$ only through the $i$-th channel $(w_{kv}^T)_i$, we have

$$
\frac{\partial L_T}{\partial (W_v)_{i,j}} = \frac{\partial L_T}{\partial (w_{kv}^T)_i} \frac{\partial (w_{kv}^T)_i}{\partial (W_v)_{i,j}} \tag{5}
$$

The first part of the above equation contains trivial operations like output layers and other layers of time-mixing, which can be proven inductively. The second part of the above equation can be bounded as

$$
\frac{\partial (w_{kv}^T)_i}{\partial (W_v)_{i,j}} = \frac{\partial E_i[(v_t)_i]}{\partial (W_v)_{i,j}} = \left| \frac{\partial E_i[(x_t)_j]}{\partial (W_v)_{i,j}} \right| \leq \max_t |(x_t)_j| \tag{6}
$$

which is irrelevant to $T$. Similarly,

$$
\frac{\partial (w_{kv}^T)_i}{\partial (W_k)_{i,j}} = \frac{\partial S_i[(v_t)_i]}{S_i(1) \frac{\partial}{\partial (W_k)_{i,j}}} = \frac{E_i[(x_t)_j(v_t)_i] - E_i

[(x_t)_j]E_i[(v_t)_i]}{\text{cov}_i((x_t)_j, (v_t)_i)} \tag{7}
$$

can also be bounded. Note that $w_{kv}$'s softmax operation contains at least two non-zero terms ($u$ and $w$), so the above "covariance" will not degenerate into 0.




## G Model Behavior Visualization
> [!blank-container|right-medium]
> ![[RWKV_figure9.svg]]
> > **Figure 9**: Model behavior visualizations of the RWKV model.


In Figure 9, we present visualizations of some behavior of the RWKV model.

The top plot illustrates the time decays ($e^{-w}$) in each layer of the RWKV-169M model, sorted along the channel axis. Notably, several decays in the last layers are very close or equal to one, implying that certain information is preserved and propagated throughout the model’s temporal context. Meanwhile, many decays in the initial layer are close to zero, which corresponds to local operations in wkv (14), likely to be associated with tasks such as text parsing or lexical analysis. (Note that the local operations in wkv is due to the extra parameter $u$, when $e^{-w}$ is degenerated into 0.) These patterns of time decays are partly learned, but also come from parameter initialization as it speeds up training.

The bottom plot shows the information retrieval and propagation path in the RWKV-430M model. The experiment follows the causal trace method introduced by Meng et al. (2022), where we

1. Run the model once, and record all states and activation of each layer during the computation;
2. Corrupt the input embeddings of the subject using noise (“The Eiffel Tower” in this example);
3. Restore the states and activation of a certain layer at a certain token during the computation, and record the log-probability of the model outputting the correct answer (“Paris”).

Unlike transformers, RWKV relies on recursive propagation of information in the time dimension. In this case, the fact that "the Eiffel Tower is located in Paris" is retrieved in layer 4. It is then passed down to the subsequent layers. In layer 20, mostly, the information is propagated through time until reaching where it is needed. Finally, it is passed down to the last layer for outputting the answer.


<table>
    <thead>
    <tr>
        <th>Model</th>
        <th>Params B</th>
        <th>PIQA acc</th>
        <th>StoryCloze acc_norm</th>
        <th>HellaSwag acc</th>
        <th>WinoGrande acc</th>
        <th>ARC-e acc</th>
        <th>ARC-c acc_norm</th>
        <th>OBQA acc_norm</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>RWKV-4</td>
        <td>0.17</td>
        <td>65.07</td>
        <td>58.79</td>
        <td>32.26</td>
        <td>50.83</td>
        <td>47.47</td>
        <td>24.15</td>
        <td>29.60</td>
    </tr>
    <tr>
        <td>Pythia</td>
        <td>0.16</td>
        <td>62.68</td>
        <td>58.47</td>
        <td>31.63</td>
        <td>52.01</td>
        <td>45.12</td>
        <td>23.81</td>
        <td>29.20</td>
    </tr>
    <tr>
        <td>GPT-Neo</td>
        <td>0.16</td>
        <td>63.06</td>
        <td>58.26</td>
        <td>30.42</td>
        <td>50.43</td>
        <td>43.73</td>
        <td>23.12</td>
        <td>26.20</td>
    </tr>
    <tr>
        <td>RWKV-4</td>
        <td>0.43</td>
        <td>67.52</td>
        <td>63.87</td>
        <td>40.90</td>
        <td>51.14</td>
        <td>52.86</td>
        <td>25.17</td>
        <td>32.40</td>
    </tr>
    <tr>
        <td>Pythia</td>
        <td>0.40</td>
        <td>66.70</td>
        <td>62.64</td>
        <td>39.10</td>
        <td>53.35</td>
        <td>50.38</td>
        <td>25.77</td>
        <td>30.00</td>
    </tr>
    <tr>
        <td>GPT-Neo</td>
        <td>0.40</td>
        <td>65.07</td>
        <td>61.04</td>
        <td>37.64</td>
        <td>51.14</td>
        <td>48.91</td>
        <td>25.34</td>
        <td>30.60</td>
    </tr>
    <tr>
        <td>RWKV-4</td>
        <td>1.5</td>
        <td>72.36</td>
        <td>68.73</td>
        <td>52.48</td>
        <td>54.62</td>
        <td>60.48</td>
        <td>29.44</td>
        <td>34.00</td>
    </tr>
    <tr>
        <td>Pythia</td>
        <td>1.4</td>
        <td>71.11</td>
        <td>67.66</td>
        <td>50.82</td>
        <td>56.51</td>
        <td>57.74</td>
        <td>28.58</td>
        <td>30.80</td>
    </tr>
    <tr>
        <td>GPT-Neo</td>
        <td>1.4</td>
        <td>71.16</td>
        <td>67.72</td>
        <td>48.94</td>
        <td>54.93</td>
        <td>56.19</td>
        <td>25.85</td>
        <td>33.60</td>
    </tr>
    <tr>
        <td>RWKV-4</td>
        <td>3.0</td>
        <td>74.16</td>
        <td>70.71</td>
        <td>59.89</td>
        <td>59.59</td>
        <td>65.19</td>
        <td>33.11</td>
        <td>37.00</td>
    </tr>
    <tr>
        <td>Pythia</td>
        <td>2.8</td>
        <td>73.83</td>
        <td>70.71</td>
        <td>59.46</td>
        <td>61.25</td>
        <td>62.84</td>
        <td>32.25</td>
        <td>35.20</td>
    </tr>
    <tr>
        <td>GPT-Neo</td>
        <td>2.8</td>
        <td>72.14</td>
        <td>69.54</td>
        <td>55.82</td>
        <td>57.62</td>
        <td>61.07</td>
        <td>30.20</td>
        <td>33.20</td>
    </tr>
    <tr>
        <td>RWKV-4</td>
        <td>7.4</td>
        <td>76.06</td>
        <td>73.44</td>
        <td>65.51</td>
        <td>61.01</td>
        <td>67.80</td>
        <td>37.46</td>
        <td>40.20</td>
    </tr>
    <tr>
        <td>Pythia</td>
        <td>6.9</td>
        <td>74.54</td>
        <td>72.96</td>
        <td>63.92</td>
        <td>61.01</td>
        <td>66.79</td>
        <td>35.07</td>
        <td>38.00</td>
    </tr>
    <tr>
        <td>GPT-J</td>
        <td>6.1</td>
        <td>75.41</td>
        <td>74.02</td>
        <td>66.25</td>
        <td>64.09</td>
        <td>66.92</td>
        <td>36.60</td>
        <td>38.20</td>
    </tr>
    <tr>
        <td>RWKV-4</td>
        <td>14.2</td>
        <td>77.48</td>
        <td>76.06</td>
        <td>70.65</td>
        <td>63.85</td>
        <td>70.24</td>
        <td>37.99</td>
        <td>39.27</td>
    </tr>
    <tr>
        <td>GPT-level∗</td>
        <td>14.2</td>
        <td>76.49</td>
        <td>74.97</td>
        <td>68.72</td>
        <td>65.14</td>
        <td>70.77</td>
        <td>37.99</td>
        <td>39.27</td>
    </tr>
    <tr>
        <td>Pythia (c.f.)</td>
        <td>11.8</td>
        <td>75.90</td>
        <td>74.40</td>
        <td>67.38</td>
        <td>64.72</td>
        <td>69.82</td>
        <td>36.77</td>
        <td>38.80</td>
    </tr>
    <tr>
        <td>GPT-NeoX (c.f.)</td>
        <td>20.6</td>
        <td>77.69</td>
        <td>76.11</td>
        <td>71.42</td>
        <td>65.98</td>
        <td>72.69</td>
        <td>40.44</td>
        <td>40.20</td>
    </tr>
    </tbody>
</table>



