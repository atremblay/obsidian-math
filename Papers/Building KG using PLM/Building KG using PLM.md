---
title: "Building Knowledge Graph using Pre-trained Language Model for Learning Entity-aware Relationships"
authors: 
    - Abhijeet Kumar
    - Abhishek Pandey
    - Rohit Gadia
    - Mridul Mishra
tags:
    - knowledge_graph KG
    - entity_relation
    - relation_extraction RE
    - lm
note: 3
aliases:
    - Kumar et al.
---


## TLDR

This does not discover entities. They are given and even provided to the model by using \# and \$ markers around the entities. 

> \#Fish\# without the pyloric ceca have digestive enzyme production in the \$liver\$

The type of relation is a classification problem as well as the directionality. 

Not very interesting. 


#### Abstract

Relations exhibited among entities from textual content can be a potential source of information for any business domain. This paper encompasses a wholesome approach to mine entity-relation and building knowledge graph from textual documents. The paper concentrates on two approaches to classify directional entity relations. We build on extending pretrained language model i.e. BERT for text classification along-side providing entity and directionality information as input making it entity-aware BERT classifier. We also did ablation studies of presented model in terms of various ways of providing entity information on the learning capabilities of model. We demonstrate the end to end pipeline for building an entityrelation extraction system in a business application. The techniques proposed in the paper are also evaluated against [[../../Machine Learning/Dataset/SemEval 2010/Task 8|SemEval-2010 Task 8]], a popular relation classification dataset. The experimental results demonstrate that learning entity-aware relations through language models outperforms almost all the previous state-of-the-art (SOTA) models.

## 1. INTRODUCTION

Entity relationship classification is the task of identifying the semantic relation between the entities present in the text. For example,

> Amazon recently acquired PillPack for $1 billion.

Here, Amazon and PillPack are two entities which has relation of "acquisition". In another complex example,

> He was chief executive of Worldpay when it was acquired by Vantiv in a £9.3 billion takeover in 2017.

Vantiv and Worldpay are two entities with relation "acquisition". At times, it is also crucial to know the directionality of the relation. In first example, directionality of relation Acquisition was from Amazon (e1) to PillPack (e2) whereas the same relation is from Vantiv (e2) to Worldpay (e1) in second example.

This task is indispensable in various NLP applications. Imagine with a plethora of news and articles getting generated every day, there is immense potential of knowledge that can be extracted for any domain. Knowledge graph is a powerful tool for supporting a gamut of search applications along with ranking, recommendation and exploratory search [1]. A knowledge graph aggregates information around entities in a structural way across multiple content sources and links these entities together. Also, it has the capability to provide entity specific properties such as type, specific relation at the same time. We created one such financial knowledge graph using Neo4J database [2] to visualize the relationships among entities like Persons, Organizations and Locations.

There are numerous approaches for extracting relationship among entities (feature based and kernel-based) which rely on NLP stack using POS tagging and dependency parsing [3][10]. The major concern of these approaches is inability to capture numerous ways of expressing the similar relationship. The challenging variability in syntactic and semantic nature of a sentence creates a need for effective solution which can extract entities as well as context that defines the relationship. This paper presents a language-model based solution which captures relationship between entities contextually and not only syntactically.

In recent years, the research has moved towards deep networks which are effective in contextual learning for a given task. Various architectures of RNN, LSTM and CNNs have been proposed for sequence modeling or classification task. These models have been effectively implemented for entity relation classification also [11]-[15]. More recently, Attention based models became SOTA techniques for most of NLP tasks. A lot of research with attention-based models for learning entity relations had been presented as SOTA models in past [16]-[18]. In last couple of years, language models have taken the driving seat in this research direction where a pre-trained language model (like ELMO [19], GPT [20], BERT [21] etc.) has been trained on huge texts in a self-supervised way to learn the syntactics and semantics of text sentences.

This paper demonstrates two BERT based architectures which is effective for relationship classification between entities. Major contributions presented are:

1. Two proposed techniques are 
    1. Training a full $\left(\mathrm{K}^{*} 2+1\right)$ way classifier where $K$ is number of relation classes. We keep an artificial class `Others` for capturing sentences where entities are present but without relation of our interest. 
    2. Training two classifiers, one binary classifier for directionality prediction between entities and another K-way classifier for relation class prediction and further combining the results.
2. Through ablation study, we concluded the best format to provide entity information to finetuned language model (BERT) classifier (See Section V.B).
3. We achieved SOTA results for relationship classification task with F1-score of 88.4% and 89.41% on the SemEval 2010 Task 8 dataset, outperforming all previous results [11]-[18][22][23].

## 2. RELATED WORK

There is extensive research ranging from unsupervised to supervised techniques that attempts to solve for relation extraction and knowledge mining. Rink et al. [5] used handcrafted features that capture the context, semantic role affiliation, and possible pre-existing relations between entities. Research efforts have been made with deep learning methods for learning long term dependency in entity relationships. Zhang et al. [12] and Socher et al. [15] applied RNN and MVRNN model respectively for relation classification which implements variants of recursive neural network.

Deep convolutional neural network models were also employed for extracting lexical and sentence level features using convolution kernels [11]. Dos Santos et al. proposed model for relation-classification task using a $\mathrm{CNN}$ that performs classification by ranking (CR-CNN) [14]. Research efforts were made in employing attention levels in deep network models in order to capture specific segment of sentence which dictates the relations well [16][17][18]. Wang et al. proposed $\mathrm{CNN}$ model relying on two levels of attention in order to better discern patterns in heterogeneous contexts [17]. Lee et al. employed multi-head entity aware self-attention based Bi-LSTM models [18]. Yu et al. (2014) proposed a Factor-based Compositional Embedding Model (FCM) by utilizing sentence-level and substructure embeddings from word embeddings, employing dependency trees and named entities recognition (NER) [22].

Recently, few researchers have made efforts in exploring pretrained language models like BERT for relationship extraction. Yao et al. trained a knowledge graph KG-BERT to train triplet of entities and relation description as input to BERT [24]. Soares et al. presents a method of training relation representation without any supervision from a knowledge graph or human annotators by matching the blanks [25]. Wu et al. proposed entity embeddings as well as the sentence encoding from BERT as the input to a multi-layer neural network for classification [23].

Although these models achieve good results, ideally, A simple yet effective solution would be one which does not require handcrafted features, dependency parsing or training multiple models. In Section 4, through experiments we show that finetuning a pretrained language model with entity aware raw text input simply achieves this, while also obtaining considerable improvements in F1 scores. Section 5 writes about error analysis and ablation studies to arrive at best format of providing text input to model. In next section, both the architectures are discussed along with overall system pipeline design and knowledge graph generation.

## 3. PROPOSED METHODOLOGY

### A. System Design

We formulated a pipeline which extracts predefined entities and their relations (from classification), and populates the searchable, visual and extensive knowledge graph.


> [!blank-container]
> ![[figure1.png]]
> 
> > Fig. 1. Relation Extraction System Design

Figure. 1 depicts 5-step process flow of proposed relation extraction system with various components. Given an English article or document, a sentence tokenizer (step 1) is called and then the entity recognizer (step 2) will demarcate possible entities in sentence like location, person or organization. In the following steps (3 and 4), a BERT based relation classifier is run and binary entity-relation triplet is generated if relationship like acquisition, relative-of, located-in and works-at is found. Once the system has labeled entity-relations triplets, it populates the nodes and links in graph database for knowledge graph representation (step 5).

### B. Model Architecture-Entity-aware BERT

We fine-tuned BERT [21] model in a specific way for providing input as described in the later sub-sections. Any language model building has two phases of training: generic pretraining step followed by finetuning on custom dataset (mostly limited dataset task). Often the pretraining of these models are also extended as an additional step in case of learning specific domain.

#### 1) Input Representation

As per original BERT paper [21], a special classification token [CLS] is the first token of the input sequence and end token is [SEP]. In order to explicitly provide location information, we inserted special symbolic tokens \# and \$ without gap to enclose the entities. Hence making it entity-aware BERT classifier. Given a sentence $s=\left\{\mathrm{w}_{1}\right.$, $\left.<\mathrm{e} 1>\mathrm{w}_{\mathrm{h}}</ \mathrm{e} 1>, \ldots .<\mathrm{e} 2>\mathrm{w}_{\mathrm{t}}</ \mathrm{e} 2>, \ldots \mathrm{w}_{\mathrm{n}}\right\}$ with marked entities $(\mathrm{h}$, t), we convert it to $s=\left\{[C L S], \mathrm{w}_{1}, \ldots, \# \mathrm{w}_{\mathrm{h}} \#, \ldots \$ \mathrm{w}_{\mathrm{t}} \$, \ldots \mathrm{w}_{\mathrm{n}}\right.$, $[\mathrm{SEP}]\}$. For example, an input text sequence is passed to model in the following manner.

[CLS] Louise is a director, audit chair and finance \#committee\# \$member\$ of a public health board and was a director of AFAANZ for 3 years [SEP]"

The above sentence is a training sentence of SemEval dataset [28] from Member-Collection (e2, e1) relation class with directionality e2 to e1.

#### 2) Model Architecture

We have utilized pre-trained model BERT base from Google's BERT TensorFlow implementation [27] and finetuned it for entity-relation classification. The model architecture shown in Figure. 2 depicts the way input sequence is provided to make it entity-aware.

> [!blank-container|right-medium]
> ![[figure2.png]]
> 
> > Fig. 2. Entity-aware Finetuned BERT classifier

The final hidden state corresponding to special classification token ([CLS]) is used as the aggregated sequence representation for classification tasks [21]. The classification parameters introduced during fine-tuning are $\mathrm{W} \in \mathrm{R}^{(K* 2+1) \times 768}$, where $\mathrm{K}$ is the no. of relation classes. We compute a typical softmax loss for classification with $\mathrm{C}$ and $\mathrm{W}$, i.e., $\log \left(\operatorname{softmax}\left(\mathrm{CW}^{\mathrm{T}}\right)\right)$. The scoring function for a relation classification is

$$
\mathrm{F}(\mathrm{r})=\operatorname{softmax}\left(\mathrm{C} \cdot \mathrm{W}^{\mathrm{T}}\right)
$$

The hyperparameters settings for BERT based implementations are as follows:

TABLE I. BERT-BASE HYPERPARAMTERS

| Hyperparameters        | Value            |
|:---------------------- |:---------------- |
| Max Sequence Length    | 64               |
| No. of Training Epochs | 5                |
| Train Batch Size       | 64               |
| Warmup Proportion      | 0.1              |
| Learning Rate          | $5 \mathrm{e}-5$ |
| Dropout                | 0.1              |

#### C) BERT based 2-Model Architecture

We propose another approach "2-model BERT" for solving the relationship classification task with directionality. We trained two BERT based classifier on the same training data in a similar way as directed in section B. First classifier learns the relations class whereas second binary classifier learns the directionality of the relationship provided the entity located with markers.

> [!blank-container|right-medium]
> ![[figure3.png]]
> 
> > Fig. 3. 2-Model BERT classifier design

The proposed 2-model architecture is shown in Figure 3. As shown in the diagram, Directionality is considered only if relation classifier predicts some relation class, not the artificial class "Other". We examined the performance of both the model individually. Only relation classification model achieved F1-score of $86.6 \%$ whereas binary directional classifier achieved F1-score of $97 \%$ on SemEval 2010 dataset. Moreover, we achieved results better than most of the previous works and comparable to SOTA models on combining both the relation and directionality classification models (See section 4.B).

#### D) Knowledge Graph

A knowledge graph is developed using graphical method that keeps association as well as hierarchical knowledge of domain. We employed Neo4J for visualizing and searching entity-relationships as shown in a sample snippet below (Fig 4).

> [!blank-container|left-large]
> ![[figure4.png]]
> 
> > Fig. 4. Entity-aware Finetuned BERT classifier

## 4. EXPERIMENT AND RESULTS

### A. SemEval 2010 Dataset

We used SemEval-2010 Task 8 dataset for benchmarking both the proposed architectures [28]. This dataset consisted of 8,000 examples as training data, and 2717 sentences as testing set. There are nine types of annotated relations between entities along with demarcation of entities. Refer to Table IV for all nine label names of annotated relations. There is an additional relation type "Other" (artificial class) which indicates that the relation expressed in the text is not among the nine types. In this dataset, each of the relation types, directionality information is also present which implies that relationship between entities can be from e1 to e2 or vice versa. For example, Entity-Origin (e1, e2) and Entity-Origin (e2, e1) can be considered two distinct relations, so the total number of relationship classes is 19 .

### B. Experimental Results \& Comparison

As discussed earlier in the paper, we experimented with two proposed approaches for this dataset. Details about the model architecture are described in section 3.

The official scorer was used to evaluate all our model experiments in terms of the Macro-F1 score over the nine relationship pairs taking directionality into account. We also compare the results with scores quoted in previous works on this dataset (shown below).

TABLE II. SEMEVAL-2010 TASK 8 (APPROACH-1)

| Models                           | F1-Score |
|:-------------------------------- |:-------- |
| SVM [5]                          | 82.2     |
| RNN [15]                         | 77.6     |
| MVRNN [15]                       | 82.4     |
| FCM [22]                         | 83.0     |
| CNN+ Softmax [11]                | 82.7     |
| CR-CNN [14]                      | 84.1     |
| Attention CNN [16]               | 85.9     |
| DRNNs [13]                       | 85.8     |
| Multi-level Att-Pooling-CNN [17] | 88.0     |
| Entity Attention Bi-LSTM [18]    | 85.2     |
| R-BERT                           | 89.25    |
| 2-Model BERT                     | 88.44    |
| Entity-aware BERT                | 89.41    |

## 5. ABLATION STUDIES AND ERROR ANALYSIS

### A. Effect of Entity Information

In order to understand the importance of providing entity information, we ran the proposed model (Approach-I) without entity marking. The experimental results are shown in the
Table III. Intuitively, entity marking helps a lot in learning as BERT is heavily based on multi-head self-attention layers.

TABLE III. SEMEVAL-2010 TASK 8 (APPROACH-1)

| Models                            | F1-Score             |
|:--------------------------------- |:-------------------- |
| 2-Model BERT with No Entity info. | 78.84                |
| 2-model BERT                      | $\mathbf{8 8 . 4 4}$ |
| BERT with No Entity Info.         | 81.40                |
| Entity-aware BERT                 | $\mathbf{8 9 . 4 1}$ |

### B. Effect of Entity Markers

The idea behind performing these experiments was to understand if choosing entity marker also impacts the learning of BERT language model. From numerous experiments, it was concluded that symbolic markers like \$, \# performs better than alphabetic marker. One of the possible reasons would be alphabetic markers surrounding entities distorts the language representation learnt by language models.

Also, entities with symbolic markers without leaving space marginally performs better than leaving space. Table. 1 shows effect of entity markers in terms of F1-score for each class as well as overall macro-averaged score by semval-2010 offline scorer.

TABLE IV. SEMEVAL-2010 TASK 8 (APPROACH-1)

|                                Classes                                 |                                 F1-score (different entity markers)                                 |        |                   |
|:----------------------------------------------------------------------:|:---------------------------------------------------------------------------------------------------:|:------:|:-----------------:|
|                                                                        | $\begin{array}{c}\text { E1 start, E1 } \\ \text { end, E2 start, } \\ \text { E2 end }\end{array}$ | \$, \# | \$, \# (no space) |
|                              Cause-Effect                              |                                                93.53                                                | 94.42  |       93.74       |
|                            Component-Whole                             |                                                84.58                                                | 83.52  |       86.35       |
|                           Content-Container                            |                                                90.16                                                | 87.94  |       90.82       |
|                           Entity-Destination                           |                                                91.76                                                | 93.31  |       92.97       |
|                             Entity-Origin                              |                                                88.68                                                | 88.05  |       89.27       |
|                           Instrument-Agency                            |                                                79.01                                                | 81.39  |       82.96       |
| $\begin{array}{l}\text { Member- } \\ \text { Collection }\end{array}$ |                                                87.12                                                | 88.37  |       88.37       |
|                             Message-Topic                              |                                                92.05                                                | 91.68  |       91.71       |
|                            Product-Producer                            |                                                86.15                                                | 87.90  |       88.51       |
|                                 _Other                                 |                                                63.67                                                | 65.31  |       64.69       |
|                              All Classes                               |                                                88.12                                                | 88.51  |       89.41       |

### C.  Effect of Training Epochs

Optimal number of epochs for finetuning BERT was found to be 5 for the dataset as shown in Figure.5.

![](https://cdn.mathpix.com/cropped/2023_09_16_36232fe88c483240bee2g-5.jpg?height=431&width=767&top_left_y=175&top_left_x=210)

Fig. 5. F1-score versus Training epochs (optimal=5)

### D. Error Analysis: Where it failed?

We examined the misclassified examples produced by the model to understand the possible reasons of failure. Interestingly, approximately $80 \%$ of the misclassifications were result of either incorrectly predicting a relation as artificial class "Other" or vice versa.

Firstly, some of the examples wrongly predicted as "Other" category contains words or context phrases (shown in bold) which were unseen in the training sentences. The model possibly could not get the context and predicted the artificial class wrongly. Few examples of such misclassifications are shown in Table V. Another intuition is the generic nature of text sequences in "Other" class may confuse model and lead to incorrect prediction for some of such sentences of actual relation classes also.

TABLE V. MissclassificATION EXAMPLES (PREDICTED-OTHER)

| Text                                                                                                                                                    | Target                  | Prediction |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- | ---------- |
| \#Fish\# without the pyloric ceca have digestive enzyme production in the \$liver\$                                                                     | Component-Whole(e2,e1)  | Other      |
| The \#body\# unleashes its extraterrestrial \$passenger\$, which proceeds to infect the student population at a breakneck pace.                         | Entity-Origin(e2,e1)    | Other      |
| These \#observations\# determine the high spatial resolution stellar \$kinematics\$  within the nuclei of these galaxies.                               | Message-Topic(e1,e2)    | Other      |
| The Postmodernism Generator was written by Andrew C. Bulhak using the Dada Engine, a system for generating random \#text\# from recursive \$grammars\$. | Product-Producer(e1,e2) | Other      |

Secondly, the model may learn relation classes focusing on the prepositional phrases between entities through attention mechanism e.g. "is part of" phrase mostly shows either Component-Whole or Member-Collection relation class in training data. But some of the examples from Other class in test set have such relation defining phrases which confuses the model (few are shown in Table VI).
TABLE VI. MISSCLASSIFICATION EXAMPLES (ACTUAL-OTHER)

| text                                                                                                                                | Target | Prediction             |
| ----------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------------------- |
| \#Danger\# is part of the Palestinian journalist's daily \$routine\$                                                                | Other  | Component-Whole(e2,e1) |
| The painting shows a historical view of the \#damage\# caused by the 1693 Catania earthquake and the \$reconstruction\$ activities. | Other  | Cause-Effect(e2,e1)    |
| The \#yeast\# is an ingredient for making \$beer\$.                                                                                 | Other  | Entity-Origin(e2,e1)   |

## 6. CONCLUSION AND FUTURE WORK

The objective of the research and experimentation was to construct in-domain large scale searchable knowledge graph to visualize various entities and the way they are related to each other. In this paper, we utilized the advantage of language knowledge already learnt by language models and enriched them by incorporating entity aware information to provide attention for entity relation classification $(2 * k+1$ way). We also proposed two-model architecture to separately deal with relation class as well as directionality of relation. Through ablation studies, we found that symbolic markers enclosing the entity words without gap is the best way to provide text input format for relation classification with finetuned BERT. We conduct experiments on the SemEval-2010 task 8 benchmark dataset. Both the proposed models namely entity-aware BERT and 2-model BERT achieve SOTA results of $88.44 \%$ and $89.41 \%$ F1-score respectively in SemEval-2010 Task 8 without any explicit embeddings or high-level features from NLP tools.

As future directions, we intend to explore multi-label classification task for scenarios where presence of more than two entities in a sentence makes relationship extraction ambiguous. A limitation of our proposed system in the paper is that it extracts relation on the sentence level. Relations can span over sentences also. We intend to modify our systems to capture long range relations. We also look forward to employing domain specific custom pre-trained model in order to improve accuracy of system.


