# Automatically Constructing a Corpus of Sentential Paraphrases

###### Abstract

An obstacle to research in automatic paraphrase identification and generation is the lack of large-scale, publicly available labeled corpora of sentential paraphrases. This paper describes the creation of the recently-released Microsoft Research Paraphrase Corpus, which contains 5801 sentence pairs, each hand-labeled with a binary judg- ment as to whether the pair constitutes a paraphrase. The corpus was created using heuristic extraction techniques in conjunction with an SVM-based classi- fier to select likely sentence-level para- phrases from a large corpus of topic- clustered news data. These pairs were then submitted to human judges, who confirmed that 67% were in fact se- mantically equivalent. In addition to describing the corpus itself, we explore a number of issues that arose in defin- ing guidelines for the human raters.

[Paper](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/I05-50025B15D.pdf)

status: #to/read 
tags: #MRPC #dataset


### HuggingFace

```python
from datasets import load_dataset
dataset = load_dataset('super_glue', 'mrpc')
```

```python
DatasetDict({
    train: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 3668
    })
    validation: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 408
    })
    test: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 1725
    })
})
```

#### Features
```python
{
	'sentence1': Value(dtype='string', id=None), 
	'sentence2': Value(dtype='string', id=None), 
	'label': ClassLabel(num_classes=2, names=['not_equivalent', 'equivalent'], names_file=None, id=None), 
	'idx': Value(dtype='int32', id=None)}
```

#### Example
```json
{
	'sentence2': 'Referring to him as only " the witness " , Amrozi accused his brother of deliberately distorting his evidence .',
	'idx': 0,
	'sentence1': 'Amrozi accused his brother , whom he called " the witness " , of deliberately distorting his evidence .',
	'label': 1
 }
```
