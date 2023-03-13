# 1.1

###### Abstract

We present the Stanford Question Answering Dataset (SQuAD), a new reading comprehension dataset consisting of 100,000+ questions posed by crowdworkers on a set of Wikipedia articles, where the answer to each question is a segment of text from the corresponding reading passage. We analyze the dataset to understand the types of reasoning required to answer the questions, leaning heavily on dependency and constituency trees. We build a strong logistic regression model, which achieves an F1 score of 51.0%, a significant improvement over a simple baseline (20%). However, human performance (86.8%) is much higher, indicating that the dataset presents a good challenge problem for future research.   
The dataset is freely available at [this https URL](https://stanford-qa.com/)

[Paper](https://arxiv.org/pdf/1606.05250)
status: #to/read 

# 2.0 
###### Abstract

Extractive reading comprehension systems can often locate the correct answer to a question in a context document, but they also tend to make unreliable guesses on questions for which the correct answer is not stated in the context. Existing datasets either focus exclusively on answerable questions, or use automatically generated unanswerable questions that are easy to identify. To address these weaknesses, we present SQuAD 2.0, the latest version of the Stanford Question Answering Dataset (SQuAD). SQuAD 2.0 combines existing SQuAD data with over 50,000 unanswerable questions written adversarially by crowdworkers to look similar to answerable ones. To do well on SQuAD 2.0, systems must not only answer questions when possible, but also determine when no answer is supported by the paragraph and abstain from answering. SQuAD 2.0 is a challenging natural language understanding task for existing models: a strong neural system that gets 86% F1 on SQuAD 1.1 achieves only 66% F1 on SQuAD 2.0.

[Paper](https://arxiv.org/abs/1806.03822)

status: #to/read 
tags: #dataset #QNLI #SQuAD


### HuggingFace

```python
from datasets import load_dataset
dataset = load_dataset('glue', 'qnli')
```

```python
DatasetDict({
    train: Dataset({
        features: ['question', 'sentence', 'label', 'idx'],
        num_rows: 104743
    })
    validation: Dataset({
        features: ['question', 'sentence', 'label', 'idx'],
        num_rows: 5463
    })
    test: Dataset({
        features: ['question', 'sentence', 'label', 'idx'],
        num_rows: 5463
    })
})
```

#### Features
```python
{
	'question': Value(dtype='string', id=None), 
	'sentence': Value(dtype='string', id=None), 
	'label': ClassLabel(num_classes=2, names=['entailment', 'not_entailment'], names_file=None, id=None), 
	'idx': Value(dtype='int32', id=None)
}
```

#### Example
```json
{
	'idx': 0, 
	'sentence': 'Unlike the two seasons before it and most of the seasons that followed, Digimon Tamers takes a darker and more realistic approach to its story featuring Digimon who do not reincarnate after their deaths and more complex character development in the original Japanese.', 
	'question': 'When did the third Digimon series begin?', 
	'label': 1
}
```