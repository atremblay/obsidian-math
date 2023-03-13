###### Abstract

This paper introduces the Multi-Genre Natural Language Inference (MultiNLI) corpus, a dataset designed for use in the development and evaluation of machine learning models for sentence understanding. At 433k examples, this resource is one of the largest corpora available for natural language inference (a.k.a. recognizing textual entailment), improving upon available resources in both its coverage and difficulty. MultiNLI accomplishes this by offering data from ten distinct genres of written and spoken English, making it possible to evaluate systems on nearly the full complexity of the language, while supplying an explicit setting for evaluating cross-genre domain adaptation. In addition, an evaluation using existing machine learning models designed for the Stanford NLI corpus shows that it represents a substantially more difficult task than does that corpus, despite the two showing similar levels of inter-annotator agreement.

[Paper](https://cims.nyu.edu/~sbowman/multinli/paper.pdf) 

status: #to/read 
tags: #MultiNLI #dataset 

### HuggingFace

```python
from datasets import load_dataset
dataset = load_dataset('glue', 'mnli')
```

```python
DatasetDict({
    train: Dataset({
        features: ['premise', 'hypothesis', 'label', 'idx'],
        num_rows: 392702
    })
    validation_matched: Dataset({
        features: ['premise', 'hypothesis', 'label', 'idx'],
        num_rows: 9815
    })
    validation_mismatched: Dataset({
        features: ['premise', 'hypothesis', 'label', 'idx'],
        num_rows: 9832
    })
    test_matched: Dataset({
        features: ['premise', 'hypothesis', 'label', 'idx'],
        num_rows: 9796
    })
    test_mismatched: Dataset({
        features: ['premise', 'hypothesis', 'label', 'idx'],
        num_rows: 9847
    })
})
```

#### Features
```python
{
	'premise': Value(dtype='string', id=None),
	'hypothesis': Value(dtype='string', id=None),
	'label': ClassLabel(num_classes=3, names=['entailment', 'neutral', 'contradiction'], names_file=None, id=None),
	'idx': Value(dtype='int32', id=None)
}
```

#### Example
```json
{
	'premise': 'One of our number will carry out your instructions minutely.',
	'label': 0,
	'hypothesis': 'A member of my team will execute your orders with immense precision.',
	'idx': 2
 }
```
