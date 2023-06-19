# The CommitmentBank - Investigating projection in naturally occurring discourse

###### Abstract

This paper describes a new resource, the CommitmentBank, developed for the em- pirical investigation of the projection of finite clausal complements. A clausal complement is said to project when its content is understood as a commitment of the speaker even though the clause occurs under the scope of an entailment canceling operator such as negation or a question. The study of projection is therefore part of the study of commitments expressed by speakers to non-asserted sentence content. The content of clausal complements has been a central case for the study of projection, as there is a long-standing claim that clause-taking predicates fall into two classes—factives and nonfactives—distinguished on the basis of whether the contents of their complements project. This claim identifies the embedding predicate as the primary determinant of the projection behavior of these contents. The CommitmentBank is a corpus of naturally occurring discourses whose final sentence contains a clause-embedding predicate un- der an entailment canceling operator. In this paper, we describe the CommitmentBank and present initial results of analyses designed to evaluate the factive/nonfactive distinction and to investigate additional factors which affect the projectivity of clausal complements.

[Paper](https://ojs.ub.uni-konstanz.de/sub/index.php/sub/article/download/601/456/) (automatic download)

status: #to/read 
tags: #CB #dataset

### HuggingFace Dataset

```python
from datasets import load_dataset
dataset = load_dataset('super_glue', 'cb')
```

```python
DatasetDict({
    train: Dataset({
        features: ['premise', 'hypothesis', 'idx', 'label'],
        num_rows: 250
    })
    validation: Dataset({
        features: ['premise', 'hypothesis', 'idx', 'label'],
        num_rows: 56
    })
    test: Dataset({
        features: ['premise', 'hypothesis', 'idx', 'label'],
        num_rows: 250
    })
})
```

#### Features
```python
{
	'premise': Value(dtype='string', id=None), 
	'hypothesis': Value(dtype='string', id=None), 
	'idx': Value(dtype='int32', id=None), 
	'label': ClassLabel(num_classes=3, names=['entailment', 'contradiction', 'neutral'], names_file=None, id=None)
}
```

#### Examples
```json
{
	'label': 0,
	'idx': 0,
	'premise': 'It was a complex language. Not written down but handed down. One might say it was peeled down.',
	'hypothesis': 'the language was peeled down'
 }
```

```json
{
	'premise': "He's weird enough to have undressed me without thinking, according to some mad notion of the ``proper'' thing to do. Perhaps he thought I couldn't lie in bed with my clothes on.",
	'hypothesis': "she couldn't lie in bed with her clothes on",
	'idx': 23,
	'label': 1
 }
```

```json
{
	'premise': "``I hope you are settling down and the cat is well.'' This was a lie. She did not hope the cat was well.",
	'hypothesis': 'the cat was well',
	'idx': 33,
	'label': 2
 }
```


I do not understand what this dataset is