---
title: General Language Understanding Evaluation
url:
    - "[paper](https://openreview.net/pdf?id=rJ4km2R5t7)"
tags:
    - dataset
authors:
    - Alex Wang
    - Amanpreet Singh
    - Julian Michael
    - Felix Hill  
    - Omer Levy
    - Samuel R. Bowman
---

#### Abstract

For natural language understanding (NLU) technology to be maximally useful, it must be able to process language in a way that is not exclusive to a single task, genre, or dataset. In pursuit of this objective, we introduce the General Language Understanding Evaluation (GLUE) benchmark, a collection of tools for evaluating the performance of models across a diverse set of existing NLU tasks. By including tasks with limited training data, GLUE is designed to favor and encourage models that share general linguistic knowledge across tasks. GLUE also includes a hand-crafted diagnostic test suite that enables detailed linguistic analysis of models. We evaluate baselines based on current methods for transfer and representation learning and find that multi-task training on all tasks performs better than training a separate model per task. However, the low absolute performance of our best model indicates the need for improved general NLU systems.

## Tasks
Made up of 9 differents [tasks](https://gluebenchmark.com/tasks). 

|Name|ID|Paper|Download|More Info|Metric|
|---|---|---|---|---|
|The Corpus of Linguistic Acceptability|#COLA|[[../../../Explorance/Research/Dataset/CoLA|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/data/CoLA.zip)|[Link](https://nyu-mll.github.io/CoLA/)|Matthew's Corr|
|The Stanford Sentiment Treebank|#SST-2|[[../../../Explorance/Research/Dataset/SST-2|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/data/SST-2.zip)|[Link](https://nlp.stanford.edu/sentiment/index.html)|Accuracy|
|Microsoft Research Paraphrase Corpus|#MPRC|[[../../../Explorance/Research/Dataset/MRPC|Link]]|[Link](https://www.microsoft.com/en-us/download/details.aspx?id=52398)|[Link](https://microsoft.com/en-us/download/details.aspx?id=52398)|F1 / Accuracy|
|Semantic Textual Similarity Benchmark|#STS-B|[[../../../Explorance/Research/Dataset/STSBenchmark|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/data/STS-B.zip)|[Link](http://ixa2.si.ehu.es/stswiki/index.php/STSbenchmark)|Pearson-Spearman Corr|
|Quora Question Pairs|#QQP|[[../../../Explorance/Research/Dataset/Quora Question Pairs|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/data/QQP-clean.zip)|[Link](https://data.quora.com/First-Quora-Dataset-Release-Question-Pairs)|F1 / Accuracy|
|MultiNLI Matched|#MultiNLI|[[A Broad-Coverage Challenge Corpus for Sentence Understanding through Inference\|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/data/MNLI.zip)|[Link](http://www.nyu.edu/projects/bowman/multinli/)|Accuracy|
|MultiNLI Mismatched|#MultiNLI|[[A Broad-Coverage Challenge Corpus for Sentence Understanding through Inference\|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/data/MNLI.zip)|[Link](http://www.nyu.edu/projects/bowman/multinli/)|Accuracy|
|Question NLI|#QNLI|[[../../../Explorance/Research/Dataset/SQuAD The Stanford Question Answering Dataset|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/data/QNLIv2.zip)|[Link](https://rajpurkar.github.io/SQuAD-explorer/)|Accuracy|
|Recognizing Textual Entailment|#RTE|[[../../../Explorance/Research/Dataset/Recognizing Textual Entailment|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/data/RTE.zip)|[Link](https://aclweb.org/aclwiki/Recognizing_Textual_Entailment)|Accuracy|
|Winograd NLI|#WNLI #WSC|[[../../../Explorance/Research/Dataset/WSC - Winograd Schema Challenge|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/data/WNLI.zip)|[Link](https://cs.nyu.edu/faculty/davise/papers/WinogradSchemas/WS.html)|Accuracy|
|Diagnostics Main|||[Link](https://dl.fbaipublicfiles.com/glue/data/AX.tsv)|[Link](https://gluebenchmark.com/diagnostics)|Matthew's Corr|

>  None of the datasets in GLUE were created from scratch for the benchmark; we rely on preexisting datasets because they have been implicitly agreed upon by the NLP community as challenging and interesting.

### Single sentence task
- [[../../../Explorance/Research/Dataset/CoLA|CoLA]]: The Corpus of Linguistic Acceptability
- [[../../../Explorance/Research/Dataset/SST-2|SST-2]]: The Stanford Sentiment Treebank

### Similarity and Paraphrase Tasks

 - [[../../../Explorance/Research/Dataset/MRPC|MRPC]]: Microsoft Research Paraphrase Corpus
 - STS-B: Semantic Textual Similarity Benchmark
 - QQP: Quora Question Pairs


### Inference Tasks

- [[A Broad-Coverage Challenge Corpus for Sentence Understanding through Inference|MNLI]]:  Multi-Genre Natural Language Inference Corpus
- [QNLI](https://rajpurkar.github.io/SQuAD-explorer/):  The Stanford Question Answering Dataset
- [RTE](https://aclweb.org/aclwiki/Recognizing_Textual_Entailment): Recognizing Textual Entailment
- [[../../../Explorance/Research/Dataset/WSC - Winograd Schema Challenge|WNLI]]: Winograd NLI
