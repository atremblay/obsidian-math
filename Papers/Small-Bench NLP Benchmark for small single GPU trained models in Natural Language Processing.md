###### Abstract

Recent progress in the Natural Language Processing domain has given us several State-of-the-Art (SOTA) pretrained models which can be finetuned for specific tasks. These large models with billions of parameters trained on numerous GPUs/TPUs over weeks are leading in the benchmark leaderboards. In this paper, we discuss the need for a benchmark for cost and time effective smaller models trained on a single GPU. This will enable researchers with resource constraints experiment with novel and innovative ideas on tokenization, pretraining tasks, architecture, fine tuning methods etc. We set up Small-Bench NLP, a benchmark for small efficient neural language models trained on a single GPU. Small-Bench NLP benchmark comprises of eight NLP tasks on the publicly available [[GLUE|GLUE]] datasets and a leaderboard to track the progress of the community. Our ELECTRA-DeBERTa (15M parameters) small model architecture achieves an average score of 81.53 which is comparable to that of BERT-Base's 82.20 (110M parameters). Our models, code and leaderboard are available at [this https URL](https://github.com/smallbenchnlp)

status: #to/read 
tags: #model #nlu

[Paper](https://arxiv.org/pdf/2109.10847)