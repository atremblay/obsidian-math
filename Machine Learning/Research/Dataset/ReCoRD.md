# Bridging the Gap between Human and Machine Commonsense Reading Comprehension

###### Abstract

We present a large-scale dataset, ReCoRD, for machine reading comprehension requiring commonsense reasoning. Experiments on this dataset demonstrate that the performance of state-of-the-art MRC systems fall far behind human performance. ReCoRD represents a challenge for future research to bridge the gap between human and machine commonsense reading comprehension. ReCoRD is available at http://nlp.jhu.edu/record.

[Paper](https://arxiv.org/pdf/1810.12885.pdf)

status: #to/read
tags: #ReCoRD #dataset


### HuggingFace dataset

```python
from datasets import load_dataset
dataset = load_dataset('super_glue', 'record')
```

```python
DatasetDict({
    train: Dataset({
        features: ['passage', 'query', 'entities', 'answers', 'idx'],
        num_rows: 100730
    })
    validation: Dataset({
        features: ['passage', 'query', 'entities', 'answers', 'idx'],
        num_rows: 10000
    })
    test: Dataset({
        features: ['passage', 'query', 'entities', 'answers', 'idx'],
        num_rows: 10000
    })
})
```

#### Features
```python
{
	'passage': Value(dtype='string', id=None), 
	'query': Value(dtype='string', id=None), 
	'entities': Sequence(feature=Value(dtype='string', id=None), length=-1, id=None), 
	'answers': Sequence(feature=Value(dtype='string', id=None), length=-1, id=None), 
	'idx': {
		'passage': Value(dtype='int32', id=None), 
		'query': Value(dtype='int32', id=None)
	}
}
```

#### Example
```json
{
	'entities': [
		'CNN',
		'GOP',
		'Gingrich',
		'Newt Gingrich',
		'Paul',
		'Republican',
		'Republicanism',
		'Ron Paul',
		'Stanley',
		'Timothy Stanley'
	],
	'answers': ['GOP'],
	'query': 'All of this means the @placeholder can no longer ignore its libertarian "fringe."',
	'passage': "(CNN) -- Newt Gingrich quit the presidential race on Wednesday. Long after he exhausted the patience of the voters, he finally concluded that the mathematical probability of winning the Republican nomination was next to nil. Why spend money and raise false hopes if you can't win? Best to get out now and join the veepstakes. That's the kind of logic that an ordinary, candidate-focused campaign employs. Ron Paul, on the other hand, refuses to drop out. Commanding a plurality of delegates in only one state, and having taken just 10.61 percent nationally so far, it could be argued that the 76-year-old libertarian has even less reason to carry on than Gingrich -- except perhaps to collect the air miles.\n@highlight\nTimothy Stanley says Ron Paul has gained influence, if not a nomination\n@highlight\nPaul's brand of libertarian Republicanism deserves GOP notice, he says\n@highlight\nPaul represents a message that is bigger than the candidate, Stanley says",
	'idx': {'passage': 65707, 'query': 100728}
}
 ```
 
 In `query`, replace `@placeholder` by each `entity`. There is only one good `answer`. So I guess it's a bunch of binary classifiers for each examples.