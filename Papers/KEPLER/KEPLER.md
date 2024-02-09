---
title: "KEPLER: A Unified Model for Knowledge Embedding and Pre-trained Language Representation"
aliases:
    - KEPLER
authors:
  - Xiaozhi Wang
  - Tianyu Gao
  - Zhaocheng Zhu
  - Zhengyan Zhang
  - Zhiyuan Liu
  - Juanzi Li
  - Jian Tang
published: 2019-11-13
arXiv: "https://arxiv.org/abs/1911.06136"
paper: "https://arxiv.org/pdf/1911.06136.pdf"
tags:
    - Wikidata5M dataset
    - knowledge_graph KG
    - knowledge_embedding KE
---

# TL;DR
- Embed a entity description (node in the graph) with LM
- Train both LM and KG embeddings together
- Works well for inductive Knowledge Embedding model on Knowledge Graph link perdiction
- New benchmark dataset Wikidata5M

#### Abstract

Pre-trained language representation models (PLMs) cannot well capture factual knowledge from text. In contrast, knowledge embedding (KE) methods can effectively represent the relational facts in knowledge graphs (KGs) with informative entity embeddings, but conventional KE models cannot take full advantage of the abundant textual information. In this paper, we propose a unified model for Knowledge Embedding and Pre-trained LanguagE Representation (KEPLER), which can not only better integrate factual knowledge into PLMs but also produce effective text-enhanced KE with the strong PLMs. In KEPLER, we encode textual entity descriptions with a PLM as their embeddings, and then jointly optimize the KE and language modeling objectives. Experimental results show that KEPLER achieves state-of-the-art performances on various NLP tasks, and also works remarkably well as an inductive KE model on KG link prediction. Furthermore, for pretraining and evaluating KEPLER, we construct Wikidata5M[^1] , a large-scale KG dataset with aligned entity descriptions, and benchmark state-of-the-art KE methods on it. It shall serve as a new KE benchmark and facilitate the research on large KG, inductive KE, and KG with text. The source code can be obtained from https://github.com/THU-KEG/KEPLER

[^1]: https://deepgraphlearning.github.io/project/wikidata5m

## 1. Introduction

Recent pre-trained language representation models (PLMs) such as [[../BERT|BERT]] and [[../RoBERTa|RoBERTa]] learn effective language representation from large-scale unstructured corpora with language modeling objectives and have achieved superior performances on various natural language processing (NLP) tasks. Existing PLMs learn useful linguistic knowledge from unlabeled text (Liu et al., 2019a), but they generally cannot well capture the world facts, which are typically sparse and have complex forms in text (Petroni et al., 2019; Logan et al., 2019).

> [!blank-container]
> ![[figure1.svg]]
> 
> > Figure 1: An example of a KG with entity descriptions. The figure suggests that descriptions contain abundant information about entities and can help to represent the relational facts between them.
> 

By contrast, knowledge graphs (KGs) contain extensive structural facts, and knowledge embedding (KE) methods (Bordes et al., 2013; Yang et al., 2015; Sun et al., 2019) can effectively embed them into continuous entity and relation embeddings. These embeddings can not only help with the KG completion but also benefit various NLP applications (Yang and Mitchell, 2017; Zaremoodi et al., 2018; Han et al., 2018a). As shown in Figure 1, textual entity descriptions contain abundant information. Intuitively, KE methods can provide factual knowledge for PLMs, while the informative text data can also benefit KE. Inspired by Xie et al. (2016), we use entity descriptions to bridge the gap between KE and PLM, and align the semantic space of text to the symbol space of KGs (Logeswaran et al., 2019). We propose KEPLER, a unified model for Knowledge Embedding and Pre-trained LanguagE Representation. We encode the texts and entities into a unified semantic space with the same PLM as the encoder, and jointly optimize the KE and the masked language modeling (MLM) objectives. For the KE objective, we encode the entity descriptions as entity embeddings and then train them in the same way as conventional KE methods. For the MLM objective, we follow the approach of existing PLMs (Devlin et al., 2019; Liu et al., 2019c). KEPLER has the following strengths:

As a PLM, 
1. KEPLER is able to integrate factual knowledge into language representation with the supervision from KG by the KE objective. 
2. KEPLER inherits the strong ability of language understanding from PLMs by the MLM objective. 
3. The KE objective enhances the ability of KEPLER to extract knowledge from text since it requires the model to encode the entities from their corresponding descriptions. 
4. KEPLER can be directly adopted in a wide range of NLP tasks without additional inference overhead compared to conventional PLMs since we just add new training objectives without modifying model structures.

There are also some recent works (Zhang et al., 2019; Peters et al., 2019; Liu et al., 2020) directly integrating fixed entity embeddings into PLMs to provide external knowledge. However, 
1. their entity embeddings are learned by a separate KE model, and thus cannot be easily aligned with the language representation space. 
2. They require an entity linker to link the text to the corresponding entities, making them suffer from the error propagation problem. 
3. Compared to vanilla PLMs, their sophisticated mechanisms to link and use entity embeddings lead to additional inference overhead.

As a KE model, 
1. KEPLER can take full advantage of the abundant information from entity descriptions with the help of the MLM objective. 
2. KEPLER is capable of performing KE in the inductive setting, i.e., it can produce embeddings for unseen entities from their descriptions, while conventional KE methods are inherently transductive and they can only learn representations for the shown entities during training. Inductive KE is essential for many real-world applications, such as updating KGs with emerging entities and KG construction, and thus is worth more investigation.

For pre-training and evaluating KEPLER, we need a KG with 
1. large amounts of knowledge facts, 
2. aligned entity descriptions, and
3. reasonable inductive-setting data split, which cannot be satisfied by existing KE benchmarks. 
Therefore, we construct Wikidata5M[^1], containing about 5M entities, 20M triplets, and aligned entity descriptions from Wikipedia. To the best of our knowledge, it is the largest general-domain KG dataset. We also benchmark several classical KE methods and give data splits for both the transductive and the inductive settings to facilitate future research.

To summarize, our contribution is three-fold:
1. We propose KEPLER, a knowledge-enhanced PLM by jointly optimizing the KE and MLM objectives, which brings great improvements on a wide range of NLP tasks. 
2. By encoding text descriptions as entity embeddings, KEPLER shows its effectiveness as a KE model, especially in the inductive setting. 
3. We also introduce Wikidata5M[^1], a new large-scale KG dataset, which shall promote the research on large-scale KG, inductive KE, and the interactions between KG and NLP.

## 2. KEPLER

As shown in [[#^cfec69|Figure 2]], KEPLER implicitly incorporates factual knowledge into language representations by jointly training with two objectives. In this section, we detailedly introduce the encoder structure, the KE and MLM objectives, and how we combine the two as a unified model.

> [!blank-container]
> 
> ![[figure2.svg]]
> > Figure 2: The KEPLER framework. We encode entity descriptions as entity embeddings and jointly train the knowledge embedding (KE) and masked language modeling (MLM) objectives on the same PLM.
^cfec69


### 2.1. Encoder

For the text encoder, we use [[../Attention is all you need|Transformer architecture]] in the same way as [[../BERT|BERT]] and Liu et al. (2019c). The encoder takes a sequence of $N$ tokens $\left(x_{1}, \ldots, x_{N}\right)$ as inputs, and computes $L$ layers of $d$-dimensional contextualized representations $\mathbf{H}_{i} \in \mathbb{R}^{N \times d}, 1 \leq$ $i \leq L$. Each layer of the encoder $E_{i}$ is a combination of a multi-head self-attention network and a multi-layer perceptron, and the encoder gets the representation of each layer by $\mathbf{H}_{i}=\mathrm{E}_{i}\left(\mathbf{H}_{i-1}\right)$. Eventually, we get a contextualized representation for each position, which could be further used in downstream tasks. Usually, there is a special token $\langle CLS \rangle$ added to the beginning of the text, and the output at $\langle CLS \rangle$ is regarded sentence representation. We denote the representation function as $\mathrm{E}_{\langle CLS\rangle}(\cdot)$.

The encoder requires a tokenizer to convert plain texts into sequences of tokens. Here we use the same tokenization as RoBERTa: the Byte-Pair Encoding (BPE) (Sennrich et al., 2016).

Unlike previous knowledge-enhanced PLM works ([[../ERNIE|ERNIE]]; [[../Knowledge Enhanced Contextual Word Representations|Peters et al., 2019]]), we do not modify the Transformer encoder structure to add external entity linkers or knowledge integration layers. It means that our model has no additional inference overhead compared to vanilla PLMs, and it makes applying KEPLER in downstream tasks as easy as [[../RoBERTa|RoBERTa]].

### 2.2. Knowledge Embedding

To integrate factual knowledge into KEPLER, we adopt the knowledge embedding (KE) objective in our pre-training. KE encodes entities and relations in knowledge graphs (KGs) as distributed representations, which benefits lots of downstream tasks, such as link prediction and relation extraction.

We first define KGs: a KG is a graph with entities as its nodes and relations between entities as its edges. We use a triplet $(h, r, t)$ to describe a relational fact, where $h, t$ are the head entity and the tail entity, and $r$ is the relation type within a predefined relation set $\mathcal{R}$. In conventional KE models, each entity and relation is assigned a $d$-dimensional vector, and a scoring function is defined for training the embeddings and predicting links.

In KEPLER, instead of using stored embeddings, we encode entities into vectors by using their corresponding text. By choosing different textual data and different KE scoring functions, we have multiple variants for the KE objective of KEPLER. In this paper, we explore three simple but effective ways: entity descriptions as embeddings, entity and relation descriptions as embeddings, and entity embeddings conditioned on relations. We leave exploring advanced KE methods as our future work.

##### Entity Descriptions as Embeddings 
For a relational triplet $(h, r, t)$, we have:

$$
\begin{aligned}
\mathbf{h} & =\mathrm{E}_{\langle CLS\rangle}\left(\operatorname{text}_{h}\right), \\
\mathbf{t} & =\mathrm{E}_{\langle CLS\rangle}\left(\operatorname{text}_{t}\right), \\
\mathbf{r} & =\mathbf{T}_{r},
\end{aligned} \tag{1}
$$

^ecd929

where $text_{h}$ and $text_{t}$ are the descriptions for $h$ and $t$, with a special token $\langle CLS\rangle$ at the beginning. $\mathbf{T} \in \mathbb{R}^{|\mathcal{R}| \times d}$ is the relation embeddings and $\mathbf{h}, \mathbf{t}, \mathbf{r}$ are the embeddings for $h, t$ and $r$.

We use the loss from Sun et al. (2019) as our KE objective, which adopts negative sampling (Mikolov et al., 2013) for efficient optimization:

$$
\mathcal{L}_{\mathrm{KE}}=-\log \sigma\left(\gamma-d_{r}(\mathbf{h}, \mathbf{t})\right) 
-\sum_{i=1}^{n} \frac{1}{n} \log \sigma\left(d_{r}\left(\mathbf{h}_{\mathbf{i}}^{\prime}, \mathbf{t}_{\mathbf{i}}^{\prime}\right)-\gamma\right), \tag{2}
$$

^7238d5

where $\left(h_{i}^{\prime}, r, t_{i}^{\prime}\right)$ are negative samples, $\gamma$ is the margin, $\sigma$ is the sigmoid function, and $d_{r}$ is the scoring function, for which we choose to follow TransE (Bordes et al., 2013) for its simplicity,

$$
d_{r}(\mathbf{h}, \mathbf{t})=\|\mathbf{h}+\mathbf{r}-\mathbf{t}\|_{p}, \tag{3}
$$

^230c98

where we take the norm $p$ as 1 . The negative sampling policy is to fix the head entity and randomly sample a tail entity, and vice versa.

##### Entity and Relation Descriptions as Embeddings 
A natural extension for the last method is to encode the relation descriptions as relation embeddings as well. Formally, we have,

$$
\hat{\mathbf{r}}=\mathrm{E}_{<\mathrm{s}>}\left(\text { text }_{r}\right), \tag{4}
$$

^1007c8

where $text_{r}$ is the description for the relation $r$. Then we use $\hat{\mathbf{r}}$ to replace $\mathbf{r}$ in [[#^7238d5|(2)]] and [[#^230c98|(3)]].

##### Entity Embeddings Conditioned on Relations 
In this manner, we use entity embeddings conditioned on $r$ for better KE performances. The intuition is that semantics of an entity may have multiple aspects, and different relations focus on different ones (Lin et al., 2015). So we have,

$$
\mathbf{h}_{r}=\mathrm{E}_{<\mathrm{s}>}\left(\text { text }_{h, r}\right), \tag{5}
$$

^1ec4aa

where $t e x t_{h, r}$ is the concatenation of the description for the entity $h$ and the description for the relation $r$, with the special token $\langle CLS\rangle$ at the beginning and $\langle SEP \rangle$ in between. Correspondingly, we use $\mathbf{h}_{r}$ instead of $\mathbf{h}$ for [[#^7238d5|(2)]] and [[#^230c98|(3)]].

### 2.3. Masked Language Modeling

The masked language modeling (MLM) objective is inherited from [[../BERT|BERT]] and [[../RoBERTa|RoBERTa]]. During pretraining, MLM randomly selects some of the input positions, and the objective is to predict the tokens at these selected positions within a fixed dictionary.

To be more specific, MLM randomly selects 15% of input positions, among which 80% are masked with the special token $\langle \text{mask} \rangle$, 10% are replaced by other random tokens, and the rest remain unchanged. For each selected position $j$, the last layer of the contextualized representation $\mathbf{H}_{L, j}$ is used for a $W$-way classification, where $W$ is the size of the dictionary. At last, a cross-entropy loss $\mathcal{L}_{\text {MLM }}$ is calculated over these selected positions.

We initialize our model with the pre-trained checkpoint of RoBERTa<sub>BASE</sub>. However, we still keep MLM as one of our objectives to avoid catastrophic forgetting (McCloskey and Cohen, 1989) while training towards the KE objective. Actually, as demonstrated in [[#5.1. Ablation Study|Section 5.1]], only using the KE objective leads to poor results in NLP tasks.

### 2.4. Training Objectives

To incorporate factual knowledge and language understanding into one PLM, we design a multitask loss as shown in [[#^cfec69|Figure 2]] and [[#^0ff246|(6)]],

$$
\mathcal{L}=\mathcal{L}_{\mathrm{KE}}+\mathcal{L}_{\mathrm{MLM}} \tag{6}
$$

^0ff246

where $\mathcal{L}_{\mathrm{KE}}$ and $\mathcal{L}_{\mathrm{MLM}}$ are the losses for KE and MLM correspondingly. Jointly optimizing the two objectives can implicitly integrate knowledge from external KGs into the text encoder, while preserving the strong abilities of PLMs for syntactic and semantic understanding. Note that those two tasks only share the text encoder, and for each minibatch, text data sampled for KE and MLM are not (necessarily) the same. This is because seeing a variety of text (instead of just entity descriptions) in MLM can help the model to have better language understanding ability.

### 2.5. Variants and Implementations

We introduce the variants of KEPLER and the pretraining implementations here. The fine-tuning details will be introduced in [[#4. Experiments|§4]] .

#### KEPLER Variants

We implement multiple versions of KEPLER in experiments to explore the effectiveness of our pretraining framework. We use the same denotations in [[#4. Experiments|§4]] as below.

**KEPLER-Wiki** is the principal model in our experiments, which adopts Wikidata5M ([[#3. Wikidata5M|§3]]) as the KG and the entity-description-as-embedding method (Equation 1). All other variants, if not specified, use the same settings. KEPLER-Wiki achieves the best performances on most tasks. ^17c09d

**KEPLER-WordNet** uses the WordNet (Miller, 1995) as its KG source. WordNet is an English lexical graph, where nodes are lemmas and synsets, and edges are their relations. Intuitively, incorporating WordNet can bring lexical knowledge and thus benefits NLP tasks. We use the same WordNet 3.0 as in KnowBert (Peters et al., 2019), which is extracted from the `nltk`[^2] package. ^4751ae

[^2]: https://www.nltk.org

**KEPLER-W+W** takes both Wikidata5M and WordNet as its KGs. To jointly train with two KG datasets, we modify the objective in [[#^0ff246|(6)]] as ^b68011

$$
\mathcal{L}=\mathcal{L}_{\text {Wiki }}+\mathcal{L}_{\text {WordNet }}+\mathcal{L}_{\text {MLM }},\tag{7}
$$

where $\mathcal{L}_{\text {Wiki }}$ and $\mathcal{L}_{\text {WordNet }}$ are losses from Wikidata5M and WordNet respectively.

**KEPLER-Rel** uses the entity and relation descriptions as embeddings method [[#^1007c8|(4)]]. As the relation descriptions in Wikidata are short (11.7 words on average) and homogeneous, encoding relation descriptions as relation embeddings results in worse performance as shown in [[#4. Experiments|§4]]. ^d9a2b7

**KEPLER-Cond** uses the entity-embedding conditioned-on-relation method [[#^1ec4aa|(5)]]. This model achieves superior results in link prediction tasks, both transductive and inductive ([[#4.3 KE Tasks|§4.3]]). ^2fb7b1


> [!blank-container|right-large]
> | Dataset    |    \#entity | \#relation |   \#training | \#validation | \#test |
> |:---------- | -----------:| ----------:| ------------:| ------------:| ------:|
> | FB15K      |      14,951 |      1,345 |      483,142 |       50,000 | 59,071 |
> | WN18       |      40,943 |         18 |      141,442 |        5,000 |  5,000 |
> | FB15K-237  |      14,541 |        237 |      272,115 |       17,535 | 20,466 |
> | WN18RR     |      40,943 |         11 |       86,835 |        3,034 |  3,134 |
> | Wikidata5M | $4,594,485$ |        822 | $20,614,279$ |        5,163 |  5,133 |
> 
> > Table 1: Statistics of Wikidata5M (transductive setting) compared with existing KE benchmarks.
^f8f8fb

**KEPLER-OnlyDesc** trains the MLM objective directly on the entity descriptions from the KE objective rather than uses the English Wikipedia and BookCorpus as other versions of KEPLER. However, as the entity description data are smaller (2.3 GB vs 13 GB) and homogeneous, it harms the general language understanding ability and thus performs worse (Section 4.2). ^f92afb

**KEPLER-KE** only adopts the KE objective in pre-training, which is an ablated version of KEPLER-Wiki. It is used to show the necessity of the MLM objective for language understanding. ^cc18b2

#### Pre-training Implementation

In practice, we choose [[../RoBERTa|RoBERTa]] as our base model and implement KEPLER in the fairseq framework (Ott et al., 2019) for pretraining. Due to the computing resource limit, we choose the BASE size $(L=12, d=768)$ and use the released `roberta.base` parameters for initialization, which is a common practice to save pre-training time (Zhang et al., 2019; Peters et al., 2019). For the MLM objective, we use the English Wikipedia (2,500M words) and BookCorpus (800M words) (Zhu et al., 2015) as our pre-training corpora (except [[#^f92afb|KEPLER-OnlyDesc]]). We extract text from these two corpora in the same way as Devlin et al. (2019). For the KE objective, we encode the first 512 tokens of entity descriptions from the English Wikipedia as entity embeddings.

We set the $\gamma$ in [[#^7238d5|(2)]] as 4 and 9 for NLP and KE tasks respectively, and we use the models pre-trained with 10 and 30 epochs for NLP and KE. Specially, the $\gamma$ is 1 for [[#^4751ae|KEPLER-WordNet]]. The two hyper-parameters are tuned by multiple trials for $\gamma$ in $\{1,2,4,6,9\}$ and the number of epochs in $\{5,10,20,30,40\}$, and we select the model by performances on TACRED (F-1) and inductive link prediction (HITS@10). We use gradient accumulation to achieve a batch size of 12,288 .

> [!blank-container|right-medium]
> | Entity Type      |  Occurrence | Percentage |
> |:---------------- | -----------:| ----------:|
> | Human            | 1,517,591 |  33.0 % |
> | Taxon            |     363,882 |   7.9% |
> | Wikimedia list   |     118,823 |   2.6% |
> | Film             |     114,266 |   2.5% |
> | Human Settlement |     110,939 |   2.4% |
> | Total            | 2,225,501 |  48.4% |
> 
> > Table 2: Top-5 entity categories in Wikidata5M.

^95287b

## 3. Wikidata5M

As shown in [[#2. KEPLER|§2]], to train KEPLER, the KG dataset should 
1. be large enough, 
2. contain high quality textual descriptions for its entities and relations, and 
3. have a reasonable inductive setting, which most existing KG datasets do not provide. 

Thus, based on Wikidata[^3] and English Wikipedia[^4], we construct Wikidata5M, a large-scale KG dataset with aligned text descriptions from corresponding Wikipedia pages, and also an inductive test set. In the following sections, we first introduce the data collection ([[#3.1. Data Collection|§ 3.1]]) and the data split (Section 3.2), and then provide the results of representative KE methods on the dataset (Section 3.3).

[^3]: https://www.wikidata.org 
[^4]: https://en.wikipedia.org

### 3.1. Data Collection

We use the July 2019 dump of Wikidata and Wikipedia. For each entity in Wikidata, we align it to its Wikipedia page and extract the first section as its description. Entities with no pages or with descriptions fewer than 5 words are discarded.

We retrieve all the relational facts in Wikidata. A fact is considered to be valid when both of its entities are not discarded, and its relation has a nonempty page in Wikidata. The final KG contains 4, 594,485 entities, 822 relations and 20,624, 575 triplets. Statistics of Wikidata5M along with four other widely-used benchmarks are shown in Table 1. Top- 5 entity categories are listed in [[#^95287b|Table 2]] . We can see that Wikidata5M is much larger than other KG datasets, covering various domains.

### 3.2. Data Split

For Wikidata5M, we take two different settings: the transductive setting and the inductive setting. 

The transductive setting (shown in [[#^f8f8fb|Table 1]]) is adopted in most KG datasets, where the entities are shared and the triplet sets are disjoint across training, validation and test. In this case, KE models are expected to learn effective entity embeddings only for the entities in the training set.

> [!blank-container|right-medium]
> | Subset     |    entity |  relation |     triplet |
> |:---------- | -----------:| ----------:| ------------:|
> | Training   | 4,579,609 |        822 | 20,496,514 |
> | Validation |       7,374 |        199 |        6,699 |
> | Test       |       7,475 |        201 |        6,894 |
> 
> > Table 3: Statistics of Wikidata5M inductive setting.
> ^878d55

In the inductive setting (shown in [[#^878d55|Table 3]]), the entities and triplets are mutually disjoint across training, validation and test. We randomly sample some connected subgraphs as the validation and test set. In the inductive setting, the KE models should produce embeddings for the unseen entities given side features like descriptions, neighbors, etc. The inductive setting is more challenging and also meaningful in real-world applications, where entities in KGs experience open-ended growth, and the inductive ability is crucial for online KE methods.

Although Wikidata5M contains massive entities and triplets, our validation and test set are not large, which is limited by the standard evaluation method of link prediction ([[#3.3. Benchmark|§ 3.3]]). Each episode of evaluation requires $|\mathcal{E}| \times|\mathcal{T}| \times 2$ times of KE score calculation, where $|\mathcal{E}|$ and $|\mathcal{T}|$ are the total number of entities and the number of triplets in test set respectively. As Wikidata5M contains massive entities, the evaluation is very time-consuming, hence we have to limit the test set to thousands of triplets to ensure tractable evaluations. This indicates that large-scale KE urges a more efficient evaluation protocol. We will leave exploring it to future work.

### 3.3. Benchmark

To assess the challenges of Wikidata5M, we benchmark several popular KE models on our dataset in the transductive setting (as they inherently do not support the inductive setting). Because their original implementations do not scale to Wikidata5M, we benchmark these methods with [GraphVite](https://graphvite.io/), a multi-GPU KE toolkit.

> [!blank-container|right]
> | Method | MR | MRR | HITS@ 1 | HITS@3 | HITS@ 10 |
> | :--- | ---: | ---: | ---: | ---: | ---: |
> | TransE (Bordes et al., 2013) | 109370 | 25.3 | 17.0 | 31.1 | 39.2 |
> | DistMult (Yang et al., 2015) | 211030 | 25.3 | 20.8 | 27.8 | 33.4 |
> | ComplEx (Trouillon et al., 2016) | 244540 | 28.1 | 22.8 | 31.0 | 37.3 |
> | SimplE (Kazemi and Poole, 2018) | 115263 | 29.6 | 25.2 | 31.7 | 37.7 |
> | RotatE (Sun et al., 2019) | 89459 | 29.0 | 23.4 | 32.2 | 39.0 |
> > Table 4: Performances of different KE models on Wikidata5M (\% except MR).

In the transductive setting, for each test triplet $(h, r, t)$, the model ranks all the entities by scoring $\left(h, r, t^{\prime}\right), t^{\prime} \in \mathcal{E}$, where $\mathcal{E}$ is the entity set excluding other correct $t$. The evaluation metrics, MRR (mean reciprocal rank), MR (mean rank), and HITS@ $\{1,3,10\}$, are based on the rank of the correct tail entity $t$ among all the entities in $\mathcal{E}$. Then we do the same thing for the head entities. We report the average results over all test triplets and over both head and tail entity predictions.

Table 4 shows the results of popular KE methods on Wikidata5M, which are all significantly lower than on existing KG datasets like FB15K237, WN18RR, etc. It demonstrates that Wikidata5M is more challenging due to its large scale and high coverage. The results advocate for more efforts towards large-scale KE.

## 4. Experiments

In this section, we introduce the experiment settings and results of our model on various NLP and KE tasks, along with some analyses on KEPLER.

### 4.1. Experimental Setting

#### Baselines
In our experiments, [[../RoBERTa|RoBERTa]] is an important baseline since KEPLER is based on it (all mentioned models are of BASE size if not specified). As we cannot afford the full RoBERTa corpora (126 GB, and we only use $13 \mathrm{~GB}$ ) in KEPLER pre-training, we implement Our RoBERTa for direct comparisons to KEPLER. It is initialized by RoBERTa $_{\text {BASE }}$ and is further trained with the MLM objective on the same corpora as KEPLER.

We also evaluate recent knowledge-enhanced PLMs, including ERNIE<sub>BERT</sub> (Zhang et al., 2019) and KnowBert<sub>BERT</sub> (Peters et al., 2019). As ERNIE and our principal model [[#^17c09d|KEPLER-Wiki]] only use Wikidata, we take KnowBert-Wiki in the experiments to ensure fair comparisons with the same knowledge source. Considering KEPLER is based on RoBERTa, we reproduce the two models with RoBERTa too (ERNIE<sub>RoBERTa</sub> and KnowBert<sub>RoBERTa</sub>). The reproduction of KnowBert is based on its original implementation[^5]. On relation classification, we also compare with MTB (Baldini Soares et al., 2019), which adopts "matching the blank" pre-training. Different from other baselines, the original MTB is based on BERT<sub>LARGE</sub> (denoted by MTB (BERT<sub>LARGE</sub>) ). For a fair comparison under the same model size, we reimplement MTB with BERT BASE $_{\text {(MTB). }}$

[^5]: https://github.com/allenai/kb

#### Hyper-parameter 
The pre-training settings are in Section 2.5. For fine-tuning on downstream tasks, we set KEPLER hyper-parameters the same as reported in KnowBert on TACRED and OpenEntity. On FewRel, we set the learning rate as 2e-5 and batch size as 20 and 4 for the Proto and PAIR frameworks respectively. For [[../GLUE|GLUE]], we follow the hyper-parameters reported in RoBERTa. For baselines, we keep their original hyper-parameters unchanged or use the best trial in KEPLER searching space if no original settings are available.

### 4.2. NLP Tasks

In this section, we demonstrate the performance of KEPLER and its baselines on various NLP tasks.

#### Relation Classification


> [!blank-container|right-medium]
> 
> |             Model             |   **P**   |  **R**   |  **F-1**  |
> |:-----------------------------:|:---------:|:--------:|:---------:|
> |             BERT              |   67.2    |   64.8   |   66.0    |
> |     BERT<sub>LARGE</sub>      |     -     |    -     |   70.1    |
> |              MTB              |   69.7    |   67.9   |   68.8    |
> |   MTB(BERT<sub>LARGE</sub>)    |     -     |    -     |   71.5    |
> |     ERNIE<sub>BERT</sub>      |   70.0    |   66.1   |   68.0    |
> |    KnowBert<sub>BERT</sub>    | **73.5**  |   64.1   |   68.5    |
> |            RoBERTa            |   70.4    |   71.1   |   70.7    |
> |    ERNIE<sub>RoBERTa</sub>    | ** 73.5** |   68.0   |   70.7    |
> |  KnowBert<sub>RoBERTa</sub>   |   71.9    |   69.9   |   70.9    |
> |          Our RoBERTa          |   70.8    |   69.6   |   70.2    |
> |   [[#^17c09d\|KEPLER-Wiki]]   |   71.5    | **72.5** | **72 .0** |
> | [[#^4751ae\|KEPLER-WordNet]]  |   71.4    |   71.3   |   71.3    |
> |   [[#^b68011\|KEPLER-W+W]]    |   71.1    |   72.0   |   71.5    |
> |   [[#^d9a2b7\|KEPLER-Rel]]    |   71.3    |   70.9   |   71.1    |
> |   [[#^2fb7b1\|KEPLER-Cond]]   |   72.1    |   70.7   |   71.4    |
> | [[#^f92afb\|KEPLER-OnlyDesc]] |   72.3    |   69.1   |   70.7    |
> |    [[#^cc18b2\|KEPLER-KE]]    |   63.5    |   60.5   |   62.0    |
> 
> > Table 5: Precision, recall and F-1 on TACRED (\%). KnowBert results are different from the original paper since different task settings are used.


Relation classification requires models to classify relation types between two given entities from text. We evaluate KEPLER and other baselines on two widely-used benchmarks: TACRED and FewRel.

TACRED (Zhang et al., 2017) has 42 relations and 106, 264 sentences. Here we follow the settings of Baldini Soares et al. (2019), where we add four special tokens before and after the two entity mentions, and concatenate the representations at the beginnings of the two entities for classification. Note that the original KnowBert also takes entity types as inputs, which is different from Zhang et al. (2019); Baldini Soares et al. (2019). To ensure fair comparisons, we re-evaluate KnowBert with the same setting as other baselines, thus the reported results are different from the original paper.

From the TACRED results in Table 5, we can observe that: 
1. [[#^17c09d|KEPLER-Wiki]] is the best one among KEPLER variants and significantly outperforms all the baselines, while other versions of KEPLER also achieve good results. It demonstrates the effectiveness of KEPLER on integrating factual knowledge into PLMs. Based on the result, we use [[#^17c09d|KEPLER-Wiki]] as the principal model in the following experiments. 
2. [[#^4751ae|KEPLER-WordNet]] shows a marginal improvement over Our RoBERTa, while [[#^b68011|KEPLER-W+W]] underperforms [[#^17c09d|KEPLER-Wiki]]. It suggests that pre-training with WordNet only has limited benefits in the KEPLER framework. We will explore how to better combine different KGs in our future work.

FewRel (Han et al., 2018b) is a few-shot relation classification dataset with 100 relations and 70, 000 instances, which is constructed with Wikipedia text and Wikidata facts. Furthermore, Gao et al. (2019) propose [[../FewRel 2.0|FewRel 2.0]], adding a domain adaptation challenge with a new medical-domain test set.

FewRel takes the $N$-way $K$-shot setting. Relations in the training and test sets are disjoint. For every evaluation episode, $N$ relations, $K$ supporting samples for each relation, and several query sentences are sampled from the test set. The models are required to classify queries into one of the $N$ relations only given the sampled $N \times K$ instances.

> [!blank-container]
> | Model | FewRel 1.0 |  |  |  | FewRel 2.0 |  |  |  |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> |  | 5-1 | 5-5 | 10-1 | 10-5 | 5-1 | 5-5 | 10-1 | 10-5 |
> | $\operatorname{MTB}\left(\mathrm{BERT}_{\mathrm{LARGE}}\right)^{\dagger}$ | 93.86 | 97.06 | 89.20 | 94.27 | - | - | - | - |
> | Proto (BERT) | 80.68 | 89.60 | 71.48 | 82.89 | 40.12 | 51.50 | 26.45 | 36.93 |
> | Proto (MTB) | 81.39 | 91.05 | 71.55 | 83.47 | 52.13 | 76.67 | 48.28 | 69.75 |
> | Proto $\left(\mathrm{ERNIE}_{\mathrm{BERT}}\right)^{\dagger}$ | **89.43** | 94.66 | 84.23 | 90.83 | 49.40 | 65.55 | 34.99 | 49.68 |
> | Proto $\left(\text { KnowBert }_{\mathrm{BERT}}\right)^{\dagger}$ | 86.64 | 93.22 | 79.52 | 88.35 | 64.40 | 79.87 | 51.66 | 69.71 |
> | Proto (RoBERTa) | 85.78 | 95.78 | 77.65 | 92.26 | 64.65 | 82.76 | 50.80 | 71.84 |
> | Proto (Our RoBERTa) | 84.42 | 95.30 | 76.43 | 91.74 | 61.98 | 83.11 | 48.56 | 72.19 |
> | Proto $\left(\text { ERNIE }_{\text {RoBERTa }}\right)^{\dagger}$ | 87.76 | 95.62 | 80.14 | 91.47 | 54.43 | 80.48 | 37.97 | 66.26 |
> | Proto $\left(\text { KnowBert }{ }_{\text {RoBERTa }}\right)^{\dagger}$ | 82.39 | 93.62 | 76.21 | 88.57 | 55.68 | 71.82 | 41.90 | 58.55 |
> | Proto ([[#^17c09d\|KEPLER-Wiki]]) | 88.30 | **95.94** | 81.10 | 92.67 | 66.41 | 84.02 | $\mathbf{5 1 . 8 5}$ | 73.60 |
> | PAIR (BERT) | 88.32 | 93.22 | 80.63 | 87.02 | 67.41 | 78.57 | 54.89 | 66.85 |
> | PAIR (MTB) | 83.01 | 87.64 | 73.42 | 78.47 | 46.18 | 70.50 | 36.92 | 55.17 |
> | PAIR $\left(\text { ERNIE }_{\text {BERT }}\right)^{\dagger}$ | 92.53 | 94.27 | 87.08 | 89.13 | 56.18 | 68.97 | 43.40 | 54.35 |
> | PAIR $\left(\text { KnowBert }_{\mathrm{BERT}}\right)^{\dagger}$ | 88.48 | 92.75 | 82.57 | 86.18 | 66.05 | 77.88 | 50.86 | 67.19 |
> | PAIR (RoBERTa) | 89.32 | 93.70 | 82.49 | 88.43 | 66.78 | 81.84 | 53.99 | 70.85 |
> | PAIR (Our RoBERTa) | 89.26 | 93.71 | 83.32 | 89.02 | 63.22 | 77.66 | 49.28 | 65.97 |
> | PAIR $\left(\text { ERNIE }_{\text {RoBERTa }}\right)^{\dagger}$ | 87.46 | 94.11 | 81.68 | 87.83 | 59.29 | 72.91 | 48.51 | 60.26 |
> | PAIR (KnowBert RoBERTa $^{\dagger}$ | 85.05 | 91.34 | 76.04 | 85.25 | 50.68 | 66.04 | 37.10 | 51.13 |
> | PAIR ([[#^17c09d\|KEPLER-Wiki]]) | 90.31 | 94.28 | 85.48 | 90.51 | 67.23 | 82.09 | 54.32 | 71.01 |
> 
> > Table 6: Accuracies (\%) on the FewRel dataset. $N$-K indicates the $N$-way $K$-shot setting. MTB uses the LARGE size and all the other models use the BASE size. ${ }^{\dagger}$ indicates oracle models which may have seen facts in the FewRel 1.0 test set during pre-training.



We use two state-of-the-art few-shot frameworks: Proto ([[../Prototypical Networks for Few-shot Learning|Snell et al., 2017]]) and [[../FewRel 2.0|PAIR]]. We replace the text encoders with our baselines and KEPLER and compare the performance. Since FewRel 1.0 is constructed with Wikidata, we remove all the triplets in its test set from Wikidata5M to avoid information leakage for KEPLER. However, we cannot control the KGs used in our baselines. We mark the models utilizing Wikidata and have information leakage risk with ${ }^{\dagger}$ in Table 6.
As Table 6 shows, [[#^17c09d|KEPLER-Wiki]] achieves the best performance over the BASE-size PLMs in most settings. From the results, we also have some interesting observations: 
1. RoBERTa consistently outperforms [[../BERT|BERT]] on various NLP tasks (Liu et al., 2019c), yet the RoBERTa-based models here are comparable or even worse than [[../BERT|BERT]]-based models in the PAIR framework. Since PAIR uses sentence concatenation, this result may be credited to the next sentence prediction (NSP) objective of [[../BERT|BERT]]. 
2. KEPLER brings improvements on FewRel 2.0, while ERNIE and KnowBert even degenerate in most of the settings. It indicates that the paradigms of ERNIE and KnowBert cannot well generalize to new domains which may require much different entity linkers and entity embeddings. On the other hand, KEPLER not only learns better entity representations but also acquires a general ability to extract factual knowledge from the context across different domains. We further verify this in [[#5.5. Understanding Text or Storing Knowledge|§5.5]]. 
3. KnowBert underperforms ERNIE in FewRel while it typically achieves better results on other tasks. This may be because it uses the TuckER (Balazevic et al., 2019) KE model while ERNIE and KEPLER follow TransE (Bordes et al., 2013). We will explore the effects of different KE methods in the future.

We also have another two observations with regard to ERNIE and MTB: 
1. ERNIE performs the best on 1-shot settings of FewRel 1.0. We believe this is because that the knowledge embedding injection of ERNIE has particular advantages in this case, since it directly brings knowledge about entities. When using 5-shot (supporting text provides more information) and FewRel 2.0 (ERNIE does not have knowledge for biomedical entities), KEPLER outperforms ERNIE. 
2. Though MTB ([[../BERT|BERT]] LARGE $_{\text {) }}$ is the state-of-the-art model on FewRel, its [[../BERT|BERT]]<sub>BASE</sub> version does not outperform other knowledge-enhanced PLMs, which suggests that using large models contributes much to its gain. We also notice that when combined with PAIR, MTB suffers an obvious performance drop, which may be because its pre-training objective degenerates sentence-pair tasks.

#### Entity Typing


> [!blank-container|right-medium]
> 
> | Model | P | R | F-1 |
> | :--- | :---: | :---: | :---: |
> | UFET (Choi et al., 2018) | 77.4 | 60.6 | 68.0 |
> | BERT | 76.4 | 71.0 | 73.6 |
> | ERNIE<sub>BERT</sub> | 78.4 | 72.9 | 75.6 |
> | KnowBert<sub>BERT</sub> | 77.9 | 71.2 | 74.4 |
> | RoBERTa $_{\text {ERNIE }_{\text {RoBERTa }}}$ | 77.4 | 73.6 | 75.4 |
> | KnowBert<sub>RoBERTa</sub> | 78.3 | 70.2 | 74.9 |
> | Our RoBERTa | 75.1 | 73.4 | 75.6 |
> | [[#^17c09d\|KEPLER-Wiki]] | 77.8 | 74.6 | $\mathbf{7 6 . 2}$ |
> 
> > Table 7: Entity typing results on OpenEntity (\%).
> 


Entity typing requires to classify given entity mentions into pre-defined types. For this task, we carry out evaluations on OpenEntity (Choi et al., 2018) following the settings in Zhang et al. (2019). OpenEntity has 6 entity types and 2,000 instances for training, validation and test each.

To identify the entity mentions of interest, we add two special tokens before and after the entity spans, and use the representations of the first special tokens for classification. As shown in Table 7, [[#^17c09d|KEPLER-Wiki]] achieves state-of-the-art results. Note that the KnowBert results are different from the original paper since we use KnowBertWiki here rather than KnowBert-W+W to ensure the same knowledge resource and fair comparisons. KEPLER does not perform linking or entity embedding pre-training like ERNIE and KnowBert, which bring them special advantages in entity span tasks. However, KEPLER still outperforms these baselines, which proves its effectiveness.

#### GLUE


> [!blank-container]
> | Model           | $\begin{array}{c}\text { MNLI }(\mathbf{m} / \mathbf{m m}) \\ \mathbf{3 9 2}\end{array}$ | $\begin{array}{c}\text { QQP } \\ \mathbf{3 6 3 K}\end{array}$ | $\begin{array}{c}\text { QNLI } \\ \mathbf{1 0 4 K}\end{array}$ | $\begin{array}{c}\text { SST-2 } \\ \mathbf{6 7 K}\end{array}$ |
> |:--------------- |:----------------------------------------------------------------------------------------:|:--------------------------------------------------------------:|:---------------------------------------------------------------:|:--------------------------------------------------------------:|
> | RoBERTa         |                                      87.5 / 87.2                                       |                              91.9                              |                              92.7                               |                              94.8                              |
> | Our RoBERTa     |                                      87.1 / 86.8                                       |                              90.9                              |                              92.5                               |                              94.7                              |
> | [[#^17c09d\|KEPLER-Wiki]]     |                                      87.2 / 86.5                                       |                              91.7                              |                              92.4                               |                              94.5                              |
> | [[#^f92afb\|KEPLER-OnlyDesc]] |                                      85.9 / 85.6                                       |                              90.8                              |                              92.4                               |                              94.4                              |
> | Model           |                                           CoLA                                           |                             STS-B                              |                              MRPC                               |                              RTE                               |
> |                 |                                    **8.5K**                                    |                       **5.7K**                     |                       **3.5K**                        |                       **2.5K**                      |
> | RoBERTa         |                                    63.6                                   |                              91.2                              |                              90.2                               |                              80.9                              |
> | Our RoBERTa     |                                    63.4                                   |                              91.1                              |                              88.4                               |                              82.3                              |
> | [[#^17c09d\|KEPLER-Wiki]]     |                                           63.6                                           |                              91.2                              |                              89.3                               |                              85.2                              |
> | [[#^f92afb\|KEPLER-OnlyDesc]] |                                           55.8                                           |                              90.2                              |                              88.5                               |                              78.3                              |
> 
> > Table 8: GLUE results on the dev set (\%). All the results are medians over 5 runs. We report F-1 scores for QQP and MRPC, Spearman correlations for STS-B, and accuracy scores for the other tasks. The " $\mathrm{m} / \mathrm{mm}$ " stands for matched/mismatched evaluation sets for MNLI (Williams et al., 2018).
> 

The General Language Understanding Evaluation (GLUE) (Wang et al., 2019b) collects several natural language understanding tasks and is widely used for evaluating PLMs. In general, solving GLUE does not require factual knowledge (Zhang et al., 2019) and we use it to examine whether KEPLER harms the general language understanding ability.

Table 8 shows the GLUE results. We can observe that [[#^17c09d|KEPLER-Wiki]] is close to Our RoBERTa, suggesting that while incorporating factual knowledge, KEPLER maintains a strong language understanding ability. However, there are significant performance drops of [[#^f92afb|KEPLER-OnlyDesc]], which indicates that the small-scale entity description data are not sufficient for training KEPLER with MLM.

For the small datasets STS-B, MRPC and RTE, directly fine-tuning models on them typically result in unstable performance. Hence we fine-tune models on a large-scale dataset (here we use MNLI) first and then further fine-tune them on the small datasets. The method has been shown to be effective (Wang et al., 2019a) and is also used in the original RoBERTa paper (Liu et al., 2019c).

### 4.3 KE Tasks

We show how KEPLER works as a KE model, and evaluate it on Wikidata5M in both the transductive link prediction setting and the inductive setting.

#### Experimental Settings

In link prediction, the entity and relation embeddings of KEPLER are obtained as described in [[#2.2. Knowledge Embedding|Section 2.2]] and [[#2.5. Variants and Implementations|2.5]]. The evaluation method is described in [[#3.3. Benchmark|Section 3.3]]. We also add RoBERTa and Our RoBERTa as baselines. They adopt [[#^ecd929|Equation 1]] and 4 to acquire entity and relation embeddings, and use [[#^230c98|(3)]] as their scoring function.

In the transductive setting, we compare our models with TransE (Bordes et al., 2013). We set its dimension as 512 , negative sampling size as 64 , batch size as 2048 and learning rate as 0.001 after hyper-parameter searching. The negative sampling size is crucial for the performance on KE tasks, but limited by the model complexity, KEPLER can only take a negative size of 1 . For a direct comparison to intuitively show the benefits of pre-training, we set a baseline $\mathbf{TransE}^{\dagger}$, which also uses 1 as the negative sampling size and keeps the other hyperparameters unchanged.

Since conventional KE methods like TransE inherently cannot provide embeddings for unseen entities, we take DKRL (Xie et al., 2016) as our baseline in the KE experiments, which utilizes convolutional neural networks to encode entity descriptions as embeddings. We set its dimension as 768 , negative sampling size as 64, batch size as 1024 and learning rate as 0.0005 .

#### Transductive Setting

> [!blank-container|right-large]
> 
> | Model | MR | MRR | HITS@ 1 | HITS@ 3 | HITS@ 10 |
> | :--- | ---: | ---: | ---: | ---: | ---: |
> | TransE (Bordes et al., 2013) | 109370 | $\mathbf{2 5 . 3}$ | 17.0 | $\mathbf{3 1 . 1}$ | $\mathbf{3 9 . 2}$ |
> | TransE $^{\dagger}$ | 406957 | 6.0 | 1.8 | 8.0 | 13.6 |
> | DKRL (Xie et al., 2016) | 31566 | 16.0 | 12.0 | 18.1 | 22.9 |
> | RoBERTa | 1381597 | 0.1 | 0.0 | 0.1 | 0.3 |
> | Our RoBERTa | 1756130 | 0.1 | 0.0 | 0.1 | 0.2 |
> | [[#^cc18b2\|KEPLER-KE]] | 76735 | 8.2 | 4.9 | 8.9 | 15.1 |
> | [[#^d9a2b7\|KEPLER-Rel]] | 15820 | 6.6 | 3.7 | 7.0 | 11.7 |
> | [[#^17c09d\|KEPLER-Wiki]] | $\mathbf{1 4 4 5 4}$ | 15.4 | 10.5 | 17.4 | 24.4 |
> | <sub>LARGE</sub> | 20267 | 21.0 | $\mathbf{1 7 . 3}$ | 22.4 | 27.7 |
> 
> > Table 9a: Link prediction results on Wikidata5M transductive (\% except MR). TransE ${ }^{\dagger}$ denotes a TransE modeled trained with the same negative sampling size (1) as KEPLER.
> > 
> 

Table 9a shows the results of the transductive setting. We observe that:

1. KEPLER underperforms TransE. It is reasonable since KEPLER is limited by its large model size, and thus cannot use a large negative sampling size ( 1 for KEPLER, while typical KE methods use 64 or more) and more training epochs (30 vs 1000 for TransE), which are crucial for KE (Zhu et al., 2019). On the other hand, KEPLER and its variants perform much better than $\mathrm{TransE}^{\dagger}$ (with a negative sampling size of 1 ), showing that using the same negative sampling size, KEPLER can benefit from pre-trained language representations and textual entity descriptions so that outperform TransE. In the future, we will explore reducing the model size of KEPLER to take advantage of both large negative sampling size and pre-training.
2. The vanilla RoBERTa perform poorly in KE while KEPLER achieves favorable performances, which demonstrates the effectiveness of our multitask pre-training to infuse factual knowledge.
3. Among the KEPLER variants, KEPLERCond has superior results, which substantiates the intuition in Section 2.2. [[#^d9a2b7|KEPLER-Rel]] performs worst, which we believe is due to the short and homogeneous relation descriptions of Wikidata. [[#^cc18b2|KEPLER-KE]] significantly underperforms [[#^17c09d|KEPLER-Wiki]], which suggests that the MLM objective is necessary as well for the KE tasks to build effective language representation.
4. We also notice that DKRL performs well on the transductive setting and the result is close to KEPLER. We believe this is because DKRL takes a much smaller encoder (CNN) and thus is easier to train. In the more difficult inductive setting, the gap between DKRL and KEPLER is larger, which better shows the language understanding ability of KEPLER to utilize textual entity descriptions.

#### Inductive Setting


> [!blank-container|left-large]
> | Model | MR | MRR | HITS@ 1 | HITS@3 | HITS@ 10 |
> | :--- | ---: | ---: | ---: | ---: | ---: |
> | DKRL (Xie et al., 2016) | 78 | 23.1 | 5.9 | 32.0 | 54.6 |
> | RoBERTa | 723 | 7.4 | 0.7 | 1.0 | 19.6 |
> | Our RoBERTa | 1070 | 5.8 | 1.9 | 6.3 | 13.0 |
> | [[#^cc18b2\|KEPLER-KE]] | 138 | 17.8 | 5.7 | 22.9 | 40.7 |
> | [[#^d9a2b7\|KEPLER-Rel]] | 35 | 33.4 | 15.9 | 43.5 | 66.1 |
> | [[#^17c09d\|KEPLER-Wiki]] | 32 | 35.1 | 15.4 | 46.9 | 71.9 |
> | <sub>LARGE</sub> | $\mathbf{2 8}$ | $\mathbf{4 0 . 2}$ | $\mathbf{2 2 . 2}$ | $\mathbf{5 1 . 4}$ | $\mathbf{7 3 . 0}$ |
> 
> > Table 9b: Table 9: Link prediction results on Wikidata5M inductive settings on Wikidata5M (\% except MR).


Table 9b shows the Wikidata5M inductive results. KEPLER outperforms DKRL and RoBERTa by a large margin, demonstrating the effectiveness of our joint training method. But KEPLER results are still far from ideal performances required by practical applications (constructing KG from scratch, etc.), which urges further efforts on inductive KE. Comparisons among KEPLER variants are consistent with in the transductive setting.

In addition, we clarify why results in the inductive setting are much higher than the transductive setting, while the inductive setting is more difficult: As shown in [[#^f8f8fb|Table 1]] and 3, the entities involved in the inductive evaluation is much less than the transductive setting $(7,475$ vs. $4,594,485)$. Considering the KE evaluation metrics are based on entity ranking, it is reasonable to see higher values in the inductive setting. The performance in different settings should not be directly compared.

## 5. Analysis

In this section, we analyze the effectiveness and efficiency of KEPLER with experiments. All the hyper-parameters are the same as reported in Section 4.1, including models in the ablation study.

### 5.1. Ablation Study

> [!blank-container|right-small]
> 
> | Model | P | R | F-1 |
> | :--- | :---: | :---: | :---: |
> | Our RoBERTa | 70.8 | 69.6 | 70.2 |
> | [[#^cc18b2\|KEPLER-KE]] | 63.5 | 60.5 | 62.0 |
> | [[#^17c09d\|KEPLER-Wiki]] | 71.5 | 72.5 | 72.0 |
> 
> > Table 10: Ablation study results on TACRED (%).
> 

As shown in [[#^0ff246|(6)]], KEPLER takes a multitask loss. To demonstrate the effectiveness of the joint objective, we compare full KEPLER with models trained with only the MLM loss (Our RoBERTa) and only the KE loss ([[#^cc18b2|KEPLER-KE]]) on TACRED. As demonstrated in Table 10, compared to [[#^17c09d|KEPLER-Wiki]], both ablation models suffer significant drops. It suggests that the performance gain of KEPLER is credited to the joint training towards both objectives.

### 5.2. Knowledge Probing Experiment

Section 4.2 shows that KEPLER can achieve significant improvements on NLP tasks requiring factual knowledge. To further verify whether KEPLER can better integrate factual knowledge into PLMs and help to recall them, we conduct experiments on LAMA (Petroni et al., 2019), a widelyused knowledge probe. LAMA examines PLMs' abilities on recalling relational facts by cloze-style questions. For instance, given a natural language template "Paris is the capital of \<mask\>", PLMs are required to predict the masked token without fine-tuning. LAMA reports the micro-averaged precision at one (P@1) scores. 
However, Poerner et al. (2020) present that LAMA contains some easy questions which can be answered with superficial clues like entity names. Hence we also evaluate the models on LAMA-UHN (Poerner et al., 2020), which filters out the questionable templates from the Google-RE and T-REx corpora of LAMA.



> [!blank-container|right-large]
> | Model | LAMA |  |  |  |  | LAMA-UHN |  |
> | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> |  | Google-RE | T-REx | ConceptNet | SQuAD | Google-RE | T-REx |  |
> | BERT | 9.8 | 31.1 | 15.6 | 14.1 | 4.7 | 21.8 |  |
> | RoBERTa | 5.3 | 24.7 | 19.5 | 9.1 | 2.2 | 17.0 |  |
> | Our RoBERTa | 7.0 | 23.2 | **19.0** | 8.0 | 2.8 | 15.7 |  |
> | [[#^17c09d\|KEPLER-Wiki]] | **7.3** | **24.6** | 18.7 | **14.3** | 3.3 | 16.5 |  |
> | [[#^b68011\|KEPLER-W+W]] | **7.3** | 24.4 | 17.6 | 10.8 | **4.1** | **17.1** |  |
> 
> > Table 11: P@1 results on knowledge probing benchmark LAMA and LAMA-UHN.

The evaluation results are shown in Table 11, from which we have the following observations: 
1. KEPLER consistently outperforms the vanilla PLM baseline Our [[../RoBERTa|RoBERTa]] in almost all the settings except ConceptNet, which focuses on commonsense knowledge rather than factual knowledge. It indicates that KEPLER can indeed better integrate factual knowledge. 
2. Although [[#^b68011|KEPLER-W+W]] cannot outperform [[#^17c09d|KEPLER-Wiki]] on NLP tasks (Section 4.2), it shows significant improvements in LAMA-UHN, which suggests that we should explore which kind of knowledge is needed on different scenarios in the future. 
3. All the RoBERTa-based models perform worse than vanilla BERT $_{\mathrm{BASE}}$ by a large margin, which is consistent with the results of Wang et al. (2020). This may be due to different vocabularies used in [[../BERT|BERT]] and [[../RoBERTa|RoBERTa]], which presents the vulnerability of LAMA-style probing again (Kassner and Schütze, 2020). We will leave developing a better knowledge probing framework as our future work.

###  5.3 Running Time Comparison


> [!blank-container]
> | Model | $\begin{array}{c}\text { Entity } \\ \text { Linking }\end{array}$ | $\begin{array}{c}\text { Fine- } \\ \text { tuning }\end{array}$ | Inference |
> | :--- | :---: | :---: | :---: |
> | ERNIE<sub>RoBERTa</sub> | $780 \mathrm{~s}$ | $730 \mathrm{~s}$ | $194 \mathrm{~s}$ |
> | KnowBert<sub>RoBERTa</sub> | $190 \mathrm{~s}$ | $677 \mathrm{~s}$ | $235 \mathrm{~s}$ |
> | KEPLER | $\mathbf{0 s}$ | $\mathbf{5 0 8 s}$ | $\mathbf{1 5 2 s}$ |
> 
> > Table 12: Three parts of running time for one epoch of TACRED training set.
> 

Compared to vanilla PLMs, KEPLER does not introduce any additional parameters or computations during fine-tuning and inference, which is efficient for practice use. We compare the running time of KEPLER and other knowledge-enhanced PLMs (ERNIE and KnowBert) in Table 12. The time is evaluated on TACRED training set for one epoch with one NVIDIA Tesla V100 (32 GB), and all models use 32 batch size and 128 sequence length. The "entity linking" time of KnowBert is for entity candidate generation. We can observe that KEPLER requires much less running time since it does not need entity linking or entity embedding fusion, which will benefit time-sensitive applications.




> [!blank-container|right-large]
> 
> ![[figure3.svg]]
> > Figure 3: TACRED performance (F-1) of KEPLER and RoBERTa change with the rate of entity mentions being masked.



### 5.4. Correlation with Entity Frequency

To better understand how KEPLER helps the entity-centric tasks, we provide analyses on the correlations between KEPLER performance and entity frequency in this section. The motivation is to verify a natural hypothesis that KEPLER improvements mainly come from better representing the entity mentions in text, especially the rare entities, which do not show up frequently in the pre-training corpora and thus cannot be well learned by the language modeling objectives.

We perform entity linking for the TACRED dataset with BLINK (Wu et al., 2020) to link the entity mentions in text to their corresponding Wikipedia identifiers. Then we count the occurrences of the entities in Wikipedia with the hyperlinks in rich text, denoting the entity frequencies. We conduct two experiments to analyze the correlations between KEPLER performance and entity frequency: 
1. In Table 13, we divide the entity mentions into five parts by their frequencies, and compare the TACRED performances while only keeping entities in one part and masking the other. 
2. In Figure 3, we sequentially mask the entity mentions in the ascending order of entity frequencies and see the F-1 changes.

From the results, we can observe that:
1. Figure 3 shows that when the entity masking rate is low, the improvements of KEPLER over RoBERTa are generally much higher than when the entity masking rate is high. It indicates that the improvements of KEPLER do mainly come from better modeling entities in context. However, even when all the entity mentions are masked, KEPLER still outperforms RoBERTa. We claim this is because the KE objective can also help to learn to understand fact-related text since it requires the model to recall facts from textual descriptions. This claim is further substantiated in Section 5.5.
2. From Table 13, we can observe that the improvement in the " $0 \%-20 \%$ " setting is marginally higher than the other settings, which demonstrates that KEPLER does have special advantages on modeling rare entities compared to vanilla PLMs. But the improvements in the frequent settings are also significant and we cannot say that the overall improvements of KEPLER are mostly from the rare entities. In general, the results in Table 13 show that KEPLER can better model all the entities, no matter rare or frequent.

> [!blank-container]
> | Entity Frequency | $\mathbf{0 \% - 2 0 \%}$ | $\mathbf{2 0 \% - 4 0 \%}$ | $\mathbf{4 0 \% - 6 0 \%}$ | $\mathbf{6 0 \% - 8 0 \%}$ | $\mathbf{8 0 \% - 1 0 0 \%}$ |
> | :--- | :---: | :---: | :---: | :---: | :---: |
> | [[#^17c09d\|KEPLER-Wiki]] | 64.7 | 64.4 | 64.8 | 64.7 | 68.8 |
> | Our RoBERTa | 64.1 | 64.3 | 64.5 | 64.3 | 68.5 |
> | Improvement | +0.6 | +0.1 | +0.3 | +0.4 | +0.3 |
> 
> > Table 13: F-1 scores on TACRED (\%) under different settings by entity frequencies. We sort the entity mentions in TACRED by their corresponding entity frequencies in Wikipedia. The " $0 \%-20 \%$ " setting indicates only keeping the least frequent $20 \%$ entity mentions and masking all the other entity mentions (for both training and validation), and so on. The results are averaged over 5 runs.
> 


> [!blank-container]
> | Model | ME | OE |
> | :--- | :---: | :---: |
> | Our RoBERTa | 54.0 | 46.8 |
> | [[#^cc18b2\|KEPLER-KE]] | 40.2 | 47.0 |
> | [[#^17c09d\|KEPLER-Wiki]] | 54.8 | 48.9 |
> 
> > Table 14: Masked-entity (ME) and only-entity (OE) F-1 scores on TACRED (\%).
> 

### 5.5. Understanding Text or Storing Knowledge

We argue that by jointly training the KE and the MLM objectives, KEPLER (1) can better understand fact-related text and better extract knowledge from text, and also [[#^7238d5|(2)]] can remember factual knowledge. To investigate the two abilities of KEPLER in a quantitative aspect, we carry out an experiment on TACRED, in which the head and tail entity mentions are masked (masked-entity, ME) or only head and tail entity mentions are shown (onlyentity, OE). The ME setting shows to what extent the models can extract facts only from the textual context without the clues in entity names. The $\mathrm{OE}$ setting demonstrates to what extent the models can store and predict factual knowledge, as only the entity names are given to the models.

As shown in Table 14, [[#^17c09d|KEPLER-Wiki]] shows significant improvements over Our RoBERTa in both settings, which suggests that KEPLER has indeed possessed superior abilities on both extracting and storing knowledge compared to vanilla PLMs without knowledge infusion. And the KEPLERKE model performs poorly on the ME setting but achieves marginal improvements on the OE setting. It indicates that without the help of the MLM objective, KEPLER only learns the entity description embeddings and degenerates in general language understanding, while it can still remember knowl- edge into entity names to some extent.

## 6. Related Work

Pre-training in NLP There has been a long history of pre-training in NLP. Early works focus on distributed word representations (Collobert and Weston, 2008; Mikolov et al., 2013; Pennington et al., 2014), many of which are often adopted in current models as word embeddings. These pre-trained embeddings can capture the semantics of words from large-scale corpora and thus benefit NLP applications. Peters et al. (2018) push this trend a step forward by using a bidirectional LSTM to form contextualized word embeddings (ELMo) for richer semantic meanings under different circumstances.

Apart from word embeddings, there is another trend exploring pre-trained language models. Dai and Le (2015) propose to train an auto-encoder on unlabeled textual data and then fine-tune it on downstream tasks. Howard and Ruder (2018) propose a universal language model (ULMFiT). With the powerful Transformer architecture (Vaswani et al., 2017), Radford et al. (2018) demonstrate an effective pre-trained generative model (GPT). Later, Devlin et al. (2019) release a pre-trained deep Bidirectional Encoder Representation from Transformers (BERT), achieving state-of-the-art performance on a wide range of NLP benchmarks.

After BERT, similar PLMs spring up recently. Yang et al. (2019) propose a permutation language model (XLNet). Later, Liu et al. (2019c) show that more data and more parameter tuning can benefit PLMs, and release a new state-of-the-art model (RoBERTa). Other works explore how to add more tasks (Liu et al., 2019b) and more parameters (Raffel et al., 2020; Lan et al., 2020) to PLMs.

Knowledge-Enhanced PLMs Recently, many works have investigated how to incorporate knowledge into PLMs. MTB (Baldini Soares et al., 2019) takes a straightforward "matching the blank" pretraining objective to help the relation classification task. ERNIE (Zhang et al., 2019) identifies entity mentions in text and links pre-processed knowledge embeddings to the corresponding positions, which shows improvements on several NLP benchmarks. With a similar idea as ERNIE, KnowBert (Peters et al., 2019) incorporates an integrated entity linker in their model and adopts end-to-end training. Besides, Logan et al. (2019) and Hayashi et al. (2020) utilize relations between entities inside one sentence to train better generation models. Xiong et al. (2019) adopt entity replacement knowledge learning for improving entity-related tasks.

Some contemporaneous or following works try to inject factual knowledge into PLMs in different ways. E-BERT (Poerner et al., 2020) aligns entity embeddings with word embeddings and then directly adds the aligned embeddings into [[../BERT|BERT]] to avoid additional pre-training. K-Adapter (Wang et al., 2020) injects knowledge with additional neural adapters to support continuous learning.

Knowledge Embedding KE methods have been extensively studied. Conventional KE models define different scoring functions for relational triplets. For example, TransE (Bordes et al., 2013) treats tail entities as translations of head entities and uses $L_{1}$-norm or $L_{2}$-norm to score triplets, while DistMult (Yang et al., 2015) uses matrix multiplications and ComplEx (Trouillon et al., 2016) adopts complex operations based on it. RotatE (Sun et al., 2019) combines the advantages of both of them.

Inductive Embedding Above KE methods learn entity embeddings only from KG and are inherently transductive, while some works (Wang et al., 2014; Xie et al., 2016; Yamada et al., 2016; Cao et al., 2017; Shi and Weninger, 2018; Cao et al., 2018) incorporate textual metadata such as entity names or descriptions to enhance the KE methods and hence can do inductive KE to some extent. Besides KG, it is also common for general inductive graph embedding methods (Hamilton et al., 2017; Bojchevski and Günnemann, 2018) to utilize additional node features like text attributes, degrees, etc. KEPLER follows this line of studies and takes full advantage of textual information with an effective PLM.

Hamaguchi et al. (2017) and Wang et al. (2019c) perform inductive KE by aggregating the trained embeddings of the known neighboring nodes with graph neural networks, and thus do not need ad- ditional features. But these methods require the unseen nodes to be surrounded by known nodes and cannot embed new (sub)graphs. We leave how to develop KEPLER to do fully inductive KE without additional features as future work.

## 7. Conclusion and Future Work

In this paper, we propose KEPLER, a simple but effective unified model for knowledge embedding and pre-trained language representation. We train KEPLER with both the KE and MLM objectives to align the factual knowledge and language representation into the same semantic space, and experimental results on extensive tasks demonstrate its effectiveness on both NLP and KE applications. Besides, we propose Wikidata5M, a large-scale KG dataset to facilitate future research.

In the future, we will 
1. explore advanced ways for more smoothly unifying the two semantic space, including different KE forms and different training objectives, and 
2. investigate better knowledge probing methods for PLMs to shed light on knowledge-integrating mechanisms.

