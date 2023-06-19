---
title: "Parameter-Efficient Transfer Learning for NLP"
tags:
    - plm
    - peft
    - transformer
url: 
    - "[Paper](https://arxiv.org/pdf/1902.00751.pdf)"
    - "[arxiv](https://arxiv.org/abs/1902.00751)"
published: "2019-02-02"
authors:
    - Neil Houlsby
    - Andrei Giurgiu
    - Stanisław Jastrzębski
    - Bruna Morrone
    - Quentin de Laroussilhe
    - Andrea Gesmundo
    - Mona Attariyan
    - Sylvain Gelly
status: "to read"
aliases:
    - "Houlsby et al., 2019"
---
# Parameter-Efficient Transfer Learning for NLP
###### Authors
<ul>
<li class="author">Neil Houlsby</li>
<li class="separator author">|</li>
<li class="author">Andrei Giurgiu</li>
<li class="separator author">|</li>
<li class="author">Stanisław Jastrzębski</li>
<li class="separator author">|</li>
<li class="author">Bruna Morrone</li>
<li class="separator author">|</li>
<li class="author">Quentin de Laroussilhe</li>
<li class="separator author">|</li>
<li class="author">Andrea Gesmundo</li>
<li class="separator author">|</li>
<li class="author">Mona Attariyan</li>
<li class="separator author">|</li>
<li class="author">Sylvain Gelly</li>
</ul>

```dataview
table without ID url from "Mathematics/Machine Learning/Papers/PEFT.md"
```

## TL;DR


> [!blank-container|right-medium]
> ![[../Research/Images/PEFT_figure2.svg]]
> > *Figure 2*. Architecture of the adapter module and its integration with the Transformer. **Left**: We add the adapter module twice to each Transformer layer: after the projection following multi-headed attention and after the two feed-forward layers. Right: **The** adapter consists of a bottleneck which contains few parameters relative to the attention and feedforward layers in the original model. The adapter also contains a skip-connection. During adapter tuning, the green layers are trained on the downstream data, this includes the adapter, the layer normalization parameters, and the final classification layer (not shown in the figure).


- The paper proposes the use of adapter modules for parameter-efficient tuning in NLP tasks. 
- The proposed strategy almost matches the performance of fully fine-tuned BERT on the GLUE benchmark, but uses only 3% task-specific parameters compared to 100% in fine-tuning. 
- Similar results are observed on 17 public text datasets and SQuAD extractive question answering. 
- Adapter-based tuning yields a single, extensible model that attains near state-of-the-art performance in text classification.
- Unlike multi-task learning, adapters do not require simultaneous access to tasks during training. The architecture used is a bottleneck adapter module, which can be represented mathematically as: $$ \text{Adapter}(x) = (\underbrace{W_2\underbrace{\text{ReLU}(\underbrace{W_1x+b_1}_{\text{bottleneck}})}_{\text{Non linearity}}+b_2}_{\text{up}})\underbrace{+x}_{\text{skip connection}} $$ where $x$ is the input to the adapter, $W_1 \in \mathbb{R}^{d \times m}$, $W_2 \in \mathbb{R}^{m \times d}$, $b_1  \in \mathbb{R}^{m}$, and $b_2  \in \mathbb{R}^{d \times d}$  are learnable parameters 
- Set $m \ll d$ to significantly reduce the number of learnable parameters. The bottleneck dimension, $m$, provides a simple means to tradeoff performance with parameter efficiency.
- Number of learnable parameters: $2md+d+m$

###### Abstract
Fine-tuning large pre-trained models is an effective transfer mechanism in NLP. However, in the presence of many downstream tasks, fine-tuning is parameter inefficient: an entire new model is required for every task. As an alternative, we propose transfer with adapter modules. Adapter modules yield a compact and extensible model; they add only a few trainable parameters per task, and new tasks can be added without revisiting previous ones. The parameters of the original network remain fixed, yielding a high degree of parameter sharing. To demonstrate adapter's effectiveness, we transfer the recently proposed BERT Transformer model to 26 diverse text classification tasks, including the GLUE benchmark. Adapters attain near state-of-the-art performance, whilst adding only a few parameters per task. On GLUE, we attain within 0.4% of the performance of full fine-tuning, adding only 3.6% parameters per task. By contrast, fine-tuning trains 100% of the parameters per task.

