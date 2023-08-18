---
title: "Incorporating Hierarchy into Text Encoder: a Contrastive Learning Approach for Hierarchical Text Classification"
authors:
    - Zihan Wang
    - Peiyi Wang
    - Lianzhe Huang
    - Xin Sun
    - Houfeng Wang
published: "2022-03-23"
url:
    - "[paper](https://arxiv.org/pdf/2203.03825.pdf)"
    - "[arXiv](https://arxiv.org/abs/2203.03825)"
---

# Incorporating Hierarchy into Text Encoder: a Contrastive Learning Approach for Hierarchical Text Classification
###### Authors
<ul>
<li class="author">Zihan Wang</li>
<li class="separator author">|</li>
<li class="author">Peiyi Wang</li>
<li class="separator author">|</li>
<li class="author">Lianzhe Huang</li>
<li class="separator author">|</li>
<li class="author">Xin Sun</li>
<li class="separator author">|</li>
<li class="author">Houfeng Wang</li>
</ul>

```dataview
table without ID url from "Mathematics/Machine Learning/Papers/HGCLR/HGCLR.md"
```


## TLDR
The paper "Incorporating Hierarchy into Text Encoder: a Contrastive Learning Approach for Hierarchical Text Classification" proposes a method called Hierarchy guided Contrastive Learning (HGCLR) for hierarchical text classification. The goal of hierarchical text classification is to categorize text into a set of labels that are organized in a structured hierarchy.

The paper starts by discussing the challenges of hierarchical text classification and the existing methods that encode text and label hierarchy separately. The authors argue that the interaction between text and label hierarchy is redundant and less effective in these methods. Instead, they propose to directly embed the hierarchy into a text encoder using contrastive learning.

Contrastive learning is a method that aims to learn meaningful representations by pulling together positive samples and pushing apart negative samples. In the context of hierarchical text classification, positive samples are constructed by selecting important tokens under the guidance of the label hierarchy. The authors use a graph encoder, specifically a modified version of Graphormer, to encode the label hierarchy and generate label features. They calculate the attention weight of each token embedding on each label and select tokens with weights above a threshold as important tokens. These important tokens are used to construct positive samples.

The proposed HGCLR model consists of a text encoder, a graph encoder, a positive sample generation module, and a contrastive learning module. The text encoder is based on BERT, a powerful text encoding model. The graph encoder, based on Graphormer, encodes the label hierarchy and generates label features. The positive sample generation module selects important tokens based on the label hierarchy. The contrastive learning module pulls together positive samples and pushes apart negative samples to learn meaningful representations.

The paper presents experimental results on three benchmark datasets: Web-of-Science (WOS), NYTimes (NYT), and RCV1-V2. The results show that the proposed HGCLR model outperforms existing methods on these datasets, achieving improvements in terms of Micro-F1 and Macro-F1 scores. The authors also analyze the impact of different components of the model, such as the graph encoder and the positive sample generation strategy.

In conclusion, the paper proposes a novel approach, HGCLR, for hierarchical text classification. The approach incorporates the label hierarchy into the text encoder using contrastive learning. Experimental results demonstrate the effectiveness of the proposed approach compared to existing methods.

#### Abstract

Hierarchical text classification is a challenging subtask of multi-label classification due to its complex label hierarchy. Existing methods encode text and label hierarchy separately and mix their representations for classification, where the hierarchy remains unchanged for all input text. Instead of modeling them separately, in this work, we propose Hierarchy guided Contrastive Learning (HGCLR) to directly embed the hierarchy into a text encoder. During training, HGCLR constructs positive samples for input text under the guidance of the label hierarchy. By pulling together the input text and its positive sample, the text encoder can learn to generate the hierarchy-aware text representation independently. Therefore, after training, the HGCLR enhanced text encoder can dispense with the redundant hierarchy. Extensive experiments on three benchmark datasets verify the effectiveness of HGCLR.


## 1. Introduction

Hierarchical Text Classification (HTC) aims to categorize text into a set of labels that are organized in a structured hierarchy (Silla and Freitas, 2011). The taxonomic hierarchy is commonly modeled as a tree or a directed acyclic graph, in which each node is a label to be classified. As a subtask of multi-label classification, the key challenge of HTC is how to model the large-scale, imbalanced, and structured label hierarchy (Mao et al., 2019).


> [!blank-container|right-large]
> ![[figure_1.svg]]
> 
> > Figure 1: Two ways of introducing hierarchy information. (a) Previous work model text and labels separately and find a mixed representation. (b) Our method incorporating hierarchy information into text encoder for a hierarchy-aware text representation.
> 


The existing methods of HTC have variously introduced hierarchical information. Among recent researches, the state-of-the-art models encode text and label hierarchy separately and aggregate two representations before being classified by a mixed feature (Zhou et al., 2020; Deng et al., 2021). As denoted in the left part of Figure 1, their main goal is to sufficiently interact between text and structure to achieve a mixed representation (Chen et al., 2021), which is highly useful for classification (Chen et al., 2020a). However, since the label hierarchy remains unchanged for all text inputs, the graph encoder provides exactly the same representation regardless of the input. Therefore, the text representation interacts with constant hierarchy representation and thus the interaction seems redundant and less effective. Alternatively, we attempt to inject the constant hierarchy representation into the text encoder. So that after being fully trained, a hierarchy-aware text representation can be acquired without the constant label feature. As in the right part of Figure 1 , instead of modeling text and labels separately, migrating label hierarchy into text encoding may benefit HTC by a proper representation learning method.


To this end, we adopt contrastive learning for the hierarchy-aware representation. Contrastive learning, which aims to concentrate positive samples and push apart negative samples, has been considered as effective in constructing meaningful representations (Kim et al., 2021). Previous work on contrastive learning illustrates that it is critical to building challenging samples (Alzantot et al., 2018; Wang et al., 2021b; Tan et al., 2020; Wu et al., 2020). For multi-label classification, we attempt to construct high-quality positive examples. Existing methods for positive example generation includes data augmentation (Meng et al., 2021; Wu et al., 2020), dropout (Gao et al., 2021), and adversarial attack (Wang et al., 2021b; Pan et al., 2021). These techniques are either unsupervised or taskunspecific: the generation of positive samples has no relation with the HTC task and thus are incompetent to acquire hierarchy-aware representations. As mentioned, we argue that both the ground-truth label as well as the taxonomic hierarchy should be considered for the HTC task.

To construct positive samples which are both label-guided and hierarchy-involved, our approach is motivated by a preliminary observation. Notice that when we classify text into a certain category, most words or tokens are not important. For instance, when a paragraph of news report about a lately sports match is classified as "basketball", few keywords like "NBA" or "backboard" have large impacts while the game result has less influence. So, given a sequence and its labels, a shorten sequence that only keeps few keywords should maintain the labels. In fact, this idea is similar to adversarial attack, which aims to find "important tokens" which affect classification most (Zhang et al., 2020). The difference is that adversarial attack tries to modify "important tokens" to fool the model, whereas our approach modifies "unimportant tokens" to keep the classification result unchanged.

Under such observation, we construct positive samples as pairs of input sequences and theirs shorten counterparts, and propose HierarchyGuided Contrastive Learning (HGCLR) for HTC. In order to locate keywords under given labels, we directly calculate the attention weight of each token embedding on each label, and tokens with weight above a threshold are considered important to according label. We use a graph encoder to encode label hierarchy and output label features. Unlike previous studies with GCN or GAT, we modify a Graphormer (Ying et al., 2021) as our graph encoder. Graphormer encodes graphs by Transformer blocks and outperforms other graph encoders on several graph-related tasks. It models the graph from multiple dimensions, which can be customized easily for HTC task. The main contribution of our work can be summarized as follows:

- We propose Hierarchy-Guided Contrastive Learning (HGCLR) to obtain hierarchy-aware text representation for HTC. To our knowledge, this is the first work that adopts contrastive learning on HTC.
- For contrastive learning, we construct positive samples by a novel approach guided by label hierarchy. The model employs a modified Graphormer, which is a new state-of-the-art graph encoder.
- Experiments demonstrate that the proposed model achieves improvements on three datasets. Our code is available at https://github.com/wzh9969/contrastive-htc.


## 2. Related Work

### 2.1. Hierarchical Text Classification

Existing work for HTC could be categorized into local and global approaches based on their ways of treating the label hierarchy (Zhou et al., 2020). Local approaches build classifiers for each node or level while the global ones build only one classifier for the entire graph. Banerjee et al. (2019) builds one classifier per label and transfers parameters of the parent model for child models. Wehrmann et al. (2018) proposes a hybrid model combining local and global optimizations. Shimura et al. (2018) applies CNN to utilize the data in the upper levels to contribute categorization in the lower levels.

The early global approaches neglect the hierarchical structure of labels and view the problem as a flat multi-label classification (Johnson and Zhang, 2015). Later on, some work tries to coalesce the label structure by recursive regularization (Gopal and Yang, 2013), reinforcement learning (Mao et al., 2019), capsule network (Peng et al., 2019), and meta-learning (Wu et al., 2019). Although such methods can capture the hierarchical information, recent researches demonstrate that encoding the holistic label structure directly by a structure encoder can further improve performance. Zhou et al. (2020) designs a structure encoder that integrates the label prior hierarchy knowledge to learn label representations. Chen et al. (2020a) embeds word and label hierarchies jointly in the hyperbolic space. Zhang et al. (2021) extracts text features according to different hierarchy levels. Deng et al. (2021) introduces information maximization to constrain label representation learning. Zhao et al. (2021) designs a self-adaption fusion strategy to extract features from text and label. Chen et al. (2021) views the problem as semantic matching and tries BERT as text encoder. Wang et al. (2021a) proposes a cognitive structure learning model for HTC. Similar to other work, they model text and label separately.

### 2.2. Contrastive Learning

Contrastive learning is originally proposed in Computer Vision $(\mathrm{CV})$ as a weak-supervised representation learning method. Works such as MoCo (He et al., 2020) and SimCLR (Chen et al., 2020b) have bridged the gap between self-supervised learning and supervised learning on multiple CV datasets. A key component for applying contrastive learning on NLP is how to build positive pairs (Pan et al., 2021). Data augmentation techniques such as back-translation (Fang et al., 2020), word or span permutation (Wu et al., 2020), and random masking (Meng et al., 2021) can generate pair of data with similar meanings. Gao et al. (2021) uses different dropout masks on the same data to generate positive pairs. Kim et al. (2021) utilizes BERT representation by a fixed copy of BERT. These methods do not rely on downstream tasks while some researchers leverage supervised information for better performance on text classification. Wang et al. (2021b) constructs both positive and negative pairs especially for sentimental classification by word replacement. Pan et al. (2021) proposes to regularize Transformer-based encoders for text classification tasks by FGSM (Goodfellow et al., 2014), an adversarial attack method based on gradient. Though methods above are designed for classification, the construction of positive samples hardly relies on their categories, neglecting the connection and diversity between different labels. For HTC, the taxonomic hierarchy models the relation between labels, which we believe can help positive sample generation.

## 3. Problem Definition

Given a input text $x=\left\{x_{1}, x_{2}, \ldots, x_{n}\right\}$, Hierarchical Text Classification (HTC) aims to predict a subset $y$ of label set $Y$, where $n$ is the length of the input sequence and $k$ is the size of set $Y$. The candidate labels $y_{i} \in Y$ are predefined and organized as a Directed Acyclic Graph (DAG) $G=(Y, E)$, where node set $Y$ are labels and edge set $E$ denotes their hierarchy. For simplicity, we do not distinguish a label with its node in the hierarchy so that $y_{i}$ is both a label and a node. Since a non-root label of HTC has one and only one father, the taxonomic hierarchy can be converted to a tree-like hierarchy. The subset $y$ corresponds to one or more paths in $G$ : for any non-root label $y_{j} \in y$, a father node (label) of $y_{j}$ is in the subset $y$.

## 4. Methodology

In this section, we will describe the proposed HGCLR in detail. Figure 2 shows the overall architecture of the model.


> [!blank-container]
> ![[figure_2.svg]]
> 
> > **Figure 2**: An overview of HGCLR under a batch of 3. HGCLR adopts a contrastive learning framework to regularize BERT representations. We construct positive samples by masking unimportant tokens under the guidance of hierarchy and labels. By pulling together and pushing apart representations, the hierarchy information can be injected into the BERT encoder.
> 


### 4.1. Text Encoder

Our approach needs a strong text encoder for hierarchy injection, so we choose BERT ([[../BERT|Devlin et al., 2019]]) as the text encoder. Given an input token sequence:

$$
x=\left\{[\mathrm{CLS}], x_{1}, x_{2}, \ldots, x_{n-2},[\mathrm{SEP}]\right\} \tag{1}
$$

where $[\mathrm{CLS}]$ and $[\mathrm{SEP}]$ are two special tokens indicating the beginning and the end of the sequence, the input is fed into BERT. For convenience, we denote the length of the sequence as $n$. The text encoder outputs hidden representation for each token:

$$
H=\operatorname{BERT}(\mathrm{x}) \tag{2}
$$

where $H \in \mathbb{R}^{n \times d_{h}}$ and $d_{h}$ is the hidden size. We use the hidden state of the first token ([CLS]) for representing the whole sequence $h_{x}=h_{\text {[CLS] }}$.

### 4.2. Graph Encoder

We model the label hierarchy with a customized Graphormer (Ying et al., 2021). Graphormer models graphs on the base of Transformer layer ([[../Attention is all you need|Vaswani et al., 2017]]) with spatial encoding and edge encoding, so it can leverage the most powerful sequential modeling network in the graph domain. We organize the original feature for node $y_{i}$ as the sum of label embedding and its name embedding:

$$
f_{i}=\text{label\_emb}\left(y_{i}\right)+\text{name\_emb}\left(y_{i}\right) \tag{3}
$$

Label embedding is a learnable embedding that takes a label as input and outputs a vector with size $d_{h}$. Name embedding takes the advantage of the name of the label, which we believe contains fruitful information as a summary of the entire class. We use the average of BERT token embedding of the label as its name embedding, which also has a size of $d_{h}$. Unlike previous work which only adopts names on initialization, we share embedding weights across text and labels to make label features more instructive. With all node features stack as a matrix $F \in \mathbb{R}^{k \times d_{h}}$, a standard self-attention layer can then be used for feature migration.

To leverage the structural information, spatial encoding and edge encoding modify the QueryKey product matrix $A^{G}$ in the self-attention layer:

$$
A_{i j}^{G}=\frac{\left(f_{i} W_{Q}^{G}\right)\left(f_{j} W_{K}^{G}\right)^{T}}{\sqrt{d_{h}}}+c_{i j}+b_{\phi\left(y_{i}, y_{j}\right)} \tag{4}
$$

where $c_{i j}=\frac{1}{D} \sum_{n=1}^{D} w_{e_{n}}$ and $D=\phi\left(y_{i}, y_{j}\right)$. The first term in Equation 4 is the standard scaledot attention, and query and key are projected by $W_{Q}^{G} \in \mathbb{R}^{d_{h} \times d_{h}}$ and $W_{K}^{G} \in \mathbb{R}^{d_{h} \times d_{h}}$. $c_{i j}$ is the edge encoding and $\phi\left(y_{i}, y_{j}\right)$ denotes the distance between two nodes $y_{i}$ and $y_{j}$. Since the graph is a tree in our problem, for node $y_{i}$ and $y_{j}$, one and only one path $\left(e_{1}, e_{2}, \ldots, e_{D}\right)$ can be found between them in the underlying graph $G^{\prime}$ so that $c_{i j}$ denotes the edge information between two nodes and $w_{e_{i}} \in \mathbb{R}^{1}$ is a learnable weight for each edge. $b_{\phi\left(y_{i}, y_{j}\right)}$ is the spatial encoding, which measures the connectivity between two nodes. It is a learnable scalar indexed by $\phi\left(y_{i}, y_{j}\right)$.

The graph-involved attention weight matrix $A^{G}$ is then followed by Softmax, multiplying with value matrix and residual connection & layer normalization to calculate the self-attention,

$$
L=\operatorname{LayerNorm}\left(\operatorname{softmax}\left(\mathrm{A}^{\mathrm{G}}\right) \mathrm{V}+\mathrm{F}\right) \tag{5}
$$

We use $L$ as the label feature for the next step. The Graphormer we use is a variant of the selfattention layer, for more details on the full structure of Graphormer, please refer to the original paper.

### 4.3. Positive Sample Generation

As mentioned, the goal for the positive sample generation is to keep a fraction of tokens while retaining the labels. Given a token sequence as Equation 1, the token embedding of BERT is defined as:

$$
\left\{e_{1}, e_{2}, \ldots, e_{n}\right\}=\text { BERT\_emb }(x) \tag{6}
$$

The scale-dot attention weight between token embedding and label feature is first calculated to determine the importance of a token on a label,

$$
q_{i}=e_{i} W_{Q}, k_{j}=l_{j} W_{K}, A_{i j}=\frac{q_{i} k_{j}^{T}}{\sqrt{d_{h}}} \tag{7}
$$

The query and key are token embeddings and label features respectively, and $W_{Q} \in \mathbb{R}^{d_{h} \times d_{h}}$ and $W_{K} \in \mathbb{R}^{d_{h} \times d_{h}}$ are two weight matrices. Thus, for a certain $x_{i}$, its probability of belonging to label $y_{j}$ can be normalized by a Softmax function.

Next, given a label $y_{j}$, we can sample key tokens from that distribution and form a positive sample $\hat{x}$. To make the sampling differentiable, we replace the Softmax function with Gumbel-Softmax (Jang et al., 2016) to simulate the sampling operation:

$$
P_{i j}=\text { gumbel\_softmax }\left(A_{i 1}, A_{i 2}, \ldots, A_{i k}\right)_{j} \tag{8}
$$

Notice that a token can impact more than one label, so we do not discretize the probability as one-hot vectors in this step. Instead, we keep tokens for positive examples if their probabilities of being sampled exceed a certain threshold $\gamma$, which can also control the fraction of tokens to be retrained. For multi-label classification, we simply add the probabilities of all ground-truth labels and obtain the probability of a token $x_{i}$ regarding its ground truth label set $y$ as:

$$
P_{i}=\sum_{j \in y} P_{i j} \tag{9}
$$

Finally, the positive sample $\hat{x}$ is constructed as:

$$
\hat{x}=\left\{x_{i} \text { if } P_{i}>\gamma \text { else } \mathbf{0}\right\} \tag{10}
$$

where $\mathbf{0}$ is a special token that has an embedding of all zeros so that key tokens can keep their positions. The select operation is not differentiable, so we implement it differently to make sure the whole model can be trained end-to-end. Details are illustrated in Appendix A.

The positive sample is fed to the same BERT as the original one,

$$
\hat{H}=\operatorname{BERT}(\hat{x}) \tag{11}
$$

and get a sequence representation $\hat{h}_{x}$ with the first token before being classified. We assume the positive sample should retain the labels, so we use classification loss of the positive sample as a guidance of the graph encoder and the positive sample generation.

### 4.4. Contrastive Learning Module

Intuitively, given a pair of token sequences and their positive counterpart, their encoded sentence-level representation should be as similar to each other as possible. Meanwhile, examples not from the same pair should be farther away in the representation space.

Concretely, with a batch of $N$ hidden state of positive pairs $\left(h_{i}, \hat{h_{i}}\right)$, we add a non-linear layer on top of them:

$$
\begin{aligned}
& c_{i}=W_{2} \operatorname{ReLU}\left(W_{1} h_{i}\right) \\
& \hat{c_{i}}=W_{2} \operatorname{ReLU}\left(W_{1} \hat{h_{i}}\right)
\end{aligned} \tag{12}
$$

where $W_{1} \in \mathrm{R}^{d_{h} \times d_{h}}, W_{2} \in \mathrm{R}^{d_{h} \times d_{h}}$. For each example, there are $2(N-1)$ negative pairs, i.e., all the remaining examples in the batch are negative examples. Thus, for a batch of $2 N$ examples $\mathbf{Z}=$ $\left\{z \in\left\{c_{i}\right\} \cup\left\{\hat{c}_{i}\right\}\right\}$, we compute the NT-Xent loss (Chen et al., 2020b) for $z_{m}$ as:

$$
L_{m}^{c o n}=-\log \frac{\exp \left(\operatorname{sim}\left(z_{m}, \mu\left(z_{m}\right)\right) / \tau\right)}{\sum_{i=1, i \neq m}^{2 N} \exp \left(\operatorname{sim}\left(z_{m}, z_{i}\right) / \tau\right)} \tag{13}
$$

where $\operatorname{sim}$ is the cosine similarity function as $\operatorname{sim}(u, v)=u \cdot v /\|u\|\|v\|$ and $\mu$ is a matching function as:

$$
\mu\left(z_{m}\right)=\left\{\begin{array}{l}
c_{i}, \text { if } z_{m}=\hat{c_{i}} \\
\hat{c}_{i}, \text { if } z_{m}=c_{i}
\end{array}\right. \tag{14}
$$

$\tau$ is a temperature hyperparameter.

The total contrastive loss is the mean loss of all examples:

$$
L^{c o n}=\frac{1}{2 N} \sum_{m=1}^{2 N} L_{m}^{c o n} \tag{15}
$$

### 4.5. Classification and Objective Function

Following previous work (Zhou et al., 2020), we flatten the hierarchy for multi-label classification. The hidden feature is fed into a linear layer, and a sigmoid function is used for calculating the probability. The probability of text $i$ on label $j$ is:

$$
p_{i j}=\operatorname{sigmoid}\left(W_{c} h_{i}+b_{c}\right)_{j} \tag{16}
$$

where $W_{C} \in \mathbb{R}^{k \times d_{h}}$ and $b_{c} \in \mathbb{R}^{k}$ are weights and bias. For multi-label classification, we use a binary cross-entropy loss function for text $i$ on label $j$,

$$
\begin{gather}
L_{i j}^{C}=-y_{i j} \log \left(p_{i j}\right)-\left(1-y_{i j}\right) \log \left(1-p_{i j}\right) \tag{17}\\
L^{C}=\sum_{i=1}^{N} \sum_{j=1}^{k} L_{i j}^{C} \tag{18}
\end{gather} 
$$

where $y_{i j}$ is the ground truth. The classification loss of the constructed positive examples $\hat{L^{C}}$ can be calculated similarly by Equation 16 and Equation 18 with $\hat{h_{i}}$ substituting for $h_{i}$.

The final loss function is the combination of classification loss of original data, classification loss of the constructed positive samples, and the contrastive learning loss:

$$
L=L^{C}+\hat{L^{C}}+\lambda L^{c o n} \tag{19}
$$

where $\lambda$ is a hyperparameter controlling the weight of contrastive loss.

During testing, we only use the text encoder for classification and the model degenerates to a BERT encoder with a classification head.

## 5. Experiments

### 5.1. Experiment Setup

Datasets and Evaluation Metrics We experiment on Web-of-Science (WOS) (Kowsari et al., 2017), NYTimes (NYT) (Sandhaus, 2008), and RCV1-V2 (Lewis et al., 2004) datasets for comparison and analysis. WOS contains abstracts of published papers from Web of Science while NYT and RCV1-V2 are both news categorization corpora. We follow the data processing of previous work (Zhou et al., 2020). WOS is for single-path HTC while NYT and RCV1-V2 include multi-path taxonomic labels. The statistic details are illustrated in Table 1. Similar to previous work, We measure the experimental results with Macro-F1 and Micro-F1.


| Dataset | $\|Y\|$ | Depth | $\operatorname{Avg}\left(\left\|y_{i}\right\|\right)$ | Train  |  Dev  |  Test   |
|:-------:|:-------:|:-----:|:-----------------------------------------------------:|:------:|:-----:|:-------:|
|   WOS   |   141   |   2   |                          2.0                          | 30,070 | 7,518 |  9,397  |
|   NYT   |   166   |   8   |                          7.6                          | 23,345 | 5,834 |  7,292  |
| RCV1-V2 |   103   |   4   |                         3.24                          | 20,833 | 2,316 | 781,265 |

> **Table 1**: Data Statistics. $|Y|$ is the number of classes. Depth is the maximum level of hierarchy. $\operatorname{Avg}\left(\left|y_{i}\right|\right)$ is the average number of classes per sample.


Implement Details For text encoder, we use bert-base-uncased from Transformers (Wolf et al., 2020) as the base architecture. Notice that we denote the attention layer in Eq. 4 and Eq. 7 as single-head attentions but they can be extended to multi-head attentions as the original Transformer block. For Graphormer, we set the attention head to 8 and feature size $d_{h}$ to 768 . The batch size is set to 12 . The optimizer is Adam with a learning rate of $3 e-5$. We implement our model in PyTorch and train end-to-end. We train the model with train set and evaluate on development set after every epoch, and stop training if the Macro-F1 does not increase for 6 epochs. The threshold $\gamma$ is set to 0.02 on WOS and 0.005 on NYT and RCV1-V2. The loss weight $\lambda$ is set to 0.1 on WOS and RCV1-V2 and 0.3 on NYT. $\gamma$ and $\lambda$ are selected by grid search on development set. The temperature of contrastive module is fixed to 1 since we have achieved promising results with this default setting in preliminary experiments.

Baselines We select a few recent work as baselines. HiAGM (Zhou et al., 2020), HTCInfoMax (Deng et al., 2021), and HiMatch (Chen et al., 2021) are a branch of work that propose fusion strategies for mixed text-hierarchy representation. HiAGM applies soft attention over text feature and label feature for the mixed feature. HTCInfoMax improves HiAGM by regularizing the label representation with a prior distribution. HiMatch matches text representation with label representation in a joint embedding space and uses joint representation for classification. HiMatch is the state-of-the-art before our work. All approaches except HiMatch adopt TextRCNN (Lai et al., 2015) as text encoder so that we implement them with BERT for a fair comparison.

### 5.2. Experimental Results

Main results are shown in Table 2. Instead of modeling text and labels separately, our model can make more use of the strong text encoder by migrating hierarchy information directly into BERT encoder. On WOS, the proposed HGCLR can achieve $1.5 \%$ and $2.1 \%$ improvement on MicroF1 and Macro-F1 respectively comparing to BERT and is better than HiMatch even if its base model has far better performance.

BERT was trained on news corpus so that the base model already has decent performance on NYT and RCV1-V2, outperforming post-pretrain models by a large amount. On NYT, our approach observes a 2.3\% boost on Macro-F1 comparing to BERT while sightly increases on Micro-F1 and outperform previous methods on both measurements.

On RCV1-V2, all baselines hardly improve Micro-F1 and only influence Macro-F1 comparing to BERT. HTCInfoMax experiences a decrease because its constraint on text representation may contradict with BERT on this dataset. HiMatch behaves extremely well on RCV1-V2 with MacroF1 as measurement while our approach achieves state-of-the-art on Micro-F1. Besides the potential implement difference on BERT encoder, RCV1-V2 dataset provides no label name, which invalids our name embedding for label representation. Baselines like HiAGM and HiMatch only initialize labl embedding with their names so that this flaw has less impact. We will discuss more on name embedding in next section.

### 5.3. Analysis

The main differences between our work and previous ones are the graph encoder and contrastive learning. To illustrate the effectiveness of these two parts, we test our model with them replaced or removed. We report the results on the development set of WOS for illustration. We first replace Graphormer with GCN and GAT (r.p. GCN and r.p. GAT), results are in Table 3. We find that Graphormer outperforms both graph encoders on this task. GAT also involves the attention mechanism but a node can only attend to its neighbors. Graphormer adopts global attention where each node can attend to all others in the graph, which is proven empirically more effective on this task. When the graph encoder is removed entirely (-r.m. graph encoder), the results drop significantly, showing the necessity of incorporating graph encoder for HTC task.


| Model | WOS |  | NYT |  | RCV1-V2 |  |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  | Micro-F1 | Macro-F1 | Micro-F1 | Macro-F1 | Micro-F1 | Macro-F1 |
| Hierarchy-Aware Models |  |  |  |  |  |  |
| TextRCNN (Zhou et al., 2020) | 83.55 | 76.99 | 70.83 | 56.18 | 81.57 | 59.25 |
| HiAGM (Zhou et al., 2020) | 85.82 | 80.28 | 74.97 | 60.83 | 83.96 | 63.35 |
| HTCInfoMax (Deng et al., 2021) | 85.58 | 80.05 | - | - | 83.51 | 62.71 |
| HiMatch (Chen et al., 2021) | 86.20 | 80.53 | - | - | 84.73 | 64.11 |
| Pretrained Language Models |  |  |  |  |  |  |
| BERT (Our implement) | 85.63 | 79.07 | 78.24 | 65.62 | 85.65 | 67.02 |
| BERT (Chen et al., 2021) | 86.26 | 80.58 | - | - | 86.26 | 67.35 |
| BERT+HiAGM (Our implement) | 86.04 | 80.19 | 78.64 | 66.76 | 85.58 | 67.93 |
| BERT+HTCInfoMax (Our implement) | 86.30 | 79.97 | 78.75 | 67.31 | 85.53 | 67.09 |
| BERT+HiMatch (Chen et al., 2021) | 86.70 | 81.06 | - | - | 86.33 | 68.66 |
| HGCLR | 87.11 | 81.20 | 78.86 | 67.96 | 86.49 | 68.31 |

> **Table 2**: Experimental results of our proposed model on several datasets. For a fair comparison, we implement some baseline with BERT encoder. We cannot reproduce the BERT results reported in Chen et al. (2021) so that we also report the results of our version of BERT.

| Ablation Models | Micro-F1 | Macro-F1 |
| :--- | :---: | :---: |
| BERT | 85.75 | 79.36 |
| HGCLR | $\mathbf{8 7 . 4 6}$ | $\mathbf{8 1 . 5 2}$ |
| -r.p. GCN | 87.06 | 80.63 |
| -r.p. GAT | 87.18 | 81.45 |
| -r.m. graph encoder | 86.67 | 80.11 |
| -r.m. contrastive loss | 86.72 | 80.97 |

> **Table 3**: Performance when replace or remove some components of HGCLR on the development set of WOS. r.p. stands for replace and r.m. stands for remove. We remove the contrastive loss by setting $\lambda=0$.


The model without contrastive loss is similar to a pure data augmentation approach, where positive examples stand as augment data. As the last row of Table 3, on development set, both the positive pair generation strategy and the contrastive learning framework have contributions to the model. Our data generation strategy is effective even without contrastive learning, improving BERT encoder by around $1 \%$ on two measurements. Contrastive learning can further boost performance by regularizing text representation.

We further analyze the effect of incorporating label hierarchy, the Graphormer, and the positive samples generation strategy in detail.

#### 5.3.1. Effect of Hierarchy

Our approach attempts to incorporating hierarchy into the text representation, which is fed into a linear layer for probabilities as in Equation 16. The weight matrix $W_{C}$ can be viewed as label representations and we plot theirs T-SNE projections under default configuration. Since a label and its father should be classified simultaneously, the representation of a label and its father should be similar. Thus, if the hierarchy is injected into the text representation, labels with the same father should have more similar representation to each other than those with a different father. As illustrated in Figure 3, label representations of BERT are scattered while label representations of our approach are clustered, which demonstrates that our text encoder can learn a hierarchy-aware representation.

| Variants of Graphormer | Micro-F1 | Macro-F1 |
| :--- | :---: | :---: |
| Base architecture | $\mathbf{8 7 . 4 6}$ | $\mathbf{8 1 . 5 2}$ |
| -w/o name embedding | 86.40 | 80.40 |
| -w/o spatial encoding | 86.88 | 80.42 |
| -w/o edge encoding | 87.25 | 80.54 |

> **Table 4**: Performance with variants of Graphormer on development set of WOS. We remove name embedding, spatial encoding, and edge encoding respectively. "w/o" stands for "without".

| Generation Strategy | Micro-F1 | Macro-F1 |
| :--- | :---: | :---: |
| Hierarchy-guided | $\mathbf{8 7 . 4 6}$ | $\mathbf{8 1 . 5 2}$ |
| Dropout | 86.94 | 79.91 |
| Random masking | 87.19 | 81.16 |
| Adversarial attack | 86.67 | 80.24 |

> **Table 5**: Impact of different positive example generation techniques on the development set of WOS. Hierarchy-guided is the proposed method. We control the valid tokens in positive samples roughly the same for random methods. We select FGSM as the attack algorithm following Pan et al. (2021).


#### 5.3.2. Effect of Graphormer

As for the components of the Graphormer, we validate the utility of name embedding, spatial encoding, and edge encoding. As in Table 4, all three components contribute to embedding the graph. Edge encoding is the least useful among these three components. Edge encoding is supposed to model the edge features provided by the graph, but the hierarchy of HTC has no such information so that the effect of edge encoding is not fully embodied in this task. Name embedding contributes most among components. Previous work only initialize embedding weights with label name but we treat it as a part of input features. As a result, neglecting name embedding observes the largest drop, which may explain the poor performance on RCV1-V2.

#### 5.3.3. Effect of Positive Example Generation

To further illustrate the effect of our data generation approach, we compare it with a few generation strategies. Dropout (Gao et al., 2021) uses no positive sample generation techniques but contrasts on the randomness of the Dropout function using two identical models. Random masking (Meng et al., 2021) is similar to our approach except the remained tokens are randomly selected. Adversarial attack (Pan et al., 2021) generates positive examples by an attack on gradients.

> [!multi-column]
> > [!blank-container]
> > (a) *A high degree of uncertainty associated with the emission inventory for China tends to degrade the performance of chemical transport models in predicting PM2.5 concentrations especially on a daily basis. In this study a novel machine learning algorithm, Geographically Weighted Gradient Boosting Machine (GW-GBM), was developed by improving GBM through building spatial smoothing kernels to weigh the loss function...*
> > **Tags: CS, Machine Learning**
> 
> > [!blank-container]
> > (b) *Posterior reversible encephalopathy syndrome (PRES) is a reversible clinical and neuroradiological syndrome which may appear at any age and characterized by headache, altered consciousness, seizures, and cortical blindness...*
> > **Tags: Medical, Headache**
> 
> > **Figure 4**: Two fragments of the generated positive examples. Tokens in red are kept for positive examples. We omit a few unrelated tokens (such as $a$, the, or comma) for clarity.

As in Table 5, a duplication of the model as positive examples is effective but performs poorly. Instead of dropping information at neuron level, random masking drops entire tokens and boosts Macro-F1 by over 1\%, indicating the necessity of building hard enough contrastive examples. The adversarial attack can build hard-enough samples by gradient ascending and disturbance in the embedding space. But the disturbance is not regularized by hierarchy or labels so that it is less effective since there is no guarantee that the adversarial examples remain the label. Our approach guided the example construction by both the hierarchy and the labels, which accommodates with HTC most and achieves the best performance.

In Figure 4, we select two cases to further illustrate the effect of labels on positive samples generation. In the first case, word machine strongly indicates this passage belongs to Machine Learning so that it is kept for positive examples. In the second case, syndrome is related to Medical and PRES occurs several times among Headache. Because of the randomness of sampling, our approach cannot construct an example with all keywords. For instance, learning in case one or headache in case two is omitted in this trial, which adds more difficulties for contrastive examples.

## 6. Conclusion

In this paper, we present Hierarchy-guided Contrastive Learning (HGCLR) for hierarchy text classification. We adopt contrastive learning for migrating taxonomy hierarchy information into BERT encoding. To this end, we construct positive examples for contrastive learning under the guidance of a graph encoder, which learns label features from taxonomy hierarchy. We modify Graphormer, a state-of-the-art graph encoder, for better graph understanding. Comparing to previous approaches, our approach empirically achieves consistent improvements on two distinct datasets and comparable results on another one. All of the components we designed are proven to be effective.



##  A Trick for Token Selection

To make sure $P_{i}$ in Equation 10 can acquire gradients, we choose to modify token embedding instead of the token itself. As in Equation 6, $e_{i}$ is the token embedding of $x_{i}$ and can have gradient. The positive counterpart of $e_{i}$ is denoted as:

$\hat{e_{i}}=e_{i}\left(\left(P_{i}+\operatorname{Detach}\left(1-P_{i}\right)\right)\right.$ if $P_{i}>\gamma$ else 0$)$,

where Detach is a function that ignores the gradient of its input. Numerically, $\hat{e}_{i}$ is either $e_{i}$ or 0 depending on the threshold $\gamma$, which serves the same purpose as Equation 10. As for gradient,

$$
\frac{\partial \hat{e_{i}}}{\partial P_{i}}=e_{i} \text { if } P_{i}>\gamma \text { else } 0
$$

which makes $P_{i}$ can be updated by backpropagation.

