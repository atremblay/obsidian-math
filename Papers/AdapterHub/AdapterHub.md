---
title: "AdapterHub: A Framework for Adapting Transformers"
published: "2020-07-15"
url: 
    - "[paper](https://arxiv.org/pdf/2007.07779.pdf)"
    - "[arxiv](https://arxiv.org/abs/2007.07779)"
    - "[AdapterHub](https://adapterhub.ml)"
authors:
    - Jonas Pfeiffer
    - Andreas Rücklé
    - Clifton Poth
    - Aishwarya Kamath
    - Ivan Vulic
    - Sebastian Ruder
    - Kyunghyun Cho
    - Iryna Gurevych
tags:
    - plm
---

# AdapterHub: A Framework for Adapting Transformers
###### Authors
<ul>
<li class="author">Jonas Pfeiffer</li>
<li class="separator author">|</li>
<li class="author">Andreas Rücklé</li>
<li class="separator author">|</li>
<li class="author">Clifton Poth</li>
<li class="separator author">|</li>
<li class="author">Aishwarya Kamath</li>
<li class="separator author">|</li>
<li class="author">Ivan Vulic</li>
<li class="separator author">|</li>
<li class="author">Sebastian Ruder</li>
<li class="separator author">|</li>
<li class="author">Kyunghyun Cho</li>
<li class="separator author">|</li>
<li class="author">Iryna Gurevych</li>
</ul>

^22ed79

```dataview
table without ID url from "Mathematics/Machine Learning/Papers/AdapterHub/AdapterHub.md"
```

#### Abstract

The current modus operandi in NLP involves downloading and fine-tuning pre-trained models consisting of hundreds of millions, or even billions of parameters. Storing and sharing such large trained models is expensive, slow, and time-consuming, which impedes progress towards more general and versatile NLP methods that learn from and for many tasks. Adapters-small learnt bottleneck layers inserted within each layer of a pretrained model- ameliorate this issue by avoiding full fine-tuning of the entire model. However, sharing and integrating adapter layers is not straightforward. We propose AdapterHub, a framework that allows dynamic "stiching-in" of pre-trained adapters for different tasks and languages. The framework, built on top of the popular HuggingFace Transformers library, enables extremely easy and quick adaptations of state-of-the-art pre-trained models (e.g., BERT, RoBERTa, XLM-R) across tasks and languages. Downloading, sharing, and training adapters is as seamless as possible using minimal changes to the training scripts and a specialized infrastructure. Our framework enables scalable and easy access to sharing of task-specific models, particularly in low resource scenarios. AdapterHub includes all recent adapter architectures and can be found at [AdapterHub.ml](https://adapterhub.ml).


## Introduction

Recent advances in NLP leverage transformer based language models ([[../Attention is all you need|Attention is all you need]]), pretrained on large amounts of text data ([[../BERT|BERT]]; Liu et al., 2019; Conneau et al., 2020). These models are fine-tuned on a target task and achieve state-of-the-art (SotA) performance for most natural language understanding tasks. Their performance has been shown to scale with their size (Kaplan et al., 2020) and recent models have reached billions of parameters (Raffel et al., 2019; Brown et al., 2020). While fine-tuning large pre-trained models on target task data can be done fairly efficiently (Howard and Ruder, 2018), training them for multiple tasks and sharing trained models is often prohibitive. This precludes research on more modular architectures (Shazeer et al., 2017), task composition (Andreas et al., 2016), and injecting biases and external information (e.g., world or linguistic knowledge) into large models (Lauscher et al., 2019; Wang et al., 2020).

*Adapters* ([[../PEFT|PEFT]]) have been introduced as an alternative lightweight fine-tuning strategy that achieves on-par performance to full fine-tuning (Peters et al., 2019) on most tasks. They consist of a small set of additional newly initialized weights at every layer of the transformer. These weights are then trained during fine-tuning, while the pre-trained parameters of the large model are kept frozen/fixed. This enables efficient parameter sharing between tasks by training many task-specific and language-specific adapters for the same model, which can be exchanged and combined post-hoc. Adapters have recently achieved strong results in multi-task and cross-lingual transfer learning (Pfeiffer et al., 2020a,b).

However, reusing and sharing adapters is not straightforward. Adapters are rarely released individually; their architectures differ in subtle yet important ways, and they are model, task, and language dependent. To mitigate these issues and facilitate transfer learning with adapters in a range of settings, we propose AdapterHub, a framework that enables seamless training and sharing of adapters.

AdapterHub is built on top of the popular `transformers` framework by HuggingFace[^1] (Wolf et al., 2020), which provides access to state-of-the-art pre-trained language models. We enhance `transformers` with adapter modules that can be combined with existing SotA models with minimal code edits. We additionally provide a website that enables quick and seamless upload, download, and sharing of pre-trained adapters. AdapterHub is available online at: [AdapterHub.ml](https://adapterhub.ml).

[^1]: https://github.com/huggingface/transformers 

AdapterHub for the first time enables NLP researchers and practitioners to easily and efficiently share and obtain access to models that have been trained for particular tasks, domains, and languages. This opens up the possibility of building on and combining information from many more sources than was previously possible, and makes research such as intermediate task training (Pruksachatkun et al., 2020), composing information from many tasks (Pfeiffer et al., 2020a), and training models for very low-resource languages (Pfeiffer et al., 2020b) much more accessible.

###### Contributions
1. We propose an easy-to-use and extensible adapter training and sharing framework for transformer-based models such as BERT, RoBERTa, and XLM(-R); 
2. we incorporate it into the HuggingFace transformers framework, requiring as little as two additional lines of code to train adapters with existing scripts;
3. our framework automatically extracts the adapter weights, storing them separately to the pre-trained transformer model, requiring as little as 1Mb of storage;
4. we provide an open-source framework and website that allows the community to upload their adapter weights, making them easily accessible with only one additional line of code;
5. we incorporate adapter composition as well as adapter stacking out-of-the-box and pave the way for a wide range of other extensions in the future.

## Adapters

While the predominant methodology for transfer learning is to fine-tune all weights of the pre-trained model, adapters have recently been introduced as an alternative approach, with applications in computer vision (Rebuffi et al., 2017) as well as the NLP domain ([[../PEFT|PEFT]]; Bapna and Firat, 2019; Wang et al., 2020; Pfeiffer et al., 2020a,b).

### Adapter Architecture

Adapters are neural modules with a small amount of additional newly introduced parameters $\Phi$ within a large pre-trained model with parameters $\Theta$. The parameters $\Phi$ are learnt on a target task while keeping $\Theta$ fixed; $\Phi$ thus learn to encode task-specific representations in intermediate layers of the pretrained model. Current work predominantly focuses on training adapters for each task separately ([[../PEFT|PEFT]]; Bapna and Firat, 2019; Pfeiffer et al., 2020a,b), which enables parallel training and subsequent combination of the weights.

In NLP, adapters have been mainly used within deep transformer-based architectures ([[../Attention is all you need|Attention is all you need]]). At each transformer layer $l$, a set of adapter parameters $\Phi_{l}$ is introduced. The placement and architecture of adapter parameters $\Phi$ within a pre-trained model is non-trivial and may impact their efficacy: Houlsby et al. (2019) experiment with different adapter architectures, empirically validating that a two-layer feed-forward neural network with a bottleneck works well. While this down- and up-projection has largely been agreed upon, the actual placement of adapters within each transformer block, as well as the introduction of new LayerNorms[^2] (Ba et al., 2016) varies in the literature ([[../PEFT|PEFT]]; Bapna and Firat, 2019; Stickland and Murray, 2019; Pfeiffer et al., 2020a). In order to support standard adapter architectures from the literature, as well as to enable easy extensibility, AdapterHub provides a configuration file where the architecture settings can be defined dynamically. We illustrate the different configuration possibilities in [[#^522ce5|Figure 3]], and describe them in more detail in $\S 3$.

[^2]: Layer normalization learns to normalize the inputs across the features. This is usually done by introducing a new set of features for mean and variance.

### Why Adapters?

Adapters provide numerous benefits over fully finetuning a model such as scalability, modularity, and composition. We now provide a few use-cases for adapters to illustrate their usefulness in practice.

Task-specific Layer-wise Representation Learning. Prior to the introduction of adapters, in order to achieve SotA performance on downstream tasks, the entire pre-trained transformer model needs to be fine-tuned (Peters et al., 2019). Adapters have been shown to work on-par with full fine-tuning, by adapting the representations at every layer. We present the results of fully fine-tuning the model compared to two different adapter architectures on the GLUE benchmark (Wang et al., 2018) in Table 1. The adapters of Houlsby et al. (2019, Figure 3c) and Pfeiffer et al. (2020a, Figure 3b) comprise two and one down- and up-projection


> [!blank-container]
> |  | Full | Pfeif. | Houl. |
> | :--- | :--- | :--- | :--- |
> | RTE (Wang et al., 2018) | 66.2 | 70.8 | 69.8 |
> | MRPC (Dolan and Brockett, 2005) | 90.5 | 89.7 | 91.5 |
> | STS-B (Cer et al., 2017) | 88.8 | 89.0 | 89.2 |
> | CoLA (Warstadt et al., 2019) | 59.5 | 58.9 | 59.1 |
> | SST-2 (Socher et al., 2013) | 92.6 | 92.2 | 92.8 |
> | QNLI (Rajpurkar et al., 2016) | 91.3 | 91.3 | 91.2 |
> | MNLI (Williams et al., 2018) | 84.1 | 84.1 | 84.1 |
> | QQP (Iyer et al., 2017) | 91.4 | 90.5 | 90.8 |
> 
> > **Table 1**: Mean development scores over 3 runs on GLUE (Wang et al., 2018) leveraging the BERT-Base pre-trained weights. We present the results with full fine-tuning (Full) and with the adapter architectures of Pfeiffer et al. (2020a, Pfeif., Figure 3b) and Houlsby et al. (2019, Houl., Figure 3c) both with bottleneck size 48. We show F1 for MRPC, Spearman rank correlation for STS-B, and accuracy for the rest. RTE is a combination of datasets (Dagan et al., 2005; Bar-Haim et al., 2006; Giampiccolo et al., 2007).
> 

within each transformer layer, respectively. The former adapter thus has more capacity at the cost of training and inference speed. We find that for all settings, there is no large difference in terms of performance between the model architectures, verifying that training adapters is a suitable and lightweight alternative to full fine-tuning in order to achieve SotA performance on downstream tasks.

Small, Scalable, Shareable. Transformer-based models are very deep neural networks with millions or billions of weights and large storage requirements, e.g., around 2.2Gb of compressed storage space is needed for XLM-R Large (Conneau et al., 2020). Fully fine-tuning these models for each task separately requires storing a copy of the fine-tuned model for each task. This impedes both iterating and parallelizing training, particularly in storage-restricted environments.

Adapters mitigate this problem. Depending on the model size and the adapter bottleneck size, a single task requires as little as $0.9 \mathrm{Mb}$ storage space. We present the storage requirements in Table 2. This highlights that $>99 \%$ of the parameters required for each target task are fixed during training and can be shared across all models for inference. For instance, for the popular Bert-Base model with a size of $440 \mathrm{Mb}$, storing 2 fully fine-tuned models amounts to the same storage space required by 125 models with adapters, when using a bottleneck size of 48 and adapters of Pfeiffer et al. (2020a). Moreover, when performing inference on a mobile device, adapters can be leveraged to save a significant amount of storage space, while supporting a large

> [!blank-container]
> |  | Base |  | Large |  |
> | :---: | :---: | :---: | :---: | :---: |
> | CRate | \#Params | Size | \#Params | Size |
> | 64 | $0.2 \mathrm{M}$ | $0.9 \mathrm{Mb}$ | $0.8 \mathrm{M}$ | $3.2 \mathrm{Mb}$ |
> | 16 | $0.9 \mathrm{M}$ | $3.5 \mathrm{Mb}$ | $3.1 \mathrm{M}$ | $13 \mathrm{Mb}$ |
> | 2 | $7.1 \mathrm{M}$ | $28 \mathrm{Mb}$ | $25.2 \mathrm{M}$ | 97Mb |
> 
> > **Table 2**: Number of additional parameters and compressed storage space of the adapter of Pfeiffer et al. (2020a) in (Ro)BERT(a)-Base and Large transformer architectures. The adapter of Houlsby et al. (2019) requires roughly twice as much space. CRate refers to the adapter's compression rate: e.g., a. rate of 64 means that the adapter's bottleneck layer is 64 times smaller than the underlying model's hidden layer size.

number of target tasks. Additionally, due to the small size of the adapter modules-which in many cases do not exceed the file size of an image-new tasks can be added on-the-fly. Overall, these factors make adapters a much more computationally-and ecologically (Strubell et al., 2019)—viable option compared to updating entire models (Rücklé et al., 2020). Easy access to fine-tuned models may also improve reproducibility as researchers will be able to easily rerun and evaluate trained models of previous work.

Modularity of Representations. Adapters learn to encode information of a task within designated parameters. Due to the encapsulated placement of adapters, wherein the surrounding parameters are fixed, at each layer an adapter is forced to learn an output representation compatible with the subsequent layer of the transformer model. This setting allows for modularity of components such that adapters can be stacked on top of each other, or replaced dynamically. In a recent example, Pfeiffer et al. (2020b) successfully combine adapters that have been independently trained for specific tasks and languages. This demonstrates that adapters are modular and that output representations of different adapters are compatible. As NLP tasks become more complex and require knowledge that is not directly accessible in a single monolithic pre-trained model (Ruder et al., 2019), adapters will provide NLP researchers and practitioners with many more sources of relevant information that can be easily combined in an efficient and modular way.

Non-Interfering Composition of Information. Sharing information across tasks has a longstanding history in machine learning (Ruder, 2017). Multi-task learning (MTL), which shares a set of parameters between tasks, has arguably received the most attention. However, MTL suffers from problems such as catastrophic forgetting where information learned during earlier stages of training is "overwritten" (de Masson d'Autume et al., 2019), catastrophic interference where the performance of a set of tasks deteriorates when adding new tasks (Hashimoto et al., 2017), and intricate task weighting for tasks with different distributions (Sanh et al., 2019).

The encapsulation of adapters forces them to learn output representations that are compatible across tasks. When training adapters on different downstream tasks, they store the respective information in their designated parameters. Multiple adapters can then be combined, e.g., with attention (Pfeiffer et al., 2020a). Because the respective adapters are trained separately, the necessity of sampling heuristics due to skewed data set sizes no longer arises. By separating knowledge extraction and composition, adapters mitigate the two most common pitfalls of multi-task learning, catastrophic forgetting and catastrophic interference.

Overcoming these problems together with the availability of readily available trained task-specific adapters enables researchers and practitioners to leverage information from specific tasks, domains, or languages that is often more relevant for a specific application-rather than more general pretrained counterparts. Recent work (Howard and Ruder, 2018; Phang et al., 2018; Pruksachatkun et al., 2020; Gururangan et al., 2020) has shown the benefits of such information, which was previously only available by fully fine-tuning a model on the data of interest prior to task-specific fine-tuning.

## 3 AdapterHub

AdapterHub consists of two core components: 
1. A library built on top of HuggingFace transformers, and 
2. a website that dynamically provides analysis and filtering of pre-trained adapters. 

AdapterHub provides tools for the entire life-cycle of adapters, illustrated in Figure 1 and discussed in what follows: 
1. introducing new adapter weights $\Phi$ into pre-trained transformer weights $\Theta$; 
2. training adapter weights $\Phi$ on a downstream task (while keeping $\Theta$ frozen); 
3. automatic extraction of the trained adapter weights $\Phi^{\prime}$ and open sourcing the adapters; 
4. automatic visualization of the adapters with configuration filters; 
5. on-the-fly downloading/caching the pre-trained adapter weights $\Phi^{\prime}$ and stitching the adapter into the pre-trained transformer model $\Theta$;
6. performing inference with the trained adapter transformer model.

> [!blank-container|right-medium]
> ![[figure1.svg]]
> 
> > **Figure 1**: The AdapterHub Process graph. Adapters $\Phi$ are introduced into a pre-trained transformer $\Theta$ (step (1) and are trained (2). They can then be extracted and open-sourced (3) and visualized (4)). Pre-trained adapters are downloaded on-the-fly (5) and stitched into a model that is used for inference (6).
> 

### (1) Adapters in Transformer Layers

We minimize the required changes to existing HuggingFace training scripts, resulting in only two additional lines of code. In Figure 2 we present the required code to add adapter weights (line 3) and freeze all the transformer weights $\Theta$ (line 4). In this example, the model is prepared to train a task adapter on the binary version of the Stanford Sentiment Treebank (SST; Socher et al., 2013) using the adapter architecture of Pfeiffer et al. (2020a). Similarly, language adapters can be added by setting the type parameter to AdapterType. text_language, and other adapter architectures can be chosen accordingly.

While we provide ready-made configuration files for well-known architectures in the current literature, adapters are dynamically configurable, which makes it possible to define a multitude of architectures. We illustrate the configurable components as dashed lines and objects in Figure 3. The configurable components are placements of new weights, residual connections as well as placements of LayerNorm layers (Ba et al., 2016).

The code changes within the HuggingFace transformers framework are realized through MixIns, which are inherited by the respective transformer classes. This minimizes the amount of code changes of our proposed extensions and encapsulates adapters as designated classes. It further increases readability as adapters are clearly separated from the main transformers code base, which makes it easy to keep both repositories in sync as well as to extend AdapterHub.


> [!blank-container]
> ```python
> from transformers import AutoModelForSequenceClassification, AdapterType
> model = AutoModelForSequenceClassification.from_pretrained("roberta-base")
> model.add_adapter("sst-2", AdapterType.text_task, config="pfeiffer")
> model.train_adapter(["sst-2"])
> # Train model ...
> model.save_adapter("adapters/text-task/sst-2/", "sst-2")
> # Push link to zip file to AdapterHub ...
> ```
> > Figure 2: (1) Adding new adapter weights $\Phi$ to pre-trained RoBERTa-Base weights $\Theta$ (line 3), and freezing $\Theta$ (line 4). (3) Extracting and storing the trained adapter weights $\Phi^{\prime}$ (line 6).
> 

> [!blank-container|right-medium]
> ![[figure3.svg]]
> ^522ce5
> > **Figure 3**: Dynamic customization possibilities where dashed lines in (a) show the current configuration options. These options include the placements of new weights $\Phi$ (including down and up projections as well as new LayerNorms), residual connections, bottleneck sizes as well as activation functions. All new weights $\Phi$ are illustrated within the pink boxes, everything outside belongs to the pre-trained weights $\Theta$. In addition, we provide pre-set configuration files for architectures in the literature. The resulting configurations for the architecture proposed by Pfeiffer et al. (2020a) and Houlsby et al. (2019) are illustrated in (b) and (c) respectively. We also provide a configuration file for the architecture proposed by Bapna and Firat (2019), not shown here.

### (2) Training Adapters

Adapters are trained in the same manner as full finetuning of the model. The information is passed through the different layers of the transformer where additionally to the pre-trained weights at every layer the representations are additionally passed through the adapter parameters. However, in contrast to full fine-tuning, the pre-trained weights $\Theta$ are fixed and only the adapter weights $\Phi$ and the prediction head are trained. Because $\Theta$ is fixed, the adapter weights $\Phi$ are encapsuled within the transformer weights, forcing them to learn compatible representations across tasks.

### (3) Extracting and Open-Sourcing Adapters

When training adapters instead of full fine-tuning, it is no longer necessary to store checkpoints of the entire model. Instead, only the adapter weights $\Phi^{\prime}$, as well as the prediction head need to be stored, as the base model's weights $\Theta$ remain the same. This is integrated automatically as soon as adapters are trained, which significantly reduces the required storage space during training and enables storing a large number of checkpoints simultaneously.

When adapter training has completed, the parameter file together with the corresponding adapter configuration file are zipped and uploaded to a public server. The user then enters the metadata (e.g., URL to weights, user info, description of training procedure, data set used, adapter architecture, GitHub handle, Twitter handle) into a designated YAML file and issues a pull request to the AdapterHub GitHub repository. When all automatic checks pass, the [AdapterHub.ml](https://adapterhub.ml) website is automatically regenerated with the newly available adapter, which makes it possible for users to immediately find and use these new weights described by the metadata. We hope that the ease of sharing pre-trained adapters will further facilitate and speed up new developments in transfer learning in NLP.


### (4) Finding Pre-Trained Adapters

The website [AdapterHub.ml](https://adapterhub.ml) provides a dynamic overview of the currently available pre-trained adapters. Due to the large number of tasks in many different languages as well as different transformer models, we provide an intuitively understandable hierarchical structure, as well as search options. This makes it easy for users to find adapters that are suitable for their use-case. Namely, AdapterHub's explore page is structured into three hierarchical levels. At the first level, adapters can be viewed by task or language. The second level allows for a more fine-grained distinction separating adapters into data sets of higher-level NLP tasks following a categorization similar to paperswithcode.com. For languages, the second level distinguishes the adapters by the language they were trained on. The third level separates adapters into individual datasets or domains such as SST for sentiment analysis or Wikipedia for Swahili.

When a specific dataset has been selected, the user can see the available pre-trained adapters for this setting. Adapters depend on the transformer model they were trained on and are otherwise not compatible.[^3] The user selects the model architecture and certain hyper-parameters and is shown the compatible adapters. When selecting one of the adapters, the user is provided with additional information about the adapter, which is available in the metadata (see (3) again for more information).

[^3]: We plan to look into mapping adapters between different models as future work.

### (5) Stitching-In Pre-Trained Adapters

Pre-trained adapters can be stitched into the large transformer model as easily as adding randomly initialized weights; this requires a single line of code, see Figure 4 , line 3 . When selecting an adapter on the website (see (4) again) the user is provided with sample code, which corresponds to the configuration necessary to include the specific weights. [^4]

[^4]: When selecting an adapter based on a name, we allow for string matching as long as there is no ambiguity.

> [!blank-container]
> 
> ```python
> from transformers import AutoModelForSequenceClassification, AdapterType
> model = AutoModelForSequenceClassification.from_pretrained("roberta-base")
> model.load_adapter("sst-2", config="pfeiffer")
> ```
> 
> > **Figure 4**: (5) After the correct adapter has been identified by the user on the explore page of [AdapterHub.ml](https://adapterhub.ml), they can load and stitch the pre-trained adapter weights $\Phi^{\prime}$ into the transformer $\Theta$ (line 3).
> 


### (6) Inference with Adapters

Inference with a pre-trained model that relies on adapters is in line with the standard inference practice based on full fine-tuning. Similar to training adapters, during inference the active adapter name is passed into the model together with the text tokens. At every transformer layer the information is passed through the transformer layers and the corresponding adapter parameters.

The adapters can be used for inference in the designated task they were trained on. To this end, we provide an option to upload the prediction heads together with the adapter weights. In addition, they can be used for further research such as transferring the adapter to a new task, stacking multiple adapters, fusing the information from diverse adapters, or enriching AdapterHub with adapters for other modalities, among many other possible modes of usage and future directions.

## 4 Conclusion and Future Work

We have introduced AdapterHub, a novel easy-to-use framework that enables simple and effective transfer learning via training and community sharing of adapters. Adapters are small neural modules that can be stitched into large pre-trained transformer models to facilitate, simplify, and speed up transfer learning across a range of languages and tasks. AdapterHub is built on top of the commonly used HuggingFace transformers, and it requires only adding as little as two lines of code to existing training scripts. Using adapters in AdapterHub has numerous benefits such as improved reproducibility, much better efficiency compared to full fine-tuning, easy extensibility to new models and new tasks, and easy access to trained models.

With AdapterHub, we hope to provide a suitable and stable framework for the community to train, search, and use adapters. We plan to continuously improve the framework, extend the composition and modularity possibilities, and support other transformer models, even the ones yet to come.
