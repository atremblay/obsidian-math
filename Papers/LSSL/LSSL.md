---
title: Combining Recurrent, Convolutional, and Continuous-time Models with Linear State-Space Layers
authors:
  - Albert Gu
  - Isys Johnson
  - Karan Goel
  - Khaled Saab
  - Tri Dao
  - Atri Rudra
  - Christopher Ré
published: 2021-10-26
arXiv: http://arxiv.org/abs/2110.13985
paper: http://arxiv.org/pdf/2110.13985
aliases:
  - Linear State Space Layer
---

#### Abstract

Recurrent neural networks (RNNs), temporal convolutions, and neural differential equations (NDEs) are popular families of deep learning models for time-series data, each with unique strengths and tradeoffs in modeling power and computational efficiency. We introduce a simple sequence model inspired by control systems that generalizes these approaches while addressing their shortcomings. The Linear State-Space Layer (LSSL) maps a sequence $u \mapsto y$ by simply simulating a linear continuous-time state-space representation $\dot{x} = Ax + Bu, y = Cx + Du$. Theoretically, we show that LSSL models are closely related to the three aforementioned families of models and inherit their strengths. For example, they generalize convolutions to continuous-time, explain common RNN heuristics, and share features of NDEs such as ==time-scale adaptation==. We then incorporate and generalize recent theory on continuous-time memorization to introduce a trainable subset of structured matrices $A$ that endow LSSLs with long-range memory. Empirically, stacking LSSL layers into a simple deep neural network obtains state-of-the-art results across time series benchmarks for long dependencies in sequential image classification, real-world healthcare regression tasks, and speech. On a difficult speech classification task with length-16000 sequences, LSSL outperforms prior approaches by 24 accuracy points, and even outperforms baselines that use hand-crafted features on 100x shorter sequences.



## 1 Introduction

A longstanding challenge in machine learning is efficiently modeling sequential data longer than a few thousand time steps. The usual paradigms for designing sequence models involve recurrence (e.g. RNNs), convolutions (e.g. CNNs), or differential equations (e.g. NDEs), which each come with tradeoffs. For example, RNNs are a natural stateful model for sequential data that require only constant computation/storage per time step, but are slow to train and suffer from optimization difficulties (e.g., the "vanishing gradient problem" [39]), which empirically limits their ability to handle long sequences. CNNs encode local context and enjoy fast, parallelizable training, but are not sequential, resulting in more expensive inference and an inherent limitation on the context length. NDEs are a principled mathematical model that can theoretically address continuous-time problems and long-term dependencies [37], but are very inefficient.

Ideally, a model family would combine the strengths of these paradigms, providing properties like parallelizable training (convolutional), stateful inference (recurrence) and time-scale adaptation (differential equations), while handling very long sequences in a computationally efficient way. Several recent works have turned to this question. These include the CKConv, which models a continuous convolution kernel [44]; several ODE-inspired RNNs, such as the [[../UnICORNN|UnICORNN]]; the LMU, which speeds up a specific linear recurrence using convolutions [12, 58]; and HiPPO [24], a generalization of the LMU that introduces a theoretical framework for continuous-time memorization. However, these model families come at the price of reduced expressivity: intuitively, a family that is both convolutional and recurrent should be more restrictive than either.

> [!blank-container]
> ![[Images/figure1.svg]]
> > Figure 1: (Three views of the LSSL) A Linear State Space Layer layer is a map $u_{t} \in \mathbb{R} \rightarrow y_{t} \in \mathbb{R}$, where each feature $u_{t} \mapsto y_{t}$ is defined by discretizing a state-space model $A, B, C, D$ with a parameter $\Delta t$. The underlying state space model defines a discrete recurrence through combining the state matrix $A$ and timescale $\Delta t$ into a transition matrix $\bar{A}$. (Left) As an implicit continuous model, irregularly-spaced data can be handled by discretizing the same matrix $A$ using a different timescale $\Delta t$. (Center) As a recurrent model, inference can be performed efficiently by computing the layer timewise (i.e., one vertical slice at a time $\left(u_{t}, x_{t}, y_{t}\right),\left(u_{t+1}, x_{t+1}, y_{t+1}\right), \ldots$ ), by unrolling the linear recurrence. (Right) As a convolutional model, training can be performed efficiently by computing the layer depthwise in parallel (i.e., one horizontal slice at a time $\left(u_{t}\right)_{t \in[L]},\left(y_{t}\right)_{t \in[L]}, \ldots$ ), by convolving with a particular filter.
> 


Our first goal is to construct an expressive model family that combines all 3 paradigms while preserving their strengths. The Linear State-Space Layer (LSSL) is a simple sequence model that maps a 1-dimensional function or sequence $u(t) \mapsto y(t)$ through an implicit state $x(t)$ by simulating a linear continuous-time state-space representation in discrete-time

$$
\begin{align}
& \dot{x}(t)=A x(t)+B u(t) \tag{1} \\
& y(t)=C x(t)+D u(t) \tag{2}
\end{align}
$$

where $A$ controls the evolution of the system and $B, C, D$ are projection parameters. The LSSL can be viewed as an instantiation of each family, inheriting their strengths (Fig. 1):

- LSSLs are recurrent. If a discrete step-size $\Delta t$ is specified, the LSSL can be discretized into a linear recurrence using standard techniques, and simulated during inference as a stateful recurrent model with constant memory and computation per time step.
- LSSLs are convolutional. The linear time-invariant systems defined by (1)+(2) are known to be explicitly representable as a continuous convolution. Moreover, the discrete-time version can be parallelized during training using convolutions [12, 44].
- LSSLs are continuous-time. The LSSL itself is a differential equation. As such, it can perform unique applications of continuous-time models, such as simulating continuous processes, handling missing data [45], and adapting to different timescales.

Surprisingly, we show that LSSLs do not sacrifice expressivity, and in fact generalize convolutions and RNNs. First, classical results from control theory imply that all 1-D convolutional kernels can be approximated by an LSSL [59]. Additionally, we provide two results relating RNNs and ODEs that may be of broader interest, e.g. showing that some RNN architectural heuristics (such as gating mechanisms) are related to the step-size $\Delta t$ and can actually be derived from ODE approximations. As corollaries of these results, we show that popular RNN methods are special cases of LSSLs.

The generality of LSSLs does come with tradeoffs. In particular, we describe and address two challenges that naive LSSL instantiations face when handling long sequences: 
    (i) they inherit the limitations of both RNNs and CNNs at remembering long dependencies, and 
    (ii) choosing the state matrix $A$ and timescale $\Delta t$ appropriately are critical to their performance, yet learning them is computationally infeasible. 

We simultaneously address these challenges by specializing LSSLs using a carefully chosen class of structured matrices $A$, such that 
    (i) these matrices generalize prior work on continuous-time memory [24] and mathematically capture long dependencies with respect to a learnable family of measures, and 
    (ii) with new algorithms, LSSLs with these matrices $A$ can be theoretically sped up under certain computation models, even while learning the measure $A$ and timescale $\Delta t$.

We empirically validate that LSSLs are widely effective on benchmark datasets and very long time series from healthcare sensor data, images, and speech.

- On benchmark datasets, LSSLs obtain SoTA over recent RNN, CNN, and NDE-based methods across sequential image classification tasks (e.g., by over $10 \%$ accuracy on sequential CIFAR) and healthcare regression tasks with length-4000 time series (by up to $80 \%$ reduction in RMSE).
- To showcase the potential of LSSLs to unlock applications with extremely long sequences, we introduce a new sequential CelebA classification task with length-38000 sequences. A small LSSL comes within 2.16 accuracy points of a specialized ResNet-18 vision architecture that has 10x more parameters and is trained directly on images.
- Finally, we test LSSLs on a difficult dataset of high-resolution speech clips, where usual speech pipelines pre-process the signals to reduce the length by 100x. When training on the raw length-16000 signals, the LSSL not only (i) outperforms previous methods by over 20 accuracy points in $1 / 5$ the training time, but (ii) outperforms all baselines that use the pre-processed length-160 sequences, overcoming the limitations of hand-crafted feature engineering.


### Summary of Contributions

- We introduce Linear State-Space Layers (LSSLs), a simple sequence-to-sequence transformation that shares the modeling advantages of recurrent, convolutional, and continuous-time methods. Conversely, we show that RNNs and CNNs can be seen as special cases of LSSLs ([[#3 Linear State-Space Layers (LSSL)|§3]]).
- We prove that a structured subclass of LSSLs can learn representations that solve continuous-time memorization, allowing it to adapt its measure and timescale ([[#4.1 Incorporating Long Dependencies into LSSLs|§4.1]]). We also provide new algorithms for these LSSLs, showing that they can be sped up computationally under an arithmetic complexity model [[#4.2 Theoretically Efficient Algorithms for the LSSL|§4.2]].
- Empirically, we show that LSSLs stacked into a deep neural network are widely effective on time series data, even (or especially) on extremely long sequences (Section 5).


## 2 Technical Background

We summarize the preliminaries on differential equations that are necessary for this work. We first introduce two standard approximation schemes for differential equations that we will use to convert continuous-time models to discrete-time, and will be used in our results on understanding RNNs. We give further context on the step size or timescale $\Delta t$, which is a particularly important parameter involved in this approximation process. Finally, we provide a summary of the HiPPO framework for continuous-time memorization [24], which will give us a mathematical tool for constructing LSSLs that can address long-term dependencies.

###### Approximations of differential equations
Any differential equation $\dot{x}(t)=f(t, x(t))$ has an equivalent integral equation $x(t)=x\left(t_{0}\right)+\int_{t_{0}}^{t} f(s, x(s)) d s$. This can be numerically solved by storing some approximation for $x$, and keeping it fixed inside $f(t, x)$ while iterating the equation. For example, Picard iteration is often used to prove the existence of solutions to ODEs by iterating the equation $x_{i+1}(t):=x_{i}\left(t_{0}\right)+\int_{t_{0}}^{t} f\left(s, x_{i}(s)\right) d s$ . In other words, it finds a sequence of functions $x_{0}(t), x_{1}(t), \ldots$ that approximate the solution $x(t)$ of the integral equation.

###### Discretization
On the other hand, for a desired sequence of discrete times $t_{i}$, approximations to $x\left(t_{0}\right), x\left(t_{1}\right), \ldots$ can be found by iterating the equation $x\left(t_{i+1}\right)=x\left(t_{i}\right)+\int_{t_{i}}^{t_{i+1}} f(s, x(s)) d s$. Different ways of approximating the RHS integral lead to different discretization schemes. We single out a discretization method called the generalized bilinear transform (GBT) which is specialized to linear ODEs of the form (1). Given a step size $\Delta t$, the GBT update is

$$
x(t+\Delta t)=(I-\alpha \Delta t \cdot A)^{-1}(I+(1-\alpha) \Delta t \cdot A) x(t)+\Delta t(I-\alpha \Delta t \cdot A)^{-1} B \cdot u(t) .
$$

Three important cases are: $\alpha=0$ becomes the classic Euler method which is simply the first-order approximation $x(t+\Delta t)=x(t)+\Delta t \cdot x^{\prime}(t) ; \alpha=1$ is called the backward Euler method; and $\alpha=\frac{1}{2}$ is called the bilinear method, which preserves the stability of the system 61].

In Section 3.2 we will show that the backward Euler method and Picard iteration are actually related to RNNs. On the other hand, the bilinear discretization will be our main method for computing accurate discrete-time approximations of our continuous-time models. In particular, define $\bar{A}$ and $\bar{B}$ to be the matrices appearing in (3) for $\alpha=\frac{1}{2}$. Then the discrete-time state-space model is

$$
\begin{aligned}
x_{t} & =\bar{A} x_{t-1}+\bar{B} u_{t} \\
y_{t} & =C x_{t}+D u_{t}
\end{aligned}
$$

###### $\Delta t$ as a timescale
In most models, the length of dependencies they can capture is roughly proportional to $\frac{1}{\Delta t}$. Thus we also refer to the step size $\Delta t$ as a timescale. This is an intrinsic part of converting a continuous-time ODE into a discrete-time recurrence, and most ODE-based RNN models have it as an important and non-trainable hyperparameter [24, 47, 58]. On the other hand, in Section 3.2 we show that the gating mechanism of classical RNNs is a version of learning $\Delta t$. Moreover when viewed as a CNN, the timescale $\Delta t$ can be viewed as controlling the width of the convolution kernel (Section 3.2). Ideally, all ODE-based sequence models would be able to automatically learn the proper timescales.

###### Continuous-time memory
Consider an input function $u(t)$, a fixed probability measure $\omega(t)$, and a sequence of $N$ basis functions such as polynomials. At every time $t$, the history of $u$ before time $t$ can be projected onto this basis, which yields a vector of coefficients $x(t) \in \mathbb{R}^{N}$ that represents an optimal approximation of the history of $u$ with respect to the provided measure $\omega$. The map taking the function $u(t) \in \mathbb{R}$ to coefficients $x(t) \in \mathbb{R}^{N}$ is called the High-Order Polynomial Projection Operator (HiPPO) with respect to the measure $\omega$. In special cases such as the uniform measure $\omega=\mathbb{I}\{[0,1]\}$ and the exponentially-decaying measure $\omega(t)=\exp (-t)$, Gu et al. [24] showed that $x(t)$ satisfies a differential equation $\dot{x}(t)=A(t) x(t)+B(t) u(t)$ (i.e., (1)) and derived closed forms for the matrix $A$. Their framework provides a principled way to design memory models handling long dependencies; however, they prove only these few special cases.

## 3 Linear State-Space Layers (LSSL)

We define our main abstraction, a model family that generalizes recurrence and convolutions. Section 3.1 first formally defines the LSSL, then discusses how to compute it with multiple views. Conversely, Section 3.2 shows that LSSLs are related to mechanisms of the most popular RNNs.

### 3.1 Different Views of the LSSL

Given a fixed state space representation $A, B, C, D$, an LSSL is the sequence-to-sequence mapping defined by discretizing the linear state-space model (1) and (2).

Concretely, an LSSL layer has parameters $A, B, C, D$, and $\Delta t$. It operates on an input $u \in \mathbb{R}^{L \times H}$ representing a sequence of length $L$ where each timestep has an $H$-dimensional feature vector. Each feature $h \in[H]$ defines a sequence $\left(u_{t}^{(h)}\right)_{t \in[L]}$, which is combined with a timescale $\Delta t_{h}$ to define an output $y^{(h)} \in \mathbb{R}^{L}$ via the discretized state-space model $(4)+(5)$.

Computationally, the discrete-time LSSL can be viewed in multiple ways (Fig. 1).

###### As a recurrence
The recurrent state $x_{t-1} \in \mathbb{R}^{H \times N}$ carries the context of all inputs before time $t$. The current state $x_{t}$ and output $y_{t}$ can be computed by simply following equations (4) + (5). Thus the LSSL is a recurrent model with efficient and stateful inference, which can consume a (potentially unbounded) sequence of inputs while requiring fixed computation/storage per time step.

###### As a convolution
For simplicity let the initial state be $x_{-1}=0$. Then (4) $+(5)$ explicitly yields

$$
y_{k}=C(\bar{A})^{k} \bar{B} u_{0}+C(\bar{A})^{k-1} \bar{B} u_{1}+\cdots+C \overline{A B} u_{k-1}+\bar{B} u_{k}+D u_{k}
$$

Then $y$ is simply the (non-circular) convolution $y=\mathcal{K}_{L}(\bar{A}, \bar{B}, C) * u+D u$, where

$$
\mathcal{K}_{L}(A, B, C)=\left(C A^{i} B\right)_{i \in[L]} \in \mathbb{R}^{L}=\left(C B, C A B, \ldots, C A^{L-1} B\right) .
$$

Thus the LSSL can be viewed as a convolutional model where the entire output $y \in \mathbb{R}^{H \times L}$ can be computed at once by a convolution, which can be efficiently implemented with three FFTs.

###### The computational bottleneck
We make a note that the bottleneck of 
1. the recurrence view is matrix-vector multiplication (MVM) by the discretized state matrix $\bar{A}$ when simulating (4), and 
2. the convolutional view is computing the Krylov function $\mathcal{K}_{L}$ (7). 
Throughout this section we assumed the LSSL parameters were fixed, which means that $\bar{A}$ and $\mathcal{K}_{L}(A, B, C)$ can be cached for efficiency. However, learning the parameters $\bar{A}$ and $\Delta t$ would involve repeatedly re-computing these, which is infeasible in practice. We revisit and solve this problem in Section 4.2

### 3.2 $\quad$ Expressivity of LSSLs

For a model to be both recurrent and convolutional, one might expect it to be limited in other ways. Indeed, while [12, 44] also observe that certain recurrences can be replaced with a convolution, they note that it is not obvious if convolutions can be replaced by recurrences. Moreover, while the LSSL is a linear recurrence, popular RNN models are nonlinear sequence models with activation functions between each time step. We now show that LSSLs surprisingly do not have limited expressivity.

###### Convolutions are LSSLs
A well-known fact about state-space systems (1) +2 is that the output $y$ is related to the input $u$ by a convolution $y(t)=\int h(\tau) u(t-\tau) d \tau$ with the impulse response $h$ of the system. Conversely, a convolutional filter $h$ that is a rational function of degree $N$ can be represented by a state-space model of size $N$ [59]. Thus, an arbitrary convolutional filter $h$ can be approximated by a rational function (e.g., by Padé approximants) and represented by an LSSL.

In the particular case of LSSLs with HiPPO matrices (Sections 2 and 4.1), there is another intuitive interpretation of how LSSL relate to convolutions. Consider the special case when $A$ corresponds to a uniform measure (in the literature known as the LMU [58] or HiPPO-LegT [24] matrix). Then for a fixed $d t$, equation (1) is simply memorizing the input within sliding windows of $\frac{1}{\Delta t}$ elements, and equation (2) extracts features from this window. Thus the LSSL can be interpreted as automatically learning convolution filters with a learnable kernel width.

###### RNNs are LSSLs
We show two results about RNNs that may be of broader interest. Our first result says that the ubiquitous gating mechanism of RNNs, commonly perceived as a heuristic to smooth optimization [28], is actually the analog of a step size or timescale $\Delta t$.

Lemma 3.1. A (1-D) gated recurrence $x_{t}=(1-\sigma(z)) x_{t-1}+\sigma(z) u_{t}$, where $\sigma$ is the sigmoid function and $z$ is an arbitrary expression, can be viewed as the GBT $(\alpha=1)$ (i.e., backwards-Euler) discretization of a 1-D linear $O D E \dot{x}(t)=-x(t)+u(t)$.

**Proof**. Applying a discretization requires a positive step size $\Delta t$. The simplest way to parameterize a positive function is via the exponential function $\Delta t=\exp (z)$ applied to any expression $z$. Substituting this into (3) with $A=-1, B=1, \alpha=1$ exactly produces the gated recurrence.

While Lemma 3.1 involves approximating continuous systems using discretization, the second result is about approximating them using Picard iteration (Section 2). Roughly speaking, each layer of a deep linear RNN can be viewed as successive Picard iterates $x_{0}(t), x_{1}(t), \ldots$ approximating a function $x(t)$ defined by a non-linear ODE. This shows that we do not lose modeling power by using linear instead of non-linear recurrences, and that the nonlinearity can instead be "moved" to the depth direction of deep neural networks to improve speed without sacrificing expressivity.

Lemma 3.2. (Infinitely) deep stacked LSSL layers of order $N=1$ with position-wise non-linear functions can approximate any non-linear $O D E \dot{x}(t)=-x+f(t, x(t))$.

We note that many of the most popular and effective RNN variants such as the LSTM [28], GRU [14], QRNN [5], and SRU [33], involve a hidden state $x_{t} \in \mathbb{R}^{H}$ that involves independently "gating" the $H$ hidden units. Applying Lemma 3.1, they actually also approximate an ODE of the form in Lemma 3.2. Thus LSSLs and these popular RNN models can be seen to all approximate the same type of underlying continuous dynamics, by using Picard approximations in the depth direction and discretization (gates) in the time direction. Appendix C gives precise statements and proofs.

### 3.3 Deep LSSLs

The basic LSSL is defined as a sequence-to-sequence map from $\mathbb{R}^{L} \rightarrow \mathbb{R}^{L}$ on $1 \mathrm{D}$ sequences of length $L$, parameterized by parameters $A \in \mathbb{R}^{N \times N}, B \in \mathbb{R}^{N \times 1}, C \in \mathbb{R}^{1 \times N}, D \in \mathbb{R}^{1 \times 1}, \Delta t \in \mathbb{R}$. Given an input sequence with hidden dimension $H$ (in other words a feature dimension greater than 1), we simply broadcast the parameters $B, C, D, \Delta t$ with an extra dimension $H$. Each of these $H$ copies is learned independently, so that there are $H$ different versions of a 1D LSSL processing each of the input features independently. Overall, the standalone LSSL layer is a sequence-to-sequence map with the same interface as standard sequence model layers such as RNNs, CNNs, and Transformers.

The full LSSL architecture in a deep neural network is defined similarly to standard sequence models such as deep ResNets and Transformers, involving stacking LSSL layers connected with normalization layers and residual connections. Full architecture details are described in Appendix B, including the initialization of $A$ and $\Delta t$, computational details, and other architectural details.

## 4 Combining LSSLs with Continuous-time Memorization

In Section 3 we introduced the LSSL model and showed that it shares the strengths of convolutions and recurrences while also generalizing them. We now discuss and address its main limitations, in particular handling long dependencies (Section 4.1) and efficient computation (Section 4.2).

### 4.1 Incorporating Long Dependencies into LSSLs

The generality of LSSLs means they can inherit the issues of recurrences and convolutions at addressing long dependencies (Section 1). For example, viewed as a recurrence, repeated multiplication by $\bar{A}$ could suffer from the vanishing gradients problem [39, 44]. We confirm empirically that LSSLs with random state matrices $A$ are actually not effective (Section 5.4) as a generic sequence model.

However, one advantage of these mathematical continuous-time models is that they are theoretically analyzable, and specific $A$ matrices can be derived to address this issue. In particular, the HiPPO framework (Section 2) describes how to memorize a function in continuous time with respect to a measure $\omega$ [24]. This operator mapping a function to a continuous representation of its past is denoted hippo( $\omega)$, and was shown to have the form of equation (1) in three special cases. However, these matrices are non-trainable in the sense that no other $A$ matrices were known to be hippo operators.

To address this, we theoretically resolve the open question from [24], showing that hippo( $\omega$ ) for any measure $\omega 1$ results in (1) with a structured matrix $A$.

Theorem 1 (Informal). For an arbitrary measure $\omega$, the optimal memorization operator hippo $(\omega)$ has the form $\dot{x}(t)=A x(t)+B u(t)$ (1) for a low recurrence-width (LRW) [17] state matrix $A$.

For measures covering the classical orthogonal polynomials (OPs) [52] (in particular, corresponding to Jacobi and Laguerre polynomials), there is even more structure.

Corollary 4.1. For $\omega$ corresponding to the classical OPs, hippo( $\omega$ ) is 3-quasiseparable.

Although beyond the scope of this section, we mention that LRW matrices are a type of structured matrix that have linear MVM [17]. In Appendix D we define this class and prove Theorem 1. Quasi-separable matrices are a related class of structured matrices with additional algorithmic properties. We define these matrices in Definition 4 and prove Corollary 4.1 in Appendix D. 3.

Theorem 1 tells us that a LSSL that uses a state matrix $A$ within a particular class of structured matrices would carry the theoretical interpretation of continuous-time memorization. Ideally, we would be able to automatically learn the best $A$ within this class; however, this runs into computational challenges which we address next (Section 4.2) . For now, we define the LSSL-fixed or LSSL-f to be one where the $A$ matrix is fixed to one of the HiPPO matrices prescribed by [24].

### 4.2 Theoretically Efficient Algorithms for the LSSL

Although $A$ and $\Delta t$ are the most critical parameters of an LSSL which govern the state-space (c.f. Section 4.1) and timescale (Sections 2 and 3.2), they are not feasible to train in a naive LSSL. In particular, Section 3.1 noted that it would require efficient matrix-vector multiplication (MVM) and Krylov function (7) for $A$ to compute the recurrent and convolutional views, respectively. However, the former seems to involve a matrix inversion (3), while the latter seems to require powering $\bar{A}$ up $L$ times.

In this section, we show that the same restriction of $A$ to the class of quasiseparable (Corollary 4.1), which gives an LSSL the ability to theoretically remember long dependencies, simultaneously grants it computational efficiency.

First of all, it is known that quasiseparable matrices have efficient (linear-time) MVM [40]. We show that they also have fast Krylov functions, allowing efficient training with convolutions.

**Theorem 2**. For any k-quasiseparable matrix $A$ (with constant $k$ ) and arbitrary $B, C$, the Krylov function $\mathcal{K}_{L}(A, B, C)$ can be computed in quasi-linear time and space $\tilde{O}(N+L)$ and logarithmic depth (i.e., is parallelizable). The operation count is in an exact arithmetic model, not accounting for bit complexity or numerical stability.

We remark that Theorem 2 is non-obvious. To illustrate, it is easy to see that unrolling (7) for a general matrix $A$ takes time $L N^{2}$. Even if $A$ is extremely structured with linear computation, it requires $L N$ operations and linear depth. The depth can be reduced with the squaring technique (batch multiply by $\left.A, A^{2}, A^{4}, \ldots\right)$, but this then requires $L N$ intermediate storage. In fact, the algorithm for Theorem 2 is quite sophisticated (Appendix E) and involves a divide-and-conquer recursion over matrices of polynomials, using the observation that (7) is related to the power series $C(I-A x)^{-1} B$.

Unless specified otherwise, the full LSSL refers to an LSSL with $A$ satisfying Corollary 4.1. In conclusion, learning within this structured matrix family simultaneously endows LSSLs with long-range memory through Theorem 1 and is theoretically computationally feasible through Theorem 2. We note the caveat that Theorem 2 is over exact arithmetic and not floating point numbers, and thus is treated more as a proof of concept that LSSLs can be computationally efficient in theory. We comment more on the limitations of the LSSL in Section 6 .[^0]

Table 1: (Pixel-by-pixel image classification.) (Top) our methods. (Middle) recurrent baselines. (Bottom) convolutional + other baselines.

| Model | sMNIST | pMNIST | sCIFAR |
| :--- | :--- | :--- | :--- |
| LSSL | $\mathbf{9 9 . 5 3}$ | $\mathbf{9 8 . 7 6}$ | $\mathbf{8 4 . 6 5}$ |
| LSSL-fixed | $\mathbf{9 9 . 5 0}$ | $\mathbf{9 8 . 6 0}$ | $\mathbf{8 1 . 9 7}$ |
| LipschitzRNN | $\underline{99.4}$ | 96.3 | 64.2 |
| LMUFFT [12] | - | 98.49 | - |
| UNIcoRNN [47] | - | 98.4 | - |
| HiPPO-RNN [24] | 98.9 | 98.3 | 61.1 |
| URGRU [25] | 99.27 | 96.51 | $\underline{74.4}$ |
| IndRNN [34] | 99.0 | 96.0 | - |
| Dilated RNN [8] | 98.0 | 96.1 | - |
| r-LSTM [56] | 98.4 | 95.2 | 72.2 |
| CKConv [44] | 99.32 | 98.54 | 63.74 |
| TrellisNet [4] | 99.20 | 98.13 | 73.42 |
| TCN [3] | 99.0 | 97.2 | - |
| Transformer [56] | 98.9 | 97.9 | 62.2 |

Table 2: (Vital signs prediction.) RMSE for predicting respiratory rate $(\mathrm{RR})$, heart rate $(\mathrm{HR})$, and blood oxygen ( $\mathrm{SpO} 2)$. * indicates our own runs to complete results for the strongest baselines.

| Model | $\mathrm{RR}$ | $\mathrm{HR}$ | $\mathrm{SpO} 2$ |
| :--- | :--- | :--- | :--- |
| LSSL | $\mathbf{0 . 3 5 0}$ | $\mathbf{0 . 4 3 2}$ | $\mathbf{0 . 1 4 1}$ |
| LSSL-fixed | $\mathbf{0 . 3 7 8}$ | $\mathbf{0 . 5 6 1}$ | $\mathbf{0 . 2 2 1}$ |
| UnICORNN [47] | $\underline{1.06}$ | $\underline{1.39}$ | $\underline{0.869^{*}}$ |
| coRNN [47] | 1.45 | 1.81 | - |
| CKConv | $1.214^{*}$ | $2.05^{*}$ | $1.051^{*}$ |
| NRDE [37] | 1.49 | 2.97 | 1.29 |
| IndRNN [47] | 1.47 | 2.1 | - |
| expRNN [47] | 1.57 | 1.87 | - |
| LSTM | 2.28 | 10.7 | - |
| Transformer | $2.61^{*}$ | $12.2^{*}$ | $3.02^{*}$ |
| XGBoost [55] | 1.67 | 4.72 | 1.52 |
| Random Forest [55] | 1.85 | 5.69 | 1.74 |
| Ridge Regress. [55] | 3.86 | 17.3 | 4.16 |

## 5 Empirical Evaluation

We test LSSLs empirically on a range of time series datasets with sequences from length 160 up to 38000 (Sections 5.1 and 5.2), where they substantially improve over prior work. We additionally validate the computational and modeling benefits of LSSLs from generalizing all three main model families (Section 5.3), and analyze the benefits of incorporating principled memory representations that can be learned (Section 5.4).

###### Baselines
Our tasks have extensive prior work and we evaluate against previously reported best results. We highlight our primary baselines, three very recent works explicitly designed for long sequences: CKConv (a continuous-time CNN) [44], UnICORNN (an ODE-inspired RNN) [47], and Neural Controlled/Rough Differential Equations (NCDE/NRDE) (a sophisticated NDE) 31, 37. These are the only models we are aware of that have experimented with sequences of length $>10 \mathrm{k}$.

### 5.1 Image and Time Series Benchmarks

We test on the sequential MNIST, permuted MNIST, and sequential CIFAR tasks (Table 1), popular benchmarks which were originally designed to test the ability of recurrent models to capture long-term dependencies of length up to 1k [2]. LSSL sets SoTA on sCIFAR by more than 10 points. We note that all results were achieved with at least $5 \mathrm{x}$ fewer parameters than the previous SoTA (Appendix F).

We additionally use the BDIMC healthcare datasets (Table 2), a suite of widely studied time series regression problems of length 4000 on estimating vital signs. LSSL reduces RMSE by more than two-thirds on all datasets.
Table 3: (Sequential CelebA Classification.)

|  | LSSL-f | ResNet |
| :--- | :--- | :--- |
| Att. | 78.89 | 81.35 |
| MSO | 92.36 | 93.92 |
| Smil. | 90.95 | 92.89 |
| WL | 90.57 | 93.25 |

### 5.2 Speech and Image Classification for Very Long Time Series

Raw speech is challenging for ML models due to high-frequency sampling resulting in very long sequences. Traditional systems involve complex pipelines that require feeding mixed-and-matched hand-crafted features into DNNs 42]. Table 4 reports results for the Speech Commands (SC) dataset [31] for classification of 1-second audio clips. Few methods have made progress on the raw speech signal, instead requiring preprocessing with standard mel-frequency cepstrum coefficients (MFCC). By contrast, LSSL sets SoTA on this dataset while training on the raw signal. We note that MFCC extracts sliding window frequency coefficients and thus is related to the coefficients $x(t)$ defined by LSSL-f (Section 2, Section 4.1, [24], Appendix D). Consequently, LSSL may be interpreted as automatically learning MFCC-type features in a trainable basis.

|  | LSSL | LSSL-f | CKConv | UnICORNN | N(C/R)DE | ODE-RNN [45] | GRU-ODE [16] |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| $1 \rightarrow 1$ | $\mathbf{9 5 . 8 7}$ | 90.64 | 71.66 | 11.02 | 16.49 | $\boldsymbol{x}$ | $\boldsymbol{x}$ |
| $1 \rightarrow \frac{1}{2}$ | $\mathbf{8 8 . 6 6}$ | 78.01 | 65.96 | 11.07 | 15.12 | $\boldsymbol{x}$ | $\boldsymbol{x}$ |
| MFCC | 93.58 | 92.55 | $\mathbf{9 5 . 3}$ | 90.64 | 89.8 | 65.9 | 47.9 |
> Table 4: (Raw Speech Classification; Timescale Shift.) (Top): Raw signals (length 16000); $1 \rightarrow f$ indicates test-time change in sampling rate by a factor of $f$. (Bottom): Pre-processed MFCC features used in prior work (length 161). $\boldsymbol{x}$ denotes computationally infeasible.


|  | Permuted MNIST |  |  | BDIMC Heart Rate |  |  | Speech Commands RAW |  |  |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  | 98\% Acc. | PSoTA | Time | 1.5 RMSE | PSoTA | Time | $65 \%$ Acc. | PSoTA | Time |
| LSSL-fixed | $16 \mathrm{ep.}$ | $104 \mathrm{ep.}$ | $0.19 \times$ | 9 ep. | $10 \mathrm{ep.}$ | $0.07 \times$ | $9 \mathrm{ep}$. | $10 \mathrm{ep.}$ | $0.14 \times$ |
| CKConv | 118 ep. | 200 ep. | $1.0 \times$ | $x$ |  |  | 188 ep. | $280 \mathrm{ep}$. | $1.0 \times$ |
| UnICORNN | $75 \mathrm{ep.}$ | $x$ | $x$ | 116 ep. | 467 ep. | $1.0 x$ | $x$ | $x$ | $x$ |
> Table 5: (Modeling and Computational Benefits of LSSLs.) In each benchmark category, we compare the number of epochs (ep.) it takes a LSSL-f to reach the previous SoTA (PSoTA) results as well as a near-SoTA target. We also report the wall clock time it took to reach PSoTA relative to the previous best model.


To stress-test the LSSL's ability to handle extremely long sequences, we create a challenging new sequentialCelebA task, where we classify $178 \times 218$ images $=\mathbf{3 8 0 0 0}$-length sequences for 4 facial attributes: Attractive (Att.), Mouth Slightly Open (MSO), Smiling (Smil.), Wearing Lipstick (WL) [36]. We chose the 4 most class-balanced attributes to avoid well-known problems with class imbalance. LSSL-f comes close to matching the performance of a specialized ResNet-18 image classification architecture that has $10 \times$ the parameters (Table 3). We emphasize we are the first to demonstrate that this is possible to do with a generic sequence model.

### 5.3 Advantages of Recurrent, Convolutional, and Continuous-time Models

We validate that the generality of LSSLs endows it with the strengths of all three families.

###### Convergence Speed
As a recurrent and NDE model that incorporates new theory for continuous-time memory (Section 4.1), the LSSL has strong inductive bias for sequential data, and converges rapidly to SoTA results on our benchmarks. With its convolutional view, training can be parallelized and it is also computationally efficient in practice. Table 5 compares the time it takes the LSSL-f to achieve SoTA, in either sample (measured by epochs) or computational (measured by wall clock) complexity. In all cases, LSSLs reached the target in a fraction of the time of the previous model.

###### Timescale Adaptation
Table 4 also reports the results of continuous-time models that are able to handle unique settings such as missing data in time series, or test-time shift in timescale (we note that this is a realistic problem, e.g., when deployed healthcare models are tested on EEG signals that are sampled at a different rate [48, 49]). We note that many of these baselines were custom designed for such settings, which is of independent interest. On the other hand, LSSLs perform timescale adaptation by simply changing its $\Delta t$ values at inference time, while still outperforming the performance of prior methods with no shift. Additional results on the CharacterTrajectories dataset from prior work [31, 44] are in Appendix F where LSSL is competitive with the best baselines.

### 5.4 LSSL Ablations: Learning the Memory Dynamics and Timescale

We demonstrate that the $\Delta t$ and $A$ parameters, which LSSLs are able to automatically learn in contrast to prior work, are indeed critical to the performance of these continuous-time models. We note that learning $\Delta t$
adds only $O(H)$ parameters and learning $A$ adds $O(N)$ parameters, adding less than $1 \%$ parameter count compared to the base models with $O(H N)$ parameters.

###### Memory dynamics $A$
We validate that vanilla LSSLs suffer from the modeling issues described in Section 4 . We tested that LSSLs with random $A$ matrices (normalized appropriately) perform very poorly (e.g., $62 \%$ on pMNIST). Further, we note the consistent increase in performance from LSSL-f to LSSL despite the negligible parameter difference. These ablations show that (i) incorporating the theory of Theorem 1 is actually necessary for LSSLs, and (ii) further training the structured $A$ is additionally helpful, which can be interpreted as learning the measure for memorization (Section 4.1).

###### Timescale $\Delta t$
Section 3.2 showed that LSSL's ability to learn $\Delta t$ is its direct generalization of the critical gating mechanism of popular RNNs, which previous ODE-based RNN models [12, 24, 47, 58], cannot learn. We note that on sCIFAR, LSSL-f with poorly-specified $\Delta t$ gets only $49.3 \%$ accuracy. Additional results in Appendix F show that learning $\Delta t$ alone provides an orthogonal boost to learning $A$, and visualizes the noticeable change in $\Delta t$ over the course of training.

## 6 Discussion

In this work we introduced a simple and principled model (LSSL) inspired by a fundamental representation of physical systems. We showed theoretically and empirically that it generalizes and inherits the strengths of the main families of modern time series models, that its main limitations of long-term memory can be resolved with new theory on continuous-time memorization, and that it is empirically effective on difficult tasks with very long sequences.

###### Related work
The LSSL is related to several rich lines of work on recurrent, convolutional, and continuous-time models, as well as sequence models addressing long dependencies. Appendix A provides an extended related work connecting these topics.

###### Tuning
Our models are very simple, consisting of identical L(inear)SSL layers with simple position-wise non-linear modules between layers (Appendix B). Our models were able to train at much higher learning rates than baselines and were not sensitive to hyperparameters, of which we did light tuning primarily on learning rate and dropout. In contrast to previous baselines [4, 31, 44], we did not use hyperparameters for improving stability and regularization such as weight decay, gradient clipping, weight norm, input dropout, etc. While the most competitive recent works introduce at least one hyperparameter of critical importance (e.g. depth and step size 37], $\alpha$ and $\Delta t$ [47], $\omega_{0}$ [44]) that are difficult to tune, the LSSL-fixed has only $\Delta t$, which the full LSSL can even learn automatically (at the expense of speed).

###### Limitations
Sections 1 and 3 and Fig. 1 mention that a potential benefit of having the recurrent representation of LSSLs may endow it with efficient inference. While this is theoretically possible, this work did not experiment on any applications that leverage this. Follow-up work showed that it is indeed possible in practice to speed up some applications at inference time.

Theorem 2 s algorithm is sophisticated (Appendix D) and was not implemented in the first version of this work. A follow-up to this paper found that it is not numerically stable and thus not usable on hardware. Thus the algorithmic contributions in Theorem 2 serve the purpose of a proof-of-concept that fast algorithms for the LSSL do exist in other computation models (i.e., arithmetic operations instead of floating point operations), and leave an open question as to whether fast, numerically stable, and practical algorithms for the LSSL exist.

As described in Appendix B by freezing the $A$ matrix and $\Delta t$ timescale, the LSSL-fixed is able to be computed much faster than the full LSSL, and is comparable to prior models in practice (Table 5). However, beyond computational complexity, there is also a consideration of space efficiency. Both the LSSL and LSSL-fixed suffer from a large amount of space overhead (described in Appendix B) - using $O(N L)$ instead of $O(L)$ space when working on a 1D sequence of length $L$ - that essentially stems from using the latent state representation of dimension $N$. Consequently, the LSSL can be space inefficient and we used multi-GPU training for our largest experiments (speech and high resolution images, Tables 3 and 4).

These fundamental issues with computation and space complexity were revisited and resolved in followup work to this paper, where a new state space model (the Structured State Space) provided a new parameterization and algorithms for state spaces.

###### Conclusion and future work
Modern deep learning models struggle in applications with very long
temporal data such as speech, videos, and medical time-series. We hope that our conceptual and technical contributions can lead to new capabilities with simple, principled, and less engineered models. We note that our pixel-level image classification experiments, which use no heuristics (batch norm, auxiliary losses) or extra information (data augmentation), perform similar to early convnet models with vastly more parameters, and is in the spirit of recent attempts at unifying data modalities with a generic sequence model [18]. Our speech results demonstrate the possibility of learning better features than hand-crafted processing pipelines used widely in speech applications. We are excited about potential downstream applications, such as training other downstream models on top of pre-trained state space features.

## References

[1] George B. (George Brown) Arfken, Hans Jürgen Weber, and Frank E Harris. Mathematical methods for physicists : a comprehensive guide / George B. Arfken, Hans J. Weber, Frank E. Harris. Academic Press, Amsterdam, 7th ed. edition, 2013. ISBN 0-12-384654-4.

[2] Martin Arjovsky, Amar Shah, and Yoshua Bengio. Unitary evolution recurrent neural networks. In The International Conference on Machine Learning (ICML), pages 1120-1128, 2016.

[3] Shaojie Bai, J Zico Kolter, and Vladlen Koltun. An empirical evaluation of generic convolutional and recurrent networks for sequence modeling. arXiv preprint arXiv:1803.01271, 2018.

[4] Shaojie Bai, J Zico Kolter, and Vladlen Koltun. Trellis networks for sequence modeling. In The International Conference on Learning Representations (ICLR), 2019.

[5] James Bradbury, Stephen Merity, Caiming Xiong, and Richard Socher. Quasi-recurrent neural networks. arXiv preprint arXiv:1611.01576, 2016.

[6] John Charles Butcher and Nicolette Goodwin. Numerical methods for ordinary differential equations, volume 2. Wiley Online Library, 2008.

[7] Bo Chang, Minmin Chen, Eldad Haber, and Ed H Chi. Antisymmetricrnn: A dynamical system view on recurrent neural networks. In International Conference on Learning Representations, 2019.

[8] Shiyu Chang, Yang Zhang, Wei Han, Mo Yu, Xiaoxiao Guo, Wei Tan, Xiaodong Cui, Michael Witbrock, Mark Hasegawa-Johnson, and Thomas S Huang. Dilated recurrent neural networks. In Advances in Neural Information Processing Systems (NeurIPS), 2017.

[9] Zhengping Che, Sanjay Purushotham, Kyunghyun Cho, David Sontag, and Yan Liu. Recurrent neural networks for multivariate time series with missing values. Scientific reports, 8(1):1-12, 2018.

[10] Tian Qi Chen, Yulia Rubanova, Jesse Bettencourt, and David K Duvenaud. Neural ordinary differential equations. In Advances in neural information processing systems, pages 6571-6583, 2018.

[11] T. S. Chihara. An introduction to orthogonal polynomials. Dover Books on Mathematics. Dover Publications, 2011. ISBN 9780486479293.

[12] Narsimha Chilkuri and Chris Eliasmith. Parallelizing legendre memory unit training. The International Conference on Machine Learning (ICML), 2021.

[13] François Chollet. Xception: Deep learning with depthwise separable convolutions. In Proceedings of the IEEE conference on computer vision and pattern recognition, pages 1251-1258, 2017.

[14] Junyoung Chung, Caglar Gulcehre, KyungHyun Cho, and Yoshua Bengio. Empirical evaluation of gated recurrent neural networks on sequence modeling. arXiv preprint arXiv:1412.3555, 2014.

[15] Jared Quincy Davis, Albert Gu, Tri Dao, Krzysztof Choromanski, Christopher Ré, Percy Liang, and Chelsea Finn. Catformer: Designing stable transformers via sensitivity analysis. In The International Conference on Machine Learning (ICML), 2021.

[16] Edward De Brouwer, Jaak Simm, Adam Arany, and Yves Moreau. Gru-ode-bayes: Continuous modeling of sporadically-observed time series. In Advances in Neural Information Processing Systems (NeurIPS), 2019 .

[17] Christopher De Sa, Albert Gu, Rohan Puttagunta, Christopher Ré, and Atri Rudra. A two-pronged progress in structured dense matrix vector multiplication. In Proceedings of the Twenty-Ninth Annual ACM-SIAM Symposium on Discrete Algorithms, pages 1060-1079. SIAM, 2018.

[18] Alexey Dosovitskiy, Lucas Beyer, Alexander Kolesnikov, Dirk Weissenborn, Xiaohua Zhai, Thomas Unterthiner, Mostafa Dehghani, Matthias Minderer, Georg Heigold, Sylvain Gelly, et al. An image is worth 16x16 words: Transformers for image recognition at scale. arXiv preprint arXiv:2010.11929, 2020.

[19] Y. Eidelman and I. Gohberg. On a new class of structured matrices. Integral Equations and Operator Theory, 34, 1999.

[20] N Benjamin Erichson, Omri Azencot, Alejandro Queiruga, Liam Hodgkinson, and Michael W Mahoney. Lipschitz recurrent neural networks. In International Conference on Learning Representations, 2021.

[21] Karl J Friston, Lee Harrison, and Will Penny. Dynamic causal modelling. Neuroimage, 19(4):1273-1302, 2003.

[22] Ken-ichi Funahashi and Yuichi Nakamura. Approximation of dynamical systems by continuous time recurrent neural networks. Neural networks, 6(6):801-806, 1993.

[23] Gene H Golub and Charles F Van Loan. Matrix computations, volume 3. JHU press, 2013.

[24] Albert Gu, Tri Dao, Stefano Ermon, Atri Rudra, and Christopher Ré. Hippo: Recurrent memory with optimal polynomial projections. In Hugo Larochelle, Marc'Aurelio Ranzato, Raia Hadsell, Maria-Florina Balcan, and Hsuan-Tien Lin, editors, Advances in Neural Information Processing Systems 33: Annual Conference on Neural Information Processing Systems 2020, NeurIPS 2020, December 6-12, 2020, virtual, 2020. URL https://proceedings.neurips.cc/paper/2020/hash/ 102f0bb6efb3a6128a3c750dd16729be-Abstract.html.

[25] Albert Gu, Caglar Gulcehre, Tom Le Paine, Matt Hoffman, and Razvan Pascanu. Improving the gating mechanism of recurrent neural networks. In The International Conference on Machine Learning (ICML), 2020 .

[26] Ramin Hasani, Mathias Lechner, Alexander Amini, Lucas Liebenwein, Max Tschaikowski, Gerald Teschl, and Daniela Rus. Closed-form continuous-depth models. arXiv preprint arXiv:2106.13898, 2021.

[27] Ramin Hasani, Mathias Lechner, Alexander Amini, Daniela Rus, and Radu Grosu. Liquid time-constant networks. In Proceedings of the AAAI Conference on Artificial Intelligence, 2021.

[28] Sepp Hochreiter and Jürgen Schmidhuber. Long short-term memory. Neural computation, 9(8):1735-1780, 1997.

[29] Ian D Jordan, Piotr Aleksander Sokół, and Il Memming Park. Gated recurrent units viewed through the lens of continuous time dynamical systems. Frontiers in computational neuroscience, page 67, 2021.

[30] Anil Kag, Ziming Zhang, and Venkatesh Saligrama. Rnns incrementally evolving on an equilibrium manifold: A panacea for vanishing and exploding gradients? In International Conference on Learning Representations, 2020.

[31] Patrick Kidger, James Morrill, James Foster, and Terry Lyons. Neural controlled differential equations for irregular time series. arXiv preprint arXiv:2005.08926, 2020.

[32] Mathias Lechner and Ramin Hasani. Learning long-term dependencies in irregularly-sampled time series. arXiv preprint arXiv:2006.04418, 2020.

[33] Tao Lei, Yu Zhang, Sida I Wang, Hui Dai, and Yoav Artzi. Simple recurrent units for highly parallelizable recurrence. arXiv preprint arXiv:1709.02755, 2017.

[34] Shuai Li, Wanqing Li, Chris Cook, Ce Zhu, and Yanbo Gao. Independently recurrent neural network (IndRNN): Building a longer and deeper RNN. In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition, pages 5457-5466, 2018.

[35] Liyuan Liu, Xiaodong Liu, Jianfeng Gao, Weizhu Chen, and Jiawei Han. Understanding the difficulty of training transformers. The International Conference on Machine Learning (ICML), 2020.

[36] Ziwei Liu, Ping Luo, Xiaogang Wang, and Xiaoou Tang. Deep learning face attributes in the wild. In ICCV, pages 3730-3738. IEEE Computer Society, 2015. ISBN 978-1-4673-8391-2. URL http: //dblp.uni-trier.de/db/conf/iccv/iccv2015.html\#LiuLWT15

[37] James Morrill, Cristopher Salvi, Patrick Kidger, James Foster, and Terry Lyons. Neural rough differential equations for long time series. The International Conference on Machine Learning (ICML), 2021.

[38] Murphy Yuezhen Niu, Lior Horesh, and Isaac Chuang. Recurrent neural networks in the eye of differential equations. arXiv preprint arXiv:1904.12933, 2019.

[39] Razvan Pascanu, Tomas Mikolov, and Yoshua Bengio. On the difficulty of training recurrent neural networks. In International conference on machine learning, pages 1310-1318, 2013.

[40] Clément Pernet. Computing with quasiseparable matrices. In Proceedings of the ACM on International Symposium on Symbolic and Algebraic Computation, pages 389-396, 2016.

[41] KB Petersen and MS Pedersen. The matrix cookbook, version 20121115. Technical Univ. Denmark, Kongens Lyngby, Denmark, Tech. Rep, 3274, 2012.

[42] M. Ravanelli, T. Parcollet, and Y. Bengio. The pytorch-kaldi speech recognition toolkit. In In Proc. of ICASSP, 2019.

[43] David W Romero, Robert-Jan Bruintjes, Jakub M Tomczak, Erik J Bekkers, Mark Hoogendoorn, and Jan $\mathrm{C}$ van Gemert. Flexconv: Continuous kernel convolutions with differentiable kernel sizes. arXiv preprint arXiv:2110.08059, 2021.

[44] David W Romero, Anna Kuzina, Erik J Bekkers, Jakub M Tomczak, and Mark Hoogendoorn. Ckconv: Continuous kernel convolution for sequential data. arXiv preprint arXiv:2102.02611, 2021.

[45] Yulia Rubanova, Tian Qi Chen, and David K Duvenaud. Latent ordinary differential equations for irregularly-sampled time series. In Advances in Neural Information Processing Systems, pages 5321-5331, 2019.

[46] T. Konstantin Rusch and Siddhartha Mishra. Coupled oscillatory recurrent neural network (cornn): An accurate and (gradient) stable architecture for learning long time dependencies. In International Conference on Learning Representations, 2021.

[47] T Konstantin Rusch and Siddhartha Mishra. Unicornn: A recurrent model for learning very long time dependencies. The International Conference on Machine Learning (ICML), 2021.

[48] Khaled Saab, Jared Dunnmon, Christopher Ré, Daniel Rubin, and Christopher Lee-Messer. Weak supervision as an efficient approach for automated seizure detection in electroencephalography. NPJ Digital Medicine, 3(1):1-12, 2020.

[49] Vinit Shah, Eva Von Weltin, Silvia Lopez, James Riley McHugh, Lillian Veloso, Meysam Golmohammadi, Iyad Obeid, and Joseph Picone. The Temple University hospital seizure detection corpus. Frontiers in neuroinformatics, 12:83, 2018.

[50] Jie Shen, Tao Tang, and Li-Lian Wang. Orthogonal Polynomials and Related Approximation Results, pages 47-140. Springer Berlin Heidelberg, Berlin, Heidelberg, 2011. ISBN 978-3-540-71041-7. doi: 10.1007/978-3-540-71041-7_3. URL https://doi.org/10.1007/978-3-540-71041-7_3.

[51] Victor Shoup. A computational introduction to number theory and algebra. Cambridge university press, 2009.

[52] G. Szegö. Orthogonal Polynomials. Number v. 23 in American Mathematical Society colloquium publications. American Mathematical Society, 1967. ISBN 9780821889527.

[53] G. Szegő. Orthogonal Polynomials. American Mathematical Society: Colloquium publications. American Mathematical Society, 1975. ISBN 9780821810231.

[54] Corentin Tallec and Yann Ollivier. Can recurrent neural networks warp time? In The International Conference on Learning Representations (ICLR), 2018.

[55] Chang Wei Tan, Christoph Bergmeir, Francois Petitjean, and Geoffrey I Webb. Time series extrinsic regression. Data Mining and Knowledge Discovery, pages 1-29, 2021. doi: https://doi.org/10.1007/ s10618-021-00745-9.

[56] Trieu H Trinh, Andrew M Dai, Minh-Thang Luong, and Quoc V Le. Learning longer-term dependencies in RNNs with auxiliary losses. In The International Conference on Machine Learning (ICML), 2018.

[57] Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Lukasz Kaiser, and Illia Polosukhin. Attention is all you need. In Advances in Neural Information Processing Systems (NeurIPS), 2017.

[58] Aaron Voelker, Ivana Kajić, and Chris Eliasmith. Legendre memory units: Continuous-time representation in recurrent neural networks. In Advances in Neural Information Processing Systems, pages 15544-15553, 2019 .

[59] Robert L Williams, Douglas A Lawrence, et al. Linear state-space control systems. Wiley Online Library, 2007.

[60] Max A Woodbury. Inverting modified matrices. Memorandum report, 42:106, 1950.

[61] Guofeng Zhang, Tongwen Chen, and Xiang Chen. Performance recovery in digital implementation of analogue systems. SIAM journal on control and optimization, 45(6):2207-2223, 2007.

[62] Huaguang Zhang, Zhanshan Wang, and Derong Liu. A comprehensive review of stability analysis of continuous-time recurrent neural networks. IEEE Transactions on Neural Networks and Learning Systems, 25(7):1229-1262, 2014.

## A Related Work

We provide an extended related work comparing the LSSL to previous recurrent, convolutional, and continuoustime models.

HiPPO The LSSL is most closely related to the HiPPO framework for continuous-time memory 24 and its predecessor, the Legendre Memory Unit (LMU) 58. The HiPPO-RNN and the LMU define dynamics of the form of equation (1), and incorporate it into an RNN architecture. A successor to the LMU, the LMU-FFT 12 keeps the original linear dynamics, allowing the LMU to be computed with a cached convolution kernel.

These methods all suffer from two main limitations. First, the state matrix $A$ and discretization timescale $\Delta t$ cannot be trained due to both limitations in theoretical understanding of which $A$ matrices are effective, as well as computational limitations. Second, (1) is a 1-D to $N$-D map, requiring states to be projected back down to 1-D. This creates an overall 1-D bottleneck in the state, limiting the expressivity of the model.

Compared to these, the LSSL does not use a conventional RNN architecture, instead keeping the linear recurrence (4) and downprojecting it with the second part of the state space representation (5). To avoid the 1-D feature bottlneck, it simply computes $H$ copies of this 1-D to 1-D independently, creating an overall $H$-dimensional sequence-to-sequence model. However, this exacerbates the computational issue, since the work is increased by a factor of $H$.

This work resolves the expressivity issue with new theory. Compared to HiPPO and the LMU, LSSL allows training the $A$ matrix by showing generalized theoretical results for the HiPPO framework, showing that there is a parameterized class of structured state spaces that are HiPPO operators.

The LSSL makes progress towards the second issue with new algorithms for these structured matrices (Theorem 22. However, as noted in Sections 4.2 and 6, the algorithm presented in Theorem 2 was later found to be not practical, and an improved representation and algorithm was found in subsequent work.

###### Continuous-time CNNs
The CKConv is the only example of a continuous-time CNN that we are aware of, and is perhaps the strongest baseline in our experiments. Rather than storing a finite sequence of weights for a convolution kernel, the CKConv parameterizes it as an implicit function from $[0,1] \rightarrow \mathbb{R}$ which allows sampling it at any resolution. A successor to the CKConv is the FlexConv [43], which learns convolutional kernels with a flexible width. This is similar to the convolution interpretation of LSSL when using certain HiPPO bases (Section 3.2).

###### Continuous-time RNNs
The connection from RNNs to continuous-time models have been known since their inception, and recent years have seen an explosion of CT-RNN (continuous-time RNN) models based on dynamical systems or ODEs. We briefly mention a few classic and modern works along these lines, categorizing them into a few main topics.

First are theoretical works that analyze the expressivity of RNNs from a continuous-time perspective. The connection between RNNs and dynamical systems has been studied since the 90s [22], fleshing out the correspondence between different dynamical systems and RNN architectures 38. Modern treatments have focused on analyzing the stability 62 and dynamics [29] of RNNs.

Second, a large class of modern RNNs have been designed that aim to combat vanishing gradients from a dynamical systems analysis. These include include the AntisymmetricRNN [7], iRNN [30], and LipschitzRNN [20], which address the exploding/vanishing gradient problem by reparatermizing the architecture or recurrent matrix based on insights from an underlying dynamical system.

Third is a class of models that are based on an explicit underlying ODE introduced to satisfy various properties. This category includes the UnICORNN [47] and its predecessor coRNN [46] which discretize a second-order ODE inspired by oscillatory systems. Other models include the Liquid Time-Constant Networks (LTC) [27] and successor CfC [26], which use underlying dynamical systems with varying time-constants with stable behavior and provable rates of expressivity measured by trajectory length. The LTC is based on earlier dynamic causal models (DCM) 21, which are a particular ODE related to state spaces with an extra bilinear term. Finally, the LMU [58] and HiPPO [24] also fall in this category, whose underlying ODEs are mathematically derived for continuous-time memorization.

Fourth, the recent family of neural ODEs [10], originally introduced as continuous-depth models, have been adapted to continuous-time, spawning a series of "ODE-RNN" models. Examples include the ODE-RNN [45],

GRU-ODE-Bayes [16], and ODE-LSTM [32], which extend adjoint-based neural ODEs to the discrete input setting as an alternative to standard RNNs. Neural Controlled Differential Equations (NCDE) [31] and Neural Rough Differential Equations (NRDE) 37] are memory efficient versions that integrate observations more smoothly and can be extended to very long time series.

###### Gating mechanisms
As a special case of continuous-time RNNs, some works have observed the relation between gating mechanisms and damped dynamical systems [54]. Some examples of continuous-time RNNs based on such damped dynamical systems include the LTC [27] and iRNN [30]. Compared to these, Lemma 3.1 shows a stronger result that sigmoid gates are not just motivated by being an arbitrary monotonic function with range $(0,1)$, but the exact formula appears out of discretizing a damped ODE.

## B Model Details

### B. 1 (M)LSSL Computation

Section 3.1 noted that some of the computations for using the LSSL are expensive to compute. When the LSSL fixes the $A$ and $\Delta t$ parameters (e.g. when they are not trained, or at inference time), these computational difficulties can be circumvented by caching particular computations. In particular, this case applies to the LSSL-f. Note that in this case, the other state-space matrices $C$ and $D$ comprise the $O(H N)$ trainable parameters of the fixed-transition LSSL.

In particular, we assume that there is a black-box inference algorithm for this system, i.e. matrix-vector multiplication by $\bar{A}$ (an example of implementing this black box for a particular structured class is in Appendix E.2). We then compute and cache

- the transition matrix $\bar{A}$, which is computed by applying the black-box $\bar{A}$ MVM algorithm to the identity matrix $I$.
- the Krylov matrix

$$
K(\bar{A}, \bar{B})=\left(\bar{B}, \overline{A B},(\bar{A})^{2} \bar{B}, \ldots\right) \in \mathbb{R}^{N \times L}
$$

which is computed in a parallelized manner by the squaring technique for exponentiation, i.e. batch multiply by $\bar{A},(\bar{A})^{2},(\bar{A})^{4}, \ldots$

At inference time, the model can be unrolled recurrently with $\bar{A}$. At training time, the convolutional filter

$K_{L}(\bar{A}, \bar{B}, C)$ (equation (7)) is computed with a matrix multiplication $C \cdot K(\bar{A}, \bar{B})$ before convolving with the input $u$.

Table 7 provides more detailed complexity of this version of the LSSL with fixed $A, \Delta t$.

Note that as mentioned in Section 6, this cached algorithm is fairly fast, but the main drawback is that materializing the Krylov matrix (8) requires $O(N L)$ instead of $O(L)$ space.

### B. 2 Initialization of $A$

The LSSL initializes the $A$ parameter in (1) to the HiPPO-LegS operator, which was derived to solve a particular continuous-time memorization problem. This matrix $A \in \mathbb{R}^{N \times N}$ is

$$
A_{n k}= \begin{cases}(2 n+1)^{1 / 2}(2 k+1)^{1 / 2} & \text { if } n>k \\ n+1 & \text { if } n=k \\ 0 & \text { if } n<k\end{cases}
$$

Note that the LSSL-f is the LSSL with a non-trainable $A$ (and $\Delta t$ ), so that $A$ is fixed to the above matrix.

### B. 3 Initialization of $\Delta t$

One distinction between the LSSL and the most related prior work is that the inclusion of the projection (2) makes the layer a 1-dimensional to 1-dimensional map, instead of 1-D to $N$-D [24, 58]. This enables us to concatenate $H$ copies of this map (at the expense of computation, cf. Section 4.2 and Appendix D). Even when $\Delta t$ is not trained as in the LSSL-f, these $H$ copies allow multiple timescales to be considered by setting $\Delta t$ differently for each copy.

In particular, we initialize $\Delta t$ log-uniformly in a range $\Delta t_{\min }, \Delta t_{\max }$ (i.e., $\Delta t$ is initialized within this range, such that $\log \Delta t$ is uniformly distributed). The maximum and minimum values were generally chosen to be a factor of 100 apart such that the length of the sequences in the dataset are contained in this range. Specific values for each model and dataset are in Appendix F We did not search over these as a hyperparameter, but we note that it can be tuned for additional performance improvements in our experiments.

### B. 4 Deep Neural Network Architecture

The Deep LSSL models used in our experiments simply stack together LSSL layers in a simple deep neural network architecture. We note the following architecture details.

###### Channels
The state-space model (1) $+(2$ ) accepts a 1-dimensional input $u$, but does not strictly have to return a 1-dimensional output $y$. By making the matrices in (2) dimension $C \in \mathbb{R}^{M \times N}, D \in \mathbb{R}^{M \times 1}$, the output $y$ will be dimension $M$ instead of 1 .

We call $M$ the number of channels in the model.

Feedforward. There are two drawbacks with the current definition of LSSL:

- They are defined by running $H$ independent copies of a state-space model, which means the $H$ input features do not interact at all.
- If the channel dimension is $M>1$, then the LSSL is a map from dimension 1 to $M$, which means residuals cannot be applied.

These are both addressed by introducing a position-wise feedforward layer after the LSSL of shape $H \cdot M \rightarrow H$. This simultaneously mixes the hidden features, and projects the output back to dimension 1 if necessary. There is also an optional non-linearity in between the LSSL and this feedforward projection; we fix it to the GeLU activation function in our models.

We note that this factorization of parallel convolutions on the $H$ features followed by a position-wise linear map is very similar to depth-wise separable convolutions [13].

###### Residuals and normalization
To stack multiple layers of LSSLs together, we use very standard architectures for deep neural networks. In particular, we use residual connections and a layer normalization (either pre-norm or post-norm) in the style of standard Transformer architectures. Whether to use pre-norm or post-norm was chosen on a per-dataset basis, and depended on whether the model overfit; recent results have shown that pre-norm architectures are more stable [15, 35], so we used it on harder datasets with less overfitting. We note that we could have additionally inserted MLP modules in between LSSL layers, in the style of Transformers [57], but did not experiment with this.

###### Parameter count
The overall parameter count of an LSSL model is $M \cdot H \cdot(H+N)$.

We primarily used two model sizes in our experiments, which were chosen simply to produce round numbers of parameters:

- LSSL small $(\approx 200 K$ parameters): 6 layers, $H=128, N=128, M=1$.
- LSSL large $(\approx 2 M$ parameters): 4 layers, $H=256, N=256, M=4$.

We did not search over additional sizes, but for some datasets reduced the model size for computational reasons.

## C LSSL Proofs

This section gives refinements of the statements in Section 3, additional results, and proofs of all results.

Appendix C. 1 has a more detailed (and self-contained) summary of basic methods in ODE approximation which will be used in the results and proofs.

Appendix C.2 give more general statements and proofs of Lemma 3.1 and Lemma 3.2 in Lemma C. 1 and Theorem 4, respectively.

### C. 1 Approximations of ODEs

We consider the standard setting of a first-order initial value problem (IVP) ordinary differential equation (ODE) for a continuous function $f(t, x)$

$$
\begin{aligned}
\dot{x}(t) & =f(t, x(t)) \\
x\left(t_{0}\right) & =x_{0}
\end{aligned}
$$

This differential form has an equivalent integral form

$$
x(t)=x_{0}+\int_{t_{0}}^{t} f(s, x(s)) d s
$$

Appendices C.1.1 and C.1.2 overview the Picard theorem and first-order numerical integration methods, which apply to any IVP (9). Appendix C.1.3 then shows how to specialize it to linear systems as in equation (1).

At a high level, the basic approximation methods considered here use the integral form (10) and approximate the integral in the right-hand side by simple techniques.

#### C.1.1 Picard Iteration

The Picard-Lindelöf Theorem gives sufficient conditions for the existence and uniqueness of solutions to an IVP. As part of the proof, it provides an iteration scheme to compute this solution.

Theorem 3 (Picard-Lindelöf). In the IVP (9), if there is an interval around $t_{0}$ such that $f$ is Lipschitz in its second argument, then there is an open interval $I \ni t_{0}$ such that there exists a unique solution $x(t)$ to the IVP in I. Furthermore, the sequence of Picard iterates $x^{(0)}, x^{(1)}, \ldots$ defined by

$$
\begin{aligned}
& x^{(0)}(t)=x_{0} \\
& x^{(\ell)}(t)=x_{0}+\int_{t_{0}}^{t} f\left(s, x^{(\ell-1)}(s)\right) d s
\end{aligned}
$$

converges to $x$.

The Picard iteration can be viewed as approximating (10) by holding the previous estimate of the solution $x^{(\ell-1)}$ fixed inside the RHS integral.

#### C.1.2 Numerical Integration Methods

Many methods for numerical integration of ODEs exist, which calculate discrete-time approximations of the solution. We discuss a few of the simplest methods, which are first-order methods with local error $O\left(h^{2}\right)[6]$.

These methods start by discretizing (10) into the form

$$
x\left(t_{k}\right)-x\left(t_{k-1}\right)=\int_{t_{k-1}}^{t_{k}} f(s, x(s)) d s
$$

Here we assume a sequence of discrete times $t_{0}, t_{1}, t_{2}, \ldots$ is fixed. For convenience, let $x_{k}$ denote $x\left(t_{k}\right)$ and let $\Delta t_{k}:=t_{k}-t_{k-1}$. The goal is now to approximate the integral in the RHS of (11).

###### Euler method
The Euler method approximates (11) by holding the left endpoint constant throughout the integral (i.e., the "rectangle rule" with left endpoint), $f(s, x(s)) \approx f\left(t_{k-1}, x\left(t_{k-1}\right)\right)$. The discrete-time update becomes

$$
\begin{aligned}
x_{k}-x_{k-1} & =\left(t_{k}-t_{k-1}\right) f\left(t_{k-1}, x\left(t_{k-1}\right)\right) \\
& =\Delta t_{k} f\left(t_{k-1}, x_{k-1}\right)
\end{aligned}
$$

###### Backward Euler method
The backward Euler method approximates 11 by holding the right endpoint constant throughout the integral (i.e., the "rectangle rule" with right endpoint), $f(s, x(s)) \approx f\left(t_{k}, x\left(t_{k}\right)\right)$. The discrete-time update becomes

$$
\begin{aligned}
x_{k}-x_{k-1} & =\left(t_{k}-t_{k-1}\right) f\left(t_{k}, x\left(t_{k}\right)\right) \\
& =\Delta t_{k} f\left(t_{k}, x_{k}\right)
\end{aligned}
$$

#### C.1.3 Discretization of State-Space Models

In the case of a linear system, the IVP is specialized to the case

$$
f(t, x(t))=A x(t)+B u(t)
$$

Note that here $u$ is treated as a fixed external input, which is constant from the point of view of this ODE in $x$. Let $u_{k}$ denote the average value in each discrete time interval,

$$
u_{k}=\frac{1}{\Delta t_{k}} \int_{t_{k-1}}^{t_{k}} u(s) d s
$$

The integral equation 11 can be specialized to this case, and more generally a convex combination of the left and right endpoints can be taken to approximate the integral, weighing them by $1-\alpha$ and $\alpha$ respectively. Note that the case $\alpha=0,1$ are specializations of the forward and backward Euler method, and the case $\alpha=\frac{1}{2}$ is the classic "trapezoid rule" for numerical integration.

$$
\begin{aligned}
x\left(t_{k}\right)-x\left(t_{k-1}\right) & =\int_{t_{k-1}}^{t_{k}} A x(s) d s+\int_{t_{k-1}}^{t_{k}} B u(s) d s \\
& =\int_{t_{k-1}}^{t_{k}} A x(s) d s+\Delta t_{k} B u_{k} \\
& \approx \Delta t_{k}\left[(1-\alpha) A x_{k-1}+\alpha A x_{k}\right]+\Delta t_{k} B u_{k} .
\end{aligned}
$$

Rearranging yields

$$
\begin{aligned}
\left(I-\alpha \Delta t_{k} \cdot A\right) x_{k} & =\left(I+(1-\alpha) \Delta t_{k} \cdot A\right) x_{k-1}+\Delta t_{k} \cdot B u_{k} \\
x_{k} & =\left(I-\alpha \Delta t_{k} \cdot A\right)^{-1}\left(I+(1-\alpha) \Delta t_{k} \cdot A\right) x_{k-1}+\left(I-\alpha \Delta t_{k} \cdot A\right)^{-1} \Delta t_{k} \cdot B u_{k}
\end{aligned}
$$

This derives the generalized bilinear transform (GBT) 61]. The bilinear method is the case $\alpha=\frac{1}{2}$ of special significance, and was numerically found to be better than the forward and backward Euler methods $\alpha=0,1$ both in synthetic function approximation settings and in end-to-end experiments [24, Figure $4]$.

### C. 2 RNNs are LSSLs: Proof of Results in Section 3.2

We provide more detailed statements of Lemmas 3.1 and 3.2 from Section 3.2 . In summary, LSSLs and popular families of RNN methods all approximate the same continuous-time dynamics by viewing them with a combination of two techniques.

$$
\dot{x}(t)=-x+f(t, x(t))
$$



We note that these results are about two of the most commonly used architecture modifications for RNNs. First, the gating mechanism is ubiquitous in RNNs, and usually thought of as a heuristic for smoothing optimization [28]. Second, many of the effective large-scale RNNs use linear (gated) recurrences and deeper models, which is usually thought of as a heuristic for computational efficiency [5]. Our results suggest that neither of these are heuristics after all, and arise from standard ways to approximate ODEs.

| Method <br> Variant <br> Special cases | RNN <br> Gated <br> LSTM [28, GRU [14] | RNN <br> Gated, linear <br> QRNN [5], SRU 33] | LSSL <br> Discrete $4+5$ | LSSL <br> Continuous $11+2$ |
| :---: | :---: | :---: | :---: | :---: |
| Deep? <br> Continuous? <br> Linear? | Single-layer <br> Discrete-time <br> Non-linear | Deep <br> Discrete-time <br> Linear | Deep <br> Discrete-time <br> Linear | Deep <br> Continuous-time <br> Linear |
| Approximation <br> Depth-wise <br> Time-wise | - Backwards Euler | Picard iteration <br> $\operatorname{GBT}(\alpha=1)$ | Picard iteration <br> $\operatorname{GBT}\left(\alpha=\frac{1}{2}\right)$ (i.e. Bilinear) | Picard iteration <br> - |
> Table 6: A summary of the characteristics of popular RNN methods and their approximation mechanisms for capturing the dynamics $\dot{x}(t)=-x(t)+f(t, x(t))$ (equation (14)). The LSSL entries are for the very specific case with order $N=1$ and $A=-1, B=1, C=1, D=0$; LSSLs are more general.


To be more specific, we show that:

- Non-linear RNNs discretize the dynamics (14) by applying backwards Euler discretization to the linear term, which arises in the gating mechanism of RNNs (Appendix C.2.2. Lemma C.1).
- A special case of LSSLs approximates the dynamics (14) (in continuous-time) by applying Picard iteration to the non-linear term (Appendix C.2.3. Theorem 4).
- Deep linear RNNs approximate the dynamics (14) with both Picard iteration in the depth direction to linearize the non-linear term, and discretization (gates) in the time direction to discretize the equation (Appendix C.2.4, Corollary C.3).

A comparison is summarized in Table 6 .

In the remainder of this section, we assume that there is an underlying function $x(t)$ that satisfies (14) on some interval for any initial condition, and that $f$ is continuous and Lipschitz in its second argument. Our goal is to show that several families of models approximate this in various ways.

#### C.2.1 Intuition / Proof Sketches

We sketch the idea of how LSSLs capture popular RNNs. More precisely, we will show how approximating the dynamics (14) in various ways lead to types of RNNs and LSSLs.

The first step is to look at the simpler dynamics

$$
\dot{x}(t)=-x(t)+u(t)
$$

where there is some input $u(t)$ that is independent of $x$. (In other words, in (14), the function $f(t, x)$ does not depend on the second argument.)

By directing applying the GBT discretization with $\alpha=1$, this leads to a gated recurrence (Lemma 3.1).

The second step is that by applying the backwards Euler discretization more directly to (14), this leads to a gated RNN where the input can depend on the state (Lemma C.1).

Alternatively, we can apply Picard iteration on (14), which says that the iteration

$$
x^{(\ell)}(t)=x_{0}+\int_{t_{0}}^{t}-x^{(\ell-1)}(s) d s+\int_{t_{0}}^{t} f\left(s, x^{(\ell-1)}(s)\right) d s
$$

converges to the solution $x(t)$.

However, the first integral term is simple and can be tightened. We can instead try to apply Picard iteration on only the second term, leaving the first integral in terms of $x^{(\ell)}$. Intuitively this should still
converge to the right solution, since this is a weaker iteration; we're only using the Picard approximation on the second term.

$$
x^{(\ell)}(t)=x_{0}+\int_{t_{0}}^{t}-x^{(\ell)}(s) d s+\int_{t_{0}}^{t} f\left(s, x^{(\ell-1)}(s)\right) d s
$$

Differentiating, this equation is the ODE

$$
\dot{x}^{(\ell)}(t)=-x^{(\ell)}(t)+f\left(t, x^{(\ell-1)}(t)\right)
$$

This implies that alternating point-wise functions with a simple linear ODE $\dot{x}^{(\ell)}(t)=-x^{(\ell)}(t)+u^{(\ell)}(t)$ also captures the dynamics (14). But this is essentially what an LSSL is.

To move to discrete-time, this continuous-time layer can be discretized with gates as in Lemma 3.1, leading to deep linear RNNs such as the QRNN, or with the bilinear discretization, leading to the discrete-time LSSL. We note again that in the discrete-time LSSL, $\bar{A}$ and $\bar{B}$ play the role of the gates $\sigma, 1-\sigma$.

#### C.2.2 Capturing gates through discretization

Lemma C.1. Consider an $R N N$ of the form

$$
x_{k}=\left(1-\sigma\left(z_{k}\right)\right) x_{k-1}+\sigma\left(z_{k}\right) \bar{f}\left(k, x_{k-1}\right),
$$

where $\bar{f}(k, x)$ is an arbitrary function that is Lipschitz in its second argument (e.g., it may depend on an external input $\left.u_{k}\right)$.

Then equation 15 is a discretization of the dynamics with step sizes $\Delta t_{k}=\exp \left(z_{k}\right)$, i.e. $x_{k} \approx x\left(t_{k}\right)$ where $t_{k}=\sum_{i=1}^{k} \Delta t_{i}$.

Proof. Apply the backwards Euler discretization (13) to equation (14) to get

$$
\begin{aligned}
x_{k}-x_{k-1} & =\Delta t_{k}\left[-x_{k}+f\left(t_{k}, x_{k}\right)\right] \\
\left(1+\Delta t_{k}\right) x_{k} & =x_{k-1}+\Delta t_{k} f\left(t_{k}, x_{k}\right) \\
x_{k} & =\frac{1}{1+\Delta t_{k}} x_{k-1}+\frac{\Delta t_{k}}{1+\Delta t_{k}} f\left(t_{k}, x_{k}\right) .
\end{aligned}
$$

Note that $\frac{\Delta t_{k}}{1+\Delta t_{k}}=\frac{e^{z_{k}}}{1+e^{z_{k}}}=\frac{1}{1+e^{-z_{k}}}$ and $\frac{1}{1+\Delta t_{k}}=1-\frac{\Delta t_{k}}{1+\Delta t_{k}}$, thus

$$
x_{k}=\left(1-\sigma\left(z_{k}\right)\right) x_{k-1}+\sigma\left(z_{k}\right) \bar{f}\left(k, x_{k-1}\right) .
$$

Here we are denoting $\bar{f}(k, x)=f\left(t_{k}, x\right)$ to be a discrete-time version of $f$ evaluatable at the given timesteps $t_{k}$.

Note that a potential external input function $u(t)$ or sequence $u_{k}$ is captured through the abstraction $f(t, x)$. For example, a basic RNN could define $\bar{f}(k, x)=f\left(t_{k}, x\right)=\tanh \left(W x+U u_{k}\right)$.

#### C.2.3 Capturing non-linearities through Picard iteration

The main result of this section is Theorem4 4 showing that LSSLs can approximate the same dynamics as the RNNs in the previous section. This follows from a technical lemma.

Lemma C.2. Let $f(t, x)$ be any function that satisfies the conditions of the Picard-Lindelöf Theorem (Theorem 3).

Define a sequence of functions $x^{(\ell)}$ by alternating the (point-wise) function $f$ with solving an ODE

$$
\begin{aligned}
& x^{(0)}(t)=x_{0} \\
& u^{(\ell)}(t)=f\left(t, x^{(\ell-1)}(t)\right) \\
& \dot{x}^{(\ell)}(t)=A x^{(\ell)}(t)+u^{(\ell)}(t) .
\end{aligned}
$$

Then $x^{(\ell)}$ converges to a solution $x^{(\ell)}(t) \rightarrow x(t)$ of the IVP

$$
\begin{aligned}
\dot{x}(t) & =A x(t)+f(t, x(t)) \\
x\left(t_{0}\right) & =x_{0} .
\end{aligned}
$$

Theorem 4. A (continuous-time) deep LSSL with order $N=1$ and $A=-1, B=1, C=1, D=0$ approximates the non-linear dynamics (14).

Proof. Applying the definition of an LSSL (equations (1) $+(2)$ ) with these parameters results in a layer mapping $u(t) \mapsto y(t)$ where $y$ is defined implicitly through the ODE

$$
\dot{y}(t)=-y(t)+u(t) .
$$

This can be seen since the choice of $C, D$ implies $y(t)=x(t)$ and the choice of $A, B$ gives the above equation.

Consider the deep LSSL defined by alternating this LSSL with position-wise (in time) non-linear functions

$$
\begin{aligned}
u^{(\ell)}(t) & =f\left(t, y^{(\ell-1)}(t)\right) \\
\dot{y}^{(\ell)}(t) & =-y^{(\ell)}(t)+u^{(\ell)}(t)
\end{aligned}
$$

But this is exactly a special case of Lemma C.2 so that we know $y^{(\ell)}(t) \rightarrow y(t)$ such that $y(t)$ satisfies

$$
\dot{y}(t)=-y(t)+f(t, y(t))
$$

as desired.

Proof of Lemma C.2. Let

$$
z(t)=e^{-A t} x(t)
$$

(and $\left.z_{0}=z\left(t_{0}\right)=x\left(t_{0}\right)=x_{0}\right)$. Note that

$$
\begin{aligned}
\dot{z}(t) & =e^{-A t}[\dot{x}(t)-A x(t)] \\
& =e^{-A t} f(t, x(t)) \\
& =e^{-A t} f\left(t, e^{A t} z(t)\right) .
\end{aligned}
$$

Since $f$ satisfies the conditions of the Picard Theorem (i.e., is continuous in the first argument and Lipschitz in the second), so does the function $g$ where $g(t, x):=e^{-A t} f\left(t, e^{A t} x\right)$ for some interval around the initial time.

By Theorem 3 , the iterates $z^{(\ell)}$ defined by

$$
z^{(\ell)}(t)=z_{0}+\int_{t_{0}}^{t} e^{-A s} f\left(s, e^{A s} z^{(\ell-1)}(s)\right) d s
$$

converges to $z$.

Define $x^{(\ell)}(t)=e^{A t} z^{(\ell)}(t)$. Differentiate 16 to get

$$
\begin{aligned}
\dot{z}^{(\ell)}(t) & =e^{-A t} f\left(t, e^{A t} z^{(\ell-1)}(t)\right) \\
& =e^{-A t} f\left(t, x^{(\ell-1)}(t)\right) \\
& =e^{-A t} u^{(\ell)}(t)
\end{aligned}
$$

But

$$
\dot{z}^{(\ell)}(t)=e^{-A t}\left[\dot{x}^{(\ell)}(t)-A x^{(\ell)}(t)\right],
$$

so

$$
\dot{x}^{(\ell)}(t)=A x^{(\ell)}(t)+u^{(\ell)}(t)
$$

Since $z^{(\ell)} \rightarrow z$ and $x^{(\ell)}(t)=e^{A t} z^{(\ell)}(t)$ and $x(t)=e^{A t} z(t)$, we have $x^{(\ell)} \rightarrow x$.

#### C.2.4 Capturing Deep, Linear, Gated RNNs

We finally note that several types of RNNs exist which were originally motivated by approximating linearizing gated RNNs for speed. Although these were treated as a heuristic for efficiency reasons, they are explained by combining our two main technical results.

Lemma C. 1 shows that a single-layer, discrete-time, non-linear RNN approximates the dynamics (14) through discretization, which arises in the gating mechanism.

Theorem 4 shows that a deep, continuous-time, linear RNN approximates (14) through Picard iteration, where the non-linearity is moved to the depth direction.

Combining these two results leads to Corollary C.3, which says that a deep, discrete-time, linear RNN can also approximate the same dynamics (14).

Corollary C.3. Consider a deep, linear $R N N$ of the form

$$
\begin{aligned}
& x_{k}^{(\ell)}=\left(1-\sigma\left(z_{k}\right)\right) x_{k-1}^{(\ell)}+\sigma\left(z_{k}\right) u_{k}^{(\ell)} \\
& u_{k}^{(\ell)}=\bar{f}\left(k, x_{k}^{(\ell-1)}\right) .
\end{aligned}
$$

This is a discretization of the dynamics (14) with step sizes $\Delta t_{k}=\exp \left(z_{k}\right)$, i.e. $x_{k} \approx x\left(t_{k}\right)$ where $t_{k}=$ $\sum_{i=1}^{k} \Delta t_{i}$.

Proof. By Lemma C.1 the first equation is a discretization of the continuous-time equation

$$
\dot{x}^{(\ell)}(t)=-x^{(\ell)}(t)+u^{(\ell)}(t)
$$

where

$$
u^{(\ell)}(t)=f\left(t, x^{(\ell-1)}(t)\right)
$$

uses the continuous-time version $f$ of $\bar{f}$. But by Lemma C.2. this is an approximation of the dynamics (14) using Picard iteration.

Notable examples of this type of model include the Quasi-RNN or QRNN [5] and the Simple Recurrent Unit (SRU) 33, which are among the most effective models in practice. We remark that these are the closest models to the LSSL and suggest that their efficacy is a consequence of the results of this section, which shows that they are not heuristics.

We note that there are many more RNN variants that use a combination of these gating and linearization techniques that were not mentioned in this section, and can be explained similarly.

## D LSSL Proofs and Algorithms

This section proves the results in Section 4.1, and is organized as follows:

- Appendix D.1 gives a self-contained synopsis of the HiPPO framework [24].
- Appendix D. 2 proves Theorem 1, which shows that the hippo operators for any measure lead to a simple linear ODE of the form of equation (1).
- Appendix D. 3 proves Corollary 4.1 including a formal definition of quasiseparable matrices (i.e., how LSSL matrices are defined) in Definition 4.

Notation This section is technically involved and we adopt notation to simplify reasoning about the shapes of objects. In particular, we use bold capitals (e.g. A) to denote matrices and bold lowercase (e.g. b) to denote vectors. For example, equation (1) becomes $\dot{x}=\mathbf{A} x+\mathbf{b} u$. These conventions are adopted throughout Appendices D and E

### D. 1 Preliminaries: HiPPO Framework and Recurrence Width

This section summarizes technical preliminaries taken directly from prior work. We include this section so that this work is self-contained and uses consistent notation, which may deviate from prior work. For example, we use modified notation from Gu et al. [24] in order to follow conventions in control theory (e.g., we denote input by $u$ and state by $x$ as in (1)).

Appendix D.1.1 formally defines the HiPPO operator mathematically as in [24, Section 2.2], and Appendix D.1.2 overviews the steps to derive the HiPPO operator as in 24, Appendix C]. Appendix D.1.3 defines the class of Low Recurrence Width (LRW) matrices, which is the class of matrices that our generalization of the HiPPO results (Theorem 1) uses.

#### D.1.1 Definition of HiPPO Operator

Definition 1 (24, Definition 1). Given a time-varying measure $\mu^{(t)}$ supported on $(-\infty, t]$, an $N$-dimensional subspace $\mathcal{G}$ of polynomials, and a continuous function $u: \mathbb{R}_{\geq 0} \rightarrow \mathbb{R}$, HiPPO defines a projection operator $\operatorname{proj}_{t}$ and a coefficient extraction operator coef ${ }_{t}$ at every time $t$, with the following properties:

1. $\operatorname{proj}_{t}$ takes a function $u$ restricted up to time $t, u_{\leq t}:=\left.u(x)\right|_{x \leq t}$, and maps it to a polynomial $g^{(t)} \in \mathcal{G}$, that minimizes the approximation error $\left\|u_{\leq t}-g^{(\bar{t})}\right\|_{L_{2}\left(\mu^{(t)}\right)}$.
2. $\operatorname{coef}_{t}: \mathcal{G} \rightarrow \mathbb{R}^{N}$ maps the polynomial $g^{(t)}$ to the coefficients $c(t) \in \mathbb{R}^{N}$ of the basis of orthogonal polynomials defined with respect to the measure $\mu^{(t)}$.

The composition $\operatorname{coef}_{t} \circ \operatorname{proj}_{t}$ is called hippo, which is an operator mapping a function $u: \mathbb{R}_{\geq 0} \rightarrow \mathbb{R}$ to the optimal projection coefficients $c: \mathbb{R}_{\geq 0} \rightarrow \mathbb{R}^{N}$ (i.e $(\operatorname{hippo}(u))(t)=\operatorname{coef}_{t}\left(\operatorname{proj}_{t}(f)\right)$.

#### D.1.2 HiPPO Framework for Deriving the HiPPO Operator

The main ingredients of HiPPO consists of an approximation measure and an orthogonal polynomial basis. We recall how they are defined in [24], (we note that compared to Gu et al. 24, our notation has changed from input $f(t)$ coefficients (state) $c(t)$ to input $u(t)$ and coefficients (state) $x(t)$, following conventions in controls).

Approximation Measures At every $t$, the approximation quality is defined with respect to a measure $\mu^{(t)}$ supported on $(-\infty, t]$. We assume that the measures $\mu^{(t)}$ have densities $\omega(t, Y):=\frac{d \mu^{(t)}}{d Y}$. Note that this implies that integrating with respect to $d \mu^{(t)}$ is the same as integrating with respect to $\omega(t, Y) d Y$.

Orthogonal Polynomial basis Let $\left\{P_{n}^{(t)}\right\}_{n \in \mathbb{N}}$ denote a sequence of orthogonal polynomials with respect to some time-varying measure $\mu^{(t)}$. Let $p_{n}^{(t)}$ be the normalized version of of orthogonal $P_{n}^{(t)}$, and define

$$
p_{n}(t, Y)=p_{n}^{(t)}(Y)
$$

In particular, the above implies that

$$
\int_{-\infty}^{t} p_{n}^{(t)}(Y) \cdot p_{m}^{(t)}(Y) \omega(t, Y) d Y=\delta_{m, n}
$$

In the general framework, HiPPO does not require an orthogonal polynomial basis as the selected basis. The choice of basis is generalized by tilting with $\chi$.

Tilted measure and basis For any scaling function $\chi(t, Y)$, the functions $p_{n}(t, Y) \chi(t, Y)$ are orthogonal with respect to the density $\frac{\omega}{\chi^{2}}$ at every time $t$. Define $\nu(t)$ to be the normalized measure with density proportional to $\frac{\omega}{\chi^{2}}$, with normalization constant $\zeta(t)=\int_{0}^{t} \frac{\omega(t, Y)}{\chi(t, Y)^{2}} \mathrm{~d} x$.

We express the coefficients $x_{n}(t)$ calculated by the HiPPO framework as:

$$
x_{n}(t)=\frac{1}{\sqrt{\zeta(t)}} \int_{0}^{t} u(Y) p_{n}(t, Y) \frac{\omega(t, Y)}{\chi(t, Y)} d Y
$$

To use this to derive $\dot{x}_{n}(t)$, let $h(t, Y)=u(Y) p_{n}(t, Y) \omega(t, Y)$. We see that

$$
\begin{aligned}
\dot{x}_{n}(t)= & \frac{\mathrm{d}}{\mathrm{d} t} \int_{0}^{t} u(Y) p_{n}(t, Y) \omega(t, Y) \mathrm{d} Y \\
= & \frac{\mathrm{d}}{\mathrm{d} t} \int_{0}^{t} h(t, Y) \mathrm{d} Y \\
= & \int_{0}^{t} \frac{\partial}{\partial t} h(t, Y) \mathrm{d} Y+h(t, t) \\
= & \int_{0}^{t} u(Y)\left(\frac{\partial}{\partial t} p_{n}(t, Y)\right) \omega(t, Y) \mathrm{d} Y+\int_{0}^{t} f(Y) p_{n}(t, Y)\left(\frac{\partial}{\partial t} \omega(t, Y)\right) \mathrm{d} Y \\
& +u(t) p_{n}(t, t) \omega(t, t) .
\end{aligned}
$$

This allows $\dot{x}_{n}(t)$ to be written as

$$
\begin{aligned}
\dot{x}_{n}(t)= & u(t) p_{n}(t, t) \omega(t, t)+\int_{0}^{t} u(Y)\left(\frac{\partial}{\partial t} p_{n}(t, Y)\right) \omega(t, Y) \mathrm{d} Y \\
& +\int_{0}^{t} f(Y) p_{n}(t, Y)\left(\frac{\partial}{\partial t} \omega(t, Y)\right) \mathrm{d} Y
\end{aligned}
$$

Although Gu et al. 24 describe the framework in the full generality above and use $\chi$ as another degree of freedom, in their concrete derivations they always fix $\chi=\omega$. Our general results also use this setting. For the remainder of this section, we assume the "full tilting" case $\chi=\omega$. In particular, this means that in Eq. (18), we essentially substitute $\omega$ above with 1 and divide each term by the inverse square root of our normalization constant, $\zeta$, to get the coefficient dynamics that we will use in our arguments:

$$
\dot{x}_{n}(t)=\frac{1}{\sqrt{\zeta(t)}} u(t) p_{n}(t, t)+\frac{1}{\sqrt{\zeta(t)}} \int_{0}^{t} u(Y)\left(\frac{\partial}{\partial t} p_{n}(t, Y)\right) \mathrm{d} Y
$$

Now, if we can show that each of the integrated terms in (18) are linear combinations of $x_{n}(t)$, this would be the same as saying that $\dot{x}_{n}(t)=\mathbf{A}(t) x(t)+\mathbf{b}(t) u(t)$ for some $\mathbf{A}(t)$. Therefore, the incremental update operation would be bounded by the runtime of the matrix-vector operation $\mathbf{A}(t) x(t)$.

#### D.1.3 Recurrence Width

Our final goal is to show that $\dot{x}_{n}(t)=\mathbf{A}(t) x(t)+\mathbf{b}(t) u(t)$ for some $\mathbf{A}(t)$ with constant recurrence width (see Definition 2). This will show Theorem 1, and also imply that the MVM A $(t) x(t)$ can be computed in $\tilde{O}(N)$ time. To build this argument, we borrow the fact that OPs all have recurrence width 2 and results regarding matrix-vector multiplication of matrices with constant recurrence width along with their inverses.

Definition 2 ([17]). An $N \times N$ matrix $\mathbf{A}$ has recurrence width $t$ if the polynomials $a_{i}(X)=\sum_{j=0}^{N-1} \mathbf{A}[i, j] X^{j}$ satisfy $\operatorname{deg}\left(a_{i}\right) \leq i$ for $i<t$, and

$$
a_{i}(X)=\sum_{j=1}^{t} g_{i, j}(X) a_{i-j}(X)
$$

for $i \geq t$, where the polynomials $g_{i, j} \in \mathbb{R}[X]$ have degree at most $j$.

Theorem 5 ([17], Theorem 4.4). For any $N \times N$ matrix A with constant recurrence width, any vector $\mathbf{x} \in \mathbb{R}^{n}$, Ax can be computed with $\tilde{O}(N)$ operations over $\mathbb{R}$.

Theorem 6 ([17], Theorem 7.1). For any $N \times N$ matrix A with constant recurrence width, any vector $\mathbf{x} \in \mathbb{R}^{n}, \mathbf{A}^{-1} \mathbf{x}$ can be computed with $\tilde{O}(N)$ operations over $\mathbb{R}$.

For the rest of the note we'll assume that any operation over $\mathbb{R}$ can be done in constant time. It would be useful for us to define $\mathbf{P} \in \mathbb{R}^{N \times N}$ such that the coefficients of the OP $p_{i}(X)$, i.e. $p_{i}(X)=\sum_{j=0}^{N-1} \mathbf{P}[i, j] X^{j}$.

### D. 2 Proof of Theorem 1

This section proves Theorem 1, which is restated formally in Corollary D.4 Appendix D.2.1 proves some results relating orthogonal polynomials to recurrence width (Appendix D.1.3). Appendix D.2.2 proves Corollary D.4 Appendices D.2.3 and D.2.4 provides examples showing how Corollary D.4 can be specialized to exactly recover the HiPPO-LegT, HiPPO-LagT, HiPPO-LegS methods [24].

#### D.2.1 Relating Orthogonal Polynomials and Recurrence Width

Next we introduce the following lemma, which will be useful in our arguments:

Lemma D.1. For any $n$, there exists ordered sets of coefficients $\alpha_{n}=\left\{\alpha_{n, i}\right\}, \beta_{n}=\left\{\beta_{n, i}\right\}$,

(i) $p_{n}^{\prime}(Z)=\sum_{i=0}^{n-1} \alpha_{n, i} p_{i}(Z)$

(ii) $Z p_{n}^{\prime}(Z)=\sum_{i=0}^{n-1} \beta_{n, i} p_{i}(Z)$

Proof. Follows from the fact that $p_{i}(z)$ for $0 \leq i<N$ forms a basis and the observation of the degrees of the polynomials on the LHS.

The following matrices will aid in showing verifying that matrix vector multiplication with a given matrix A can be computed in $O(\tilde{N})$ time.

Definition 3. $\mathbf{D}_{1}, \mathbf{D}_{2} \in \mathbb{R}^{N \times N}$ are the matrices such that

$$
p_{i}^{\prime}(Z)=\sum_{j=0}^{N-1} \mathbf{D}_{1}[i, j] Z^{j}, \text { and } Z p_{i}^{\prime}(Z)=\sum_{j=0}^{N-1} \mathbf{D}_{2}[i, j] Z^{j}
$$

Let $\mathbf{S}$ be the "right shift" matrix, i.e. for any matrix $\mathbf{M}, \mathbf{M S}$ has the columns of $\mathbf{M}$ shifted to right by one. Note that $\mathbf{S}^{T}$ corresponds to the "left shift" matrix.

We now note that:

Lemma D.2. $\mathbf{D}_{1}=\mathbf{P} \cdot \operatorname{diag}(0,1, \ldots, N-1) \cdot \mathbf{S}^{T}$ and $\mathbf{D}_{2}=\mathbf{P} \cdot \operatorname{diag}(0,1, \ldots, N-1)$. In particular, $\mathbf{D}_{1} \mathbf{z}$ and $\mathbf{D}_{2} \mathbf{z}$ can be computed in $\tilde{O}(N)$ time for any $\mathbf{z} \in \mathbb{R}^{N}$.

Proof. Recall that $\mathbf{P}$ has the coefficients of the OP polynomials $p_{0}(Z), \ldots, p_{N-1}(Z)$ as its rows. Then note that

$$
p_{n}^{\prime}(Z)=\sum_{i=0}^{n-1} i \cdot \mathbf{P}[n, i] \cdot Z^{i-1}
$$

The claim on $\mathbf{D}_{1}=\mathbf{P} \cdot \operatorname{diag}(0,1, \ldots, N-1) \cdot \mathbf{S}^{T}$ follows from the above. Recall that $\mathbf{D}$ has recurrence width of 2. The claim on the runtime of computing $\mathbf{D}_{1} \mathbf{z}$ then follows from Theorem 5 and the fact that both $\operatorname{diag}(0,1, \ldots, N-1)$ and $\mathbf{S}^{T}$ is $n$-sparse.

From Eq. 20 , it is easy to see that

$$
Z p_{n}^{\prime}(Z)=\sum_{i=0}^{n-1} i \cdot \mathbf{P}[n, i] \cdot Z^{i}
$$

The claim on the structure of $\mathbf{D}_{2}$ then follows from the above expression. The claim on runtime of computing $\mathbf{D}_{2} \mathbf{z}$ follows from essentially the same argument as for $\mathbf{D}_{1} \mathbf{z}$.

Finally, we make the following observation:

Lemma D.3. Let $\mathbf{A}^{\prime}$ and $\mathbf{B}^{\prime}$ be defined such that $\mathbf{A}^{\prime}[n, i]=\alpha_{n, i}$ and $\mathbf{B}^{\prime}[n, i]=\beta_{n, i}$. Then both $\mathbf{A}^{\prime}$ and $\mathbf{B}^{\prime}$ are both products of three matrices: two of which have recurrence width at most 2 and the third is the inverse of a matrix that has recurrence width 2.

Proof. We note that since $\mathbf{P}$ expresses the orthogonal polynomials in standard basis, $\mathbf{P}^{-1}$ changes from OP basis to standard basis. This along with Lemma D. 2 implies that $\mathbf{A}=\mathbf{P} \cdot\left(\operatorname{diag}(0,1, \ldots, N-1) \cdot \mathbf{S}^{T}\right) \cdot \mathbf{P}^{-1}$. It is easy to check that $\operatorname{diag}(0,1, \ldots, N-1) \cdot \mathbf{S}^{T}$ has recurrence width 1 and the claim on $\mathbf{A}^{\prime}$ follows since $\mathbf{P}$ has recurrence width 2. A similar argument proves the claim on $\mathbf{B}^{\prime}$.

#### D.2.2 HiPPO for General Measures

Let $\theta: \mathbb{R}_{\geq 0} \mapsto \mathbb{R}_{\geq 0}$ be a function such that for all $t, \theta(t) \leq t$ and $\theta(t)$ is differentiable.

In what follows, define

We note that

$$
z=\frac{2(Y-t)}{\theta(t)}+1
$$

$$
\frac{\mathrm{d} z}{\mathrm{~d} Y}=\frac{2}{\theta(t)}
$$

Further, note that:

$$
\begin{aligned}
\frac{\mathrm{d} z}{\mathrm{~d} t} & =\frac{\mathrm{d}}{\mathrm{d} t}\left(\frac{2(Y-t)}{\theta(t)}+1\right) \\
& =-\frac{2}{\theta(t)}-\frac{2(Y-t) \theta^{\prime}(t)}{\theta^{2}(t)} \\
& =-\frac{2}{\theta^{2}(t)}\left(\theta(t)+(Y-t) \theta^{\prime}(t)\right) .
\end{aligned}
$$

From the definition of $z$, we see that $Y-t=\frac{(z-1) \theta(t)}{2}$. Then

$$
\begin{aligned}
\frac{\mathrm{d} z}{\mathrm{~d} t} & =-\frac{2}{\theta(t)}\left(1+\left(\frac{z-1}{2}\right) \theta^{\prime}(t)\right) \\
& =-\frac{2}{\theta(t)}-\frac{(z-1) \theta^{\prime}(t)}{\theta(t)}
\end{aligned}
$$

Additionally, given a measure $\omega$ on $[-1,1]$ and OP family $p_{0}(Y), p_{1}(Y), \ldots$ such that for all $i \neq j$,

$$
\int_{-1}^{1} p_{i}(Y) p_{j}(Y) \omega(Y) \mathrm{d} Y=\delta_{i, j}
$$

define

$$
\omega(Y, t)=\frac{2}{\theta(t)} \omega(z) \text { and } p_{n}(Y, t)=p_{n}(z)
$$

Then we can adjust 17 to:

$$
x_{n}(t)=\int_{t-\theta(t)}^{t} u(Y) p_{n}(z) \frac{2}{\theta(t)} \mathrm{d} Y
$$

The Leibniz integral rule states that

$$
\frac{\partial}{\partial t} \int_{\alpha(t)}^{\beta(t)} h(t, Y) \mathrm{d} Y=\int_{\alpha(t)}^{\beta(t)} \frac{\partial}{\partial t} h(t, Y) \mathrm{d} Y-\alpha^{\prime}(t) h(\alpha(t), t)+\beta^{\prime}(t) h(t, t) .
$$

If we let $\alpha(t)=t-\theta(t)$ and $\beta(t)=t$, then applying the Leibniz rule to 23 we get:

$$
\begin{aligned}
\dot{x}_{n}(t)= & \int_{t-\theta(t)}^{t} u(Y) \frac{\partial}{\partial t}\left(p_{n}(z) \frac{2}{\theta(t)}\right) \mathrm{d} Y-\left(1-\theta^{\prime}(t)\right) u(t-\theta(t)) p_{n}(t-\theta(t), t) \frac{2}{\theta(t)} \\
& +u(t) p_{n}(t, t) \frac{2}{\theta(t)} \\
= & -\left(1-\theta^{\prime}(t)\right) u(t-\theta(t)) p_{n}(-1) \frac{2}{\theta(t)}+u(t) p_{n}(1) \frac{2}{\theta(t)} \\
& +\int_{t-\theta(t)}^{t} u(Y) \frac{\mathrm{d} z}{\mathrm{~d} t} p_{n}^{\prime}(z) \frac{2}{\theta(t)} \mathrm{d} Y-\frac{\theta^{\prime}(t)}{\theta(t)} \int_{t-\theta(t)}^{t} u(Y) p_{n}(z) \frac{2}{\theta(t)} \mathrm{d} Y .
\end{aligned}
$$

From $(22)$, it follows that

$$
\begin{gathered}
\dot{x}_{n}(t)=-\frac{2\left(1-\theta^{\prime}(t)\right) u(t-\theta(t)) p_{n}(-1)}{\theta(t)}+\frac{2 \cdot u(t) p_{n}(1)}{\theta(t)}-\frac{2}{\theta(t)} \int_{t-\theta(t)}^{t} u(Y) p_{n}^{\prime}(z) \frac{2}{\theta(t)} \mathrm{d} Y- \\
\frac{\theta^{\prime}(t)}{\theta(t)} \int_{t-\theta(t)}^{t} u(Y)(z-1) p_{n}^{\prime}(z) \frac{2}{\theta(t)} \mathrm{d} Y-\frac{\theta^{\prime}(t)}{\theta(t)} \int_{t-\theta(t)}^{t} u(Y) p_{n}(z) \frac{2}{\theta(t)} \mathrm{d} Y .
\end{gathered}
$$

Because $\operatorname{deg}\left(p_{n}^{\prime}(z)\right) \leq n-1$ and $\operatorname{deg}\left((z-1) p_{n}^{\prime}(z)\right) \leq n$, they can be written as a linear combination of $\left\{p_{i}^{\prime}\right\}_{i \leq n}$. Let us define $\left\{\alpha_{n, j}\right\},\left\{\beta_{n, j}\right\}$ such that

$$
p_{n}^{\prime}(z)=\sum_{j=0}^{n-1} \alpha_{n, j} p_{j}(z) \text { and }(z-1) p_{n}^{\prime}(z)=\sum_{j=0}^{n} \beta_{n, j} p_{j}(z)
$$

Then by using (25) in (24), we get:

$$
\begin{aligned}
\dot{x}_{n}(t)= & -\frac{2\left(1-\theta^{\prime}(t)\right) u(t+\theta(t)) p_{n}(-1)}{\theta(t)}+\frac{2 \cdot u(t) p_{n}(1)}{\theta(t)} \\
& -\frac{2}{\theta(t)} \sum_{j=0}^{n-1} \alpha_{n, j} \int_{t-\theta(t)}^{t} u(Y) p_{j}(z) \frac{2}{\theta(t)} \mathrm{d} Y \\
& -\frac{\theta^{\prime}(t)}{\theta(t)} \sum_{j=0}^{n} \beta_{n, j} \int_{t-\theta(t)}^{t} u(Y) p_{j}(z) \frac{2}{\theta(t)} \mathrm{d} Y-\frac{\theta^{\prime}(t)}{\theta(t)} \int_{t-\theta(t)}^{t} u(Y) p_{n}(z) \frac{2}{\theta(t)} \mathrm{d} Y \\
= & -\frac{2\left(1-\theta^{\prime}(t)\right) u(t+\theta(t)) p_{n}(-1)}{\theta(t)}+\frac{2 \cdot u(t) p_{n}(1)}{\theta(t)} \\
& -\frac{2}{\theta(t)} \sum_{j=0}^{n-1} \alpha_{n, j} x_{j}(t)-\frac{\theta^{\prime}(t)}{\theta(t)} \sum_{j=0}^{n} \beta_{n, j} x_{j}(t)-\frac{\theta^{\prime}(t)}{\theta(t)} x_{n}(t) .
\end{aligned}
$$

Thus, in vector form we get

 Theorem 7.

$$
\dot{x}_{n}(t)=-\frac{1}{\theta(t)} \mathbf{A}_{1}(t) x(t)-\frac{2}{\theta(t)}\left(1-\theta^{\prime}(t)\right) u(t-\theta(t))\left[\begin{array}{c}
\vdots \\
p_{n}(-1) \\
\vdots
\end{array}\right]+\frac{2}{\theta(t)} u(t)\left[\begin{array}{c}
\vdots \\
p_{n}(1) \\
\vdots
\end{array}\right]
$$

$$
\text { where } \mathbf{A}_{1}(t)[n, k]=\left\{\begin{array}{ll}
2 \alpha_{n, k}+\theta^{\prime}(t) \beta_{n, k} & \text { if } k<n \\
\theta^{\prime}(t) \beta_{n, n}+\theta^{\prime}(t) & \text { if } k=n \\
0 & \text { otherwise }
\end{array} \text { for } \alpha_{n, k}, \beta_{n, k} \text { as defined in } \quad\right. \text { (25). }
$$

Corollary D.4. The matrix $\mathbf{A}_{1}$ in Theorem 7 can be re-written as

$$
\mathbf{A}_{1}=2 \cdot \mathbf{A}^{\prime}+\theta^{\prime}(t) \cdot \mathbf{B}^{\prime}+\theta^{\prime}(t) \cdot \mathbf{I} .
$$

In particular, both $\mathbf{A}^{\prime}$ and $\mathbf{B}^{\prime}$ both products of three matrices: two of which have recurrence width at most 2 and the third is the inverse of a matrix that has recurrence width 2.

Proof. Eq. (26) follows from Theorem 7 and defining $\mathbf{A}^{\prime}$ and $\mathbf{B}^{\prime}$ to contain the $\alpha_{n, k}$ and $\beta_{n, k}$ coefficients.

#### D.2.3 Translated HiPPO (Sliding Windows)

The case when $\theta(t)=\theta$ for all $t$ represents a constant-size sliding window, which Gu et al. [24] denote as the "Translated HiPPO" case with instantiations such as HiPPO-LegT (Translated Legendre) and HiPPO-LagT (Translated Laguerre).

We now state a corollary of Theorem 7 for the case of $\theta(t)=\theta$ for all $\mathrm{t}$.

Corollary D.5. Let $\theta(t)=\theta$ for all $t$. Then

$$
\dot{x}_{n}(t)=-\frac{1}{\theta} \mathbf{A}_{1} x(t)-\frac{2}{\theta} u(t-\theta)\left[\begin{array}{c}
\vdots \\
p_{n}(-1) \\
\vdots
\end{array}\right]+\frac{2}{\theta} u(t)\left[\begin{array}{c}
\vdots \\
p_{n}(1) \\
\vdots
\end{array}\right]
$$

where $\mathbf{A}_{1}[n, j]=\left\{\begin{array}{ll}2 \alpha_{n, k} & \text { if } k<n \\ 0 & \text { otherwise }\end{array}\right.$.

Next, we use the approximation

$$
u(x) \approx \sum_{k=0}^{N-1} x_{k}(t) p_{k}(z)
$$

to handle the $u(t-\theta)$ term in Corollary D. 5 .

Corollary D.6. Let $\theta(t)=\theta$ for all $t$. Then

$$
\dot{x}_{n}(t) \approx-\frac{1}{\theta} \mathbf{A} x(t)+\frac{2}{\theta} u(t)\left[\begin{array}{c}
\vdots \\
p_{n}(1) \\
\vdots
\end{array}\right]
$$

where $\mathbf{A}=\mathbf{A}_{1}+2 \mathbf{A}_{2}$ for $\mathbf{A}_{1}$ as defined in Corollary D.5 and $\mathbf{A}_{2}[n, k]=p_{n}(-1) p_{k}(-1)$.

Proof. To approximate $u(t-\theta)$, we note that when $Y=t-\theta, z=-1$. Then

$$
u(t-\theta) \approx \sum_{k=0}^{N-1} x_{k}(t) p_{k}(-1)
$$

Then by Corollary D.5

$$
\dot{x}_{n}(t) \approx-\frac{1}{\theta} \mathbf{A}_{1} x(t)-\frac{2}{\theta}\left(\sum_{k=0}^{N-1} x_{k}(t) p_{k}(-1)\right)\left[\begin{array}{c}
\vdots \\
p_{n}(-1) \\
\vdots
\end{array}\right]+\frac{2}{\theta} u(t)\left[\begin{array}{c}
\vdots \\
p_{n}(-1) \\
\vdots
\end{array}\right]
$$

Let us define a matrix, $\mathbf{A}_{2} \in \mathbb{R}^{N \times N}$ matrix such that $\mathbf{A}_{2}[n, k]=p_{n}(-1) p_{k}(-1)$. Then the claim follows.

We now show that the special case of Corollary D. 6 for Legendre matches the results from 24.

Corollary D.7. Let $p_{n}(z)=\left(\frac{2 n+1}{2}\right)^{1 / 2} P_{n}(z)$ where $P_{n}(z)$ are the Legendre polynomials. Then

$$
\dot{x}_{n}(t) \approx \frac{1}{\theta} \mathbf{A} x(t)+\frac{2}{\theta} \mathbf{b} u(t)
$$

where

$$
\mathbf{A}[n, k]=(2 n+1)^{\frac{1}{2}}(2 k+1)^{\frac{1}{2}} \begin{cases}1 & \text { if } k \leq n \\ (-1)^{n-k} & \text { if } k \geq n\end{cases}
$$

and $\mathbf{b}[n]=\left(\frac{2 n+1}{2}\right)^{\frac{1}{2}}$.

Proof. From Corollary D.6,

$$
\dot{x}_{n}(t) \approx-\frac{1}{\theta} \mathbf{A} x(t)+\frac{2}{\theta} u(t)\left[\begin{array}{c}
\vdots \\
p_{n}(1) \\
\vdots
\end{array}\right]
$$

where $\mathbf{A}=\mathbf{A}_{1}+2 \mathbf{A}_{2}$ for $\mathbf{A}_{1}$ as defined in Corollary D.5 and $\mathbf{A}_{2}[n, k]=p_{n}(-1) p_{k}(-1)$.

It is known from (7.21.1) and in 53 that

$$
p_{n}(-1)=\left(\frac{2 n+1}{2}\right)^{\frac{1}{2}} P_{n}(-1) \text { and } P_{n}(-1)=(-1)^{n}
$$

Further,

$$
p_{n}(1)=\left(\frac{2 n+1}{2}\right)^{\frac{1}{2}} P_{n}(1) \text { and } P_{n}(1)=1
$$

Then $\mathbf{b}[n]=\left(\frac{2 n+1}{2}\right)^{\frac{1}{2}}$ follows from Corollary D. 6 and 29 .

From the following recurrence relations [1, Chapter 12]:

$$
(2 n+1) P_{n}(z)=P_{n+1}^{\prime}(z)+P_{n-1}^{\prime}(z)
$$

implies that

$$
P_{n+1}^{\prime}(z)=(2 n+1) P_{n}(z)+(2 n+1) P_{n-2}(z)+\cdots+,
$$

which in turn implies

$$
P_{n}^{\prime}=(2 n-1) P_{n-1}(z)+(2 n-5) P_{n-3}(z)+\ldots
$$

Then

$$
\begin{aligned}
p_{n}^{\prime}(z) & =\left(\frac{2 n+1}{2}\right)^{\frac{1}{2}} \cdot P_{n}^{\prime}(z) \\
& =\left(\frac{2 n+1}{2}\right)^{\frac{1}{2}}\left((2 n-1)\left(\frac{2}{2 n-1}\right)^{\frac{1}{2}} P_{n-1}(z)+(2 n-5)\left(\frac{2}{2 n-5}\right)^{\frac{1}{2}} P_{n-3}(z)+\ldots\right) \\
& =(2 n+1)^{\frac{1}{2}}\left((2 n-1)^{\frac{1}{2}} P_{n-1}(z)+(2 n-5)^{\frac{1}{2}} P_{n-3}(z)+\ldots\right) .
\end{aligned}
$$

Thus, we have

$$
\alpha_{n, k}=\left\{\begin{array}{l}
(2 n+1)^{\frac{1}{2}}(2 k+1)^{\frac{1}{2}} \text { if } k<n \text { and } n-k \text { is odd } \\
0 \quad \text { is otherwise }
\end{array}\right.
$$

Recalling that $\mathbf{A}_{1}[n, k]=2 \alpha_{n, k}$.

We note that from $(28), \mathbf{A}_{2}[n, k]=\left(\frac{2 n+1}{2}\right)^{\frac{1}{2}}\left(\frac{2 k+1}{2}\right)^{\frac{1}{2}}(-1)^{n}(-1)^{k}=\frac{(2 n+1)^{\frac{1}{2}}(2 k+1)^{\frac{1}{2}}}{2}(-1)^{n-k}$.

Recalling $\mathbf{A}=\mathbf{A}_{1}+2 \mathbf{A}_{2}$, we get:

$$
\mathbf{A}[n, k]=(2 n+1)^{\frac{1}{2}}(2 k+1)^{\frac{1}{2}} \begin{cases}2+(-1)^{n-k} & \text { if } k<n \quad \text { and } n-k \text { is odd } \\ 0+(-1)^{n-k} & \text { if } k<n \quad \text { and } n-k \text { is even } \\ (-1)^{n-k} & \text { if } k \geq n\end{cases}
$$

Note that the above is the same as:

which completes our claim.

$$
\mathbf{A}[n, k]=(2 n+1)^{\frac{1}{2}}(2 k+1)^{\frac{1}{2}} \begin{cases}1 & \text { if } k \leq n \\ (-1)^{n-k} & \text { if } k \geq n\end{cases}
$$

#### D.2.4 Scaled HiPPO: Recovering HiPPO-LegS

We now use Theorem 7 to recover the HiPPO-LegS instantiation for the "Scaled Legendre" measure, the main method from Gu et al. [24].

Corollary D.8. Let $p_{n}(z)=\left(\frac{2 n+1}{2}\right)^{1 / 2} P_{n}(z)$ where $P_{n}(z)$ are the Legendre polynomials and let $\delta(t)=t$ for all $t$. Then

$$
\dot{x}_{n}(t)=\frac{1}{t} \mathbf{A} x(t)+\frac{2}{t} \mathbf{b} u(t)
$$

where

$$
\mathbf{A}[n, k]= \begin{cases}(2 n+1)^{\frac{1}{2}}(2 k+1)^{\frac{1}{2}} & \text { if } k<n \\ n+1 & \text { if } k=n \\ 0 & \text { if } k>n\end{cases}
$$

and $\mathbf{b}[n]=\left(\frac{2 n+1}{2}\right)^{\frac{1}{2}}$.

Proof. Let $\theta(t)=t$. By Theorem 7 and noting that $\theta(t)=1$, we get:

$$
\dot{x}_{n}(t)=-\frac{1}{t} \mathbf{A}_{1} x(t)+\frac{2}{t} u(t)\left[\begin{array}{c}
\vdots \\
p_{n}(1) \\
\vdots
\end{array}\right]
$$

where

$$
\mathbf{A}_{1}(t)[n, k]= \begin{cases}2 \alpha_{n, k}+\beta_{n, k} & \text { if } k<n \\ \beta_{n, n}+1 & \text { if } k=n \\ 0 & \text { otherwise }\end{cases}
$$

for $\alpha_{n, k}, \beta_{n, k}$ as defined in (25).

Using the same arguments as in the proof of Corollary D.7. $\mathbf{b}[n]=\left(\frac{2 n+1}{2}\right)^{\frac{1}{2}}$ follows from Corollary D. 6 and (29). Also using similar arguments as the proof of Corollary D.7 we have

$$
\alpha_{n, k}=\left\{\begin{array}{l}
(2 n+1)^{\frac{1}{2}}(2 k+1)^{\frac{1}{2}} \text { if } k<n \text { and } n-k \text { is odd } \\
0 \quad \text { is otherwise. }
\end{array}\right.
$$

From (8) in 24, we know that

$$
(z+1) P_{n}^{\prime}(z)=n P_{n}(z)+(2 n+1) P_{n-1}(z)+(2 n-3) P_{n-2}(z)+\ldots
$$

Including the normalization constant $(2 n+1)^{\frac{1}{2}}$, we note that $(z-1) p_{n}^{\prime}(z)=(z+1) p_{n}^{\prime}(z)-2 p_{n}^{\prime}(z)$. Then we get

$$
(z+1) p_{n}^{\prime}(z)=n p_{n}(z)-(2 n+1)^{\frac{1}{2}}(2 n-1)^{\frac{1}{2}} p_{n-1}(z)+(2 n+1)^{\frac{1}{2}}(2 n-3)^{\frac{1}{2}} p_{n-2}(z)-\ldots
$$

In other words,

$$
\beta_{n, k}=\left\{\begin{array}{l}
-(2 n+1)^{\frac{1}{2}}(2 k+1)^{\frac{1}{2}} \text { if } k<n \text { and } n-k \text { is odd } \\
(2 n+1)^{\frac{1}{2}}(2 k+1)^{\frac{1}{2}} \text { if } k<n \text { and } n-k \text { is even } \\
n \quad \text { if } n=k \\
0 \quad \text { otherwise. }
\end{array}\right.
$$

Recalling that the definition for $\mathbf{A}_{1}$ from (30), we get:

which completes our claim.

$$
\mathbf{A}[n, k]= \begin{cases}(2 n+1)^{\frac{1}{2}}(2 k+1)^{\frac{1}{2}} & \text { if } k<n \\ n+1 & \text { if } k=n \\ 0 & \text { if } k>n\end{cases}
$$

### D. 3 Proof of Corollary 4.1: HiPPO for Classical Orthogonal Polynomials

This section proves Corollary 4.1, showing that the HiPPO matrices for measures corresponding to classical families of orthogonal polynomials [11] are quasiseparable. We define quasi-separability in Appendix D.3.1. Theorem 8 proves the claimed result for Jacobi polynomials and Lemma D.11 proves the claimed result for Laguerre polynomials.

We note that there is a third family of classical OPs, the Hermite polynomials [11], which have a two-sided infinite measure. However, since HiPPO is about continuous-time memorization of a function's history, it requires a one-sided measure and therefore the Hermite polynomials are not appropriate.

#### D.3.1 Quasiseparable Matrices

Definition 4 (from [19]). A matrix $\mathbf{R} \in \mathbb{R}^{N \times N}$ is $(p, q)$-quasiseparable if

- Every matrix contained strictly above the diagonal has rank at most $p$.
- Every matrix contained strictly below the diagonal has rank at most $q$.

A $(q, q)$-quasiseparable matrix is called $q$-quasiseparable.

We are interested in showing the A matrices for a broad class of OPs in Corollary D. 6 are $O(1)$ quasiseperable. We now state some properties of $q$-quasiseparable matrices:

Lemma D.9. Let $\mathbf{Q}$ be q-quasiseparable. Then:

(i) For any $q^{\prime}$-quasiseparable matrix $\mathbf{Q}^{\prime} \in \mathbb{R}^{N \times N}, \mathbf{Q} \pm \mathbf{Q}^{\prime}$ is $\left(q+q^{\prime}\right)$-quasiseparable.

(ii) For any $\mathbf{E} \in \mathbb{R}^{N \times N}, \mathbf{E}$ is $r$-quasiseparable where $r=\operatorname{rank}(\mathbf{E})$.

(iii) For any two diagonal matrices $\mathbf{D}_{1}, \mathbf{D}_{2} \in \mathbb{R}^{N \times N}, \mathbf{D}_{1} \mathbf{Q D}_{2}$ is q-quasiseparable.

Proof. We argue each point separately:

(i) Any submatrix contained strictly below or above the diagonal in $\mathbf{Q}$ has rank $\leq q$ and its corresponding submatrix in $\mathbf{Q}^{\prime}$ also has rank $\leq q^{\prime}$. This implies that the corresponding submatrix in $\mathbf{Q} \pm \mathbf{Q}^{\prime}$ has rank $\leq q+q^{\prime}$. Therefore $\mathbf{Q} \pm \mathbf{Q}^{\prime}$ is $\left(q+q^{\prime}\right)$-quasiseparable.

(ii) Let the $r=\operatorname{rank}(\mathbf{E})$. Thus any submatrix in $\mathbf{E}$ has $\operatorname{rank} \leq r$. Then $\mathbf{E}$ is $r$-quasiseparable.

(iii) Multiplication by diagonal matrices only scales the rows and columns, leaving the rank of each submatrix unchanged.

#### D.3.2 Jacobi Polynomials

The Jacobi polynomial of degree $n$ with parameters $\alpha, \beta>-1$ will be denoted $J_{n}^{\alpha, \beta}(z)$. The Jacobi polynomials are orthogonal with respect to measure $\omega(z)=(1-z)^{\alpha}(1+z)^{\beta}$. In particular, it is known from (eq. (4.3.3) from [53]) that

$$
\int_{-1}^{1} J_{n}^{\alpha, \beta}(z) J_{m}^{\alpha, \beta}(z) \omega(z) \mathrm{d} z=\frac{2^{\alpha+\beta+1}}{2 n+\alpha+\beta+1} \cdot \frac{\Gamma(n+\alpha+1) \Gamma(n+\beta+1)}{\Gamma(n+\alpha+\beta+1) n !} \delta_{n, m}
$$

where $\Gamma(\cdot)$ is the gamma function. Let

$$
\lambda_{n}^{\alpha, \beta}=\left(\frac{2^{\alpha+\beta+1}}{2 n+\alpha+\beta+1} \cdot \frac{\Gamma(n+\alpha+1) \Gamma(n+\beta+1)}{\Gamma(n+\alpha+\beta+1) n !}\right)^{\frac{1}{2}}
$$

be our normalization constant. We note that the normalized Jacobi polynomials

$$
p_{n}^{\alpha, \beta}(z)=\frac{J_{n}^{\alpha, \beta}(z)}{\lambda_{n}^{\alpha, \beta}}
$$

form an orthonormal OP family.

We now discuss some useful properties of Jacobi polynomials. It is known that ([50], eq. (3.100)):

$$
J_{n}^{\alpha, \beta}(z)=\frac{1}{n+\alpha+\beta}\left[(n+\beta) J_{n}^{\alpha, \beta-1}(z)+(n+\alpha) J_{n}^{\alpha-1, \beta}(z)\right]
$$

From (4.21.7) in [53], it is known that the derivative of $J_{n}^{\alpha, \beta}(z)$ is proportional to $J_{n-1}^{\alpha+1, \beta+1}(z)$ :

$$
\frac{\partial}{\partial z} J_{n}^{\alpha, \beta}(z)=\frac{1}{2}(n+\alpha+\beta+1) J_{n-1}^{\alpha+1, \beta+1}(z) .
$$

From $(32)$ and $(33)$, it follows that

$$
\frac{\partial}{\partial z} J_{n}^{\alpha, \beta}(z)=\frac{1}{2} \cdot\left((n+\beta) J_{n-1}^{\alpha+1, \beta}(z)+(n+\alpha) J_{n-1}^{\alpha, \beta+1}(z)\right)
$$

Additionally, the Jacobi polynomials $J_{n-1}^{\alpha+1, \beta}(z)$ and $J_{n-1}^{\alpha, \beta+1}(z)$ can be written as sums of $J_{n-1}^{\alpha, \beta}(z)$ polynomials. In particular from [50] (3.112) and (3.115),

$$
J_{n-1}^{\alpha+1, \beta}(z)=\frac{\Gamma(n+\beta)}{\Gamma(n+\alpha+\beta+1)} \cdot \sum_{k=0}^{n-1} \frac{(2 k+\alpha+\beta+1) \Gamma(k+\alpha+\beta+1)}{\Gamma(k+\beta+1)} J_{k}^{\alpha, \beta}(z)
$$

and

$$
J_{n-1}^{\alpha, \beta+1}(z)=\frac{\Gamma(n+\alpha)}{\Gamma(n+\alpha+\beta+1)} \cdot \sum_{k=0}^{n-1}(-1)^{n-k-1} \frac{(2 k+\alpha+\beta+1) \Gamma(k+\alpha+\beta+1)}{\Gamma(k+\alpha+1)} J_{k}^{\alpha, \beta}(z) .
$$

Using 35 and 36 in 34 allows us to write $\frac{\partial}{\partial z} J_{n}^{\alpha, \beta}(z)$ as a sum of $\left\{J_{k}^{\alpha, \beta}(z)\right\}_{k \leq n}$ as follows:

$$
\begin{aligned}
& \frac{\partial}{\partial z} J_{n}^{\alpha, \beta}(z)=\frac{n+\beta}{2}\left(\frac{\Gamma(n+\beta)}{\Gamma(n+\alpha+\beta+1)} \sum_{k=0}^{n-1} \frac{(2 k+\alpha+\beta+1) \Gamma(k+\alpha+\beta+1)}{\Gamma(k+\beta+1)} J_{k}^{\alpha, \beta}(z)\right) \\
& -\frac{n+\alpha}{2}\left(\frac{\Gamma(n+\alpha)}{\Gamma(n+\alpha+\beta+1)} \sum_{k=0}^{n-1}(-1)^{n-k} \frac{(2 k+\alpha+\beta+1) \Gamma(k+\alpha+\beta+1)}{\Gamma(k+\alpha+1)} J_{k}^{\alpha, \beta}(z)\right) .
\end{aligned}
$$

We use these properties to write $\frac{\partial}{\partial z} p_{n}^{\alpha, \beta}(z)$ as a sum of $\left\{p_{k}^{\alpha, \beta}(z)\right\}_{k \leq n}$ :

Corollary D.10. Let $p_{n}^{\alpha, \beta}(z)$ and $\lambda_{n}^{\alpha, \beta}$ be as defined in (31).

Then

$$
\begin{aligned}
& \frac{\partial}{\partial z} \lambda_{n}^{\alpha, \beta} p_{n}^{\alpha, \beta}(z)=\frac{(n+\beta)}{2} . \\
& \quad\left(\frac{\Gamma(n+\beta)}{\Gamma(n+\alpha+\beta+1)} \sum_{k=0}^{n-1} \frac{(2 k+\alpha+\beta+1) \Gamma(k+\alpha+\beta+1)}{\Gamma(k+\beta+1)} \lambda_{k}^{\alpha, \beta} p_{k}^{\alpha, \beta}(z)\right) \\
& \quad-\frac{(n+\alpha)}{2} . \\
& \quad\left(\frac{\Gamma(n+\alpha)}{\Gamma(n+\alpha+\beta+1)} \sum_{k=0}^{n-1}(-1)^{n-k} \frac{(2 k+\alpha+\beta+1) \Gamma(k+\alpha+\beta+1)}{\Gamma(k+\beta+1)} \lambda_{k}^{\alpha, \beta} p_{k}^{\alpha, \beta}(z)\right)
\end{aligned}
$$

Proof. Recall that $J_{n}^{\alpha, \beta}(z)=\lambda_{n}^{\alpha, \beta} p_{n}^{\alpha, \beta}$. Then the claim follows from (37).

#### D.3.3 HiPPO for Jacobi Polynomials

Theorem 8. Let $p_{n}^{\alpha, \beta}(z)$ be defined as in 31) and $\omega(z)=(1-z)^{\alpha}(1+z)^{\beta}$. Then

$$
\dot{x}_{n}(t) \approx-\frac{1}{\theta} \mathbf{A} x(t)+\frac{2}{\theta} \mathbf{b} u(t)
$$

where $\mathbf{A}$ is 3-quasiseperable.

Proof. From Corollary D. 6

$$
\dot{x}_{n}(t) \approx-\frac{1}{\theta} \mathbf{A} x(t)+\frac{2}{\theta} u(t)\left[\begin{array}{c}
\vdots \\
p_{n}^{\alpha, \beta}(1) \\
\vdots
\end{array}\right]
$$

where $\mathbf{A}=\mathbf{A}_{1}+2 \mathbf{A}_{2}$ for $\mathbf{A}_{1}$ as defined in Corollary D.5 and $\mathbf{A}_{2}[n, k]=p_{n}^{\alpha, \beta}(-1) p_{n}^{\alpha, \beta}(-1)$.

From Corollary D. 10 , we observe that

![](https://cdn.mathpix.com/cropped/2024_01_04_44babd86fb78d9e96991g-35.jpg?height=206&width=1423&top_left_y=965&top_left_x=316)

Then we note that,

$$
\mathbf{A}_{1}=\mathbf{D}_{11} \mathbf{Q}_{1} \mathbf{D}_{12}-\mathbf{D}_{21} \mathbf{Q}_{1} \mathbf{D}_{22}
$$

where $\mathbf{D}_{11}, \mathbf{D}_{12}, \mathbf{D}_{21}, \mathbf{D}_{22}$ are the diagonal matrices such that

$$
\begin{gathered}
\mathbf{D}_{11}[n, n]=\frac{1}{\lambda_{n}^{\alpha, \beta}} \cdot \frac{\Gamma(n+\beta+1)}{\Gamma(n+\alpha+\beta+1)} \\
\mathbf{D}_{12}[k, k]=\frac{(2 k+\alpha+\beta+1) \Gamma(k+\alpha+\beta+1)}{\Gamma(k+\beta+1)} \lambda_{k}^{\alpha, \beta} \\
\mathbf{D}_{21}[n, n]=(-1)^{n} \cdot \frac{(1}{\lambda_{n}^{\alpha, \beta}} \cdot \frac{\Gamma(n+\alpha+1)}{\Gamma(n+\alpha+\beta+1)} \\
\mathbf{D}_{22}[k, k]=(-1)^{k} \cdot \frac{(2 k+\alpha+\beta+1) \Gamma(k+\alpha+\beta+1)}{\Gamma(k+\alpha+1)} \lambda_{k}^{\alpha, \beta},
\end{gathered}
$$

and

$$
\mathbf{Q}_{1}[n, k]= \begin{cases}1 & \text { if } k<n \\ 0 & \text { otherwise. }\end{cases}
$$

(39) makes use of the fact that $(-1)^{n+k}=(-1)^{n-k}$ along with the definitions above.

Any submatrix of $\mathbf{Q}_{1}$ below the diagonal contains all 1s, and submatrix of $\mathbf{Q}_{1}$ above the diagonal contains all 0 s. Then any submatrix above or below the diagonal has rank 1. Therefore $\mathbf{Q}_{1}$ is 1-quasiseparable. Since $\mathbf{Q}_{1}$ is 1-quasiseparable and $\mathbf{D}_{11}, \mathbf{D}_{12}, \mathbf{D}_{21}, \mathbf{D}_{22}$ are all diagonal matrices, part (iii) of Lemma D. 9 implies that the matrices $\mathbf{D}_{11} \mathbf{Q}_{1} \mathbf{D}_{12}$ and $\mathbf{D}_{21} \mathbf{Q}_{1} \mathbf{D}_{22}$ are both 1-quasiseparable. Therefore part (i) of Lemma D. 9 implies that $\mathbf{A}_{1}$ is 2-quasiseparable.

From (4.1.1) and (4.1.4) in [53], it is known that

$$
p_{n}^{\alpha, \beta}(1)=\frac{1}{\lambda_{n}^{\alpha, \beta}}\left(\begin{array}{c}
n+\alpha \\
n
\end{array}\right) \text { and } p_{n}^{\alpha, \beta}(1)=\frac{(-1)^{n}}{\lambda_{n}^{\alpha, \beta}}\left(\begin{array}{c}
n+\beta \\
n
\end{array}\right)
$$

where

$$
\left(\begin{array}{l}
z \\
n
\end{array}\right)= \begin{cases}\frac{\Gamma(z+1)}{\Gamma(n+1) \Gamma(z-n+1)} & \text { if } n \geq 0 \\
0 & \text { if } n<0\end{cases}
$$

Then $\mathbf{A}_{2}$ can be written $\mathbf{D}_{3} \mathbf{Q}_{2} \mathbf{D}_{4}$ where $\mathbf{D}_{3}, \mathbf{D}_{4}$ are the diagonal matrices such that

$$
\mathbf{D}_{3}[n, n]=\frac{(-1)^{n}}{\lambda_{n}^{\alpha, \beta}}\left(\begin{array}{c}
n+\beta \\
n
\end{array}\right), \mathbf{D}_{4}[k, k]=\frac{(-1)^{k}}{\lambda_{k}^{\alpha, \beta}}\left(\begin{array}{c}
k+\beta \\
k
\end{array}\right)
$$

where $\mathbf{Q}_{2}[n, k]=1$ for all $0 \leq n, k<N$. $\mathbf{Q}_{2}$ has rank 1 , and $\mathbf{D}_{3}, \mathbf{D}_{4}$ are diagonal matrices. Hence by part (ii) and (iii) Lemma D.9, $\mathbf{A}_{2}$ is 1-quasiseparable.

Since $\mathbf{A}_{1}$ is 2-quasiseparable and $\mathbf{A}_{2}$ is 1-quasiseparable, part (i) of Lemma D. 9 implies that $\mathbf{A}=\mathbf{A}_{1}+2 \mathbf{A}_{2}$ is 3 -quasiseparable and the claim follows.

#### D.3.4 HiPPO-LagT

The Laguerre polynomial of degree $n$ with parameters $\alpha>-1$ will be denoted $L_{n}^{\alpha}(z)$. The Laguerre polynomials are orthogonal with respect to measure $z^{\alpha} e^{-z}$. In particular, from (5.1.1) in [53] we know that

$$
\int_{-1}^{\infty} L_{n}^{\alpha}(z) L_{m}^{\alpha}(z) z^{\alpha} e^{-z} \mathrm{~d} z=\frac{\Gamma(n+\alpha+1) !}{\Gamma(n+1)} \delta_{n, m} .
$$

Let $\lambda_{n}=\left(\frac{\Gamma(n+1)}{\Gamma(n+\alpha+1)}\right)^{\frac{1}{2}}$ be our normalization constant. We note that the normalized Laguerre polynomials

$$
p_{n}(z)=\lambda_{n} L_{n}^{\alpha}(t-Y)
$$

form an orthonormal OP family with respect to measure $\omega=(t-Y)^{\alpha} e^{-(t-Y)} \mathbb{1}_{(-\infty, t)}$ for a fixed $\alpha$ and tilting $\chi=(t-Y)^{\alpha} \exp \left(-\frac{1-\beta}{2}(t-Y)\right) \mathbb{1}_{(-\infty, t)}$ for a fixed $\beta$.

We use the following result from [24]:

Theorem 9. Let $p_{n}(z)$ be defined as in 40). Then

$$
\dot{x}_{n}(t)=-\mathbf{A} x(t)+\mathbf{b} u(t)
$$

where

$$
\begin{gathered}
\mathbf{A}[n, k]= \begin{cases}\frac{1+\beta}{2} & \text { if } k=n \\
1 & \text { if } k<n, \\
0 & \text { otherwise }\end{cases} \\
\mathbf{b}[n]=\lambda_{n}\left(\begin{array}{c}
n+\alpha \\
n
\end{array}\right),
\end{gathered}
$$

We now show that $\mathbf{A}$ as defined in Theorem 9 is 1-quasiseperable.

Lemma D.11. Let $\mathbf{A}$ be defined as in Theorem 9. Then $\mathbf{A}$ is 1-quasiseperable.

Proof. From Theorem 9, we know that

$$
\begin{gathered}
\mathbf{A}[n, k]= \begin{cases}\frac{1+\beta}{2} & \text { if } k=n \\
1 & \text { if } k<n, \\
0 & \text { otherwise }\end{cases} \\
\mathbf{b}[n]=\lambda_{n}\left(\begin{array}{c}
n+\alpha \\
n
\end{array}\right) .
\end{gathered}
$$

Below the diagonal, all entries $\mathbf{A}[n, k]=1$. Then any submatrix below the diagonal has rank 1. Similarly, above the diagonal, all entries $\mathbf{A}[n, k]=0$. Then any submatrix above the diagonal also has rank 1 . Then by Definition 4, the claim follows.

## E LSSL Algorithms

- Appendix E. 1 proves Theorem 2, providing an algorithm to compute the Krylov function efficiently for LSSLs.
- Appendix E.2 shows a further simplification of Corollary 4.1 presenting an even simpler class of structured matrices that we use in our implementation of LSSL.
- Appendix E. 3 provides technical details of the implementation of LSSL, in particular for computing the MVM black box (multiplication by $\bar{A}$ ) and for computing gradients during backpropagation.


### E. 1 Proof of Theorem 2

This section addresses the computational aspects of the LSSL. In particular, we prove Theorem 2 for the computational speed of computing the Krylov function (7) for quasiseparable matrices $A$, by providing a concrete algorithm in Appendix E.1.1

We restate the Krylov function (7) here for convenience. Recall that $L$ is the length of the input sequence and $N$ is the order of the LSSL internate state, e.g. $A \in \mathbb{R}^{N \times N}$.

$$
\mathcal{K}_{L}(A, B, C)=\left(C A^{i} B\right)_{i \in[L]} \in \mathbb{R}^{L}=\left(C B, C A B, \ldots, C A^{L-1} B\right)
$$

Remark E.1. We call (7) the Krylov function following the notation of [17], since it can be written $\mathcal{K}(A, B)^{T} C$ where $\mathcal{K}(A, B)$ is the Krylov matrix defined in (8). Alternative naming suggestions are welcome.

#### E.1.1 The Algorithm

We follow the similar problem of [17, Lemma 6.6] but track the dependence on $L$ and the log factors more precisely, and optimize it in the case of stronger structure than quasiseparability, which holds in our setting (particularly Theorem 11).

The first step is to observe that the Krylov function $\mathcal{K}_{L}(A, B, C)$ is actually the coefficient vector of $C(I-A x)^{-1} B\left(\bmod x^{L}\right)$ as a polynomial in $x$. (Note that $A x$ means simply multiplying every entry in $A$ by a scalar variable $x$.) This follows from expanding the power series $(I-A x)^{-1}=I+A x+A^{2} x^{2}+\ldots$ Thus we first compute $C(I-A x)^{-1} B$, which is a rational function of degree at most $N$ in the numerator and denominator (which can be seen by the standard adjoint formula for the matrix inverse).

The second step is simply inverting the denominator of this rational function $\left(\bmod x^{L}\right)$ and multiplying by the numerator, both of which are operations that need $L \log (L)$ time by standard results for polynomial arithmetic 51].

For the remainder of this section, we focus on computing the first part. We make two notational changes: First, we transpose $\mathbf{C}$ to make it have the same shape as $\mathbf{B}$. We consider the more general setting where $\mathbf{B}$ and $\mathbf{C}$ have multiple columns; this can be viewed as handling a "batch" problem with several queries for B, C at the same time.

Lemma E.2. Let $\mathbf{A}$ be a q-quasiseparable matrix. Then

$$
\mathbf{C}^{T}(\mathbf{I}-\mathbf{A} x)^{-1} \mathbf{B} \quad \text { where } \mathbf{A} \in \mathbb{R}^{N \times N} \quad \text { B, } \mathbf{C} \in \mathbb{R}^{N \times k}
$$

is a $k \times k$ matrix of rational functions of degree at most $N$, which can be computed in $O\left(q^{3} \log ^{4} N\right)$ operations.

The main idea is that quasiseparable matrices are recursively "self-similar", in that the principal submatrices are also quasiseparable, which leads to a divide-and-conquer algorithm. In particular, divide $\mathbf{A}=\left[\begin{array}{ll}\mathbf{A}_{00} & \mathbf{A}_{01} \\ \mathbf{A}_{10} & \mathbf{A}_{11}\end{array}\right]$ into quadrants. Then by Definition $4, \mathbf{A}_{00}, \mathbf{A}_{11}$ are both $q$-quasiseparable and $\mathbf{A}_{01}, \mathbf{A}_{10}$ are $\operatorname{rank} q$. Therefore the strategy is to view $\mathbf{I}-\mathbf{A} x$ as a low-rank perturbation of smaller quasiseparable matrices and reduce the problem to a simpler one.

Proposition 10 (Binomial Inverse Theorem or Woodbury matrix identity 23, 60). Over a commutative ring $\mathcal{R}$, let $\mathbf{A} \in \mathcal{R}^{N \times N}$ and $\mathbf{U}, \mathbf{V} \in \mathcal{R}^{N \times p}$. Suppose $\mathbf{A}$ and $\mathbf{A}+\mathbf{U V}^{T}$ are invertible. Then $\mathbf{I}_{p}+\mathbf{V}^{T} \mathbf{A}^{-1} \mathbf{U} \in \mathcal{R}^{p \times p}$ is invertible and

$$
\left(\mathbf{A}+\mathbf{U} \mathbf{V}^{T}\right)^{-1}=\mathbf{A}^{-1}-\mathbf{A}^{-1} \mathbf{U}\left(\mathbf{I}_{p}+\mathbf{V}^{T} \mathbf{A}^{-1} \mathbf{U}\right)^{-1} \mathbf{V}^{T} \mathbf{A}^{-1}
$$

For our purposes, $\mathcal{R}$ will be the ring of rational functions over $\mathbb{R}$.

Proof of Lemma E.2. Since $\mathbf{A}$ is $q$-quasiseparable, we can write $\mathbf{A}_{10}=\mathbf{U}_{L} \mathbf{V}_{L}^{T}$ and $\mathbf{A}_{01}=\mathbf{U}_{U} \mathbf{V}_{U}^{T}$ where $\mathbf{U}, \mathbf{V} . \in \mathbb{F}^{N \times q}$. Notice that we can write $\mathbf{I}-\mathbf{A} x$ as

$$
\mathbf{I}-\mathbf{A} x=\left[\begin{array}{cc}
\mathbf{I}-\mathbf{A}_{00} x & \mathbf{0} \\
\mathbf{0} & \mathbf{I}-\mathbf{A}_{11} x
\end{array}\right]+\left[\begin{array}{cc}
\mathbf{0} & \mathbf{U}_{U} \\
\mathbf{U}_{L} & \mathbf{0}
\end{array}\right]\left[\begin{array}{cc}
\mathbf{V}_{L} & \mathbf{0} \\
\mathbf{0} & \mathbf{V}_{U}
\end{array}\right]^{T} x
$$

Suppose we know the expansions of each of

$$
\begin{aligned}
& \mathbf{M}_{1} \in \mathcal{R}^{k \times k}=\mathbf{C}^{T}\left[\begin{array}{cc}
\mathbf{I}-\mathbf{A}_{00} x & \mathbf{0} \\
\mathbf{0} & \mathbf{I}-\mathbf{A}_{11} x
\end{array}\right]^{-1} \mathbf{B} \\
& \mathbf{M}_{2} \in \mathcal{R}^{k \times 2 q}=\mathbf{C}^{T}\left[\begin{array}{cc}
\mathbf{I}-\mathbf{A}_{00} x & \mathbf{0} \\
\mathbf{0} & \mathbf{I}-\mathbf{A}_{11} x
\end{array}\right]^{-1}\left[\begin{array}{cc}
\mathbf{0} & \mathbf{U}_{U} \\
\mathbf{U}_{L} & \mathbf{0}
\end{array}\right] \\
& \mathbf{M}_{3} \in \mathcal{R}^{2 q \times 2 q}=\left[\begin{array}{cc}
\mathbf{V}_{L} & \mathbf{0} \\
\mathbf{0} & \mathbf{V}_{U}
\end{array}\right]^{T}\left[\begin{array}{cc}
\mathbf{I}-\mathbf{A}_{00} x & \mathbf{0} \\
\mathbf{0} & \mathbf{I}-\mathbf{A}_{11} x
\end{array}\right]^{-1}\left[\begin{array}{cc}
\mathbf{0} & \mathbf{U}_{U} \\
\mathbf{U}_{L} & \mathbf{0}
\end{array}\right] \\
& \mathbf{M}_{4} \in \mathcal{R}^{2 q \times k}=\left[\begin{array}{cc}
\mathbf{V}_{L} & \mathbf{0} \\
\mathbf{0} & \mathbf{V}_{U}^{T}
\end{array}\right]^{T}\left[\begin{array}{cc}
\mathbf{I}-\mathbf{A}_{00} x & \mathbf{0} \\
\mathbf{0} & \mathbf{I}-\mathbf{A}_{11} x
\end{array}\right]^{-1} \mathbf{B} .
\end{aligned}
$$

By Proposition 10, the desired answer is

$$
\mathbf{C}^{T}(X-\mathbf{A})^{-1} \mathbf{B}=\mathbf{M}_{1}-\mathbf{M}_{2}\left(\mathbf{I}_{2 q}+\mathbf{M}_{3}\right)^{-1} \mathbf{M}_{4}
$$

Then the final result can be computed by inverting $\mathbf{I}_{2 t}+\mathbf{M}_{3}\left(O\left(q^{3} N \log (N)\right)\right.$ operations), multiplying by $\mathbf{M}_{2}, \mathbf{M}_{4}\left(O\left(\left(k q^{2}+k^{2} q\right) N \log (N)\right)\right.$ operations), and subtracting from $\mathbf{M}_{1}\left(O\left(k^{2} N \log (N)\right)\right.$ operations). This is a total of $O\left(\left(q^{3}+k q^{2}+k^{2} q\right) N \log (N)\right)$ operations. Note that when $k=O(q \log N)$, this becomes $O\left(q^{3} N \log ^{3} N\right)$; we will use this in the analysis shortly.

To compute $\mathbf{M}_{1}, \mathbf{M}_{2}, \mathbf{M}_{3}, \mathbf{M}_{4}$, it suffices to compute the following:

$$
\begin{array}{cc}
\mathbf{C}_{1}^{T}\left(\mathbf{I}-\mathbf{A}_{00} x\right)^{-1} \mathbf{B}_{0} & \mathbf{C}_{1}^{T}\left(\mathbf{I}-\mathbf{A}_{11} x\right)^{-1} \mathbf{B}_{1} \\
\mathbf{C}_{0}^{T}\left(\mathbf{I}-\mathbf{A}_{00} x\right)^{-1} \mathbf{U}_{U} & \mathbf{C}_{1}^{T}\left(\mathbf{I}-\mathbf{A}_{11} x\right)^{-1} \mathbf{U}_{L} \\
\mathbf{V}_{L}^{T}\left(\mathbf{I}-\mathbf{A}_{00} x\right)^{-1} \mathbf{U}_{U} & \mathbf{V}_{U}^{T}\left(\mathbf{I}-\mathbf{A}_{11} x\right)^{-1} \mathbf{U}_{L} \\
\mathbf{V}_{L}^{T}\left(\mathbf{I}-\mathbf{A}_{00} x\right)^{-1} \mathbf{B}_{0} & \mathbf{V}_{U}^{T}\left(\mathbf{I}-\mathbf{A}_{11} x\right)^{-1} \mathbf{B}_{1}
\end{array}
$$

But to compute those, it suffices to compute the following $(k+t) \times(k+t)$ matrices:

$$
\begin{aligned}
& {\left[\begin{array}{ll}
\mathbf{C}_{0} & \mathbf{V}_{L}
\end{array}\right]^{T}\left(\mathbf{I}-\mathbf{A}_{00} x\right)^{-1}\left[\begin{array}{ll}
\mathbf{B}_{0} & \mathbf{U}_{U}
\end{array}\right]} \\
& {\left[\begin{array}{ll}
\mathbf{C}_{1} & \mathbf{V}_{U}
\end{array}\right]^{T}\left(\mathbf{I}-\mathbf{A}_{11} x\right)^{-1}\left[\begin{array}{ll}
\mathbf{B}_{1} & \mathbf{U}_{L}
\end{array}\right]}
\end{aligned}
$$

Since $\mathbf{A}_{00}$ and $\mathbf{A}_{11}$ have the same form as $\mathbf{A}$, this is two recursive calls of half the size. Notice that the size of the other input (dimensions of $\mathbf{B}, \mathbf{C}$ ) is growing, but when the initial input is $k=1$, it never exceeds $1+q \log N$ (since they increase by $q$ every time we go down a level). Earlier, we noticed that when $k=O(q \log N)$, the reduction step has complexity $O\left(q^{3} N \log ^{3}(N)\right)$ for any recursive call. The recursion adds an additional $\log N$ multiplicative factor on top of this.

Corollary E.3. Suppose that A is semiseparable instead of quasiseparable, and suppose $q$ is a small constant. Then the cost of Lemma E. 2 is $O\left(N \log ^{2}(N)\right)$ operations.

This follows from the fact that in the recursion 45 and 46 , the $\mathbf{U}, \mathbf{V}$ matrices do not have to be appended if they already exist in $\mathbf{B}, \mathbf{C}$. For intuition, this happens in the case when $\mathbf{A}$ is tridiagonal, so that $U, V$ have the structure $(1,0, \ldots, 0)$, or the case when the off-diagonal part of $\mathbf{A}$ is all 1 (such as the HiPPO-LegT matrix). The matrices in Appendix D.3 and Appendix E. 2 (Theorems 8, 9 and 11) actually satisfy this stronger structure, so Corollary E. 3 applies.

Combining everything, this proves Theorem 2 with the exact bound $N \log ^{2}(N)+L \log (L)$ operations. The memory claim follows similarly, and the depth of the algorithm is $\log ^{2}(N)+\log (L)$ from the divide-and-conquer recursions.

Table 7: Complexity of various sequence models in terms of length (L), batch size (B), and hidden dimension (H). Measures are parameter count, training computation, memory requirement, and inference computation for 1 sample and time-step.

|  | Convolution | RNN |
| :--- | :--- | :--- |
| Parameters | $L H^{2}$ | $H^{2}$ |
| Training | $B L H^{2}+L \log (L)\left(H^{2}+B H\right)$ | $B L H^{2}$ |
| Memory | $B L H+L H^{2}$ | $B L H$ |
| Parallel | Yes | No |
| Inference | $L H^{2}$ | $H^{2}$ |
|  | Attention | LSSL-naive |
| Parameters | $H^{2}$ | $H N+N^{2}$ |
| Training | $B\left(L^{2} H+L H^{2}\right)$ | $H N^{3}+L H N^{2}+B L \log (L) H N$ |
| Memory | $B\left(L^{2}+H L\right)$ | $H N^{2}+L H N+B L H$ |
| Parallel | Yes | Yes |
| Inference | $L^{2} H+H^{2} L$ | $H N^{2}$ |
|  | LSSL-fixed | LSSL |
| Parameters | $H N$ | $H N$ |
| Training | $B L \log (L) H N$ | $B H\left(N \log ^{2} N+L \log L\right)+B L \log (L) H$ |
| Memory | $L H N+B L H$ | $B H L$ |
| Parallel | Yes | Yes |
| Inference | $H N^{2}$ | $H N$ |

#### E.1.2 Summary of Computation Speed for LSSLs and other Mechanisms

We provide a summary of complexity requirements for various sequence model mechanisms, including several versions of the LSSL. Note that these are over exact arithmetic as in Theorem 2

First, the self-attention mechanism is another common sequence model that has an $L^{2}$ dependence on the length of the sequence, so it is not suitable for the very long sequences we consider here. (We do note that there is an active line of work on reducing this complexity.)

Second, we include additional variants of the LSSL. In Table 7. LSSL-naive denotes learning $A$ and $\Delta t$ for unstructured $A$; LSSL-fixed denotes not learning $A, \Delta t$ (see Appendix B for details); LSSL denotes the learning $A$ and $\Delta t$ for the structured class $A$.

We include brief explanations of these complexities for the LSSL variants.

##### LSSL-naive

- Parameters: $O(H N)$ in the matrices $B, C$ and $O\left(N^{2}\right)$ in the matrix $A$.
- Training: $O\left(H N^{3}\right)$ to invert compute the matrix $\bar{A}$ for all $H$ features. $O\left(L H N^{2}\right)$ to compute the Krylov matrix $C, C A, \ldots O(B L \log (L) H N$ to multiply by $B$ and convolve with $u$.
- Memory: $O\left(H N^{2}\right)$ to store $\bar{A} . O(L H N)$ to store the Krylov matrix. $O(B L H)$ to store the inputs/outputs
- Inference: $O\left(H N^{2}\right)$ to for MVM by $\bar{A}$.


##### LSSL-fixed

- Parameters: $O(H N)$ in the matrices $C$.
- Training: $O(B L \log (L) H)$ to convolve with $u$.
- Memory: $O(L H N)$ to store the Krylov matrix (but cached, so no backprop). $O(B L H)$ for inputs/outputs.
- Inference: $O\left(H N^{2}\right)$ to for MVM by $\bar{A}$.

LSSL

- Parameters: $O(H N)$ for $A, B, C, \Delta t$.
- Training: $B H \cdot \tilde{O}(N+L)$ to compute Krylov, $O(B L \log (L) H)$ for the convolution.
- Memory: $O(B H L)$ to store Krylov (and inputs/outputs).
- Inference: $O(H N)$ to multiply $x_{t}[H, N]$ by $\bar{A}[H, N, N]$


### E. 2 Further Simplification with Tridiagonal Matrices

The algorithm for Theorem 2 for general quasiseparable matrices is still difficult to implement in practice, and we make a further simplification using a particular subclass of quasiseparable matrices.

Theorem 11. The class of $N \times N$ matrices $\mathcal{S}_{N}=\left\{P\left(D+T^{-1}\right) Q\right\}$ with diagonal $D, P, Q$ and tridiagonal $T$ includes the original HiPPO-LegS, HiPPO-LegT, and HiPPO-LagT matrices [24].

Theorem 11 shows that a simple representation involving tridiagonal and diagonal matrices captures all of the original HiPPO matrices. In particular, our LSSL implementation initializes $A$ to be the HiPPO-LegS matrix (Appendix B) and learns within the class defined by Theorem 11

We note that the matrices in Theorem 11 are all 1-quasiseparable and in particular also contain the HiPPO matrices for Gegenbauer and generalized Laguerre orthogonal polynomials derived in Theorem 9 . In fact, the notion of semiseparability, which is closely related to (and actually is the predecessor of) quasiseparability, was originally motivated precisely to capture inverses of tridiagonal matrices. Thus the structured class in Theorem 11 can be viewed as an approximation of 3-quasiseparable matrices (Corollary 4.1) to 1-quasiseparable, which still contains many of the HiPPO families of interest.

Proof. We simply show that each of these specific matrices can be represented in the proposed form.

HiPPO-LegT.

Let $A$ denote the HiPPO-LegT transition matrix. Up to row/column scaling (i.e. left- and rightmultiplication by diagonal $P$ and $Q$ ), we can write

$$
A_{n k}=\left\{\begin{array}{ll}
(-1)^{n-k} & \text { if } n \leq k \\
1 & \text { if } n \geq k
\end{array} .\right.
$$

The main observation is that

$$
A^{-1}=\frac{1}{2}\left[\begin{array}{ccccccc}
1 & 1 & 0 & \ldots & 0 & 0 & 0 \\
-1 & 0 & 1 & \ldots & 0 & 0 & 0 \\
0 & -1 & 0 & \ldots & 0 & 0 & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots & \vdots & \vdots \\
0 & 0 & 0 & \ldots & 0 & 1 & 0 \\
0 & 0 & 0 & \ldots & -1 & 0 & 1 \\
0 & 0 & 0 & \ldots & 0 & -1 & 1
\end{array}\right]
$$

HiPPO-LegS. The HiPPO-LegS matrix is

$$
A_{n k}=- \begin{cases}(2 n+1)^{1 / 2}(2 k+1)^{1 / 2} & \text { if } n>k \\ n+1 & \text { if } n=k \\ 0 & \text { if } n<k\end{cases}
$$

This can be written as $-P A^{\prime} Q$ where $P=Q=\operatorname{diag}\left((2 n+1)^{\frac{1}{2}}\right)$ and

$$
A_{n k}^{\prime}= \begin{cases}1 & \text { if } n>k \\ 0 & \text { if } n<k \\ 1-\frac{n}{2 n+1} & \text { if } n=k\end{cases}
$$

Finally, $A^{\prime}=D+T^{-1}$ where $D=-\operatorname{diag}\left(\frac{n}{2 n+1}\right)$ and $T$ is the matrix with 1 on the main diagonal and -1 on the subdiagonal.

HiPPO-LagT. The HiPPO-LagT matrix is

$$
A_{n k}=- \begin{cases}1 & \text { if } n>k \\ 0 & \text { if } n<k \\ \frac{1}{2} & \text { if } n=k\end{cases}
$$

This can be written as $-P\left(D+T^{-1}\right) Q$ where $P=Q=I, D=-\frac{1}{2} I$, and $T$ is the same tridiagonal matrix as in the HiPPO-LegS case.

### E. 3 Implementation Details

In this section we provide several implementation details that are useful for implementing LSSLs in practice.

Recall that one of the main primitives of LSSLs is the matrix-vector multiplication $y=\bar{A} x$ (Section 3.1, Appendix B), where $\bar{A}$ is the state matrix $A$ discretized with step size $\Delta t$ using the bilinear method (Appendix C.1.3). In Appendix E.3.1, we describe how this MVM can be performed with simpler MVM primitives which we call the "forward difference" and "backward difference".

However, if these MVM primitives are implemented in a specialized way for particular classes of $A$ matrices (i.e., not using atoms in a standard autograd framework), then we also need to calculate several additional gradients by hand. Appendix E.3.2 shows that calculating gradients to $A, \Delta t, x$ during backpropagation can actually be reduced to those same forward/backward difference primitives.

Finally, in the case when $A$ is the structured class of matrices in Theorem 11. Appendix E. 2 shows how to efficiently calculate those primitives using a black-box tridiagonal solver. Our code[^1] implements all the algorithms in this section, with bindings to the cuSPARSE library for efficient tridiagonal solving on GPU.

#### E.3.1 Matrix-vector Multiplication by the Bilinear Discretization

Bilinear Discretization The discrete state-space system is given by (4) and (5), re-written here for convenience

$$
\begin{aligned}
x_{t} & =\bar{A} x_{t-1}+\bar{B} u_{t} \\
y_{t} & =C x_{t}+D u_{t}
\end{aligned}
$$

where $\bar{A}$ is a function of $A, \delta_{t}$ and $\bar{A}$ is a function of $A, B, \delta_{t}$. In particular, we define $\bar{A}$ to be the matrix discretized using the bilinear method (Appendix C.1.3), and the system can be written explicitly:

$$
\begin{aligned}
& x_{t}=\left(I-\frac{\Delta t A}{2}\right)^{-1}\left(\left(I+\frac{\Delta t A}{2}\right) x_{t-1}+\Delta t B u_{t}\right) \\
& y_{t}=C x_{t}+D u_{t}
\end{aligned}
$$

Thus it suffices to compute the maps

$$
F(A, \Delta t, x):=(I+\Delta t A) x
$$

and

$$
B(A, \Delta t, x):=(I+\Delta t A)^{-1} x
$$

We will call these functions the forward difference and backward difference maps, respectively. (The Euler and backward Euler discretizations (Appendix C.1.2 are also known as the "forward difference" and "backward difference" methods, which in the case of linear systems reduces down to the maps $F$ and $B$.)

#### E.3.2 Gradients through the Forward/Backward Difference Primitives

In this section we will let $y=F(A, \Delta t, x)$ or $y=B(A, \Delta t, x)$ denote the computation of interest, $L(y)$ denote a generic loss function, and $d x, d y, \ldots$ denote gradients to $x, y, \ldots$ (e.g., $d x=\frac{\partial L(y)}{\partial x}$ ).

Derivatives of backward difference. First we have the standard $\frac{\partial L(y)}{\partial x}=\frac{\partial L(y)}{\partial y} \frac{\partial y}{\partial x}=\frac{\partial L(y)}{\partial y}(I+\Delta t A)^{-1}$. This corresponds to matrix-vector multiplication by $(I+\Delta A)^{-T}$. In other words, it can be computed by the primitive $B\left(A^{T}, \Delta t, d y\right)$.

Similarly, in order to compute $\frac{\partial L(y)}{\partial \Delta t}$ we require $\frac{\partial y}{\partial \Delta t}$. We need the result $\frac{\partial Y^{-1}}{\partial x}=-Y^{-1} \frac{\partial Y}{\partial x} Y^{-1}$ for an invertible matrix $Y$ [41, equation (59)]. Then

$$
\begin{aligned}
\frac{\partial y}{\partial \Delta t} & =\frac{\partial(I+\Delta t A)^{-1}}{\partial \Delta t} x \\
& =-(I+\Delta t A)^{-1} \frac{\partial(I+\Delta t A)}{\partial \Delta t}(I+\Delta t A)^{-1} x \\
& =-(I+\Delta t A)^{-1} A(I+\Delta t A)^{-1} x
\end{aligned}
$$

and

$$
\begin{aligned}
\frac{\partial L(y)}{\partial \Delta t} & =\frac{\partial L(y)}{\partial y} \frac{\partial y}{\partial \Delta t} \\
& =-\left[\frac{\partial L(y)}{\partial y}(I+\Delta t A)^{-1}\right] A\left[(I+\Delta t A)^{-1} x\right]
\end{aligned}
$$

We can summarize this as follows. Let $y=B(A, \Delta t, x)=(I+\Delta t A)^{-1} x$ and $d y=\partial L(y) / \partial y$ (as a column vector). Then

$$
\begin{aligned}
y & =B(A, \Delta t, x) \\
d x & =B\left(A^{T}, \Delta t, d y\right) \\
d \Delta t & =-d x^{T} A y .
\end{aligned}
$$

Derivatives of forward difference. The forward case is simpler. Let $y=F(A, \Delta t, x)=(I+\Delta t A) x$. Then $\frac{\partial y}{\partial x}=I+\Delta t A$ and $\frac{\partial y}{\partial \Delta t}=A x$. Thus

$$
\begin{aligned}
y & =F(A, \Delta t, x) \\
d x & =(I+\Delta t A)^{T} d y=F\left(A^{T}, \Delta t, d y\right) \\
d \Delta t & =d y^{T} A x
\end{aligned}
$$

#### E.3.3 Computing the Forward/Backward Difference for Tridiagonal Inverse Matrices

Theorem 11 uses the classes of matrices $A=P\left(D+T^{-1}\right) Q$ for diagonal $D, P, Q$ and tridiagonal $T$. We describe how the forward and backward difference MVMs can be performed efficiently for this class of matrices by reducing to a black-box tridiagonal solver.

Table 8: Test accuracies for irregularly sampled time series on the CharacterTrajectories dataset. $p \%$ denotes percent of data that was randomly dropped.

| Model | $0 \%$ | $30 \%$ | $50 \%$ | $70 \%$ |
| :--- | :--- | :--- | :--- | :--- |
| GRU-ODE [16] | - | 92.6 | 86.7 | 89.9 |
| GRU- $\Delta t$ [31] | - | 93.6 | 91.3 | 90.4 |
| GRU-D [9] | - | 94.2 | 90.2 | 91.9 |
| ODE-RNN [45] | - | 95.4 | 96.0 | 95.3 |
| NCDE [31] | - | 98.7 | 98.8 | $\mathbf{9 8 . 6}$ |
| CKCNN [44] | $\mathbf{9 9 . 5 3}$ | $\mathbf{9 8 . 8 3}$ | 98.60 | 98.14 |
| LSSL | 99.30 | $\mathbf{9 8 . 8 3}$ | $\mathbf{9 8 . 8 3}$ | 98.37 |

Table 9: $A$ and $\Delta t$ ablations on sCIFAR.

|  | Learn $\Delta t$ | Fixed $\Delta t$ |
| :--- | :--- | :--- |
| Learn $A$ | 82.70 | 80.34 |
| Fixed $A$ | 80.61 | 80.18 |

Forward difference. It is straightforward to compute

$$
F(A, \Delta t, x)=\left(I+\Delta t \cdot P\left(D+T^{-1}\right) Q\right) x=x+\Delta t \cdot P D Q x+\Delta t \cdot P T^{-1} Q x
$$

in terms of multiplication by diagonal matrices $x \mapsto D x$ and tridiagonal solving $x \mapsto T^{-1} x$.

Backward difference. We will explicitly rewrite the inverse of the matrix $G=I+\Delta t \cdot P\left(D+T^{-1}\right) Q$. The core observation is to multiply $G$ by a choice selection of matrices to cancel out the $T^{-1}$ term:

$$
T P^{-1} G Q^{-1}=T P^{-1} Q^{-1}+\Delta t T D+\Delta t I .
$$

Rearranging yields

$$
G^{-1}=Q^{-1}\left(T P^{-1} Q^{-1}+\Delta t T D+\Delta t I\right)^{-1} T P^{-1} .
$$

Now note that the matrix in the middle is tridiagonal. Hence we have reduced MVM by $G^{-1}$, i.e. the backward difference problem, to a series of diagonal and tridiagonal MVMs (easy), and a tridiagonal inverse MVM (a.k.a. a tridiagonal solve).

## F Additional Experiments and Experiment Details

We provide additional experiments and ablations in Appendix F. 1 Appendix F. 2 describes our training methodology in more detail for each dataset. The hyperparameters for all reported results are in Table 11.

### F. 1 Additional Experiments

Missing Data on CharacterTrajectories. Table 8 has results for a setting considered in previous work involving irregularly-sampled time series. LSSL is competitive with the best prior methods, some of which were specialized to handle this setting.

$A$ and $\Delta t$ ablations. Tables 9 and 10 show results on SpeechCommands-Raw and a smaller model on sCIFAR, ablating that learning either the $A$ or $\Delta t$ parameters provides a consistent performance increase.

Finally, Fig. 2 plots the $\Delta t$ values at the beginning and end of training on the SpeechCommands-Raw dataset, confirming that training $\Delta t$ does noticeably change their values to better model the data. In particular, the $\Delta t$ values spread over time to cover a larger range of timescales.

Table 10: $A$ and $\Delta t$ ablations on SC-Raw.

|  | Learn $\Delta t$ | Fixed $\Delta t$ |
| :--- | :--- | :--- |
| Learn $A$ | 96.07 | 95.20 |
| Fixed $A$ | 91.59 | 90.51 |

### F. 2 Methodology

We describe our training procedure on each dataset for our model and any relevant baselines.

General All models and datasets used the Adam optimizer with a LR decay scheduler that reduced LR by $5 \mathrm{x}$ upon validation plateau for 10 or 20 epochs. We fixed the batch size to 50 for the MNIST/CIFAR datasets and 32 for other datasets, reducing if necessary to fit in memory.

For all models, we chose the hyperparameters that achieved the highest validation accuracy/RMSE (values in Table 11).

Error Bars We note that the results in Section 5 do not include standard deviations for formatting reasons, since most of the baselines were best results reported in previous papers without error bars. As Section 6 noted, the LSSL was actually quite stable in performance and not particularly sensitive to hyperparameters. We note that for every result in Section 5, the LSSL with error bars was at least one standard deviation above the baseline results.

#### F.2.1 Sequential and Permuted MNIST

The model architecture of LSSL(-f) was fixed to the small architecture with 200K parameters (Appendix B). Following [44], we fixed the learning rate scheduler to decay on plateau by with a factor of 0.2 , and the number of epochs to 200 . We searched hyperparameters over the product of the following learning rate values: $\{0.001,0.002,0.004,0.01\}$, and dropout values: $\{0.1,0.2\}$.

#### F.2.2 Sequential CIFAR

The model architecture of LSSL(-f) was fixed to the large architecture with 2M parameters (Appendix B). We searched over the product of the following learning rate values: $\{0.001,0.002,0.004,0.01,0.02\}$, and dropout values: $\{0.2,0.3,0.4\}$.

#### F. 2.3 BIDMC Healthcare

The BIDMC tasks aim at predicting three vital signs of a patient, respiratory rate (RR), heart rate (HR), and oxygen saturation (SpO2), based on PPG and ECG signals. The clinical data is provided by the Beth Israel Deaconess Medical Center. The PPG and ECG signals were sampled at $125 \mathrm{~Hz}$ and have a sequence length of 4000.

For this dataset, we fixed the small LSSL(-f) model (Appendix B). Following [47], we changed the scheduler to a multistep scheduler that decays on fixed epochs, and trained for 500 epochs.

For our methods, we searched over the product of the following learning rate values: $\{0.004,0.01,0.02\}$, and dropout values: $\{0.1,0.2\}$.

Baseline parameters. For CKConv, we searched over $\omega_{0} \in[10,50]$ following the guidelines of Romero et al. [44] (best value $\omega_{0}=20$ ). Since we tuned the sensitive $\omega_{0}$, we fixed the learning rate to 0.001 and dropout to 0.1 which was the default used in 44 .

The transformer model we used was a vanilla transformer with a hidden dimension of 256,8 attention heads, 4 layers, and a feedforward dimension of 1024. We used a learning rate of 0.001 and a dropout of 0 . We tried a few variants, but no transformer model was effective at all.

![](https://cdn.mathpix.com/cropped/2024_01_04_44babd86fb78d9e96991g-45.jpg?height=1164&width=682&top_left_y=282&top_left_x=710)

Figure 2: We visualize the 32 largest and smallest $\Delta t$ values at the start and end of training for the first layer of our state-of-the-art LSSL model on the Speech Commands Raw dataset. The plots visualize $\frac{1}{\Delta t}$, which can be interpreted as the timescale at which they operate (Section 2). The plots confirm that LSSL does modify the dt values in order to more appropriately model the speech data.

#### F.2.4 CelebA

For these larger datasets, we reduced the size of the order $N$ and did not tie it to $H$. These experiments were computationally heavy and we did not do any tuning (i.e., Table 11 are the only runs). The model size was picked to train in a reasonable amount of time, and the learning rate for the first attribute was picked based on general best hyperparameters for other datasets, and then reduced for subsequent experiments on the other attributes.

Baseline parameters. For ResNet-18, we used the standard implementation with a learning rate of 0.001 .

#### F.2.5 Speech Commands

For Speech Commands, we use the same dataset and preprocessing code from Kidger et al. 31, Romero et al. [44]. We consider the two settings from Kidger et al. [31]: SC-Raw uses very long time-series raw speech signals of 16000 timesteps each, while SC-MFCC uses standard MFCC features of 161 timesteps.

For our models trained over the raw data, we searched over the product of the following learning rate values: $\{0.002,0.004,0.01\}$, and dropout values: $\{0.1,0.2\}$. For our models trained over the MFCC features, we searched over the product of the following learning rate values: $\{0.0001,0.001,0.002,0.004,0.01\}$, and dropout values: $\{0.1,0.2,0.3,0.4\}$.

Table 11: The values of the best hyperparameters found for each dataset.

| Dataset | Hyperparameters |  |  |  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
|  | Learning Rate | Dropout | Batch Size | Epochs | Depth | Hidden Size $H$ | Order $N$ | Channels $M$ |
| sMNIST | 0.004 | 0.2 | 50 | 200 | 6 | 128 | 128 | 1 |
| pMNIST | 0.001 | 0.2 | 50 | 200 | 6 | 128 | 128 | 1 |
| sCIFAR | 0.02 | 0.3 | 50 | 200 | 4 | 256 | 256 | 4 |
| BIDMC-RR | 0.004 | 0.1 | 32 | 500 | 6 | 128 | 128 | 1 |
| BIDMC-HR | 0.01 | 0.2 | 32 | 500 | 6 | 128 | 128 | 1 |
| BIDMC-SpO2 | 0.01 | 0.1 | 32 | 500 | 6 | 128 | 128 | 1 |
| SC Raw | 0.01 | 0.2 | 16 | 50 | 4 | 256 | 128 | 2 |
| SC MFCC | 0.004 | 0.4 | 32 | 100 | 6 | 128 | 128 | 1 |
| SCelebA-Att. | 0.002 | 0.1 | 32 | 200 | 3 | 256 | 128 | 4 |
| sCelebA-MSO | 0.002 | 0.1 | 32 | 200 | 3 | 256 | 128 | 4 |
| sCelebA-Smil. | 0.01 | 0.1 | 32 | 200 | 3 | 256 | 128 | 4 |
| SCelebA-WL | 0.002 | 0.1 | 32 | 200 | 3 | 256 | 128 | 4 |

Baseline parameters. To get more results for the strongest baselines on very long sequences in the literature, we ran the UniCORNN [47] baseline on both Raw and MFCC variants, and the Neural Rough Differential Equations 37 baseline on the Raw variant.

For UniCORNN trained over the raw data, we searched over multiple hyperparameters. Specifically, we searched over alpha: $\{0,10,20,30,40\}, \Delta t$ values: $\{0.00001,0.0001,0.001,0.01\}$, and learning rate values: $\{0.0001,0.0004,0.001,0.004\}$. However, since the method was not able to generalize to the validation set for any hyperparameter combination, we used the authors' reported hyperparameters for the Eigenworms dataset as it also contains very long sequences $(\approx 18000)$. In particular, we used a learning rate of 0.02 , hidden dimension of 256,3 layers with dt values [0.0000281,0.0343,0.0343], dropout of 0.1 , and alpha of 0 .

For UniCORNN trained over the MFCC features, we used the authors' reported hyperparameters for the MNIST dataset (again due to similarly sized sequence lengths), and further tuned the learning rate over the values: $\{0.0001,0.001,0.005,0.01,0.02\}, \Delta t$ values: $\{0.01,0.1\}$, and alpha values: $\{10,20,30\}$.

The best model used a learning rate of 0.02 , hidden dimension of 256,3 layers with dt values of 0.19 , dropout of 0.1 , and alpha of 30.65 .

For NRDE on SC-Raw, we used depth 2, step size 4, hidden dimension 32, and 3 layers. Our results were better than unofficial numbers reported in correspondence with the authors, so we did not tune further.

#### F.2.6 Convergence Speed (Table 5)

The convergence table compared against logs directly from the corresponding baseline's SoTA models [44, 47], which were either released publicly or found in direct correspondence with the authors. To generate the wall clock numbers, we ran the baseline models on the same hardware as our models and extrapolated to the target epoch.

### F. 3 Hyperparameters

Best hyperparameters for all datasets are reported in Table 11.


[^0]:    To be precise, the measures that correspond to orthogonal polynomials 52 .

[^1]:    Available at https://github.com/HazyResearch/state-spaces

