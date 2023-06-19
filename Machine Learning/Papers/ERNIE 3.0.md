# ERNIE 3.0 Large-scale Knowledge Enhanced Pre-training for Language Understanding and Generation

###### Abstract

Pre-trained models have achieved state-of-the-art results in various Natural Language Processing (NLP) tasks. Recent works such as T5 and GPT-3 have shown that scaling up pre-trained language models can improve their generalization abilities. Particularly, the GPT-3 model with 175 billion parameters shows its strong task-agnostic zero-shot/few-shot learning capabilities. Despite their success, these large-scale models are trained on plain texts without introducing knowledge such as linguistic knowledge and world knowledge. In addition, most large-scale models are trained in an auto-regressive way. As a result, this kind of traditional fine-tuning approach demonstrates relatively weak performance when solving downstream language understanding tasks. In order to solve the above problems, we propose a unified framework named ERNIE 3.0 for pre-training large-scale knowledge enhanced models. It fuses auto-regressive network and auto-encoding network, so that the trained model can be easily tailored for both natural language understanding and generation tasks with zero-shot learning, few-shot learning or fine-tuning. We trained the model with 10 billion parameters on a 4TB corpus consisting of plain texts and a large-scale knowledge graph. Empirical results show that the model outperforms the state-of-the-art models on 54 Chinese NLP tasks, and its English version achieves the first place on the [[SuperGLUE|SuperGLUE benchmark]] (July 3, 2021), surpassing the human performance by +0.8% (90.6% vs. 89.8%).

- It is mostly a framework with a bunch of pre-training tasks. Those retraining tasks are seemingly important before doing any fine-tuning. 
- The model itself relies heavily on [[Transformer-XL|Transformer XL]] for both the common module and the task specific modules. 
	- The task specific modules are small Transformer XL
	- There are only two task specific modules, NLU and NLG. 


There are only a few different type of tasks for GLUE. Sequence classification, token classification, etc.  For each of them, the model such as `*ForSequenceClassification` Is loaded. It has a few trainable layers. So for each task of that type, a model is trained. Most of the tasks will comcatenate two sequences together and give that as input to then classify. 

I read and understood about 50%. For now it's enough. 

status: #read 
tags: #model

[Paper](https://arxiv.org/pdf/2107.02137)

![[ERNIE 3.0 framework.png]]

 
  

- We propose a unified framework ERNIE 3.0, which combines [[Autoregressive model|auto-regressive]] network and auto-encoding network so that the trained model can handle both natural language understanding and generation tasks through zero-shot learning, few-shot learning or fine-tuning.
- We pre-train large scale knowledge enhanced models with 10 billion parameters and evaluate them with a series of experiments on both natural language understanding and natural language generation tasks. Experimental results show that ERNIE 3.0 consistently outperforms the state-of-the art models on 54 benchmarks by a large margin and achieves the first place on the SuperGLUE benchmark.


###### References
[[Megatron-LM]]

[[Transformer-XL]]

[[Switch Transformers]]
