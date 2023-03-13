# Looking Beyond the Surface - A Challenge Set for Reading Comprehension over Multiple Sentences

###### Abstract

We present a reading comprehension challenge in which questions can only be answered by taking into account information from multiple sentences. We solicit and verify questions and answers for this challenge through a 4-step crowdsourcing experiment. Our challenge dataset contains ∼6k questions for +800 paragraphs across 7 different domains (elementary school science, news, travel guides, fiction stories, etc) bringing in linguistic diversity to the texts and to the questions wordings. On a subset of our dataset, we found human solvers to achieve an F1-score of 86.4%. We analyze a range of baselines, including a recent state-of-art reading comprehension system, and demonstrate the difficulty of this challenge, despite a high human performance. The dataset is the first to study multi-sentence inference at scale, with an open-ended set of question types that requires reasoning skills.

[Paper](https://aclanthology.org/N18-1023.pdf)

status: #to/read
tags: #MultiRC #dataset

MultiRC:  Multi-Sentence Reading Comprehension

### HuggingFace Dataset

```python
from datasets import load_dataset
dataset = load_dataset('super_glue', 'multirc')
```

```python
DatasetDict({
    train: Dataset({
        features: ['paragraph', 'question', 'answer', 'idx', 'label'],
        num_rows: 27243
    })
    validation: Dataset({
        features: ['paragraph', 'question', 'answer', 'idx', 'label'],
        num_rows: 4848
    })
    test: Dataset({
        features: ['paragraph', 'question', 'answer', 'idx', 'label'],
        num_rows: 9693
    })
})
```

#### Features
```python
{
	'paragraph': Value(dtype='string', id=None), 
	'question': Value(dtype='string', id=None), 
	'answer': Value(dtype='string', id=None), 
	'idx': {'paragraph': Value(dtype='int32', id=None), 
	'question': Value(dtype='int32', id=None), 
	'answer': Value(dtype='int32', id=None)}, 
	'label': ClassLabel(num_classes=2, names=['False', 'True'], names_file=None, id=None)
}
```

#### Example
```json
{
	"question": "What did the high-level effort to persuade Pakistan include?",
	"answer": "Children, Gerd, or Dorian Popa",
	"label": 0,
	"paragraph": "While this process moved along, diplomacy continued its rounds. Direct pressure on the Taliban had proved unsuccessful. As one NSC staff note put it, \"Under the Taliban, Afghanistan is not so much a state sponsor of terrorism as it is a state sponsored by terrorists.\" In early 2000, the United States began a high-level effort to persuade Pakistan to use its influence over the Taliban. In January 2000, Assistant Secretary of State Karl Inderfurth and the State Department's counterterrorism coordinator, Michael Sheehan, met with General Musharraf in Islamabad, dangling before him the possibility of a presidential visit in March as a reward for Pakistani cooperation. Such a visit was coveted by Musharraf, partly as a sign of his government's legitimacy. He told the two envoys that he would meet with Mullah Omar and press him on  Bin Laden. They left, however, reporting to Washington that Pakistan was unlikely in fact to do anything,\" given what it sees as the benefits of Taliban control of Afghanistan.\" President Clinton was scheduled to travel to India. The State Department felt that he should not visit India without also visiting Pakistan. The Secret Service and the CIA, however, warned in the strongest terms that visiting Pakistan would risk the President's life. Counterterrorism officials also argued that Pakistan had not done enough to merit a presidential visit. But President Clinton insisted on including Pakistan in the itinerary for his trip to South Asia. His one-day stopover on March 25, 2000, was the first time a U.S. president had been there since 1969. At his meeting with Musharraf and others, President Clinton concentrated on tensions between Pakistan and India and the dangers of nuclear proliferation, but also discussed  Bin Laden. President Clinton told us that when he pulled Musharraf aside for a brief, one-on-one meeting, he pleaded with the general for help regarding  Bin Laden.\" I offered him the moon when I went to see him, in terms of better relations with the United States, if he'd help us get  Bin Laden and deal with another issue or two.\" The U.S. effort continued.",
	"idx": {"paragraph": 0, "question": 0, "answer": 0}
}
```

For SuperGLUE, N possible answers for a question-paragraph pair are cast as N binary classification. The same question-paragraph pair will be used N times. Here the same question pair is used 9 times with 9 different answers.

```json
{
	'idx': [
		{'paragraph': 0, 'question': 0, 'answer': 0},
		{'paragraph': 0, 'question': 0, 'answer': 1},
		{'paragraph': 0, 'question': 0, 'answer': 2},
		{'paragraph': 0, 'question': 0, 'answer': 3},
		{'paragraph': 0, 'question': 0, 'answer': 4},
		{'paragraph': 0, 'question': 0, 'answer': 5},
		{'paragraph': 0, 'question': 0, 'answer': 6},
		{'paragraph': 0, 'question': 0, 'answer': 7},
		{'paragraph': 0, 'question': 0, 'answer': 8}
	]
 }
```

*TBD: How are the question, paragraph and answers typically fed to a model?*