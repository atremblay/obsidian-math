
# BoolQ Exploring the Surprising Difficulty of Natural Yes-No Questions

###### Abstract

In this paper we study yes/no questions that are naturally occurring — meaning that they are generated in unprompted and unconstrained settings. We build a reading comprehension dataset, BoolQ, of such questions, and show that they are unexpectedly challenging. They often query for complex, non-factoid information, and require difficult entailment-like inference to solve. We also explore the effectiveness of a range of transfer learning baselines. We find that transferring from entailment data is more effective than transferring from paraphrase or extractive QA data, and that it, surprisingly, continues to be very beneficial even when starting from massive pre-trained language models such as BERT. Our best method trains BERT on MultiNLI and then re-trains it on our train set. It achieves 80.4% accuracy compared to 90% accuracy of human annotators (and 62% majority-baseline), leaving a significant gap for future work.

[Paper](https://arxiv.org/pdf/1905.10044)

status: #to/read 
tags: #BoolQ #dataset


### HuggingFace

```python
from datasets import load_dataset
dataset = load_dataset('super_glue', 'boolq')
```

```python
DatasetDict({
    train: Dataset({
        features: ['question', 'passage', 'idx', 'label'],
        num_rows: 9427
    })
    validation: Dataset({
        features: ['question', 'passage', 'idx', 'label'],
        num_rows: 3270
    })
    test: Dataset({
        features: ['question', 'passage', 'idx', 'label'],
        num_rows: 3245
    })
})
```

#### Features
```python
{
	'question': Value(dtype='string', id=None), 
	'passage': Value(dtype='string', id=None), 
	'idx': Value(dtype='int32', id=None), 
	'label': ClassLabel(num_classes=2, names=['False', 'True'], names_file=None, id=None)
}
```

#### Example
```json
{
	'passage': 'Persian language -- Persian (/ˈpɜːrʒən, -ʃən/), also known by its endonym Farsi (فارسی fārsi (fɒːɾˈsiː) ( listen)), is one of the Western Iranian languages within the Indo-Iranian branch of the Indo-European language family. It is primarily spoken in Iran, Afghanistan (officially known as Dari since 1958), and Tajikistan (officially known as Tajiki since the Soviet era), and some other regions which historically were Persianate societies and considered part of Greater Iran. It is written in the Persian alphabet, a modified variant of the Arabic script, which itself evolved from the Aramaic alphabet.',
	'idx': 0,
	'label': 1,
	'question': 'do iran and afghanistan speak the same language'
 }
```

The `question` and `passage` are the inputs, then it's a binary classification. Answering boolean questions. I believe the are normally concatenated with a `[SEP]` token inbetween.