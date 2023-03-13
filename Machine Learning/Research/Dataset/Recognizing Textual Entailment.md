
 

The Recognizing Textual Entailment (RTE) datasets come from a series of annual textual entailment challenges. We combine the data from RTE1 (Dagan et al., 2006), RTE2 (Bar Haim etal.,2006),RTE3(Giampiccoloetal.,2007),andRTE5(Bentivoglietal.,2009).4 Examplesare constructed based on news and Wikipedia text. We convert all datasets to a two-class split, where for three-class datasets we collapse neutral and contradiction into not entailment, for consistency.

status: #to/read 
tags: #dataset #RTE 

This task is in both GLUE and SuperGLUE. Very similar, only the feature names changed.

### HuggingFace (GLUE)

```python
from datasets import load_dataset
dataset = load_dataset('glue', 'rte')
```

```python
DatasetDict({
    train: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 2490
    })
    validation: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 277
    })
    test: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 3000
    })
})
```

#### Features
```python
{
	'sentence1': Value(dtype='string', id=None), 
	'sentence2': Value(dtype='string', id=None), 
	'label': ClassLabel(num_classes=2, names=['entailment', 'not_entailment'], names_file=None, id=None), 
	'idx': Value(dtype='int32', id=None)
}
```

#### Example
```json
{
	'sentence2': 'Weapons of Mass Destruction Found in Iraq.', 
	'idx': 0, 
	'sentence1': 'No Weapons of Mass Destruction Found in Iraq Yet.', 
	'label': 1
}
```

### HuggingFace (SuperGLUE)

```python
from datasets import load_dataset
dataset = load_dataset('super_glue', 'rte')
```

```python
DatasetDict({
    train: Dataset({
        features: ['premise', 'hypothesis', 'idx', 'label'],
        num_rows: 2490
    })
    validation: Dataset({
        features: ['premise', 'hypothesis', 'idx', 'label'],
        num_rows: 277
    })
    test: Dataset({
        features: ['premise', 'hypothesis', 'idx', 'label'],
        num_rows: 3000
    })
})
```

#### Features
```python
{
	'premise': Value(dtype='string', id=None), 
	'hypothesis': Value(dtype='string', id=None), 
	'idx': Value(dtype='int32', id=None), 
	'label': ClassLabel(num_classes=2, names=['entailment', 'not_entailment'], names_file=None, id=None)}
```

#### Example
```json
{
	'premise': 'No Weapons of Mass Destruction Found in Iraq Yet.', 
	'label': 1, 
	'hypothesis': 'Weapons of Mass Destruction Found in Iraq.', 
	'idx': 0
}
```