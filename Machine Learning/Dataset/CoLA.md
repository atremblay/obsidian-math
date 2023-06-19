# Neural Network Acceptability Judgments

###### Abstract

This paper investigates the ability of artificial neural networks to judge the grammatical acceptability of a sentence, with the goal of testing their linguistic competence. We introduce the Corpus of Linguistic Acceptability (CoLA), a set of 10,657 English sentences labeled as grammatical or ungrammatical from published linguistics literature. As baselines, we train several recurrent neural network models on acceptability classification, and find that our models outperform unsupervised models by Lau et al. (2016) on CoLA. Error-analysis on specific grammatical phenomena reveals that both Lau et al.’s models and ours learn systematic generalizations like subject-verb-object order. However, all models we test perform far below human level on a wide range of gram- matical constructions.

[Paper](https://arxiv.org/pdf/1805.12471.pdf)

status: #to/read 
tags: #CoLA #dataset


### HuggingFace Dataset

```python
from datasets import load_dataset
dataset = load_dataset('glue', 'cola')
```

```python
DatasetDict({
    train: Dataset({
        features: ['sentence', 'label', 'idx'],
        num_rows: 8551
    })
    validation: Dataset({
        features: ['sentence', 'label', 'idx'],
        num_rows: 1043
    })
    test: Dataset({
        features: ['sentence', 'label', 'idx'],
        num_rows: 1063
    })
})
```

#### Features
```python
{
	'sentence': Value(dtype='string', id=None), 
	'label': ClassLabel(num_classes=2, names=['unacceptable', 'acceptable'], names_file=None, id=None), 
	'idx': Value(dtype='int32', id=None)
}
```

#### Example
```json
{
	'sentence': "Our friends won't buy this analysis, let alone the next one we propose.",
	'idx': 0,
	'label': 1
}
```

```json
{
	'sentence': "Anson left before Jenny saw himself.", 
	'idx': 8539, 
	'label': 0
}
```


There are probably many causes for a sentence to be ungrammatical. Probably a good idea to list them if not already done in the paper.