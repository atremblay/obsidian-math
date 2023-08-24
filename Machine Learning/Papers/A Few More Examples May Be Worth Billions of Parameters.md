###### Abstract

We investigate the dynamics of increasing the number of model parameters versus the number of labeled examples across a wide variety of tasks. Our exploration reveals that while scaling parameters consistently yields performance improvements, the contribution of additional examples highly depends on the task's format. Specifically, in open question answering tasks, enlarging the training set does not improve performance. In contrast, classification, extractive question answering, and multiple choice tasks benefit so much from additional examples that collecting a few hundred examples is often "worth" billions of parameters. We hypothesize that unlike open question answering, which involves recalling specific information, solving strategies for tasks with a more restricted output space transfer across examples, and can therefore be learned with small amounts of labeled data.

status: #read

[Paper](https://arxiv.org/pdf/2110.04374)

**Key points**:
- Enlarging the model’s size is consistently beneficial
- For classification task, more data has the same effect as bigger model

Language models used are T5:
 
- T5-Small: 77M parameters
- T5-Base: 250M parameters
- T5-Large: 800M parameters
- T5-XL: 3B parameters


![[../../../Explorance/Research/Images/Pasted image 20211103084548.png]]
>  Each heatmap displays the model’s performance (F1/accuracy) given its size in parameters (horizontal axis) and the number of labeled examples available during fine-tuning (vertical axis).

Basic metric to gage what is more relevant, data or parameters. 0 is data, 1 is the model.

![[../../../Explorance/Research/Images/Pasted image 20211103090609.png|350]]


This fits the intuition that one can have about data versus model size. This gives more meat to the idea.

###### References

[[GPT-3]]
[[GLUE]]
[[SuperGLUE]]
[[T5]]