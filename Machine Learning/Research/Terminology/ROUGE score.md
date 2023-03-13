https://towardsdatascience.com/the-ultimate-performance-metric-in-nlp-111df6c64460

With ROUGE-N, the N represents the n-gram that we are using. For ROUGE-1 we would be measuring the match-rate of unigrams between our model output and reference.

ROUGE-2 and ROUGE-3 would use bigrams and trigrams respectively.

**Recall**
$$
\frac{\text{number of n-grams found in model and reference}}{\text{number of n-grams in reference}}
$$

![](https://miro.medium.com/max/1400/1*ukAN98eFglZIL8S0GEnysQ.png)

**Precision**
$$
\frac{\text{number of n-grams found in model and reference}}{\text{number of n-grams in model}}
$$

![](https://miro.medium.com/max/1400/1*8Du8ThhWqVFAX7YK6zSFyw.png)

**F1-Score** calculated as usual.

#### Example
```python
from rouge import Rouge

rouge = Rouge()

model_out = "he began by starting a five person war cabinet and included chamberlain as lord president of the council"

reference = "he began his premiership by forming a five-man war cabinet which included chamberlain as lord president of the council"

rouge.get_scores(model_out, reference)

[
	{   
		'rouge-1': {   
			'f': 0.7567567517604091,
            'p': 0.7777777777777778,
			'r': 0.7368421052631579
		},
        'rouge-2': {
			'f': 0.514285709289796, 
			'p': 0.5294117647058824, 
			'r': 0.5
		},
        'rouge-l': {   
			'f': 0.7567567517604091,
			'p': 0.7777777777777778,
			'r': 0.7368421052631579
		}
	}
]
```