# Choice of plausible alternatives - An evaluation of commonsense causal reasoning

###### Abstract

SemEval-2012 Task 7 presented a deceptively simple challenge: given an English sentence as a premise, select the sentence amongst two alternatives that more plausibly has a causal relation to the premise. In this paper, we describe the development of this task and its motivation. We describe the two systems that competed in this task as part of SemEval-2012, and compare their results to those achieved in previously published research. We discuss the characteristics that make this task so difficult, and offer our thoughts on how progress can be made in the future.

[Paper](https://www.researchgate.net/publication/221251392_Choice_of_Plausible_Alternatives_An_Evaluation_of_Commonsense_Causal_Reasoning)

status: #to/read 
tags: #COPA #dataset


### HuggingFace

```python
from datasets import load_dataset
dataset = load_dataset('super_glue', 'copa')
```

```python
DatasetDict({
    train: Dataset({
        features: ['premise', 'choice1', 'choice2', 'question', 'idx', 'label'],
        num_rows: 400
    })
    validation: Dataset({
        features: ['premise', 'choice1', 'choice2', 'question', 'idx', 'label'],
        num_rows: 100
    })
    test: Dataset({
        features: ['premise', 'choice1', 'choice2', 'question', 'idx', 'label'],
        num_rows: 500
    })
})
```

#### Features
```python
{
	'premise': Value(dtype='string', id=None), 
	'choice1': Value(dtype='string', id=None), 
	'choice2': Value(dtype='string', id=None), 
	'question': Value(dtype='string', id=None), 
	'idx': Value(dtype='int32', id=None), 
	'label': ClassLabel(num_classes=2, names=['choice1', 'choice2'], names_file=None, id=None)
}
```

#### Example
```json
{
	'choice1': 'The sun was rising.',
	'premise': 'My body cast a shadow over the grass.',
	'question': 'cause',
	'label': 0,
	'choice2': 'The grass was cut.',
	'idx': 0
 }
```

There are 2 types of `question`, `cause` and `effect`. We want to know which of the two choices (`choice1`,  `choice2`) either `cause` the `premise` or is an `effect` of it. 

In the example above, `choice1` is the `cause` of the `premise`. In the example below, `choice2` is the `effect` of the `premise`.

```json
{
	'choice1': 'He waited at the bus stop.',
	'premise': "The man's watch was broken.",
	'question': 'effect',
	'label': 1,
	'choice2': 'He asked a stranger for the time.',
	'idx': 398
}
```