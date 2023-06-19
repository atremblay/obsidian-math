
 
 https://allaboutmachinelearning.com/quora-question-pairs/
 
 status: #to/read 
 tags: #dataset #QQP 
 
 
 ### HuggingFace

```python
from datasets import load_dataset
dataset = load_dataset('super_glue', 'boolq')
```

 ```python
 DatasetDict({
    train: Dataset({
        features: ['question1', 'question2', 'label', 'idx'],
        num_rows: 363846
    })
    validation: Dataset({
        features: ['question1', 'question2', 'label', 'idx'],
        num_rows: 40430
    })
    test: Dataset({
        features: ['question1', 'question2', 'label', 'idx'],
        num_rows: 390965
    })
})
```

#### Features
```python
{
	'question1': Value(dtype='string', id=None), 
	'question2': Value(dtype='string', id=None), 
	'label': ClassLabel(num_classes=2, names=['not_duplicate', 'duplicate'], names_file=None, id=None), 
	'idx': Value(dtype='int32', id=None)
}
```

#### Example
```json
{
	'question2': 'Which level of prepration is enough for the exam jlpt5?', 
	'idx': 0, 
	'label': 0, 
	'question1': 'How is the life of a math student? Could you describe your own experiences?'
}
 ```
 