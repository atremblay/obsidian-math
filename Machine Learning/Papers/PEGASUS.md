# PEGASUS: **P**re-training with **E**xtracted **Ga**p-sentences for Abstractive **Su**mmarization **S**equence-to-sequence models

 
###### Abstract

Recent work pre-training Transformers with self-supervised objectives on large text corpora has shown great success when fine-tuned on downstream NLP tasks including text summarization. However, pre-training objectives tailored for abstractive text summarization have not been explored. Furthermore there is a lack of systematic evaluation across diverse domains. In this work, we propose pre-training large Transformer-based encoder-decoder models on massive text corpora with a new self-supervised objective. In PEGASUS, important sentences are removed/masked from an input document and are generated together as one output sequence from the remaining sentences, similar to an extractive summary. We evaluated our best PEGASUS model on 12 downstream summarization tasks spanning news, science, stories, instructions, emails, patents, and legislative bills. Experiments demonstrate it achieves state-of-the-art performance on all 12 downstream datasets measured by ROUGE scores. Our model also shows surprising performance on low-resource summarization, surpassing previous state-of-the-art results on 6 datasets with only 1000 examples. Finally we validated our results using human evaluation and show that our model summaries achieve human performance on multiple datasets.

[Paper](https://arxiv.org/pdf/1912.08777.pdf)

tags: #model/lm #summarization 

Key points:
- Architecture. Uses [[Acronyms#^f91646|GSG]] and standard [[Acronyms#^a16dcb|MLM]]
- In this work, we study pre-training objectives specifically for abstractive text summarization and evaluate on 12 downstream datasets spanning news, science, short stories, instructions, emails, patents and legislative bills.
- GSG masks **important sentences** to be generated by the decoder. Deterministically choose sentences based on importance, rather than randomly.

![[PEGASUS.png|450]]
