

[Website](http://ixa2.si.ehu.eus/stswiki/index.php/STSbenchmark)

tags: #dataset #STS-B 

### HuggingFace

```python
from datasets import load_dataset
dataset = load_dataset('glue', 'stsb')
```

```python
DatasetDict({
    train: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 5749
    })
    validation: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 1500
    })
    test: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 1379
    })
})
```

#### Features
```python
{
	'sentence1': Value(dtype='string', id=None), 
	'sentence2': Value(dtype='string', id=None), 
	'label': Value(dtype='float32', id=None), 
	'idx': Value(dtype='int32', id=None)
}
```

#### Example
```json
{
	'sentence2': 'An air plane is taking off.', 
	'idx': 0, 
	'sentence1': 'A plane is taking off.', 
	'label': 5.0
}
```