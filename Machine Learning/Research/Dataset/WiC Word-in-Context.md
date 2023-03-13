# Word-in-Context Dataset for Evaluating Context-Sensitive Meaning Representations
###### Abstract

By design, word embeddings are unable to model the dynamic nature of words’ semantics, i.e., the property of words to correspond to potentially different meanings. To address this limitation, dozens of specialized meaning representation techniques such as sense or contextualized embeddings have been proposed. However, despite the popularity of research on this topic, very few evaluation benchmarks exist that specifically focus on the dynamic semantics of words. In this paper we show that existing models have surpassed the performance ceiling of the standard evaluation dataset for the purpose, i.e., Stanford Contextual Word Similarity, and highlight its short- comings. To address the lack of a suitable benchmark, we put forward a large-scale Word in Context dataset, called WiC, based on annotations curated by experts, for generic evaluation of context-sensitive representations. WiC is released in https://pilehvar.github.io/wic/.

[Paper](https://arxiv.org/pdf/1808.09121.pdf)

status: #to/read 
tags: #WiC #dataset


### HuggingFace Dataset

```python
from datasets import load_dataset
dataset = load_dataset('super_glue', 'wic')
```

```python
DatasetDict({
    train: Dataset({
        features: ['word', 'sentence1', 'sentence2', 'start1', 'start2', 'end1', 'end2', 'idx', 'label'],
        num_rows: 5428
    })
    validation: Dataset({
        features: ['word', 'sentence1', 'sentence2', 'start1', 'start2', 'end1', 'end2', 'idx', 'label'],
        num_rows: 638
    })
    test: Dataset({
        features: ['word', 'sentence1', 'sentence2', 'start1', 'start2', 'end1', 'end2', 'idx', 'label'],
        num_rows: 1400
    })
})
```

#### Features
```python
{
	'word': Value(dtype='string', id=None), 
	'sentence1': Value(dtype='string', id=None), 
	'sentence2': Value(dtype='string', id=None), 
	'start1': Value(dtype='int32', id=None), 
	'start2': Value(dtype='int32', id=None), 
	'end1': Value(dtype='int32', id=None), 
	'end2': Value(dtype='int32', id=None), 
	'idx': Value(dtype='int32', id=None), 
	'label': ClassLabel(num_classes=2, names=['False', 'True'], names_file=None, id=None)
}
```

#### Example
```json
{
	'sentence1': 'The general ordered the colonel to hold his position at all costs.',
	'idx': 3,
	'label': 1,
	'end1': 39,
	'start2': 0,
	'sentence2': 'Hold the taxi.',
	'word': 'hold',
	'start1': 35,
	'end2': 4
}
```

The same word is present in both sentences. Do they have the same meaning. `sentence1[start1: end1] == sentence2[start2: end2]` in terms of usage.

Q: Given the contextual word representation of BERT, I would assume this is an easy task. Is that the case?