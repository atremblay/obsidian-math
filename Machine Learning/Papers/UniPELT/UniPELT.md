---
title: "UniPELT: A Unified Framework for Parameter-Efficient Language Model Tuning"
authors:
    - Yuning Mao 
    - Lambert Mathias
    - Rui Hou 
    - Amjad Almahairi
    - Hao Ma
    - Jiawei Han
    - Wen-tau Yih
    - Madian Khabsa
url:
    - "[paper](https://arxiv.org/pdf/2110.07577.pdf)"
    - "[arxiv](https://arxiv.org/abs/2110.07577)"
tags:
    - plm
    - peft
published: "2021-10-14"
status: "to read"
---

# UniPELT: A Unified Framework for Parameter-Efficient Language Model Tuning
###### Authors
<ul>
<li class="author">Yuning Mao</li>
<li class="separator author">|</li>
<li class="author">Lambert Mathias</li>
<li class="separator author">|</li>
<li class="author">Rui Hou</li>
<li class="separator author">|</li>
<li class="author">Amjad Almahairi</li>
<li class="separator author">|</li>
<li class="author">Hao Ma</li>
<li class="separator author">|</li>
<li class="author">Jiawei Han</li>
<li class="separator author">|</li>
<li class="author">Wen-tau Yih</li>
<li class="separator author">|</li>
<li class="author">Madian Khabsa</li>
</ul>

```dataview
table without ID url from "Mathematics/Machine Learning/Papers/UniPELT/UniPELT.md"
```


#### Abstract

Recent parameter-efficient language model tuning (PELT) methods manage to match the performance of fine-tuning with much fewer trainable parameters and perform especially well when training data is limited. However, different PELT methods may perform rather differently on the same task, making it nontrivial to select the most appropriate method for a specific task, especially considering the fast-growing number of new PELT methods and tasks. In light of model diversity and the difficulty of model selection, we propose a unified framework, UniPELT, which incorporates different PELT methods as submodules and learns to activate the ones that best suit the current data or task setup via gating mechanism. On the [[../GLUE|GLUE]] benchmark, UniPELT consistently achieves 1 ~ 4% gains compared to the best individual PELT method that it incorporates and outperforms fine-tuning under different setups. Moreover, UniPELT generally surpasses the upper bound that takes the best performance of all its submodules used individually on each task, indicating that a mixture of multiple PELT methods may be inherently more effective than single methods.[^1]

[^1]: Our code can be found at https://github.com/morningmoni/UniPELT

## Introduction

As pre-trained language models (PLMs) ([[../BERT|BERT]]) grow larger and larger (Brown et al., 2020), it becomes increasingly infeasible to perform conventional fine-tuning, where separate replicas of the model parameters are modified per single task. To solve the issue, there has recently been a surge of studies on parameter-efficient language model tuning (PELT), namely how to effectively tune the PLMs with fewer trainable parameters.


> [!blank-container|right-medium]
> ![[Images/figure1.svg]]
> > Figure 1: Illustration of UniPELT, which subsumes existing PELT methods as submodules and controls them via gating mechanism $\mathcal{G}$. Different (combinations of) submodules can be activated for different samples. The trainable parameters are shown in blue.

Existing PELT research generally aims at achieving performance comparable to fine-tuning with
as few trainable parameters as possible, which has seen significant progress - the task-specific trainable parameters used in most recent approaches (Lester et al., 2021; Guo et al., 2021) are almost negligible compared to the total parameters of the PLM $(<1 \%)$. A more challenging yet less studied problem is whether one can achieve better performance than fine-tuning with fewer parameters. Recent studies (He et al., 2021; Li and Liang, 2021; Karimi Mahabadi et al., 2021b) find that some PELT methods are more effective than fine-tuning on certain tasks when training data is limited, possibly due to the reduced risk of overfitting. However, as found in our experiments (Table 1), different PELT methods exhibit diverse characteristics and perform rather differently on the same task, which makes it nontrivial to select the most appropriate method for a specific task, especially considering the fast-growing number of new PELT methods and tasks (Ding and Hu, 2021).

In light of the diverse performance of PELT methods and the cost of selecting the best method, we propose a unified PELT framework, named UniPELT, which incorporates different PELT methods as submodules and learns to dynamically activate the (combination of) submodules that best suit the current data or task setup. As a result, model selection is no longer needed and consistently better performance is achieved under different setups. The activation of each submodule in UniPELT is controlled by gating mechanism, which learns to favor (assign more weight to) the submodules that positively contribute to a given task. In addition, since the number of parameters introduced by each submodule is generally small, combining multiple methods leads to negligible losses in model efficiency.

We select four representative PELT methods for our study - adapter (Houlsby et al., 2019), prefixtuning (Li and Liang, 2021), LoRA (Hu et al., 2021), and BitFit (Ben Zaken et al., 2021), which largely cover the major categories of PELT methods. We perform two sets of analysis that carefully examines (i) the characteristics of individual PELT methods and (ii) their effectiveness when coordinated by UniPELT under various setups.[^2]

[^2]: BitFit is not included in UniPELT as it typically performs the worst in our preliminary experiments. individual methods.

Extensive experiments on the [[../GLUE|GLUE]] benchmark, with 32 setups (8 tasks $\times 4$ data sizes) and 1,000+ runs, not only reveal the diverse behavior of PELT methods, but also show that UniPELT is more effective and robust than using each method alone in various task and data setups. Specifically, UniPELT consistently improves the best submodule that it incorporates by $1 \sim 4$ points and even outperforms fine-tuning, achieving the best average performance on the [[../GLUE|GLUE]] benchmark under different setups. Moreover, UniPELT generally surpasses the upper bound that takes the best performance of all its submodules used individually on each task, which suggests that UniPELT maintains (near) optimal performance under different setups. The fact that UniPELT outperforms the upper bound also implies that a mixture of PELT methods involving different parts of the PLM architecture may be inherently more effective than

Contributions. (1) We conduct a comprehensive study of representative PELT methods and carefully examine their differences and commonalities in terms of performance and characteristics. (2) We propose a unified PELT framework that can incorporate existing methods as submodules and automatically learn to activate the appropriate submodules for a given task. (3) Our proposed framework achieves better average performance than finetuning and the PELT methods that it incorporates under various setups, often performing the best and never the worst at per-task level, exhibiting superior effectiveness and robustness with negligible losses in model efficiency.

## Preliminaries

### PELT Methods without Additional Parameters

PLMs can be used as feature extractors where only the top layers or prediction head are fine-tuned without additional parameters (Lee et al., 2019). However, such fine-tuning approaches generally lead to degenerate model performance that is much worse than fine-tuning all parameters (Lee et al., 2019; Pfeiffer et al., 2021). A recent method BitFit (Ben Zaken et al., 2021) only tunes the bias terms of the PLM and is shown to achieve performance comparable to fine-tuning on certain tasks when training data is limited. Therefore, we select BitFit as the representative of this category for analysis.

### PELT Methods with Additional Parameters

Alternatively, one may fix the entire PLM and introduce a small number of new trainable parameters. Notable examples in this category include adapter (Houlsby et al., 2019) and its extensions (Pfeiffer et al., 2021; Karimi Mahabadi et al., 2021b), prefix-tuning (Li and Liang, 2021) and its extensions (Lester et al., 2021), and additive methods (Guo et al., 2021; Hu et al., 2021).

Next, we will briefly describe these methods to facilitate the introduction of our proposed framework. An illustration is shown in Fig. 1 for better understanding.

Adapter. Adapter (Houlsby et al., 2019) adds a trainable bottleneck layer after the feedforward network in each Transformer layer of the PLM. A bottleneck layer consists of a down+up projection pair that shrinks and recovers the size of token hidden states. Mathematically, if we denote the output of the feedforward network after residual connection and layer normalization as $\mathbf{h}_{F N}$ with hidden size $D_{\text {hidden }}$ and bottleneck size $D_{\text {mid }}$, then the output of a bottleneck layer $\mathbf{h}_{A}$ is:

$$
\mathbf{h}_{A}=\mathbf{W}_{\mathrm{up}}^{\top} \phi\left(\mathbf{W}_{\mathrm{down}}^{\top} \mathbf{h}_{F N}\right), \tag{1}
$$

where $\mathbf{W}_{\text {down }} \in \mathbb{R}^{D_{\text {hidden }} \times D_{\text {mid }}}, \mathbf{W}_{\text {up }} \in$ $\mathbb{R}^{D_{\text {mid }} \times D_{\text {hidden }}}, \phi$ is a nonlinear activation function, and the bias terms are omitted for brevity. The parameters in layer normalization and the final prediction head sometimes are also fine-tuned depending on the specific adapter variants.

Adapter has shown to be on par with fine-tuning and sometimes exhibits better effectiveness in the low-resource setting (He et al., 2021). Later studies extend adapter to multi-lingual (Pfeiffer et al., 2020b) and multi-task (Karimi Mahabadi et al., 2021b) settings, or further reduce its trainable parameters (Karimi Mahabadi et al., 2021a), which can be easily incorporated into UniPELT as a replacement of the vanilla adapter.

Prefix-tuning. Prefix-tuning (Li and Liang, 2021) prepends a number of task-specific trainable vectors to the input of multi-head attention in each Transformer layer, which the original tokens can attend to as if they were virtual tokens. Specifically, we denote the original sequence length $L_{0}$, the number of trainable vectors (i.e., prefix length) $L$, and the Transformer layer input $\mathbf{h}_{\text {in }} \in \mathbb{R}^{D_{\text {hidden }} \times L_{0}}$. First, three linear projections $\mathbf{W}_{Q}, \mathbf{W}_{K}, \mathbf{W}_{V} \in$ $\mathbb{R}^{D_{\text {hidden }} \times D_{\text {hidden }}}$ transform $\mathbf{h}_{\text {in }}$ into Query $\mathbf{Q}$, Key $\mathbf{K}$, and Value $\mathbf{V}$. Then, two prefix matrices $\mathbf{P}_{K}$ and $\mathbf{P}_{V} \in \mathbb{R}^{D_{\text {hidden }} \times L}$ are prepended to $\mathbf{K}$ and $\mathbf{V}$. To stabilize optimization, the prefix matrix $\mathbf{P}$ is reparameterized by a feedforward network:

$$
\mathbf{P}^{\prime}=\mathbf{W}_{\text {up }}^{\top} \phi\left(\mathbf{W}_{\text {down }}^{\top} \mathbf{P}\right), \tag{2}
$$

where $\mathbf{W}_{\text {down }} \in \mathbb{R}^{D_{\text {hidden }} \times D_{\text {mid }}}, \quad \mathbf{W}_{\text {up }} \in$ $\mathbb{R}^{D_{\text {mid }} \times 2 N_{\text {layer }} D_{\text {hidden }}}$, and $N_{\text {layer }}$ denotes the number of Transformer layers. The parameters of this network can be discarded after training, and only $2 N_{\text {layer }}$ prefix matrices $\in \mathbb{R}^{D_{\text {hidden }} \times L}$ are needed (2 matrices for each layer).

Prefix-tuning is originally evaluated on natural language generation and we adapt it to understanding tasks. A follow-up method named prompt tuning (Lester et al., 2021) further reduces task specific parameters by limiting the prefix to the first layer but only performs competitively with very large model sizes (billions of total parameters), and is thus not considered in our study. Note that prefix-tuning (or prompt-tuning) is different from prompt-based fine-tuning methods (Schick and Schütze, 2021; Gao et al., 2021) (see App. A for specific differences).

Additive Methods. Additive PELT methods treat the model parameters after fine-tuning as an addition of the pre-trained parameters $\theta_{\text {pre-trained }}$ and task-specific differences $\delta_{\text {task }}$, where $\theta_{\text {pre-trained }}$ is fixed and a new (sub)set of model parameters are added on top: $\theta_{\text {task }}=\theta_{\text {pre-trained }}+\delta_{\text {task }}$. There are various ways to parameterize $\delta_{\text {task }}$, leading to different additive methods such as LoRA (Hu et al., 2021), diff pruning (Guo et al., 2021), and sidetuning (Zhang et al., 2020). We take LoRA as a representative and incorporate it into UniPELT. Other methods are conceptually similar and can be incorporated in the same fashion.

LoRA introduces trainable low-rank matrices and combines them with the original matrices in the multi-head attention. Specifically, two matrices $\mathbf{W}_{\text {down }} \in \mathbb{R}^{D_{\text {hidden }} \times D_{\text {mid }}}$ and $\mathbf{W}_{\text {up }} \in \mathbb{R}^{D_{\text {mid }} \times D_{\text {hidden }}}$ are added for the query and value projections along with the original matrix $\mathbf{W}_{Q}$ and $\mathbf{W}_{V} \in$ $\mathbb{R}^{D_{\text {hidden }} \times D_{\text {hidden }} \text { : }}$

$$
\mathbf{Q}=\left(\mathbf{W}_{Q}^{\top}+\alpha \mathbf{W}_{\text {up }}^{\top} \mathbf{W}_{\text {down }}^{\top}\right) \mathbf{h}_{\text {in }}, \tag{3}
$$

where $\alpha$ is a fixed scalar hyperparameter for scaling the task-specific differences. The form of the trainable matrices in LoRA is quite similar to those in adapter or prefix-tuning, but there is no activation function $\phi$ in between.

## Unifying PELT Methods

### Task Formulation

Given a large PLM $\mathcal{M}$ with size $|\mathcal{M}|$ that cannot be fine-tuned directly due to computational or storage cost, suppose that we have a list of PELT methods $\left\{m_{i}\right\}$, the trainable parameters of which are negligible (i.e., $\sum_{i}\left|m_{i}\right| \ll|\mathcal{M}|$ ), our goal is to design a unified PELT framework that incorporates $\left\{m_{i}\right\}$ as submodules and learns to dynamically activate (upweight) different submodules when appropriate under different scenarios, such that one could achieve satisfactory results in terms of both model effectiveness and robustness without the hassle of permuting all the method $\times$ task $\times$ data combinations.

### Proposed Method

Motivation \\& Intuition. During the analysis of individual PELT methods, we observe that different PELT methods exhibit diverse characteristics and perform rather differently on the same task. For example, prefix-tuning generally performs well on natural language inference tasks regardless of the size of training data. Also, as can be seen in Fig. 1 and Sec. 2, different PELT methods often involve different parts of the PLM architecture (e.g., before multi-head attention for prefix-tuning and after feedforward layer for adapter), making it feasible to combine multiple PELT methods without (directly) interfering with each other.

In light of the two observations above, we propose a unified PELT framework, UniPELT, which takes a hybrid approach by incorporating multiple PELT methods as submodules. At a high level, UniPELT improves over single PELT methods due to two factors. First, UniPELT learns to activate (upweight) the submodules that best suit the current task or specific data sample and deactivate (downweight) the rest. Second, we find that UniPELT generally performs better than taking the best performance of all its submodules used individually on each task, suggesting that there could be some compounding effects that lead to better model effectiveness when multiple PELT methods (that modify different parts of the PLM) are used.

Next, we will introduce how different PELT methods can be incorporated into UniPELT via gating mechanism.

Gating Mechanism. To achieve fine-grained control of submodule (de)activation, we add a trainable gate $\mathcal{G}_{m_{i}}$ for each submodule $m_{i} \in\{\mathrm{A}, \mathrm{P}, \mathrm{L}\}$ in every Transformer layer (see Fig. 1). The letters A, P, L stand for Adapter, Prefix-tuning, and LoRA, respectively. Intuitively, if $m_{i}$ is useful for a given data $\times$ task setup (or a particular instance), the gate output for $m_{i}$ would be higher such that $m_{i}$ plays a more important role. The actual interplay of submodules, however, is more complicated given the interdependency of the submodules and the compounding effects of multiple layers.

Specifically, for adapter, there is a residual connection between the feedforward network and the adapter submodule that sums the adapter input (before normalization) $\mathbf{h}_{F}$ and output $\mathbf{h}_{A}$ as its final output: $\mathbf{h}_{A}^{\prime}=\mathbf{h}_{A}+\mathbf{h}_{F}$. We design a gating function $\mathcal{G}_{A} \in(0,1)$ that estimates the importance of adapter by its direct input $\mathbf{h}_{F N}$ using a feedforward network with sigmoid activation and then scales its output: $\mathbf{h}_{A}^{\prime}=\mathcal{G}_{A} \mathbf{h}_{A}+\mathbf{h}_{F}$. The adapter submodule is effectively bypassed if $\mathcal{G}_{A} \approx 0$.

Similarly, for prefix-tuning, we design a gating function $\mathcal{G}_{P} \in(0,1)$ that is applied to the prefix vectors $\left(\mathbf{P}_{K}\right.$ and $\left.\mathbf{P}_{V}\right)$ with the representation of the original tokens ( $\mathbf{K}$ and $\mathbf{V}$ ) intact. In this way, the impact of the prefix would be diminished if the gate output of the prefix-tuning submodule is low. [^3] The gating function $\mathcal{G}_{P}$ is estimated by the Transformer layer input $\mathbf{h}_{\text {in }}$ with another feedforward network.

[^3]: Prefix-tuning cannot be fully eliminated as adapter or LoRA due to the softmax operation in multi-head attention.

As for LoRA, we note that there is already a constant scaling factor $\alpha$ in its original design that resembles the purpose of our gating mechanism. We thus simply make the factor learnable per layer by a third feedforward network that takes $\mathbf{h}_{\text {in }}$ as input instead of specifying a constant manually: $\theta_{\text {task }}=\theta_{\text {pre-trained }}+\mathcal{G}_{L} \delta_{\text {task }}$.

Despite the seeming simplicity of UniPELT, we note that it is nontrivial for a unified approach to work well under different scenarios. Naively combining different PELT methods as a hybrid approach could lead to mixed or worse performance than using individual methods, as observed in both our experiments and prior studies (Hu et al., 2021).

## 4 Experiments

We conduct extensive experiments with 8 tasks $\times$ 4 data sizes $\times 7$ methods $\times 5$ runs per setup, along with additional analysis for particular methods, resulting in $1,000+$ runs in total.

### 4.1 Experiment Setup

###### Task Setup. 
We conduct experiments on the General Language Understanding Evaluation ([[../GLUE|GLUE]]) benchmark, which involves four types of natural language understanding tasks including linguistic acceptability ([[../../Dataset/CoLA|CoLA]]), sentiment analysis ([[../../Dataset/SST-2|SST-2]]), similarity and paraphrase tasks ([[../../Dataset/MRPC|MRPC]], STS-B, [[../../Dataset/Quora Question Pairs|Quora Question Pairs|QQP]]), and natural language inference (MNLI, QNLI, RTE). We exclude the WNLI dataset following prior studies (Houlsby et al., 2019; [[../BERT|BERT]]).

###### Data Setup. 
We mainly consider a low-resource setting where training data is limited and the performance of different methods varies much. We sample a small subset of the training set for each task with size $K=\{100,500,1000\}$. As it is infeasible to submit considerable runs to the [[../GLUE|GLUE]] leaderboard (2 submissions/day), we take 1,000 samples on the training set as the development set to select the best checkpoint and use the original development set as the test set. To reduce variance, we shuffle the data with 5 random seeds and report the average performance. Additionally, we consider a high-resource setting where the whole training set is used and the best performance on the [[../GLUE|GLUE]] development set is reported.


> [!blank-container]
> | Method | SST-2 | MRPC | CoLA | RTE | QNLI | STS-B | MNLI | QQP | Avg. |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | $[K=100]$ Test Performance |  |  |  |  |  |  |  |  |  |
> | Fine-tuning | $79.61_{4.25}$ | $\underline{81.81}_{0.35}$ | $16.56_{4.34}$ | $\underline{55.88_{1.64}}$ | $69.25_{5.94}$ | $74.07_{6.51}$ | $\underline{42.56}_{3.43}$ | $60.41_{6.42}$ | $60.02_{1.8}$ |
> | BitFit | $62.94_{4.85}$ | $81.09_{0.17}$ | $2.71_{1.57}$ | $47.65_{3.56}$ | $42.46_{1.37}$ | $54.53_{0.56}$ | $38.16_{0.53}$ | $59.56_{0.39}$ | $48.64_{0.7}$ |
> | Adapter | $80.48_{2.94}$ | $81.40_{0.19}$ | $2.02_{4.04}$ | $52.78_{0.27}$ | $72.25_{0.49}$ | $77.32_{1.54}$ | $38.81_{3.64}$ | $60.88_{4.00}$ | $58.24_{0.9}$ |
> | Prefix-tuning | $60.87_{12.47}$ | $81.22_{0.00}$ | $0.00_{0.00}$ | $\mathbf{5 5 . 9 6}_{2.00}$ | $71.91_{2.69}$ | $57.69_{0.02}$ | $40.58_{2.49}$ | $15.68_{0.12}$ | $47.99_{1.7}$ |
> | $\rightarrow L=50$ | $79.52_{1.21}$ | $81.22_{0.00}$ | $5.19_{8.62}$ | $49.24_{2.08}$ | $66.33_{2.45}$ | $7.15_{10.37}$ | $33.66_{2.21}$ | $58.32_{3.18}$ | $47.56_{1.3}$ |
> | LoRA | $\underline{81.56}_{0.94}$ | $81.66_{0.81}$ | $13.31_{10.00}$ | $55.02_{1.75}$ | $\mathbf{7 3 . 5 2}_{1.20}$ | $49.35_{21.87}$ | $39.60_{4.98}$ | $0.09_{0.02}$ | $49.26_{2.1}$ |
> | UniPELT (AP) | $77.22_{3.75}$ | $\mathbf{8 1 . 8 6}_{0.70}$ | $14.42_{10.24}$ | $55.52_{2.16}$ | $72.26_{0.89}$ | $\underline{79.14_{1.97}}$ | $\mathbf{4 2 . 5 9}_{1.20}$ | $\mathbf{6 3 . 4 1}_{1.44}$ | $60.80_{1.5}$ |
> | UniPELT (APL) | $\mathbf{8 2 . 3 6}_{0.86}$ | $81.71_{0.72}$ | $\mathbf{2 3 . 6 2}_{8.83}$ | $55.45_{1.28}$ | $\underline{73.19^{0.93}}$ | $\overline{\mathbf{7 9 . 3 7}}_{1.07}$ | $42.30_{1.88}$ | $\underline{62.70_{2} .55}$ | $\overline{62.59}_{1.4}$ |
> | $[K=500]$ Test Performance |  |  |  |  |  |  |  |  |  |
> | Fine-tuning | $\mathbf{8 5 . 6 7}_{0.97}$ | $\underline{83.34_{0.55}-r_{1}-2}$ | $36.47_{2.69}$ | $59.64_{1.10}$ | $77.30_{0.49}$ | $\underline{84.96_{1.19}}$ | $55.84_{0.85}$ | $\underline{68.23}_{1.39}$ | $68.93_{0.6}$ |
> | BitFit | $83.44_{0.63}$ | $\overline{82.16}_{0.37}$ | 3.322 .59 | $61.88_{2.75}$ | $69.15_{9.91}$ | $\overline{76.30}_{0.36}$ | $40.82_{3.30}$ | $\overline{65.29}_{3.66}$ | $60.30_{1.9}$ |
> | Adapter | $84.54_{1.37}$ | $82.53_{0.36}$ | $38.65_{3.97}$ | $59.35_{3.09}$ | $77.39_{0.84}$ | $83.52_{0.33}$ | $50.04_{1.72}$ | $68.12_{0.95}$ | $68.02_{0.7}$ |
> | Prefix-tuning | $83.65_{0.69}$ | $82.96_{1.63}$ | $38.16_{2.25}$ | $63.18_{2.70}$ | $78.50_{1.12}$ | $79.75_{1.49}$ | $\mathbf{5 8 . 0 6}_{1.04}$ | $54.34_{25.91}$ | $67.32_{3.4}$ |
> | LoRA | $\underline{84.98} 1.10$ | $82.53_{0.70}$ | $\mathbf{3 9 . 8 6}_{2.71}$ | $63.03_{2.57}$ | $\mathbf{7 9 . 4 6}_{0.66}$ | $65.05_{26.31}$ | $56.54_{2.05}$ | $55.46_{27.74}$ | $65.86_{4.1}$ |
> | UniPELT (AP) | $\overline{84.84}_{0.28}$ | $83.25_{0.51}$ | 39.845 .01 | 63.321 .72 | $78.36_{1.06}$ | $84.53_{0.48}$ | $\overline{56.08}_{3.26}$ | $68.14_{1.39}$ | $69.79_{1.0}$ |
> | UniPELT (APL) | $84.91_{1.41}$ | $\mathbf{8 3 . 5 6}_{0.59}$ | $\overline{39.81}_{2.55}$ | $\overline{\mathbf{6 4 . 1 2}}_{2.45}$ | $\underline{79.28}{ }_{0.63}$ | $\mathbf{8 5 . 2 6}_{0.70}$ | $54.07_{3.74}$ | $\mathbf{6 8 . 8 7}_{0.41}$ | $\overline{69.98}_{0.4}$ |
> | $[K=1000]$ Test Performance |  |  |  |  |  |  |  |  |  |
> | Fine-tuning | $\underline{86.54_{1.01}}$ | $84.87_{0.64}$ | $43.26_{2.60}$ | $62.31_{2.10}$ | $79.03_{1.11}$ | $86.39_{0.34}$ | $61.95_{1.20}$ | 71.09 ${ }_{0.77}$ | $71.93_{0.3}$ |
> | BitFit | $83.99_{0.39}$ | $83.95_{0.81}$ | $22.44_{17.10}$ | $62.89_{1.40}$ | $77.43_{0.53}$ | $79.04_{0.61}$ | $52.87_{0.72}$ | $69.50_{0.16}$ | $66.51_{2.2}$ |
> | Adapter | $85.60_{0.63}$ | $84.49_{0.60}$ | $42.33_{1.98}$ | $61.81_{1.57}$ | $79.68_{0.23}$ | $85.52_{0.29}$ | $57.86_{2.44}$ | $70.32_{0.71}$ | $70.95_{0.5}$ |
> | Prefix-tuning | $85.09_{0.99}$ | $83.66_{1.82}$ | $44.07_{2.90}$ | $\mathbf{6 6 . 7 1}_{2.72}$ | $80.34_{0.70}$ | $82.38_{1.25}$ | $\mathbf{6 3 . 5 9}_{1.12}$ | $68.58_{0.35}$ | $71.81_{0.5}$ |
> | LoRA | $86.26_{1.22}$ | $\underline{86.04_{0.99}}$ | $\mathbf{4 5 . 5 0}_{1.11}$ | $\underline{65.63_{2.11}}$ | $\underline{81.00}_{0.98}$ | $81.56_{1.97}$ | $61.32_{1.65}$ | $70.89_{0.81}$ | $72.28_{0.6}$ |
> | UniPELT (AP) | $86.17_{0.37}$ | $\overline{85.86}_{1.05}$ | $44.33_{3.55}$ | $\overline{64.91}_{1.92}$ | $\overline{80.65}_{0.57}$ | $\underline{86.82_{0.23}}$ | $62.17_{0.99}$ | $69.95_{0.90}$ | $72.61_{0.5}$ |
> | UniPELT (APL) | $\mathbf{8 7 . 0 6}_{0.81}$ | $\mathbf{8 6 . 6 5}_{1.10}$ | $\underline{45.44_{1.97}}$ | $65.49_{1.92}$ | $\mathbf{8 1 . 2 2}_{0.51}$ | $\mathbf{8 7 . 1 0}_{0.21}$ | $\underline{62.49_{0.94}}$ | $\underline{70.99_{0.95}}$ | $\mathbf{7 3 . 3 1}_{0.5}$ |
> 
> > Table 1: Results on the [[../GLUE|GLUE]] benchmark with $K=\{100,500,1000\}$ training samples. The evaluation metrics are Matthew's Correlation for CoLA, F1 for MRPC and QQP, Spearman's correlation for STS-B, and accuracy for the rest. For MNLI, we evaluate on the matched dataset. We report average performance on five random seeds with standard deviation as the subscript. Best and 2 nd best methods under each setup are bold and underlined.

###### Compared Methods. 
We mainly compare UniPELT with fine-tuning and four representative PELT methods: adapter (Houlsby et al., 2019), prefix-tuning (Li and Liang, 2021), [[../BitFit|BitFit]], and [[../LoRA|LoRA]]. For completeness, we consider two model variants UniPELT (AP) and UniPELT (APL), which incorporate 2 and 3 PELT methods, respectively.

###### Implementation Details. 
We use [[../GLUE|GLUE]] base as the base model in the experiments. Consistent results are observed in our preliminary experiments with BART large (Lewis et al., 2020) (provided in App. C). We implement and evaluate all the methods in the same codebase to ensure a fair comparison. We largely follow the default hyperparameters of different methods and keep them the same on all the tasks for generalizability. We set the prefix length $L=10$, adapter bottleneck size $D_{\text {mid }}=48$, LoRA rank $D_{\text {mid }}=8$ if not specified otherwise.[^4] More implementation and hyperparameter details can be found in App. B.

[^4]: While these hyperparameters may lead to differences in trainable parameters, we keep them for analysis as they are used by the official implementation. Also, we observe that more trainable parameters do not guarantee better results.

### 4.2 Analysis of Individual PELT Methods

In Table 1, we show the performance of different methods on the [[../GLUE|GLUE]] benchmark with various sizes of training data. The results on the development sets are generally consistent with the test sets and provided in App. D. Although the average performance of different methods over 8 tasks is sometimes similar, the differences between tasks are quite significant under certain setups and can be as large as 5 9 points on a specific task (e.g., STS-B and MNLI, $K=500$ ) even when excluding cases where some methods fail to learn effectively (e.g., prefix-tuning on QQP, $K=100$ ).

###### Analysis of Adapter. 
The performance of adapter is relatively stable - there is no significantly better or worse result than fine-tuning consistent on different tasks or sizes of training data. In general, adapter is slightly worse than fine-tuning in most cases. We do not observe that adapter consistently outperforms fine-tuning in the low-resource setting as in He et al. (2021), possibly because they tune model hyperparameters on each task, which could be computationally prohibitive when there are considerable tasks. For example, they choose the bottleneck size $D_{\text {mid }}$ from $\{64,128$, $256\}$, while $D_{\text {mid }}=48$ is fixed across tasks in our experiments. Also, we only add one adapter in each Transformer layer instead of two following Pfeiffer et al. (2021). These two differences result in $62.4 \% \sim 90.5 \%$ fewer parameters than the adapter used in He et al. (2021).

> [!blank-container|right-medium]
> ![[Images/figure2.svg]]
> 
> > **Figure 2**: Performance changes when the bottleneck size of adapter is increased (on CoLA, $K=100$ ).
> 

To further study the effect of bottleneck size $D_{\text {mid }}$ in adapter, we increase $D_{\text {mid }}$ and re-evaluate adapter on a setup that it performs poorly (CoLA, $K=100$ ). As shown in Fig. 2, the performance of adapter is increased gradually and becomes significantly better only when $D_{\text {mid }}=256$, which involves $5.3 \times$ trainable parameters than the adapter used originally $\left(D_{\text {mid }}=48\right), 4.3 \times$ than UniPELT (AP), and $3.4 \times$ than UniPELT (APL), suggesting that a larger bottleneck size could be beneficial when adapter learns ineffectively.

On the other hand, there are certain tasks (e.g., STS-B) that adapter largely outperforms competitive methods such as prefix-tuning and LoRA regardless of the size of training data, suggesting that one should favor adapter over other PELT methods under certain scenarios as well.

###### Analysis of Prefix-tuning. 
Prefix-tuning performs poorly with $K=\{100,500\}$ and becomes on par with fine-tuning when $K$ reaches 1000 . We also observe that prefix-tuning fails to learn effectively on certain tasks when the training data is limited (e.g., $K=100$ on SST-2 and $K=500$ on QQP), leading to unsatisfactory performance and (or) large variance across different runs. Similar phenomena have been observed in a concurrent study (Gu et al., 2021) on few-shot prompt-tuning.

To ensure that the poor performance of prefixtuning is not due to its fewer trainable parameters (based on its default setting), we further increase the prefix length to $L=50$ such that its trainable parameters are comparable to adapter, and reevaluate prefix-tuning on all 8 tasks with $K=100$. For the 4 tasks where prefix-tuning $(L=10)$ performs poorly (SST2, CoLA, STS-B, and QQP), while its performance is significantly improved on 3 tasks, it also performs significantly worse on the other task (STS-B), which suggests that training instability in the low-resource regime is still an issue for prefix-tuning even with more trainable parameters[^5].  Besides, prefix-tuning $(L=50)$ still lags behind adapter or UniPELT (AP) on 3 of the 4 tasks. Furthermore, the average performance of prefix-tuning ( $L=50)$ on 8 tasks is even slightly worse than with $L=10$, which indicates that increasing prefix length may not be a panacea for all the scenarios. A larger $L$ also leads to significant training/inference slowdown due to the costly multi-head attention. More broadly, such results suggest that using more trainable parameters does not guarantee better performance.

[^5]: Tuning other hyperparameters like learning rate does not appear to alleviate the issue either.

On the bright side, prefix-tuning performs well on certain tasks such as natural language inference (RTE and MNLI) with various sizes of training data, which suggests that one should also prefer prefix-tuning in certain cases.

###### Analysis of BitFit & LoRA. 

> [!blank-container|left-medium]
> ![[Images/figure3.svg]]
> > Figure 3: Performance comparison of various scaling factors for LoRA on 2×2 task and data setups.

Tuning only the bias terms of the model does not lead to very satisfactory results in our experiments - BitFit never performs the best and generally performs the worst in different data and task setups. Therefore, we do not consider BitFit in the following experiments and exclude BitFit as a submodule of UniPELT. As for LoRA, there are a few setups where LoRA fails to learn effectively as well, such as STS-B and QQP $(K=\{100,500\})$, leading to high variance across runs. Apart from that, LoRA performs quite competitively despite using fewer trainable parameters than methods like adapter, especially when $K=1000$, achieving the best or 2 nd best performance on 4 of 8 tasks.

As LoRA has a scaling factor $\alpha$ that can be seen as a static gating function under our formulation, we further investigate its importance by evaluating LoRA with different $\alpha$. As shown in Fig. 3, LoRA is quite sensitive to the scaling factor and there seems to be no single optimal value that works well across multiple task and data setups. Such findings suggest that gating is critical and motivate us to use more fine-grained and dynamic control for UniPELT. Besides, we observe that increasing $\alpha$ consistently results in faster convergence, possibly because the trainable parameters would receive larger gradient updates with a larger $\alpha$. 

### 4.3 Analysis of UniPELT

Next, we will turn to our proposed framework UniPELT, which incorporates multiple existing PELT methods as submodules.

###### Low-Resource Performance. 
Overall, UniPELT (APL) and UniPELT (AP) consistently achieve the best and second best average performance on both the development and test sets regardless of the number of training samples. The gains are generally 1 4\% over the submodule that performs the best (when used individually). Such results demonstrate the advantages of our hybrid approach regarding model effectiveness and generalizability.

At the per-task level, UniPELT (APL) and UniPELT (AP) perform the best or second best on 7/6/7 of 8 tasks when trained with 100/500/1,000 samples, and never perform the worst in any setup. When comparing the two variants, UniPELT (APL) outperforms UniPELT (AP) on 4/6/8 of 8 tasks when trained with 100/500/1,000 samples. Such results indicate that UniPELT is quite robust and performs reliably under different scenarios. The improvements of UniPELT over its submodules are generally larger when having fewer training samples, suggesting that UniPELT performs especially well in the low-resource regime. In particular, on the tasks where other PELT methods fail to learn effectively such as CoLA and QQP ( $K=100$ ), UniPELT manages to achieve performance better than fine-tuning.

###### UniPELT vs. Upper Bound. 
In Table 2, we show the comparison of UniPELT and the upper bound that takes the best performance of its submodules on each task. We observe that both UniPELT (AP) and UniPELT (APL) perform similarly or even better than their upper bound, which suggests that UniPELT successfully learns to leverage different submodules and maintains (near) optimal performance under different setups. The fact that UniPELT can outperform the upper bound also hints that a mixture of PELT methods (involving different parts of the PLM) might be inherently more effective than single methods (with a limited scope of the PLM architecture).

> [!blank-container]
> | $K$ | $\max (\{A, P\})$ | UniPELT | $\max (\{A, P, L\})$ | UniPELT |
> | :--- | ---: | ---: | ---: | ---: |
> | 100 | 58.86 | $\mathbf{6 0 . 8 0}$ | 60.60 | $\mathbf{6 2 . 5 9}$ |
> | 500 | 69.69 | $\mathbf{6 9 . 7 9}$ | $\mathbf{7 0 . 0 2}$ | 69.98 |
> | 1000 | 72.58 | $\mathbf{7 2 . 6 1}$ | 73.19 | $\mathbf{7 3 . 3 1}$ |
> 
> > **Table 2**: Comparison of average test performance between UniPELT and the upper bound that takes the best performance of its submodules on each task.

###### High-Resource Performance. 
In Table 3, we list the performance of different methods when all training samples are used. UniPELT again achieves the best overall performance. The gains are not as significant as in the low-resource setting, which is somewhat expected as existing PELT methods typically perform on par with fine-tuning given abundant training data and the potential of improvement is not as high. That said, the performance of UniPELT is still the best or 2nd best on all 8 tasks, and generally comparable to the best submodule used individually on each task. Besides, simply combining multiple PELT methods without gating does not work well in the high-resource setting - although UniPELT-NoGate never performs the worst in each task, its average performance is unsatisfactory (-0.89 vs. UniPELT).

> [!blank-container]
> | Method | SST-2 | MRPC | CoLA | RTE | QNLI | STS-B | MNLI | QQP | Avg. |
> | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | $[K=$ all $]$ Best Performance on GLUE Dev |  |  |  |  |  |  |  |  |  |
> | Fine-tuning | 91.63 | $\underline{90.94}$ | $\mathbf{6 2 . 0 8}$ | 66.43 | 89.95 | $\mathbf{8 9 . 7 6}$ | 83.23 | $\mathbf{8 7 . 3 5}$ | 82.67 |
> | Adapter | $\mathbf{9 1 . 8 6}$ | 89.86 | 61.51 | 71.84 | $\underline{90.55}$ | 88.63 | 83.14 | 86.78 | 83.02 |
> | Prefix-tuning | 90.94 | $\mathbf{9 1 . 2 9}$ | 55.37 | $\mathbf{7 6 . 9 0}$ | 90.39 | 87.19 | 81.15 | 83.30 | 82.07 |
> | LoRA | 91.51 | 90.03 | 60.47 | 71.48 | 89.93 | 85.65 | 82.51 | 85.98 | 82.20 |
> | UniPELT (AP) | $\mathbf{9 1 . 8 6}$ | 90.28 | 61.15 | 71.84 | $\mathbf{9 0 . 7 7}$ | 88.86 | $\underline{83.41}$ | 86.74 | $\underline{83.12}$ |
> | -NoGate | $\underline{91.74}$ | 90.18 | 58.63 | 71.12 | 90.30 | 88.76 | 81.58 | 85.53 | 82.23 |
> | UniPELT (APL) | 91.51 | $\underline{90.94}$ | $\underline{61.53}$ | $\underline{73.65}$ | 90.50 | $\underline{88.93}$ | $\mathbf{8 3 . 8 9}$ | $\underline{87.12}$ | $\mathbf{8 3 . 5 0}$ |
> 
> > **Table 3**: Results on the [[../GLUE|GLUE]] benchmark when all training samples are used.

### 4.4 Efficiency of PELT Methods

We benchmark the efficiency of PELT methods and list in Table 4 their number of trainable parameters and training/inference time relative to fine-tuning. Parameter Efficiency. As the trainable parameters in PELT methods are almost negligible, combining multiple methods does not lead to significant losses in parameter efficiency. UniPELT still has few trainable parameters compared to fine-tuning (0.99%~1.26%). The parameters can be further reduced if one uses more parameter-efficient variants (e.g., Karimi Mahabadi et al. (2021a)), which can be easily swapped with the vanilla version used in our current framework. Also, note that more trainable parameters do not always lead to better performance, as shown in our experiments and prior studies (He et al., 2021; Pfeiffer et al., 2021).

> [!blank-container]
> | Method | \#Param. | Time $_{T}$ | Time $_{I}$ |
> | :--- | ---: | ---: | ---: |
> | Fine-tuning | $110 \mathrm{M}(100 \%)$ | $100 \%$ | $100 \%$ |
> | BitFit | $103 \mathrm{~K}(0.09 \%)$ | $65 \%$ | $102 \%$ |
> | Prefix-tuning | $184 \mathrm{~K}(0.17 \%)$ | $56 \%$ | $114 \%$ |
> | LoRA | $295 \mathrm{~K}(0.27 \%)$ | $53 \%$ | $105 \%$ |
> | Adapter | $895 \mathrm{~K}(0.81 \%)$ | $55 \%$ | $107 \%$ |
> | UniPELT (AP) | $1.1 \mathrm{M}(0.99 \%)$ | $55 \%$ | $118 \%$ |
> | UniPELT (APL) | $1.4 \mathrm{M}(1.26 \%)$ | $67 \%$ | $127 \%$ |
> 
> > **Table 4**: Number of trainable parameters and Training/Inference time relative to fine-tuning.

###### Training and Inference Efficiency. 
Due to parameter efficiency, all PELT methods train $30 \% \sim 50 \%$ faster than fine-tuning and incorporating multiple PELT methods into UniPELT does not suffer from slower training. On the other hand, the inference time of PELT methods is generally longer since they involve more FLOPs. UniPELT has a slightly larger inference overhead (4\% 11\% compared to its slowest submodule), which we argue is insignificant since larger models that may achieve similar performance gains (e.g., BERT large) need around 300\% inference time (Wolf et al., 2020).

## 5 Related Work

Parameter-Efficient Tuning of PLMs. As it is increasingly infeasible to train and store full copies of large PLMs for various downstream tasks, how to efficiently tune the PLMs with few trainable parameters becomes critical. Existing PELT methods can be largely divided into two categories based on whether new trainable parameters are introduced. Specifically, one may either train a subset of the model parameters such as the prediction head (Lee et al., 2019) and bias terms (Ben Zaken et al., 2021), or introduce task-specific parameters to different parts of the PLM such as before multi-head attention (Li and Liang, 2021) or after feedforward layer (Houlsby et al., 2019). As the number of PELT methods keeps increasing, the purpose of UniPELT is to better understand and leverage the distinctions of various methods instead of proposing yet another method.

Mixture-of-Experts. UniPELT is also related to approaches that involve a high-capacity network and activate (upweight) different parts of the network given different inputs. One notable example is Mixture-of-Experts (MoE) (Jacobs et al., 1991; Shazeer et al., 2017), which maintains a set of experts (neural networks) and one or more trainable gates that select a combination of the experts specific to each input. Despite being conceptually similar, UniPELT is different from MoE: the submodules in UniPELT are not combined explicitly by summation like MoE but in sequential order and affect each other implicitly. Moreover, the "experts" are diverse in UniPELT while usually homogeneous or identical in MoE methods.

## Conclusion

In this paper, we present a comprehensive study of representative parameter-efficient language model tuning (PELT) methods and propose a unified framework, which incorporates different PELT methods as submodules and learns to activate the most appropriate submodules for a given task or data setup. Our proposed framework consistently outperforms conventional fine-tuning as well as the submodules that it incorporates under different setups, and generally surpasses the upper bound that takes the best performance of each submodule used individually on each task. Our findings suggest that a mixture of multiple PELT methods that involve different parts of the PLM may be favorable regarding both model effectiveness and robustness. For future work, we will try to better understand the discrepancy of various PELT methods in different scenarios. We also plan to investigate a multi-task setting where multiple submodules can be activated and cooperate at the task level.
