# SuperGLUE A Stickier Benchmark for General-Purpose Language Understanding Systems

###### Abstract

In the last year, new models and methods for pretraining and transfer learning have driven striking performance improvements across a range of language understand- ing tasks. The [[GLUE|GLUE]] benchmark, introduced a little over one year ago, offers a single-number metric that summarizes progress on a diverse set of such tasks, but performance on the benchmark has recently surpassed the level of non-expert humans, suggesting limited headroom for further research. In this paper we present SuperGLUE, a new benchmark styled after GLUE with a new set of more difficult language understanding tasks, a software toolkit, and a public leaderboard. SuperGLUE is available at [super.gluebenchmark.com](https://super.gluebenchmark.com/tasks).

[Paper](https://arxiv.org/pdf/1905.00537.pdf)

status: #read
tags: #SuperGLUE

# Tasks

| Name | Identifier | Paper | Download | More Info | Metric |
|---|---|---|---|---|---|
|Broadcoverage Diagnostics|#AX-b||[Link](https://dl.fbaipublicfiles.com/glue/superglue/data/v2/AX-b.zip)|[Link](https://gluebenchmark.com/diagnostics)|Matthew's Corr|
|CommitmentBank|#CB|[[CommitmentBank\|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/superglue/data/v2/CB.zip)|[Link](https://github.com/mcdm/CommitmentBank)|Avg. F1/Accuracy|
|Choice of Plausible Alternatives|#COPA|[[COPA\|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/superglue/data/v2/COPA.zip)|[Link](http://people.ict.usc.edu/~gordon/copa.html)|Accuracy|
|Multi-Sentence Reading Comprehension|#MultiRC|[[MultiRC\|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/superglue/data/v2/MultiRC.zip)|[Link](https://cogcomp.org/multirc/)|F1a/EM|
|Recognizing Textual Entailment|#RTE|[[Recognizing Textual Entailment\|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/superglue/data/v2/RTE.zip)|[Link](https://aclweb.org/aclwiki/Recognizing_Textual_Entailment)|Accuracy|
|Words in Context|#WiC|[[WiC Word-in-Context\|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/superglue/data/v2/WiC.zip)|[Link](https://pilehvar.github.io/wic/)|Accuracy|
|The Winograd Schema Challenge|#WSC|[[WSC - Winograd Schema Challenge\|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/superglue/data/v2/WSC.zip)|[Link](https://cs.nyu.edu/faculty/davise/papers/WinogradSchemas/WS.html)|Accuracy|
|BoolQ|#BoolQ|[[BoolQ\|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/superglue/data/v2/BoolQ.zip)|[Link](https://github.com/google-research-datasets/boolean-questions)|Accuracy|
|Reading Comprehension with Commonsense Reading|#ReCoRD|[[ReCoRD\|Link]]|[Link](https://dl.fbaipublicfiles.com/glue/superglue/data/v2/ReCoRD.zip)|[Link](https://sheng-z.github.io/ReCoRD-explorer/)|F1/Accuracy|
|Winogender Schema Diagnostics|#AX-g||[Link](https://dl.fbaipublicfiles.com/glue/superglue/data/v2/AX-g.zip)|[Link](https://github.com/rudinger/winogender-schemas)|Gender Parity/Accuracy|

[Download all data](https://dl.fbaipublicfiles.com/glue/superglue/data/v2/combined.zip)

---

Overtime, [[GLUE|GLUE]] got too close to human performance, leaving very little headroom for improvements.

The success of these models on GLUE has been driven by ever-increasing model capacity, compute power, and data quantity, as well as innovations in model expressivity (from recurrent to bidirectional recurrent to multi-headed transformer encoders) and degree of contextualization (from learning representation of words in isolation to using unidirectional contexts and ultimately to leveraging bidirectional contexts).

![[GLUE benchmark.png]]


#### References

[[Sentence Encoders on STILTs Supplementary Training on Intermediate Labeled-data Tasks]]
[[Multi-task deep neural networks for natural language understanding]]
[[Snorkel DryBell - A Case Study in Deploying Weak Supervision at Industrial Scale]]
