# The Winograd Schema Challenge

###### Abstract

In this paper, we present an alternative to the Turing Test that has some conceptual and practical advantages. Like the original, it involves responding to typed English sentences, and English-speaking adults will have no difficulty with it. Unlike the original, the subject is not required to engage in a conversation and fool an interrogator into believing she is dealing with a person. Moreover, the test is arranged in such a way that having full access to a large corpus of English text might not help much. Finally, the interrogator or a third party will be able to decide unambiguously after a few minutes whether or not a subject has passed the test.

[Paper](http://commonsensereasoning.org/2011/papers/Levesque.pdf)

status: #to/read 
tags: #WSC #dataset #WNLI 

This task is used in both GLUE and SuperGLUE, but with some modifications. Here are the two available datasets

### HuggingFace Dataset (GLUE)

```python
from datasets import load_dataset
dataset = load_dataset('glue', 'wnli')
```

```python
DatasetDict({
    train: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 635
    })
    validation: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 71
    })
    test: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 146
    })
})
```

#### Features
```python
{
	'sentence1': Value(dtype='string', id=None), 
	'sentence2': Value(dtype='string', id=None), 
	'label': ClassLabel(num_classes=2, names=['not_entailment', 'entailment'], names_file=None, id=None), 
	'idx': Value(dtype='int32', id=None)
}
```

#### Example
```json
{
	'sentence2': 'The carrot had a hole.', 
	'idx': 0, 
	'sentence1': 'I stuck a pin through a carrot. When I pulled the pin out, it had a hole.', 
	'label': 1
}
```


### HuggingFace Dataset (SuperGLUE)

```python
from datasets import load_dataset
dataset = load_dataset('super_glue', 'wsc')
```

```python
DatasetDict({
    train: Dataset({
        features: ['text', 'span1_index', 'span2_index', 'span1_text', 'span2_text', 'idx', 'label'],
        num_rows: 554
    })
    validation: Dataset({
        features: ['text', 'span1_index', 'span2_index', 'span1_text', 'span2_text', 'idx', 'label'],
        num_rows: 104
    })
    test: Dataset({
        features: ['text', 'span1_index', 'span2_index', 'span1_text', 'span2_text', 'idx', 'label'],
        num_rows: 146
    })
})
```

#### Features
```python
{
	'text': Value(dtype='string', id=None),
	'span1_index': Value(dtype='int32', id=None),
	'span2_index': Value(dtype='int32', id=None),
	'span1_text': Value(dtype='string', id=None),
	'span2_text': Value(dtype='string', id=None),
	'idx': Value(dtype='int32', id=None),
	'label': ClassLabel(num_classes=2, names=['False', 'True'], names_file=None, id=None)
 }
```

#### Example
```json
{
	'text': "Mark was close to Mr. Singer 's heels. He heard him calling for the captain, promising him, in the jargon everyone talked that night, that not one thing should be damaged on the ship except only the ammunition, but the captain and all his crew had best stay in the cabin until the work was over",
	'span1_index': 4,
	'span2_index': 8,
	'span1_text': 'Mr. Singer',
	'span2_text': 'He',
	'idx': 2,
	'label': 0
}
```

Coreference resolution 
- `span1_text`: The name the pronoun refers to
- `span2_text`: The pronoun refering to another name
- `span1_index`: Index of the name, split on word. 
- `span2_index`: Index of the pronoun, split on word.
- `label`: 0 if the pronoun does not refers to the name. 1 if the pronoun refers to the name.

Notice how `Mr. Signer 's` are all split up by spaces. So a simple `text.split(' ')` should suffice.

TBD, how this is all fed to a model