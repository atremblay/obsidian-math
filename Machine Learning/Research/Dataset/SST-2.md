# Recursive Deep Models for Semantic Compositionality Over a Sentiment Treebank
###### Abstract

Semantic word spaces have been very useful but cannot express the meaning of longer phrases in a principled way. Further progress towards understanding compositionality in tasks such as sentiment detection requires richer supervised training and evaluation resources and more powerful models of composition. To remedy this, we introduce a Sentiment Treebank. It includes fine grained sentiment labels for 215,154 phrases in the parse trees of 11,855 sentences and presents new challenges for sentiment composition- ality. To address them, we introduce the Recursive Neural Tensor Network. When trained on the new treebank, this model out- performs all previous methods on several metrics. It pushes the state of the art in single sentence positive/negative classification from 80% up to 85.4%. The accuracy of predicting fine-grained sentiment labels for all phrases reaches 80.7%, an improvement of 9.7% over bag of features baselines. Lastly, it is the only model that can accurately capture the effects of negation and its scope at various tree levels for both positive and negative phrases.

[Paper](https://nlp.stanford.edu/~socherr/EMNLP2013_RNTN.pdf)

status: #to/read
tags: #SST-2 #dataset

### HuggingFace

```python
from datasets import load_dataset
dataset = load_dataset('glue', 'sst2')
```

```python
DatasetDict({
    train: Dataset({
        features: ['sentence', 'label', 'idx'],
        num_rows: 67349
    })
    validation: Dataset({
        features: ['sentence', 'label', 'idx'],
        num_rows: 872
    })
    test: Dataset({
        features: ['sentence', 'label', 'idx'],
        num_rows: 1821
    })
})
```

#### Features
```python
{
	'sentence': Value(dtype='string', id=None), 
	'label': ClassLabel(num_classes=2, names=['negative', 'positive'], names_file=None, id=None), 
	'idx': Value(dtype='int32', id=None)
}
```

#### Example
```json
{
	'sentence': 'this new jangle of noise , mayhem and stupidity must be a serious contender for the title . ',
	'idx': 67348,
	'label': 0
 }
```

Standard sentiment analysis. Very little interest.
