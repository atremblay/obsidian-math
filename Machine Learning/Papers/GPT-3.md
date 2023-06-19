---
title: "Language Models are Few-Shot Learners"
authors:
    - Tom B. Brown
    - Benjamin Mann
    - Nick Ryder
    - Melanie Subbiah
    - Jared Kaplan
    - Prafulla Dhariwal
    - Arvind Neelakantan
    - Pranav Shyam
    - Girish Sastry
    - Amanda Askell
    - Sandhini Agarwal
    - Ariel Herbert-Voss
    - Gretchen Krueger
    - Tom Henighan
    - Rewon Child
    - Aditya Ramesh
    - Daniel M. Ziegler
    - Jeffrey Wu
    - Clemens Winter
    - Christopher Hesse
    - Mark Chen
    - Eric Sigler
    - Mateusz Litwin
    - Scott Gray
    - Benjamin Chess
    - Jack Clark
    - Christopher Berner
    - Sam McCandlish
    - Alec Radford
    - Ilya Sutskever
    - Dario Amodei
published: "2020-05-28"
url:
    - "[arxiv](https://arxiv.org/abs/2005.14165)"
    - "[paper](https://arxiv.org/pdf/2005.14165.pdf)"
---

# Language Models are Few-Shot Learners
###### Authors
<ul>
<li class="author">Tom B. Brown</li>
<li class="separator author">|</li>
<li class="author">Benjamin Mann</li>
<li class="separator author">|</li>
<li class="author">Nick Ryder</li>
<li class="separator author">|</li>
<li class="author">Melanie Subbiah</li>
<li class="separator author">|</li>
<li class="author">Jared Kaplan</li>
<li class="separator author">|</li>
<li class="author">Prafulla Dhariwal</li>
<li class="separator author">|</li>
<li class="author">Arvind Neelakantan</li>
<li class="separator author">|</li>
<li class="author">Pranav Shyam</li>
<li class="separator author">|</li>
<li class="author">Girish Sastry</li>
<li class="separator author">|</li>
<li class="author">Amanda Askell</li>
<li class="separator author">|</li>
<li class="author">Sandhini Agarwal</li>
<li class="separator author">|</li>
<li class="author">Ariel Herbert-Voss</li>
<li class="separator author">|</li>
<li class="author">Gretchen Krueger</li>
<li class="separator author">|</li>
<li class="author">Tom Henighan</li>
<li class="separator author">|</li>
<li class="author">Rewon Child</li>
<li class="separator author">|</li>
<li class="author">Aditya Ramesh</li>
<li class="separator author">|</li>
<li class="author">Daniel M. Ziegler</li>
<li class="separator author">|</li>
<li class="author">Jeffrey Wu</li>
<li class="separator author">|</li>
<li class="author">Clemens Winter</li>
<li class="separator author">|</li>
<li class="author">Christopher Hesse</li>
<li class="separator author">|</li>
<li class="author">Mark Chen</li>
<li class="separator author">|</li>
<li class="author">Eric Sigler</li>
<li class="separator author">|</li>
<li class="author">Mateusz Litwin</li>
<li class="separator author">|</li>
<li class="author">Scott Gray</li>
<li class="separator author">|</li>
<li class="author">Benjamin Chess</li>
<li class="separator author">|</li>
<li class="author">Jack Clark</li>
<li class="separator author">|</li>
<li class="author">Christopher Berner</li>
<li class="separator author">|</li>
<li class="author">Sam McCandlish</li>
<li class="separator author">|</li>
<li class="author">Alec Radford</li>
<li class="separator author">|</li>
<li class="author">Ilya Sutskever</li>
<li class="separator author">|</li>
<li class="author">Dario Amodei</li>
</ul>

```dataview
table without ID url from "Mathematics/Machine Learning/Papers/GPT-3.md"
```

#### Abstract

Recent work has demonstrated substantial gains on many NLP tasks and benchmarks by pre-training on a large corpus of text followed by fine-tuning on a specific task. While typically task-agnostic in architecture, this method still requires task-specific fine-tuning datasets of thousands or tens of thousands of examples. By contrast, humans can generally perform a new language task from only a few examples or from simple instructions - something which current NLP systems still largely struggle to do. Here we show that scaling up language models greatly improves task-agnostic, few-shot performance, sometimes even reaching competitiveness with prior state-of-the-art fine-tuning approaches. Specifically, we train GPT-3, an autoregressive language model with 175 billion parameters, 10x more than any previous non-sparse language model, and test its performance in the few-shot setting. For all tasks, GPT-3 is applied without any gradient updates or fine-tuning, with tasks and few-shot demonstrations specified purely via text interaction with the model. GPT-3 achieves strong performance on many NLP datasets, including translation, question-answering, and cloze tasks, as well as several tasks that require on-the-fly reasoning or domain adaptation, such as unscrambling words, using a novel word in a sentence, or performing 3-digit arithmetic. At the same time, we also identify some datasets where GPT-3's few-shot learning still struggles, as well as some datasets where GPT-3 faces methodological issues related to training on large web corpora. Finally, we find that GPT-3 can generate samples of news articles which human evaluators have difficulty distinguishing from articles written by humans. We discuss broader societal impacts of this finding and of GPT-3 in general.

[Paper](https://arxiv.org/pdf/2005.14165v4)

status: #to/read 
tags: #lm #model 