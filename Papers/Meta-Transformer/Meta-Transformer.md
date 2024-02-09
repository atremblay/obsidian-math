---
title: "Meta-Transformer: A Unified Framework for Multimodal Learning"
aliases:
    - Meta-Transformer
authors:
  - "Yiyuan Zhang"
  - "Kaixiong Gong"
  - "Kaipeng Zhang"
  - "Hongsheng Li"
  - "Yu Qiao"
  - "Wanli Ouyang"
  - "Xiangyu Yue"
published: "2023-07-20"
arXiv: "http://arxiv.org/abs/2307.10802"
paper: "http://arxiv.org/pdf/2307.10802"
---

#### Abstract

Multimodal learning aims to build models that can process and relate information from multiple modalities. Despite years of development in this field, it still remains challenging to design a unified network for processing various modalities (*e.g.* natural language, 2D images, 3D point clouds, audio, video, time series, tabular data) due to the inherent gaps among them. In this work, we propose a framework, named Meta-Transformer, that leverages a *frozen* encoder to perform multimodal perception without any paired multimodal training data. In Meta-Transformer, the raw input data from various modalities are mapped into a shared token space, allowing a subsequent encoder with frozen parameters to extract high-level semantic features of the input data. Composed of three main components: a unified data tokenizer, a modality-shared encoder, and task-specific heads for downstream tasks, Meta-Transformer is the first framework to perform unified learning across 12 modalities with unpaired data. Experiments on different benchmarks reveal that Meta-Transformer can handle a wide range of tasks including fundamental perception (text, image, point cloud, audio, video), practical application (X-Ray, infrared, hyperspectral, and IMU), and data mining (graph, tabular, and time-series). Meta-Transformer indicates a promising future for developing unified multimodal intelligence with transformers. Code will be available at https://github.com/invictus717/MetaTransformer


> [!blank-container]
> ![[images/figure1.svg]]
> > Figure 1: Unified Multimodal Learning. Meta-Transformer utilizes the same backbone to encode natural language, image, point cloud, audio, video, infrared, hyperspectral, X-ray, time-series, tabular, Inertial Measurement Unit (IMU), and graph data. It reveals the potential of transformer architectures for unified multi-modal intelligence.



## 1. Introduction

The human brain, which is considered as the inspiration for neural network models, processes information from various sensory inputs, e.g. visual, auditory, and tactile signals, simultaneously. Moreover, knowledge from one source can benefit the comprehension of another. However, in deep learning, designing a unified network capable of processing a wide range of data formats is a non-trivial task due to the significant modality gap [1-3].

Each data modality presents unique data patterns, which makes it difficult to adapt models trained on one modality to another. For instance, images exhibit a high degree of information redundancy due to densely packed pixels, which is not the case with natural language [4]. Point clouds, on the other hand, have a sparse distribution in 3D space, making them more susceptible to noise and challenging to represent [5]. Audio spectrograms are time-varying and non-stationary data patterns consisting of combinations of waves across frequency domains [6]. Video data contains a sequence of image frames, which gives it the unique capability to capture both spatial information and temporal dynamics [7]. Graph data represents entities as nodes and relationships as edges in a graph, modeling complex, many-to-many relationships between entities [8]. Owing to the substantial differences inherent to various data modalities, it is common practice to utilize distinct network architectures to encode each modality separately. For instance, Point Transformer [9] leverages vector-level position attention to extract structural information from 3D coordinates, but it cannot encode an image, a natural language paragraph, or an audio spectrogram slice. Therefore, designing a unified framework capable of utilizing a modality-shared parameter space to encode multiple data modalities remains a significant challenge. Recently, the development of unified frameworks such as VLMO [2], OFA [10], and BEiT-3 [3] have improved the ability of the network for multimodal understanding, through large-scale multimodal pretraining on paired data [3, 10, 2], but they are more focused on vision and language, and unable to share the whole encoder across modalities

The transformer architecture and attention mechanism, proposed by [[../Attention is all you need|Vaswani et al., 2017]] for natural language processing (NLP), have made a significant difference in deep learning [11-16]. These advancements have been instrumental in enhancing perception across different modalities such as 2D vision (including ViT [17, 18] and Swin Transformer [19]), 3D vision (such as Point Transformer [9] and Point-ViT [20, 21]), and audio signal processing ( AST [6]), etc. These works have demonstrated the versatility of transformer-based architectures, inspiring researchers to explore whether it's possible to develop foundation models capable of unifying multiple modalities, ultimately achieving human-level perception across all modalities.

Table 1: Comparison between Meta-Transformer and related works on perception tasks.

|                     Method                      |                                                                Modalities                                                                | Share Parameters | Unpaired Data |
|:-----------------------------------------------:|:----------------------------------------------------------------------------------------------------------------------------------------:|:----------------:|:-------------:|
|  [[../Attention is all you need\|Transformer]]  |                                                                    📖                                                                    |        ✘         |       ✘       |
|    ViT [13], Swin Transformer [19], MAE [4]     |                                                                    🌅                                                                    |        ✘         |       ✘       |
| Point Transformer [9], PCT [22], Point ViT [21] |                                                                                                                                          |        ✘         |       ✘       |
|               AST [6], SSAST [23]               |                                                                    🎧                                                                    |        ✘         |       ✘       |
|  CLIP [24], Flamingo [25], VLMO [2], OFA [10]   |                                                                   📖🌅                                                                   |        ✘         |       ✘       |
|                   BEiT-3 [3]                    |                                                                   📖🌅                                                                   |  Several Layers  |       ✘       |
|                 ImageBind $[26$                 | 📖🌅🎥🏃 ![](https://cdn.mathpix.com/cropped/2023_09_20_b7242c107c472d01d0dcg-02.jpg?height=36&width=340&top_left_y=2418&top_left_x=965) |        ✘         |       ✘       |
|             Meta-Transformer [ours]             |     ![](https://cdn.mathpix.com/cropped/2023_09_20_b7242c107c472d01d0dcg-02.jpg?height=36&width=340&top_left_y=2447&top_left_x=965)      |  Whole Backbone  |       ✔       |



In this paper, We explore the potential of transformer architecture to process 12 modalities including images, natural language, point cloud, audio spectrogram, video, infrared, hyperspectral, X-Ray, IMU, tabular, graph, and time-series data, as shown in Figure 1. We discuss the learning process with transformers for each modality and address the challenges associated with unifying them into a single framework. Consequently, we propose a novel unified framework named Meta-Transformer for multimodal learning. Meta-Transformer is the first framework to simultaneously encode data from a dozen of modalities using the same set of parameters, allowing a more cohesive approach to multimodal learning (as shown in Table 11). Meta-Transformer incorporates three simple and effective components: a modality-specialist ( $\$ 3.2)$ for data-to-sequence tokenization, a modalityshared encoder $(\$ 3.3)$ for extracting representations across modalities, and task-specific heads for downstream tasks. Specifically, Meta-Transformer first transforms multimodal data into token sequences that share a common manifold space. Then, a modality-shared encoder with frozen parameters extracts representations, which are further adapted to individual tasks by updating the parameters of downstream task heads and lightweight tokenizers only. Finally, task-specific and modality-generic representations can be effectively learned by this simple framework.

We conduct extensive experiments on various benchmarks of 12 modalities. By utilizing images of LAION-2B [24] dataset for pretraining exclusively, Meta-Transformer demonstrates remarkable performance in processing data from multiple modalities, achieving consistently superior outcomes over state-of-the-art methodologies in different multimodal learning tasks. More detailed experimental settings can be found in $\S D$.

In conclusion, our contributions can be summarized as follows:

- For multimodal research, we propose a novel framework, Meta-Transformer, which enables a unified encoder to simultaneously extract representations from multiple modalities with the same set of parameters.
- For multimodal network design, we comprehensively examine the functions of transformer components such as embeddings, tokenization, and encoders in processing various modalities. Meta-Transformer provides valuable insights and sparks a promising new direction in developing a modality-agnostic framework capable of unifying all modalities.
- Experimentally, Meta-Transformer achieves outstanding performance on various datasets regarding 12 modalities, which validates the further potential of Meta-Transformer for unified multimodal learning.


## 2. Related Work

### 2.1. Single-Modality Perception

The development of various neural networks facilitates the perception of machine intelligence [2729, 11].

##### Multi-Layer Perceptron for pattern recognition
At the beginning, support vector machine (SVM) and multi-layer perceptron (MLP) are applied to text [30], image [31], point cloud [32], and audio [33] classification. These innovative works merit the feasibility of introducing AI to pattern recognition.

##### Recurrent \& Convolutional Neural Network
Hopfield Network [34] is the original form of recurrent networks, then LSTM [35] and GRU [36] further explore the advantages of RNNs in sequence modeling and application in NLP tasks [37-39], which is also widely applied in audio synthesis [40]. Meanwhile, the success of CNNs including LeNet [41], AlexNet [42], VGG [43], GoogleNet [44] and ResNet [29] in image recognition greatly promote the application of CNNs in other fields such as text classification [45, 46], point cloud understanding [47,-49], and speech classification [50].

##### Transformer
Recently, [[../Attention is all you need|transformer architecture]] has been adopted in various tasks such as text understanding [51] and generation [52] in NLP, classification [13], detection [53] and segmentation [15] in images, point cloud understanding [22, 9], and audio recognition [6, 23].

However, similar to applications of CNNs and RNNs, these networks are modified according to distinct properties of modalities. There is no common architecture for modality-agnostic learning. More importantly, information from different modalities can be complementary [54-56], it's significant to design a framework that can encode data from different modalities and bridge these complicated representations via a shared parameter space.

### 2.2. Transformed-based Multimodal Perception

The advantages of transformers for perception are the global receptive field and similarity modeling, which prominently facilitate the development of multimodal perception. MCAN [57] proposes the deep modular co-attention networks between vision and language, which performs the cross-modal alignment by concisely maximizing the cross-attention. Then it becomes a consensus [2, 1, 10, 3] to utilize a cross-attention mechanism to bridge different modalities. With the success of pretrainfinetune paradigm, more works are getting focused on how to effectively align representations extracted across modalities by pretraining. VL-BERT [58] pioneers modality-aligned representations for generic vision-language understanding with the MLM paradigm. Then Oscar [59] described the object semantics in both visual and textural contents. Frameworks such as Vinvl [60], Simvlm [1], VLMO [2], ALBEF [61], and Florence [62] further explore the advantages of joint representations across vision-language modalities in terms of semantic consistency.

Multimodal models are also utilized for few-shot learning [25], sequence-to-sequence learning [10], contrastive learning [63]. BEiT-v3 [3] proposes to take images as a foreign language with a more finegrained cross-modal mask-and-reconstruction process, sharing partial parameters. And MoMo [64] further explores the training strategy and objective functions while using the same encoder for images and texts.

Despite these advances, there remain significant obstacles to designing unified multimodal networks due to differences between modalities. Additionally, most research in this area has focused on vision and language tasks, and may not directly contribute to challenges such as 3D point cloud understanding, audio recognition, or other modalities. The Flamingo model [25] represents a powerful few-shot learner, but its transferability to point clouds is limited, and it remains a challenge to leverage prior knowledge from one modality to benefit the others. In other means, existing multimodal methods have limited extensibility on more modalities, although they have taken expensive training costs. Addressing these discrepancies is dependent on bridging different modalities using the same set of parameters, akin to how a bridge connects multiple river banks.

## 3. Meta-Transformer

In this section, we depict the proposed framework, Meta-Transformer, in detail. Meta-Transformer unifies the multiple pipelines of processing data from different modalities and fulfills encoding texts, images, point clouds, audio, and the other 8 modalities with a shared encoder. To achieve this, Meta-Transformer is composed of a data-to-sequence tokenizer to project data to a shared embedding space, a modality-agnostic encoder to encode the embedding of different modalities, and task-specific heads to perform downstream predictions, as shown in Fig. 2 .

### 3.1. Preliminary

Formally, we denote the input space of $n$ modalities as $\left\{\mathcal{X}_{1}, \mathcal{X}_{2}, \cdots, \mathcal{X}_{n}\right\}$, while $\left\{\mathcal{Y}_{1}, \mathcal{Y}_{2}, \cdots, \mathcal{Y}_{n}\right\}$ are the corresponding label spaces. In addition, we assume there exists an effective parameter space $\Theta_{i}$ for each modality, where any parameter $\theta_{i} \in \Theta_{i}$ can be utilized for processing data $\boldsymbol{x}_{i} \in \mathcal{X}_{i}$ from that modality. We say that the essence of Meta-Transformer is to find a shared $\theta^{*}$ that satisfies:

$$
\theta^{*} \in \Theta_{1} \cap \Theta_{2} \cap \Theta_{3} \cap \cdots \Theta_{n},
$$

with the hypothesis:

$$
\Theta_{1} \cap \Theta_{2} \cap \Theta_{3} \cap \cdots \Theta_{n} \neq \varnothing .
$$

The multimodal neural networks can be formulated as a unified mapping function $\mathcal{F}: \boldsymbol{x} \in \mathcal{X} \rightarrow$ $\hat{y} \in \mathcal{Y}$, where $\boldsymbol{x}$ is the input data coming from any modality $\left\{\mathcal{X}_{1}, \mathcal{X}_{2}, \cdots, \mathcal{X}_{n}\right\}$ and $\hat{y}$ denotes the prediction of the network. Let's denote $y$ as the ground truth labels, the multimodal pipeline can be formulated as:

$$
\hat{y}=\mathcal{F}\left(\boldsymbol{x} ; \theta^{*}\right), \quad \theta^{*}=\underset{x \in \mathcal{X}}{\arg \min }[\mathcal{L}(\hat{y}, y)] .
$$

> [!blank-container]
> ![[images/figure2.svg]]
> > Figure 2: Meta-Transformer consists of data-to-sequence tokenization, unified feature encoding, and down-stream task learning. The framework is illustrated with text, image, point cloud, and audio.

### 3.2. Data-to-Sequence Tokenization

> [!blank-container]
> ![[images/figure3.svg]]
> > Figure 3: Illustration of Data-to-Sequence Tokenization 3.2. We propose the meta scheme in (a) containing grouping, convolution, and transformation progress. Then (b)-(e) represents the building blocks applied with our meta scheme on texts, images, point clouds, and audio spectrograms.


We propose a novel meta-tokenization scheme designed to transform data across various modalities into token embeddings, all within a shared manifold space. This approach is then applied to tokenization, taking into account the practical characteristics of modality, as illustrated in Figure 3 We take text, images, point clouds, and audio as examples. More details can be found in supplementary materials. In specific, we use $\boldsymbol{x}_{T}, \boldsymbol{x}_{I}, \boldsymbol{x}_{P}$, and $\boldsymbol{x}_{A}$ to denote a data sample of text, image, point cloud, and audio spectrogram.

##### Natural Language
Following the common practice [51, 65], we use WordPiece embeddings [66] with a 30,000 token vocabulary. WordPiece segments original words into subwords. For example, the original sentence: "The supermarket is hosting a sale", could be converted by WordPiece to: “_The _super market_is_host ing_a _sale".

In this case, the word "supermarket" is divided into two subwords "\_super" and "market" and the word "hosting" is divided into "\_host" and "ing", while the rest words are unchanged and still single units. The front of the first character of each original word will be stacked with a special character “\_”, indicating the beginning of a natural word. Each subword is corresponding to a unique token in a vocabulary, then is projected to a high-dimensional feature space with word embedding layers. As a result, each input text is transformed to a set of token embeddings $\boldsymbol{x} \in \mathbb{R}^{n \times D}$, where $n$ is the number of tokens and $D$ is the dimension of embedding.

##### Images
To accommodate 2D images, we reshape the image $\boldsymbol{x} \in \mathbb{R}^{H \times W \times C}$ into a sequence of flattened 2D patches $\boldsymbol{x}_{p} \in \mathbb{R}^{N_{s} \times\left(S^{2} \cdot C\right)}$, where $(H, W)$ represents the original image resolution, $C$ denotes the number of channels; $S$ is the patch size, and $N_{s}=\left(H W / S^{2}\right)$ is the resulting number of patches. After that, a projection layer is utilized to project the embedding dimension to $D$ :

$$
\boldsymbol{x}_{I} \in \mathbb{R}^{C \times H \times W} \rightarrow \boldsymbol{x}_{I}^{\prime} \in \mathbb{R}^{N_{s} \times\left(S^{2} \cdot C\right)} \rightarrow \boldsymbol{x}_{I}^{\prime \prime} \in \mathbb{R}^{N_{s} \times D} .
$$

Note that we use the same operation for infrared images but the linear projection for hyperspectral images. In addition, we simply replace 2D convolution layers with 3D convolution for video recognition. More details can be found in B.1 and B.3

##### Point Cloud
To learn 3D patterns with transformers, we convert point clouds from raw input space to the token embedding space. $\mathcal{X}=\left\{\boldsymbol{x}_{i}\right\}_{i=1}^{P}$ denotes a point cloud of $P$ points, where $\boldsymbol{x}_{i}=\left(\boldsymbol{p}_{i}, \boldsymbol{f}_{i}\right)$, $\boldsymbol{p}_{i} \in \mathbb{R}^{3}$ represents the $3 \mathrm{D}$ coordinates, and $\boldsymbol{f}_{i} \in \mathbb{R}^{c}$ is feature of the $i$-th point. Generally, $\boldsymbol{f}_{i}$ contains visual hints such as color, viewpoint, normal, etc. We employ the Farthest Point Sampling (FPS) operation to sample a representative skeleton of original point clouds with a fixed sampling ratio (1/4). Then we employ $K$-Nearest Neighbor (KNN) to group neighboring points. Based on grouped sets containing local geometric prior, we construct the adjacency matrix with center points of grouped subsets to further undercover the comprehensive structural information of 3D objects and 3D scenes.

Finally, we aggregate the structural representations from $K$ subsets. We obtain point embeddings as:

$$
\boldsymbol{x}_{P} \in \mathbb{R}^{P \times(3+c)} \rightarrow \boldsymbol{x}_{P}^{\prime} \in \mathbb{R}^{\frac{P}{4} \times \frac{D}{2}} \rightarrow \boldsymbol{x}_{P}^{\prime \prime} \in \mathbb{R}^{\frac{P}{16} \times D} .
$$

##### Audio Spectrogram
Initially, we pre-process the audio waveform with the duration of $t$ seconds with $\log$ Mel filterbank [67]. Then we employ the Hamming window with a stride of $t_{s}$ on the frequency of $f_{s}$ to split the original wave into $l=\left(t / t_{s}\right)$ intervals and further transform the original wave into $l$-dimensional filterbank.

Subsequently, we split the spectrogram into patches from time and frequency dimensions with the same patch size of $S$. Different from image patches, audio patches overlap on spectrograms. Following AST [6], we also choose to split whole spectrograms into $N_{s}=12[(100 t-16) / 10]$ patches by $S \times S$ convolution, then we flatten patches into token sequences. Finally, we summarize the process:

$$
\boldsymbol{x}_{A} \in \mathbb{R}^{T \times F} \rightarrow \boldsymbol{x}_{A}^{\prime} \in \mathbb{R}^{N_{s} \times S \times S} \rightarrow \boldsymbol{x}_{A}^{\prime \prime} \in \mathbb{R}^{\left(N_{s} \cdot D / S^{2}\right) \times D},
$$

where $T$ and $F$ denote time and frequency dimensions.

### 3.3. Unified Encoder

After transforming the raw inputs to token embedding space, we leverage a unified transformer encoder with frozen parameters to encode the sequences of token embeddings from different modalities.

##### Pretraining
We utilize ViT [13] as the backbone network and pre-train it on the LAION-2B dataset with contrastive learning, which reinforces the ability for generic token encoding. After pretraining, we freeze the parameters of the backbone network. In addition, for text understanding, we utilize the pretrained text tokenizer of CLIP [24] to segment sentences into subwords and transform subwords into word embeddings.

##### Modality-Agnostic Learning
Following common practice [51, 13], we prepend a learnable token $x_{\mathrm{CLS}}$ to the sequence of token embeddings, and the final hidden state of $x_{\mathrm{CLS}}$ token $\left(\boldsymbol{z}_{L}^{0}\right)$ serves as the summary representation of the input sequence, which is usually utilized for performing recognition.

To reinforce positional information, we incorporate position embeddings into the token embeddings. Recall that we tokenize the input data to 1D embeddings, thus, we opt for standard learnable 1D position embeddings. In addition, we do not observe substantial performance improvements using more sophisticated 2D-aware position embeddings on image recognition. We simply fuse the position embeddings and the content embeddings with an element-wise addition operation, and the resulting embedding sequences are then fed into the encoder.

The transformer encoder with a depth of $L$ compromises multiple stacked multi-head self-attention (MSA) layers and MLP blocks. The input token embeddings are fed into an MSA layer first, then an MLP block. Then the output of $(\ell-1)$-th MLP block serves as the input of $\ell$-th MSA layer. Layer Normalization (LN) is appended before each layer and the residual connection is applied after each layer. The MLP contains two linear FC layers along with a GELU non-linear activation. The formulation of the transformer is:

$$
\begin{aligned}
\boldsymbol{z}_{0} & =\left[\boldsymbol{x}_{\mathrm{CLS}} ; \boldsymbol{E}_{\boldsymbol{x}_{1}} ; \boldsymbol{E}_{\boldsymbol{x}_{2}} ; \cdots ; \boldsymbol{E}_{\boldsymbol{x}_{n}}\right]+\boldsymbol{E}_{\text {pos }}, & & \boldsymbol{E} \in \mathbb{R}^{n \times D}, \boldsymbol{E}_{\text {pos }} \in \mathbb{R}^{(n+1) \times D} \\
\boldsymbol{z}_{\ell}^{\prime} & =\operatorname{MSA}\left(\operatorname{LN}\left(\boldsymbol{z}_{\ell-1}\right)\right)+\boldsymbol{z}_{\ell-1}, & & \ell=1 \ldots L \\
\boldsymbol{z}_{\ell} & =\operatorname{MLP}\left(\operatorname{LN}\left(\boldsymbol{z}_{\ell}^{\prime}\right)\right)+\boldsymbol{z}_{\ell}^{\prime}, & & \ell=1 \ldots L \\
\boldsymbol{y} & =\operatorname{LN}\left(\boldsymbol{z}_{L}^{0}\right) & &
\end{aligned}
$$

where $\boldsymbol{E}_{x}$ denotes the token embeddings from proposed tokenizer and $n$ denotes the number of tokens. We augment patch embeddings and learnable embedding with position embeddings $\boldsymbol{E}_{\text {pos }}$.

### 3.4. Task-Specific Heads

After obtaining learning representations, we feed representations to the task-specific heads $h\left(\cdot ; \theta_{h}\right)$, which consists mainly of MLPs and varies from modalities and tasks. The learning objective of Meta-Transformer can be summarized as:

$$
\hat{\boldsymbol{y}}=\mathcal{F}\left(\boldsymbol{x} ; \theta^{*}\right)=h \circ g \circ f(\boldsymbol{x}), \quad \theta^{*}=\underset{\theta}{\arg \min } \mathcal{L}(\hat{y}, y),
$$

where $f(\cdot), g(\cdot)$, and $h(\cdot)$ denote the function of tokenizer, backbone, and heads, respectively.

## 4. Experiments

In this section, we perform experiments on each of the 12 modalities. We demonstrate the potential of Meta-Transformer for multimodal perception. A summary of our experimental design is shown in Table 2 and more experimental details can be found in $\$$ C.1

### 4.1. Experimental Setups

##### Text understanding 
For text understanding evaluation, we employ the General Language Understanding Evaluation (GLUE) benchmark [68] which incorporates several different datasets, covering a wide range of natural language understanding tasks.

##### Image understanding
1. Classification: we conduct experiments on ImageNet-1K [69] which contains approximately 1.3 million images with 1000 categories. Following common practices [70, 19, 71], base-scale models are trained for 300 epochs, while large models are pre-trained on ImageNet$22 \mathrm{~K}$ (14.2 million images) for 90 epochs and fine-tuned on ImageNet-1K for another 20 epochs. 
2. Object Detection: we conduct experiments on the MS COCO dataset [72] using Mask R-CNN [73] as the detector and training each model for 12 epochs. 
3. Semantic Segmentation: we train the segmentation head UperNet [74] on ADE20K [75] for 160k iterations, providing a fair comparison with previous $\mathrm{CNN}$-based and transformer-based backbones.

##### Infrared, X-Ray, and Hyperspectral data understanding
We conduct experiments on infrared image, X-Ray scan, and hyperspectral data recognition with RegDB [76], Chest X-Ray [77], and Indian Pine[^4] datasets, respectively.

[^4]: https://github.com/danfenghong/IEEE_TGRS_SpectralFormer/blob/main/data/IndianPine.mat 

##### Point cloud understanding
1. Classification: to assess the performance of Meta-Transformer in 3D object classification, we use the ModelNet-40 [78] benchmark, consisting of CAD models across 40 classes, with 9,843 training samples and 2,468 validation samples. 
2. Semantic segmentation: to evaluate performance in 3D point cloud segmentation, we assess the model on both S3DIS [79] and ShapeNetPart [80] datasets. The S3DIS dataset encompasses 6 large indoor areas and 13 semantic classes, comprising 271 rooms. The ShapeNetPart dataset includes 16,880 object models across 16 shape categories.

##### Audio recognition
For audio recognition, we utilize the Speech Commands V2 [81] dataset, which consists of 105,829 one-second recordings of 35 common speech commands.

##### Video recognition
For video understanding, we conduct experiments on the UCF101 [82] dataset for action recognition, with more details presented in $\S$ B.1

##### Time-series forecasting
For time-series forecasting, we conduct experiments on ETTh1 [83], Traffic[^5], Weather[^6] and Exchange [84] datasets. We use the tokenizer of Autoformer [85].

[^5]: https://pems.dot.ca.gov/ 
[^6]: https://www.bgc-jena.mpg.de/wetter/

##### Graph understanding 
We conduct experiments on the PCQM4M-LSC dataset [86], which is a large-scale dataset consisting of 4.4 million organic molecules with up to 23 heavy atoms with their corresponding quantum-mechanical properties. With the target of predicting molecular properties using machine learning, it has plenty of applications in drug discovery, and material science.

##### Tabular analysis
We conduct experiments on adult and bank marketing from UCI repository[^7] We use the tokenizer of TabTransformer [87] to encode raw tabular data.

[^7]:http://archive.ics.uci.edu/ml/ 

##### IMU recognition
To evaluate the ability of Meta-Transformer to understand the inertial motion systems, we conduct experiments of IMU sensor classification on the Ego4D [88] dataset.


|   Modalities   |        Tasks         |        Datasets        | Data Scale |
|:--------------:|:--------------------:|:----------------------:|:----------:|
|      Text      |    Classification    |     GLUE Benchmark     |    330K    |
|     Image      |    Classification    |      ImageNet-1K       |    1.3M    |
|                |      Detection       |        MS COCO         |    118K    |
|                |     Segmentation     |        ADE-20K         |    20K     |
|  Point Cloud   | Shape Classification |      ModelNet-40       |     9K     |
|                |  Scene Segmentation  |         S3DIS          | 400M Point |
|                | Obiect Segmentation  |      ShapeNetPart      |    16K     |
|     Audio      |    Classification    |   Speech commands v2   |    105K    |
|     Video      |  Action Recognition  |         UCF101         |    14K     |
|    Infrared    |    Classification    |         RegDB          |    40K     |
| Hyper-spectrum |    Classification    |      Indian Pine       |    10K     |
|     X-Ray      |    Classification    |      Chest X-Ray       |    112K    |
|      IMU       |    Classification    |         Ego4D          |    193K    |
|  Tabular data  |      Prediction      |      Adult & Bank      |  32K-45K   |
|   Graph data   |      Prediction      |       PCQM4M-LSC       |    47M     |
|  Time-series   |     Forecasting      | Exchange, Traffic, etc |   5-36K    |
> Table 2: Summary of experimental settings across different modalities. We report the task, dataset, and data scale for each modality.

##### Settings of Networks
We follow the default settings of ViT [13]. Meta-Transformer-B16<sub>F</sub> denotes Meta-Transformer with a base-scale encoder which contains 12 transformer blocks and 12 attention heads, and the image patch size is 16 . For the base-scale encoder, the embedding dimension is 768 and the output dimension of MLP is 3,072. `F` and `T` denotes that parameters of the encoder are *Frozen* and further *Tuned*, respectively.

Table 3: Experimental results for text understanding on the GLUE benchmark. We compare existing advanced methods from paraphrasing, sentiment, duplication, inference, and answering tasks, and we report the pre-training settings and performances.

![](https://cdn.mathpix.com/cropped/2023_09_20_b7242c107c472d01d0dcg-08.jpg?height=293&width=1391&top_left_y=2030&top_left_x=367)

Table 4: Experimental results for image understanding. We conduct experiments in classification, object detection, and instance segmentation tasks on the ImageNet [69], MSCOCO [72], and ADE20K [75] datasets, where Bold and underline indicate best and second best results.

![](https://cdn.mathpix.com/cropped/2023_09_20_b7242c107c472d01d0dcg-09.jpg?height=610&width=1387&top_left_y=378&top_left_x=369)


|              Method              | R@1 (\%) |   mAP%    |  Param   |
|:--------------------------------|:--------:|:---------:|:--------:|
|     AGW<sub>[TPAMI'21]</sub>     |  70.49   |   65.90   |   25M    |
|     SMCL<sub>[ICCV'21]</sub>     |  83.05   | **78.57** |   40M    |
|   MSCLNet<sub>[ECCV'22]</sub>    |  83.86   |   78.31   |   50M    |
| Meta-Transformer-B16<sub>F</sub> |  73.50   |   65.19   | **1.8M** |

(a) Infrared data understanding

| Method                                     | OA (\%) | AA (\%) |  Params   |
|:------------------------------------------ |:-------:|:-------:|:---------:|
| ViT<sub>[ICLR2 21]</sub>                   |  71.86  |  78.97  |   85.2M   |
| SpectralFormer<sub>[TGRS'21]</sub> (Pixel) |  78.55  |  84.68  |   85.2M   |
| SpectralFormer<sub>[TGRS'21]</sub> (Patch) |  81.76  |  87.81  |   85.2M   |
| Meta-Transformer-B16<sub>F</sub>           |  67.62  |  78.09  | **0.17M** |

(b) Hyperspectral data understanding
> Table 5: Experimental results for infrared and hyperspectral data understanding. We conduct experiments on classification tasks over the SYSU-MM01 and Indian Pine datasets. We report Rank-1 (R@1), mean Average Precision (mAP), Overall Accuracy (OA), Average Accuracy (AA), and the number of trainable parameters (Params).




### 4.2. Results on Natural Language Understanding

Table 3 illustrates the experimental results on the GLUE benchmark for text understanding tasks, comparing various state-of-the-art methods such as BERT [51], RoBERTa [65], and ChatGPT. The comparison centers on paraphrasing, sentiment, duplication, inference, and answering tasks. When using frozen parameters pretrained on images, Meta-Transformer-B16 $6_{\mathrm{F}}$ achieves scores of $54.6 \%$ in sentiment (SST-2), 81.1\% in paraphrase (MRPC), 66.0\% in duplication (QQP), 63.4\% in inference (MNLI), and 56.3\% in answering (QNLI) tasks. After finetuning, Meta-Transformer-B16T exhibits improved performance, with $81.3 \%$ in sentiment, $81.8 \%$ in paraphrase, $78.0 \%$ in duplication, $70.0 \%$ in inference, and $60.3 \%$ in answering tasks. Although the Meta-Transformer's performance on the GLUE benchmark might not be as impressive as that of BERT, RoBERTa, or ChatGPT, it still demonstrates competitive performance, adaptability, and potential for understanding natural language.

### 4.3. Results on Image Understanding

As shown in Table 4. Meta-Transformer exhibits outstanding performance when compared with Swin Transformer series [19, 107] and InternImage [96] on image understanding tasks. On image classification, with the help of CLIP [24] text encoder, Meta-Transformer delivers great performances under zero-shot classification with the Meta-Transformer-B16 $6_{\mathrm{F}}$ and Meta-Transformer-L14 $\mathrm{F}_{\mathrm{F}}$, achieving $69.3 \%$ and $75.3 \%$, respectively. At the same time, when the pretrained parameters are further tuned, Meta-Transformer can outperform existing advanced methods, with Meta-Transformer-B $16_{\mathrm{T}}$ and Meta-Transformer-L14 $4_{\mathrm{T}}$ achieving $85.4 \%$ and $88.1 \%$ accuracy, respectively. The latter outperforms both SwinV2-L/24 ${ }^{\ddagger}$ [107] $(87.6 \%)$ and InternImage-XL [96] ${ }^{\ddagger}(88.0 \%)$ on ImageNet [69] classification. Table 6: Experimental results for point cloud understanding. We conduct experiments on the ModelNet-40 [78], S3DIS [79], and ShapeNetPart [80] datasets. We compare existing advanced methods from classification, semantic, and object part segmentation tasks, and we report the pretraining modality (Pre-train) and trainable parameters number (Params) of each method.

|                                                             Method                                                             |   Pre-train    | ModelNet-40 |                   |                   |    S3DIS Area-5     |           |                   |      ShapeNetPart       |                         |                   |
|:------------------------------------------------------------------------------------------------------------------------------:|:--------------:|:-----------:|:-----------------:|:-----------------:|:-------------------:|:---------:|:-----------------:|:-----------------------:|:-----------------------:|:-----------------:|
|                                                                                                                                |                |  mAcc (\%)  | $\mathrm{OA}(\%)$ |      Params       | $\mathrm{mIoU}(\%)$ | mAcc (\%) |      Params       | $\mathrm{mloU}_{I}(\%)$ | $\mathrm{mIoU}_{C}(\%)$ |       Param       |
|                                                   PointNet [CVPR' 17$]$ 32]                                                    |      N/A       |    86.0     |       89.2        | $3.5 \mathrm{M}$  |        41.1         |   49.0    | $3.6 \mathrm{M}$  |          83.7           |          80.4           | $3.6 \mathrm{M}$  |
|                                                 PointNet++ [NeurIPS' 17] 5$]$                                                  |      N/A       |      -      |       91.9        | $1.5 \mathrm{M}$  |        53.5         |     -     | $1.0 \mathrm{M}$  |          85.1           |          81.9           |        1.0        |
|                                                  PointCNN [NeurIPS' 18] 47$]$                                                  |      N/A       |    88.1     |       92.5        | $0.6 \mathrm{M}$  |        57.3         |     -     | $0.6 \mathrm{M}$  |                         |                         |                   |
|                                            KPConv $\left.{ }_{[I C C V} 19\right]$                                             |      N/A       |      -      |       92.9        | $14.3 \mathrm{M}$ |        67.1         |   72.8    | $15.0 \mathrm{M}$ |          86.4           |          85.1           |         -         |
|                                 $\mathrm{DGCNN}_{\left[\mathrm{TOG}^{\prime} 19\right]}[101]$                                  |      N/A       |    90.2     |       92.9        | $1.8 \mathrm{M}$  |        52.5         |     -     | $1.3 \mathrm{M}$  |          85.2           |          82.3           |        1.3        |
|                            Point Transformer ${ }_{\left[C^{\prime} \mathrm{C}^{2} 21\right]}^{9]}$                            |      N/A       |    90.6     |       93.7        | $7.8 \mathrm{M}$  |        70.4         |     -     | $7.8 \mathrm{M}$  |          86.6           |          83.7           |        7.8        |
|                                        PointNeXt [NeurlPS' $\left.{ }_{22}\right]$ 102]                                        |      N/A       |    90.8     |       93.2        | $1.4 \mathrm{M}$  |        67.3         |   73.9    | $3.8 \mathrm{M}$  |          86.7           |          84.4           |        1.0        |
|                          Point-MLP $\left._{\left[{ }_{[I C L R}{ }^{\prime 2} 2\right]} 103\right]$                           |      N/A       |    90.9     |       93.6        | $0.68 \mathrm{M}$ |          -          |     -     |         -         |          86.1           |          84.6           |         -         |
|                                    PointMixer $\left[\mathrm{ECCV}^{\prime 22]} 104\right.$                                    |      N/A       |    91.4     |       93.6        | $3.6 \mathrm{M}$  |        71.4         |   77.4    | $6.5 \mathrm{M}$  |            -            |            -            |         -         |
|                                                    Point-BERT [CVPR'22] 20                                                     | $3 \mathrm{D}$ |      -      |       93.2        | $21.1 \mathrm{M}$ |        60.8         |   69.9    | $21.1 \mathrm{M}$ |          85.6           |          84.1           | $21.1 \mathrm{M}$ |
|                              Point-MAE $\left.{ }_{\left[\mathrm{ECCV}_{22} 2\right]} 105\right]$                              | $3 \mathrm{D}$ |      -      |       93.8        | $21.1 \mathrm{M}$ |          -          |     -     |         -         |          86.1           |          84.2           | $21.1 \mathrm{M}$ |
|                             $\mathrm{P}^{2} \mathrm{P}_{\left[\text {Neur IPS' }^{2} 2\right]} 56$                             | $2 \mathrm{D}$ |      -      |       93.1        | $1.2 \mathrm{M}$  |          -          |     -     |         -         |          86.5           |          84.1           |         -         |
| ![](https://cdn.mathpix.com/cropped/2023_09_20_b7242c107c472d01d0dcg-10.jpg?height=29&width=260&top_left_y=756&top_left_x=387) | $2 \mathrm{D}$ |      -      |       93.5        | $21.1 \mathrm{M}$ |        61.2         |   71.1    | $21.1 \mathrm{M}$ |          86.1           |          84.7           | $21.2 \mathrm{M}$ |
|                                     Meta-Transformer-B $16_{\mathrm{F} \text { [ours] }}$                                      | $2 \mathrm{D}$ |    90.5     |       93.6        | $0.6 \mathrm{M}$  |        72.3         |   83.5    | $2.3 \mathrm{M}$  |           870           |          85.2           | $2.3 \mathrm{M}$  |

When it comes to object detection and semantic segmentation, Meta-Transformer also delivers excellent performances, which further proves its generic ability on image understanding. On object detection, Meta-Transformer-B $16_{\mathrm{F}}$ and Meta-Transformer-L14 $4_{\mathrm{F}}$ achieve APs of $31.7 \%$ and $43.5 \%$, while Meta-Transformer-B $16_{\mathrm{T}}$ and Meta-Transformer-L14 ${ }_{\mathrm{T}}$ reach $46.4 \%$ and $56.3 \% \mathrm{AP}$, respectively. In semantic segmentation, the mIoUs for Meta-Transformer-B16 $\mathrm{F}_{\mathrm{F}}$ and Meta-Transformer-L14 $\mathrm{F}_{\mathrm{F}}$ are $33.4 \%$ and $41.2 \%$, while Meta-Transformer-B16 $6_{\mathrm{T}}$ and Meta-Transformer-L14 $4_{\mathrm{T}}$ achieve $51.0 \%$ and $55.0 \%$, respectively. In comparison, SwinV2-L/24 $\$$ outperforms the Meta-Transformer in both object detection $\left(58.8 \%\right.$ AP) and semantic segmentation $\left(55.9 \%\right.$ mIoU). The Meta-Transformer-L $14_{\mathrm{T}}$ model has a similar performance to InternImage-XL ${ }^{\ddagger}$ [96] in semantic segmentation (both achieving $55.0 \% \mathrm{mIoU}$ ), but outperforms it in object detection (56.3\% AP compared to 55.3\% AP). These results highlight that Meta-Transformer demonstrates a competitive performance in various image understanding tasks even compared to Swin Transformer [19] and InternImage.

### 4.4. Results on Infrared, Hyperspectral, and X-Ray data

Table 5a presents the performance comparison of Meta-Transformer and other advanced methods on the RegDB dataset [76] for infrared image recognition. Meta-Transformer-B16 $6_{\mathrm{F}}$ demonstrates competitive results with a Rank-1 accuracy of $73.50 \%$ and an mAP of $65.19 \%$. While it may not outperform the top-performing methods, Meta-Transformer proves to be a simple transferable approach for infrared image recognition tasks. These results indicate the potential of Meta-Transformer in handling the challenges associated with infrared images and contribute to advancements in this field.

Table 7: X-ray image recognition with MetaTransformer. We conduct experiments on the Chest $\mathrm{X}$-Ray dataset, we report the Accuracy (\%) and the number of trainable parameters.

| Method               | Accuracy (\%) |        Params        |
|:-------------------- |:-------------:|:--------------------:|
| ViT [13]             |     96.3      |  $86.9 \mathrm{M}$   |
| SEViT [108]          |     94.6      |  $85.8 \mathrm{M}$   |
| Meta-Transformer-B16 |     94.1      | $\mathbf{0 . 7 5 M}$ |

In addition, Table $5 \mathrm{~b}$ presents the performance of Meta-Transformer on the Indian Pine dataset for hyperspectral image recognition. SpectralFormer [100] achieves impressive accuracy scores, with a patch-wise approach. Plain vision transformer also performs well in comparison when fully tuning all parameters. Meta-Transformer-B16 demonstrates competitive results on hyperspectral image recognition with lower overall accuracy. However, Meta-Transformer stands out for its significantly fewer trainable parameters (only $0.17 \mathrm{M}$ ) compared to other methods. This reveals a promising development direction of applying the Meta-Transformer to remote sensing, environmental monitoring, and mineral exploration. For X-Ray images, similar to dealing with infrared images, we take the same image tokenizer as common visible images. From Table 7, we can observe that Meta-Transformer can achieve a competitive performance of $94.1 \%$ accuracy.

### 4.5. Results on 3D Point Cloud Understanding

Table 6 showcases the experimental results for point cloud understanding, comparing the performance of Meta-Transformer with other state-of-the-art methods on the ModelNet-40 [78], S3DIS [79], and ShapeNetPart [80] datasets. The tasks include classification, semantic segmentation, and object part segmentation. When pretrained on $2 \mathrm{D}$ data, Meta-Transformer-B $16_{\mathrm{F}}$ demonstrates competitive performance, achieving an overall accuracy $(\mathrm{OA})$ of $93.6 \%$ on ModelNet- 40 with only $0.6 \mathrm{M}$ trainable parameters, which is comparable to the best-performing models. On the S3DIS Area-5 dataset, Meta-Transformer outperforms other methods with a mean IoU (mIoU) of $72.3 \%$ and a mean accuracy (mAcc) of $83.5 \%$, using $2.3 \mathrm{M}$ parameters. Moreover, Meta-Transformer excels in the ShapeNetPart dataset, achieving the highest scores on both instances $\mathrm{mIoU}\left(\mathrm{mIoU}_{I}\right)$ and category $\mathrm{mIoU}\left(\mathrm{mIoU}_{C}\right)$ with $87.0 \%$ and $85.2 \%$, respectively, using $2.3 \mathrm{M}$ parameters. In summary, Meta-Transformer demonstrates remarkable advantages in point cloud understanding tasks, offering competitive performance with fewer trainable parameters compared to other state-of-the-art methods.

### 4.6. Results on Audio Recognition

In order to fairly compare Meta-Transformer with existing audio transformer series [6, 23] of similar scale, we conduct experiments on audio recognition using Meta-Transformer-B32. Table 8: Audio understanding with Meta-Transformer. We conduct experiments on the Speech Commands V2 dataset and report the accuracy score and the number of trainable and all parameters.

| Method                                                                                   |     Pre-train     | Acc $(\%)$         | A-Params          |       Params       |
|:---------------------------------------------------------------------------------------- |:-----------------:|:------------------ |:----------------- |:------------------:|
| AST 6] (Supervised)                                                                      |        N/A        | 92.6               | $86.9 \mathrm{M}$ | $86.9 \mathrm{M}$  |
| AST 6] (Supervised)                                                                      |   AudioSet-20K    | 96.2               | $86.9 \mathrm{M}$ | $86.9 \mathrm{M}$  |
| AST 6] (Supervised)                                                                      |    ImageNet+KD    | $\mathbf{9 8 . 1}$ | $86.9 \mathrm{M}$ | $86.9 \mathrm{M}$  |
| SSAST [23] (Self-Supervised)                                                             |    AudioSet-2M    | 97.8               | $89.3 \mathrm{M}$ | $89.3 \mathrm{M}$  |
| SSAST [23] (Self-Supervised)                                                             |    Librispeech    | 97.8               | $89.3 \mathrm{M}$ | $89.3 \mathrm{M}$  |
| SSAST [23] (Self-Supervised)                                                             | Joint Pretraining | 98.0               | $89.3 \mathrm{M}$ | $89.3 \mathrm{M}$  |
| Meta-Transformer-B32 F $\left[\right.$ [ours] $_{\text {Meta-Transformer-B32T [ours] }}$ |        2D         | 78.3               | $86.6 \mathrm{M}$ | $\mathbf{1 . 1 M}$ |
| 2D                                                                                       |       97.0        | $86.6 \mathrm{M}$  | $86.3 \mathrm{M}$ |                    |

Table 8 showcases the performance of Meta-Transformer in the audio domain. These models are compared to existing methods such as AST [6] and SSAST [23] in terms of accuracy, all parameters (A-Params), and trainable parameters (T-Params). With frozen parameters, Meta-Transformer-B32F achieves an accuracy of $78.3 \%$ while requiring only $1.1 \mathrm{M}$ parameters for tuning. On the other hand, the Meta-Transformer-B32T model exhibits a significantly higher accuracy of $97.0 \%$ when tuning the parameters, whereas the AST model only reaches an accuracy of $92.6 \%$. When AST is pre-trained on ImageNet and supplemented with additional Knowledge Distillation (KD), it achieves an improved performance of $98.1 \%$, but with a higher number of trainable parameters of $86.9 \mathrm{M}$. SSAST models display accuracy scores ranging from $97.8 \%$ to $98.0 \%$ while requiring $89.3 \mathrm{M}$ parameters. These results highlight that the Meta-Transformer performs competitively in the audio domain, demonstrating its versatility and effectiveness across different fields.

### 4.7. Results on Video Recognition

Table 9: Video understanding with MetaTransformer. We conduct experiments on the UCF101 [82] dataset and report the accuracy score and the number of trainable parameters, where "V" denotes video clips only.

| Method                  |   Modality   |       UCF101       |       Params       |
|:----------------------- |:------------:|:------------------:|:------------------:|
| OPN [109]               |      V       |        59.6        |         -          |
| SimCLR [110]            |      V       |        88.9        | $86.9 \mathrm{M}$  |
| VideoMAE V1 [111]       |      V       |        96.1        | $86.9 \mathrm{M}$  |
| VideoMAE V2 [112]       |      V       | $\mathbf{9 9 . 6}$ | $86.9 \mathrm{M}$  |
| ViT [13] (from scratch) | $\mathrm{V}$ |        51.4        | $86.9 \mathrm{M}$  |
| Meta-Transformer-B16    | $\mathrm{V}$ |        46.6        | $\mathbf{1 . 1 M}$ |

Table 9 presents the performance comparison of the Meta-Transformer and existing advanced methods on the UCF101 dataset for video understanding. Several state-of-theart video-tailored methods achieve accuracies of over $90 \%$. Meta-Transformer only contains a negligible amount of trainable parameters of 1.1 million to obtain an accuracy of $46.6 \%$ while other methods have to train around 86.9 million parameters. Though Meta-Transformer is not able to beat other state-of-the-art video understanding models, Meta-Transformer stands out for its significantly reduced trainable parameter count, suggesting the potential benefit of unified multi-modal learning and less architectural complexity.

### 4.8. Results on Time-series Forecasting

To explore the ability of Meta-Transformer for time-series forecasting, we conduct experiments on several widely-adopted benchmarks for Long-term forecasting tasks including ETTh1 [83], Traffic, Weather, and Exchange [84], with results shown in Table 10. Table 10: Time-series Forecasting with Meta-Transformer. Following TimesNet, we report the number of trainable parameters and average performances from 4 different prediction lengths, which is $\{96,192,336,720\}$.

![](https://cdn.mathpix.com/cropped/2023_09_20_b7242c107c472d01d0dcg-12.jpg?height=285&width=1389&top_left_y=397&top_left_x=368)

From Table 10, we can have the following observations. 1) With most of the model parameters being fixed, Meta-Transformer can still outperform existing methods including Pyraformer [117], Informer [83], LogTrans [118], and Reformer [119] on these datasets. 2) The number of trainable parameters of Meta-Transformer is very few. With only $19 \mathrm{~K}$ trainable parameters, Meta-Transformer can still outperform Informer [83]. When 2M parameters are trained, Meta-Transformer can directly outperform Pyraformer [117]. Therefore, Meta-Transformers pretrained on perception tasks can also be applied to time-series forecasting tasks, which is inspiring for this area.

### 4.9. Results on Tabular Data Understanding

Table 11: Tabular data understanding with Meta-Transformer. We report Accuracy (\%) and F1 score.

| Method                |     Adult     | Bank Marketing |      |
|:--------------------- |:-------------:|:--------------:|:----:|
|                       | Accuracy (\%) | Accuracy (\%)  |  F1  |
| LightGBM              |     87.8      |       -        | 0.39 |
| Tabmlp                |     87.2      |       -        | 0.39 |
| Tabnet                |     87.0      |       -        | 0.31 |
| Tabtransformer        |     87.1      |      93.4      | 0.42 |
| Meta-Transformer-B16F |     85.9      |      90.1      | 0.41 |

data understanding, especially on complex datasets such as Bank Marketing.

Table 12: Graph data understanding with Meta-Transformer. We conduct experiments on the PCQM4M-LSC dataset, and we report the evaluation metrics of train and validation MAE scores and the number of trainable parameters.

|            Method            |      Param.       |       train MAE        |      validate MAE      |
|:----------------------------:|:-----------------:|:----------------------:|:----------------------:|
|          GCN [120]           | $2.0 \mathrm{M}$  |         0.1318         |         0.1691         |
|          GIN [121]           | $3.8 \mathrm{M}$  |         0.1203         |         0.1537         |
|       GCN-VN [120, 8]        | $4.9 \mathrm{M}$  |         0.1225         |         0.1485         |
|       GIN-VN [121, 8]        | $6.7 \mathrm{M}$  |         0.1150         |         0.1395         |
|       GINE-VN [122]8]        | $13.2 \mathrm{M}$ |         0.1248         |         0.1430         |
|    DeeperGCN-VN [123, 8]     | $25.5 \mathrm{M}$ |         0.1059         |         0.1398         |
|   Graph Transformer [124]    | $0.6 \mathrm{M}$  |         0.0944         |         0.1400         |
| Graph Transformer-Wide [124] | $83.2 \mathrm{M}$ |         0.0955         |         0.1408         |
|          Graphormer          | $12.5 \mathrm{M}$ |         0.0778         |         0.1264         |
|       Graphormer [125]       | $47.1 \mathrm{M}$ | $\mathbf{0 . 0 5 8 2}$ | $\mathbf{0 . 1 2 3 4}$ |
|     Meta-Transformer-B16     | $1.1 \mathrm{M}$  |         0.8034         |         0.8863         |

### 4.10. Results on Graph and IMU Data Understanding

We report the performance of utilizing Meta-Transformer for graph understanding in Table 12 We compare Meta-Transformer-B16 $6_{\mathrm{F}}$ with various graph neural network models for graph data understanding on the PCQM4M-LSC dataset [86]. Among all the methods, Graphormer shows the best performance with the lowest train and validation MAE scores of 0.0582 and 0.1234 , respectively. In contrast, Meta-Transformer-B16 $6_{\mathrm{F}}$ delivers the train and validation MAE scores of 0.8034 and 0.8863 , which reveals the limited ability of current Meta-Transformer architecture for structural data learning. We will further improve this in the future. Besides, following ImageBind [26], we conduct classification on the Ego4D dataset [88], with input data, Meta-Transformer delivers an accuracy of $73.9 \%$.

## 5. Limitation

From the perspectives of complexity, methodology, and further application, the limitations of the Meta-Transformer are summarized as follows:

Complexity: Meta-Transformer requires $\mathcal{O}\left(n^{2} \times D\right)$ computation dealing with token embeddings $\left[\boldsymbol{E}_{1}, \cdots, \boldsymbol{E}_{n}\right]$. High memory cost and heavy computation burden make it difficult to scale up.

Methodology: Compared with Axial Attention mechanism in TimeSformer [7] and Graphormer [125], Meta-Transformer lacks temporal and structural awareness. This limitation may affect the overall performance of Meta-Transformer in tasks where temporal and structural modeling plays a critical role, such as video understanding, visual tracking, or social network prediction.

Application: Meta-Transformer primarily delivers its advantages in multimodal perception. It's still unknown about its ability for cross-modal generation. We will work on this in the future.

## 6. Conclusion

In the early stages of artificial intelligence development, pioneers introduced the Multi-Layer Perceptron (MLP) to address prediction tasks in machine learning. Later, recurrent and convolutional networks expanded AI capabilities in multimedia data processing, achieving significant success in extracting representations from texts, images, point clouds, and audio. MLPs have since been integrated into deep convolutional networks. In this paper, we explore the potential of plain transformers for unified multimodal learning, highlighting a promising trend toward developing unified multimodal intelligence with a transformer backbone. To some extent, this paper supports the dominant position of transformers in next-generation networks. Importantly, CNNs and MLPs are not left behind. They play essential roles in data tokenization and representation projection. This process exemplifies the law of succession in neural networks and the ongoing evolution of artificial intelligence.

## 7. References

[1] Zirui Wang, Jiahui Yu, Adams Wei Yu, Zihang Dai, Yulia Tsvetkov, and Yuan Cao. Simvlm: Simple visual language model pretraining with weak supervision. arXiv preprint arXiv:2108.10904, 2021.

[2] Wenhui Wang, Hangbo Bao, Li Dong, and Furu Wei. Vlmo: Unified vision-language pretraining with mixture-of-modality-experts. arXiv preprint arXiv:2111.02358, 2021.

[3] Wenhui Wang, Hangbo Bao, Li Dong, Johan Bjorck, Zhiliang Peng, Qiang Liu, Kriti Aggarwal, Owais Khan Mohammed, Saksham Singhal, Subhojit Som, et al. Image as a foreign language: Beit pretraining for all vision and vision-language tasks. arXiv preprint arXiv:2208.10442, 2022.

[4] Kaiming He, Xinlei Chen, Saining Xie, Yanghao Li, Piotr Dollár, and Ross Girshick. Masked autoencoders are scalable vision learners. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 16000-16009, 2022.

[5] Charles R Qi, Li Yi, Hao Su, and Leonidas J Guibas. Pointnet++: Deep hierarchical feature learning on point sets in a metric space. In NeurIPS, 2017.

[6] Yuan Gong, Yu-An Chung, and James Glass. Ast: Audio spectrogram transformer. arXiv preprint arXiv:2104.01778, 2021.

[7] Gedas Bertasius, Heng Wang, and Lorenzo Torresani. Is space-time attention all you need for video understanding? In Proceedings of the International Conference on Machine Learning (ICML), July 2021.

[8] Justin Gilmer, Samuel S Schoenholz, Patrick F Riley, Oriol Vinyals, and George E Dahl. Neural message passing for quantum chemistry. In International Conference on Machine Learning, pages 1263-1272. PMLR, 2017.

[9] Hengshuang Zhao, Li Jiang, Jiaya Jia, Philip HS Torr, and Vladlen Koltun. Point transformer. In ICCV, pages 16259-16268, 2021.

[10] Peng Wang, An Yang, Rui Men, Junyang Lin, Shuai Bai, Zhikang Li, Jianxin Ma, Chang Zhou, Jingren Zhou, and Hongxia Yang. Unifying architectures, tasks, and modalities through a simple sequence-to-sequence learning framework. arXiv preprint arXiv:2202.03052, 2022.

[11] Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N Gomez, Łukasz Kaiser, and Illia Polosukhin. Attention is all you need. Advances in neural information processing systems, 30, 2017.

[12] Nicolas Carion, Francisco Massa, Gabriel Synnaeve, Nicolas Usunier, Alexander Kirillov, and Sergey Zagoruyko. End-to-end object detection with transformers. In ECCV, 2020.

[13] Alexey Dosovitskiy, Lucas Beyer, Alexander Kolesnikov, Dirk Weissenborn, Xiaohua Zhai, Thomas Unterthiner, Mostafa Dehghani, Matthias Minderer, Georg Heigold, Sylvain Gelly, Jakob Uszkoreit, and Neil Houlsby. An image is worth 16x16 words: Transformers for image recognition at scale. ICLR, 2021.

[14] Xiaohua Zhai, Alexander Kolesnikov, Neil Houlsby, and Lucas Beyer. Scaling vision transformers. In CVPR, pages 12104-12113, 2022.

[15] Enze Xie, Wenhai Wang, Zhiding Yu, Anima Anandkumar, Jose M Alvarez, and Ping Luo. Segformer: Simple and efficient design for semantic segmentation with transformers. Advances in Neural Information Processing Systems, 34:12077-12090, 2021.

[16] Wenhai Wang, Enze Xie, Xiang Li, Deng-Ping Fan, Kaitao Song, Ding Liang, Tong Lu, Ping Luo, and Ling Shao. Pvtv2: Improved baselines with pyramid vision transformer. arXiv:2106.13797, 2021. [17] Alexey Dosovitskiy, Lucas Beyer, Alexander Kolesnikov, Dirk Weissenborn, Xiaohua Zhai, Thomas Unterthiner, Mostafa Dehghani, Matthias Minderer, Georg Heigold, Sylvain Gelly, Jakob Uszkoreit, and Neil Houlsby. An image is worth 16x16 words: Transformers for image recognition at scale. In ICLR, 2021.

[18] Zhe Chen, Yuchen Duan, Wenhai Wang, Junjun He, Tong Lu, Jifeng Dai, and Yu Qiao. Vision transformer adapter for dense predictions. arXiv preprint arXiv:2205.08534, 2022.

[19] Ze Liu, Yutong Lin, Yue Cao, Han Hu, Yixuan Wei, Zheng Zhang, Stephen Lin, and Baining Guo. Swin transformer: Hierarchical vision transformer using shifted windows. In ICCV, pages 10012-10022, 2021.

[20] Xumin Yu, Lulu Tang, Yongming Rao, Tiejun Huang, Jie Zhou, and Jiwen Lu. Point-bert: Pre-training 3d point cloud transformers with masked point modeling. In CVPR, 2022.

[21] Guocheng Qian, Xingdi Zhang, Abdullah Hamdi, and Bernard Ghanem. Pix4point: Image pretrained transformers for $3 \mathrm{~d}$ point cloud understanding. arXiv preprint arXiv:2208.12259, 2022.

[22] Meng-Hao Guo, Jun-Xiong Cai, Zheng-Ning Liu, Tai-Jiang Mu, Ralph R Martin, and Shi-Min Hu. Pct: Point cloud transformer. Computational Visual Media, 7(2):187-199, 2021.

[23] Yuan Gong, Cheng-I Lai, Yu-An Chung, and James Glass. Ssast: Self-supervised audio spectrogram transformer. In Proceedings of the AAAI Conference on Artificial Intelligence, volume 36, pages 10699-10709, 2022.

[24] Alec Radford, Jong Wook Kim, Chris Hallacy, Aditya Ramesh, Gabriel Goh, Sandhini Agarwal, Girish Sastry, Amanda Askell, Pamela Mishkin, Jack Clark, et al. Learning transferable visual models from natural language supervision. In International Conference on Machine Learning, pages 8748-8763. PMLR, 2021.

[25] Jean-Baptiste Alayrac, Jeff Donahue, Pauline Luc, Antoine Miech, Iain Barr, Yana Hasson, Karel Lenc, Arthur Mensch, Katie Millican, Malcolm Reynolds, et al. Flamingo: a visual language model for few-shot learning. arXiv preprint arXiv:2204.14198, 2022.

[26] Rohit Girdhar, Alaaeldin El-Nouby, Zhuang Liu, Mannat Singh, Kalyan Vasudev Alwala, Armand Joulin, and Ishan Misra. Imagebind: One embedding space to bind them all. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 15180-15190, 2023.

[27] Warren S McCulloch and Walter Pitts. A logical calculus of the ideas immanent in nervous activity. The bulletin of mathematical biophysics, 5:115-133, 1943.

[28] Marti A. Hearst, Susan T Dumais, Edgar Osuna, John Platt, and Bernhard Scholkopf. Support vector machines. IEEE Intelligent Systems and their applications, 13(4):18-28, 1998.

[29] Kaiming He, Xiangyu Zhang, Shaoqing Ren, and Jian Sun. Deep residual learning for image recognition. In CVPR, pages 770-778, 2016.

[30] Zhao Xu, Kai Yu, Volker Tresp, Xiaowei Xu, and Jizhi Wang. Representative sampling for text classification using support vector machines. In Advances in Information Retrieval: 25th European Conference on IR Research, ECIR 2003, Pisa, Italy, April 14-16, 2003. Proceedings 25, pages 393-407. Springer, 2003.

[31] Yann LeCun, Bernhard Boser, John Denker, Donnie Henderson, Richard Howard, Wayne Hubbard, and Lawrence Jackel. Handwritten digit recognition with a back-propagation network. Advances in neural information processing systems, 2, 1989.

[32] Charles Ruizhongtai Qi, Hao Su, Kaichun Mo, and Leonidas J. Guibas. Pointnet: Deep learning on point sets for 3d classification and segmentation. In CVPR, 2017.

[33] P Dhanalakshmi, S Palanivel, and Vennila Ramalingam. Classification of audio signals using svm and rbfnn. Expert systems with applications, 36(3):6069-6075, 2009. [34] John J Hopfield. Neural networks and physical systems with emergent collective computational abilities. Proceedings of the national academy of sciences, 79(8):2554-2558, 1982.

[35] Sepp Hochreiter and Jürgen Schmidhuber. Long short-term memory. Neural computation, 9(8):1735-1780, 1997.

[36] Junyoung Chung, Caglar Gulcehre, KyungHyun Cho, and Yoshua Bengio. Empirical evaluation of gated recurrent neural networks on sequence modeling. arXiv preprint arXiv:1412.3555, 2014.

[37] Ramesh Nallapati, Bowen Zhou, Caglar Gulcehre, Bing Xiang, et al. Abstractive text summarization using sequence-to-sequence rnns and beyond. arXiv preprint arXiv:1602.06023, 2016.

[38] Kyunghyun Cho, Bart Van Merriënboer, Caglar Gulcehre, Dzmitry Bahdanau, Fethi Bougares, Holger Schwenk, and Yoshua Bengio. Learning phrase representations using rnn encoderdecoder for statistical machine translation. arXiv preprint arXiv:1406.1078, 2014.

[39] Duyu Tang, Bing Qin, and Ting Liu. Document modeling with gated recurrent neural network for sentiment classification. In Proceedings of the 2015 conference on empirical methods in natural language processing, pages 1422-1432, 2015.

[40] Nal Kalchbrenner, Erich Elsen, Karen Simonyan, Seb Noury, Norman Casagrande, Edward Lockhart, Florian Stimberg, Aaron Oord, Sander Dieleman, and Koray Kavukcuoglu. Efficient neural audio synthesis. In International Conference on Machine Learning, pages 2410-2419. PMLR, 2018.

[41] Yann LeCun, Léon Bottou, Yoshua Bengio, and Patrick Haffner. Gradient-based learning applied to document recognition. Proceedings of the IEEE, 86(11):2278-2324, 1998.

[42] Alex Krizhevsky, Ilya Sutskever, and Geoffrey E Hinton. Imagenet classification with deep convolutional neural networks. Communications of the ACM, 60(6):84-90, 2017.

[43] Karen Simonyan and Andrew Zisserman. Very deep convolutional networks for large-scale image recognition. In ICLR, 2015.

[44] Christian Szegedy, Wei Liu, Yangqing Jia, Pierre Sermanet, Scott Reed, Dragomir Anguelov, Dumitru Erhan, Vincent Vanhoucke, and Andrew Rabinovich. Going deeper with convolutions. In Proceedings of the IEEE conference on computer vision and pattern recognition, pages 1-9, 2015.

[45] Xiang Zhang, Junbo Zhao, and Yann LeCun. Character-level convolutional networks for text classification. Advances in neural information processing systems, 28, 2015.

[46] Ye Zhang and Byron Wallace. A sensitivity analysis of (and practitioners' guide to) convolutional neural networks for sentence classification. arXiv preprint arXiv:1510.03820, 2015 .

[47] Yangyan Li, Rui Bu, Mingchao Sun, Wei Wu, Xinhan Di, and Baoquan Chen. Pointenn: Convolution on x-transformed points. Advances in neural information processing systems, 31, 2018.

[48] Daniel Maturana and Sebastian Scherer. Voxnet: A 3d convolutional neural network for real-time object recognition. In IROS, 2015.

[49] Hugues Thomas, Charles R Qi, Jean-Emmanuel Deschaud, Beatriz Marcotegui, François Goulette, and Leonidas J Guibas. Kpconv: Flexible and deformable convolution for point clouds. In ICCV, 2019.

[50] Ossama Abdel-Hamid, Abdel-rahman Mohamed, Hui Jiang, Li Deng, Gerald Penn, and Dong Yu. Convolutional neural networks for speech recognition. IEEE/ACM Transactions on audio, speech, and language processing, 22(10):1533-1545, 2014.

[51] Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. BERT: Pre-training of deep bidirectional transformers for language understanding. In NAACL-HLT, 2019. [52] Tom Brown, Benjamin Mann, Nick Ryder, Melanie Subbiah, Jared D Kaplan, Prafulla Dhariwal, Arvind Neelakantan, Pranav Shyam, Girish Sastry, Amanda Askell, et al. Language models are few-shot learners. Advances in neural information processing systems, 33:18771901, 2020.

[53] Nicolas Carion, Francisco Massa, Gabriel Synnaeve, Nicolas Usunier, Alexander Kirillov, and Sergey Zagoruyko. End-to-end object detection with transformers. In Computer Vision-ECCV 2020: 16th European Conference, Glasgow, UK, August 23-28, 2020, Proceedings, Part I 16, pages 213-229. Springer, 2020.

[54] Zhijian Liu, Haotian Tang, Alexander Amini, Xinyu Yang, Huizi Mao, Daniela Rus, and Song Han. Bevfusion: Multi-task multi-sensor fusion with unified bird's-eye view representation. arXiv preprint arXiv:2205.13542, 2022.

[55] Feihu Zhang, Jin Fang, Benjamin Wah, and Philip Torr. Deep fusionnet for point cloud semantic segmentation. In ECCV, 2020.

[56] Ziyi Wang, Xumin Yu, Yongming Rao, Jie Zhou, and Jiwen Lu. P2p: Tuning pre-trained image models for point cloud analysis with point-to-pixel prompting. arXiv preprint arXiv:2208.02812, 2022.

[57] Zhou Yu, Jun Yu, Yuhao Cui, Dacheng Tao, and Qi Tian. Deep modular co-attention networks for visual question answering. In Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, pages 6281-6290, 2019.

[58] Weijie Su, Xizhou Zhu, Yue Cao, Bin Li, Lewei Lu, Furu Wei, and Jifeng Dai. Vl-bert: Pre-training of generic visual-linguistic representations. arXiv preprint arXiv:1908.08530, 2019.

[59] Xiujun Li, Xi Yin, Chunyuan Li, Pengchuan Zhang, Xiaowei Hu, Lei Zhang, Lijuan Wang, Houdong Hu, Li Dong, Furu Wei, et al. Oscar: Object-semantics aligned pre-training for vision-language tasks. In European Conference on Computer Vision, pages 121-137. Springer, 2020.

[60] Pengchuan Zhang, Xiujun Li, Xiaowei Hu, Jianwei Yang, Lei Zhang, Lijuan Wang, Yejin Choi, and Jianfeng Gao. Vinvl: Revisiting visual representations in vision-language models. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 5579-5588, 2021.

[61] Junnan Li, Ramprasaath Selvaraju, Akhilesh Gotmare, Shafiq Joty, Caiming Xiong, and Steven Chu Hong Hoi. Align before fuse: Vision and language representation learning with momentum distillation. Advances in neural information processing systems, 34:9694-9705, 2021.

[62] Lu Yuan, Dongdong Chen, Yi-Ling Chen, Noel Codella, Xiyang Dai, Jianfeng Gao, Houdong $\mathrm{Hu}$, Xuedong Huang, Boxin Li, Chunyuan Li, et al. Florence: A new foundation model for computer vision. arXiv preprint arXiv:2111.11432, 2021.

[63] Jiahui Yu, Zirui Wang, Vijay Vasudevan, Legg Yeung, Mojtaba Seyedhosseini, and Yonghui Wu. Coca: Contrastive captioners are image-text foundation models. arXiv preprint arXiv:2205.01917, 2022.

[64] Rakesh Chada, Zhaoheng Zheng, and Pradeep Natarajan. Momo: A shared encoder model for text, image and multi-modal representations. arXiv preprint arXiv:2304.05523, 2023.

[65] Yinhan Liu, Myle Ott, Naman Goyal, Jingfei Du, Mandar Joshi, Danqi Chen, Omer Levy, Mike Lewis, Luke Zettlemoyer, and Veselin Stoyanov. Roberta: A robustly optimized bert pretraining approach. arXiv preprint arXiv:1907.11692, 2019.

[66] Yonghui Wu, Mike Schuster, Zhifeng Chen, Quoc V Le, Mohammad Norouzi, Wolfgang Macherey, Maxim Krikun, Yuan Cao, Qin Gao, Klaus Macherey, et al. Google's neural machine translation system: Bridging the gap between human and machine translation. arXiv preprint arXiv:1609.08144, 2016. [67] Steffen Schneider, Alexei Baevski, Ronan Collobert, and Michael Auli. wav2vec: Unsupervised pre-training for speech recognition. arXiv preprint arXiv:1904.05862, 2019.

[68] Alex Wang, Amanpreet Singh, Julian Michael, Felix Hill, Omer Levy, and Samuel R Bowman. Glue: A multi-task benchmark and analysis platform for natural language understanding. arXiv preprint arXiv:1804.07461, 2018.

[69] Jia Deng, Wei Dong, Richard Socher, Li-Jia Li, Kai Li, and Li Fei-Fei. Imagenet: A large-scale hierarchical image database. In CVPR, pages 248-255. Ieee, 2009.

[70] Wenhai Wang, Enze Xie, Xiang Li, Deng-Ping Fan, Kaitao Song, Ding Liang, Tong Lu, Ping Luo, and Ling Shao. Pyramid vision transformer: A versatile backbone for dense prediction without convolutions. In ICCV, 2021.

[71] Zhuang Liu, Hanzi Mao, Chao-Yuan Wu, Christoph Feichtenhofer, Trevor Darrell, and Saining Xie. A convnet for the 2020s. arXiv preprint arXiv:2201.03545, 2022.

[72] Tsung-Yi Lin, Michael Maire, Serge J. Belongie, James Hays, Pietro Perona, Deva Ramanan, Piotr Dollár, and C. Lawrence Zitnick. Microsoft coco: Common objects in context. In ECCV, 2014.

[73] Kaiming He, Georgia Gkioxari, Piotr Dollár, and Ross Girshick. Mask r-cnn. In ICCV, pages 2961-2969, 2017.

[74] Tete Xiao, Yingcheng Liu, Bolei Zhou, Yuning Jiang, and Jian Sun. Unified perceptual parsing for scene understanding. In ECCV, pages 418-434, 2018.

[75] Bolei Zhou, Hang Zhao, Xavier Puig, Sanja Fidler, Adela Barriuso, and Antonio Torralba. Scene parsing through ade20k dataset. In Proceedings of the IEEE conference on computer vision and pattern recognition, pages 633-641, 2017.

[76] Dat Tien Nguyen, Hyung Gil Hong, Ki Wan Kim, and Kang Ryoung Park. Person recognition system based on a combination of body images from visible light and thermal cameras. Sensors, 17(3):605, 2017.

[77] Tawsifur Rahman, Amith Khandakar, Muhammad Abdul Kadir, Khandaker Rejaul Islam, Khandakar F Islam, Rashid Mazhar, Tahir Hamid, Mohammad Tariqul Islam, Saad Kashem, Zaid Bin Mahbub, et al. Reliable tuberculosis detection using chest x-ray with deep learning, segmentation and visualization. IEEE Access, 8:191586-191601, 2020.

[78] Zhirong Wu, Shuran Song, Aditya Khosla, Fisher Yu, Linguang Zhang, Xiaoou Tang, and Jianxiong Xiao. 3d shapenets: A deep representation for volumetric shapes. In CVPR, 2015.

[79] Iro Armeni, Ozan Sener, Amir R Zamir, Helen Jiang, Ioannis Brilakis, Martin Fischer, and Silvio Savarese. 3d semantic parsing of large-scale indoor spaces. In CVPR, pages 1534-1543, 2016.

[80] Li Yi, Vladimir G Kim, Duygu Ceylan, I Shen, Mengyan Yan, Hao Su, ARCewu Lu, Qixing Huang, Alla Sheffer, Leonidas Guibas, et al. A scalable active framework for region annotation in 3d shape collections. ACM TOG, 35(6):210, 2016.

[81] Pete Warden. Speech commands: A dataset for limited-vocabulary speech recognition. arXiv preprint arXiv:1804.03209, 2018.

[82] Khurram Soomro, Amir Roshan Zamir, and Mubarak Shah. Ucf101: A dataset of 101 human actions classes from videos in the wild. arXiv preprint arXiv:1212.0402, 2012.

[83] Haoyi Zhou, Shanghang Zhang, Jieqi Peng, Shuai Zhang, Jianxin Li, Hui Xiong, and Wancai Zhang. Informer: Beyond efficient transformer for long sequence time-series forecasting. In AAAI, 2021.

[84] Guokun Lai, Wei-Cheng Chang, Yiming Yang, and Hanxiao Liu. Modeling long-and shortterm temporal patterns with deep neural networks. In The 41st international ACM SIGIR conference on research \& development in information retrieval, pages 95-104, 2018. [85] Haixu Wu, Jiehui Xu, Jianmin Wang, and Mingsheng Long. Autoformer: Decomposition transformers with Auto-Correlation for long-term series forecasting. In NeurIPS, 2021.

[86] Weihua Hu, Matthias Fey, Hongyu Ren, Maho Nakata, Yuxiao Dong, and Jure Leskovec. Ogb1sc: A large-scale challenge for machine learning on graphs. arXiv preprint arXiv:2103.09430, 2021.

[87] Xin Huang, Ashish Khetan, Milan Cvitkovic, and Zohar Karnin. Tabtransformer: Tabular data modeling using contextual embeddings. arXiv preprint arXiv:2012.06678, 2020.

[88] Kristen Grauman, Andrew Westbury, Eugene Byrne, Zachary Chavis, Antonino Furnari, Rohit Girdhar, Jackson Hamburger, Hao Jiang, Miao Liu, Xingyu Liu, et al. Ego4d: Around the world in 3,000 hours of egocentric video. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 18995-19012, 2022.

[89] Alec Radford, Karthik Narasimhan, Tim Salimans, Ilya Sutskever, et al. Improving language understanding by generative pre-training. 2018 .

[90] Zihang Dai, Hanxiao Liu, Quoc V Le, and Mingxing Tan. Coatnet: Marrying convolution and attention for all data sizes. Advances in Neural Information Processing Systems, 34:3965-3977, 2021.

[91] Hugo Touvron, Matthieu Cord, and Hervé Jégou. Deit iii: Revenge of the vit. In Computer Vision-ECCV 2022: 17th European Conference, Tel Aviv, Israel, October 23-27, 2022, Proceedings, Part XXIV, pages 516-533. Springer, 2022.

[92] Ze Liu, Han Hu, Yutong Lin, Zhuliang Yao, Zhenda Xie, Yixuan Wei, Jia Ning, Yue Cao, Zheng Zhang, Li Dong, et al. Swin transformer v2: Scaling up capacity and resolution. In Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, pages 12009-12019, 2022.

[93] Xiaohan Ding, Xiangyu Zhang, Yizhuang Zhou, Jungong Han, Guiguang Ding, and Jian Sun. Scaling up your kernels to 31x31: Revisiting large kernel design in cnns. In CVPR, 2022.

[94] Yongming Rao, Wenliang Zhao, Yansong Tang, Jie Zhou, Ser Nam Lim, and Jiwen Lu. Hornet: Efficient high-order spatial interactions with recursive gated convolutions. Advances in Neural Information Processing Systems, 35:10353-10366, 2022.

[95] Zhuang Liu, Hanzi Mao, Chao-Yuan Wu, Christoph Feichtenhofer, Trevor Darrell, and Saining Xie. A convnet for the 2020s. In CVPR, 2022.

[96] Wenhai Wang, Jifeng Dai, Zhe Chen, Zhenhang Huang, Zhiqi Li, Xizhou Zhu, Xiaowei Hu, Tong Lu, Lewei $\mathrm{Lu}$, Hongsheng Li, et al. Internimage: Exploring large-scale vision foundation models with deformable convolutions. arXiv preprint arXiv:2211.05778, 2022.

[97] Mang Ye, Jianbing Shen, Gaojie Lin, Tao Xiang, Ling Shao, and Steven C. H. Hoi. Deep learning for person re-identification: A survey and outlook. arXiv preprint arXiv:2001.04193, 2020.

[98] Ziyu Wei, Xi Yang, Nannan Wang, and Xinbo Gao. Syncretic modality collaborative learning for visible infrared person re-identification. In ICCV, pages 225-234, October 2021.

[99] Yiyuan Zhang, Sanyuan Zhao, Yuhao Kang, and Jianbing Shen. Modality synergy complement learning with cascaded aggregation for visible-infrared person re-identification. In Computer Vision-ECCV 2022: 17th European Conference, Tel Aviv, Israel, October 23-27, 2022, Proceedings, Part XIV, pages 462-479. Springer, 2022.

[100] Danfeng Hong, Zhu Han, Jing Yao, Lianru Gao, Bing Zhang, Antonio Plaza, and Jocelyn Chanussot. Spectralformer: Rethinking hyperspectral image classification with transformers. IEEE Transactions on Geoscience and Remote Sensing, 60:1-15, 2021.

[101] Yue Wang, Yongbin Sun, Ziwei Liu, Sanjay E Sarma, Michael M Bronstein, and Justin M Solomon. Dynamic graph cnn for learning on point clouds. TOG, 2019. [102] Guocheng Qian, Yuchen Li, Houwen Peng, Jinjie Mai, Hasan Hammoud, Mohamed Elhoseiny, and Bernard Ghanem. Pointnext: Revisiting pointnet++ with improved training and scaling strategies. In Advances in Neural Information Processing Systems (NeurIPS), 2022.

[103] Xu Ma, Can Qin, Haoxuan You, Haoxi Ran, and Yun Fu. Rethinking network design and local geometry in point cloud: A simple residual mlp framework. ICLR, 2022.

[104] Jaesung Choe, Chunghyun Park, Francois Rameau, Jaesik Park, and In So Kweon. Pointmixer: Mlp-mixer for point cloud understanding. In European Conference on Computer Vision, pages 620-640. Springer, 2022.

[105] Yatian Pang, Wenxiao Wang, Francis EH Tay, Wei Liu, Yonghong Tian, and Li Yuan. Masked autoencoders for point cloud self-supervised learning. arXiv preprint arXiv:2203.06604, 2022.

[106] Runpei Dong, Zekun Qi, Linfeng Zhang, Junbo Zhang, Jianjian Sun, Zheng Ge, Li Yi, and Kaisheng Ma. Autoencoders as cross-modal teachers: Can pretrained 2d image transformers help 3d representation learning? arXiv preprint arXiv:2212.08320, 2022.

[107] Ze Liu, Han Hu, Yutong Lin, Zhuliang Yao, Zhenda Xie, Yixuan Wei, Jia Ning, Yue Cao, Zheng Zhang, Li Dong, et al. Swin transformer v2: Scaling up capacity and resolution. In CVPR, 2022.

[108] Faris Almalik, Mohammad Yaqub, and Karthik Nandakumar. Self-ensembling vision transformer (sevit) for robust medical image classification. In Medical Image Computing and Computer Assisted Intervention-MICCAI 2022: 25th International Conference, Singapore, September 18-22, 2022, Proceedings, Part III, pages 376-386. Springer, 2022.

[109] Hsin-Ying Lee, Jia-Bin Huang, Maneesh Singh, and Ming-Hsuan Yang. Unsupervised representation learning by sorting sequence. In ICCV, 2017.

[110] Christoph Feichtenhofer, Haoqi Fan, Bo Xiong, Ross Girshick, and Kaiming He. A large-scale study on unsupervised spatiotemporal representation learning. In $C V P R, 2021$.

[111] Zhan Tong, Yibing Song, Jue Wang, and Limin Wang. Videomae: Masked autoencoders are data-efficient learners for self-supervised video pre-training. arXiv preprint arXiv:2203.12602, 2022.

[112] Limin Wang, Bingkun Huang, Zhiyu Zhao, Zhan Tong, Yinan He, Yi Wang, Yali Wang, and Yu Qiao. Videomae v2: Scaling video masked autoencoders with dual masking. arXiv preprint arXiv:2303.16727, 2023.

[113] Haixu Wu, Tengge Hu, Yong Liu, Hang Zhou, Jianmin Wang, and Mingsheng Long. Timesnet: Temporal $2 \mathrm{~d}$-variation modeling for general time series analysis. arXiv preprint arXiv:2210.02186, 2022.

[114] Gerald Woo, Chenghao Liu, Doyen Sahoo, Akshat Kumar, and Steven C. H. Hoi. Etsformer: Exponential smoothing transformers for time-series forecasting. arXiv preprint arXiv:2202.01381, 2022.

[115] Tian Zhou, Ziqing Ma, Qingsong Wen, Xue Wang, Liang Sun, and Rong Jin. FEDformer: Frequency enhanced decomposed transformer for long-term series forecasting. In ICML, 2022.

[116] Yong Liu, Haixu Wu, Jianmin Wang, and Mingsheng Long. Non-stationary transformers: Rethinking the stationarity in time series forecasting. In NeurIPS, 2022.

[117] Shizhan Liu, Hang Yu, Cong Liao, Jianguo Li, Weiyao Lin, Alex X Liu, and Schahram Dustdar. Pyraformer: Low-complexity pyramidal attention for long-range time series modeling and forecasting. In ICLR, 2021.

[118] Shiyang Li, Xiaoyong Jin, Yao Xuan, Xiyou Zhou, Wenhu Chen, Yu-Xiang Wang, and Xifeng Yan. Enhancing the locality and breaking the memory bottleneck of transformer on time series forecasting. In NeurIPS, 2019.

[119] Nikita Kitaev, Lukasz Kaiser, and Anselm Levskaya. Reformer: The efficient transformer. In ICLR, 2020. [120] Thomas N. Kipf and Max Welling. Semi-supervised classification with graph convolutional networks. In ICLR. OpenReview.net, 2017.

[121] Keyulu Xu, Weihua Hu, Jure Leskovec, and Stefanie Jegelka. How powerful are graph neural networks? In International Conference on Learning Representations, 2019.

[122] Rémy Brossard, Oriel Frigo, and David Dehaene. Graph convolutions that can finally model local structure. arXiv preprint arXiv:2011.15069, 2020.

[123] Guohao Li, Chenxin Xiong, Ali Thabet, and Bernard Ghanem. Deepergen: All you need to train deeper gcns. arXiv preprint arXiv:2006.07739, 2020.

[124] Vijay Prakash Dwivedi and Xavier Bresson. A generalization of transformer networks to graphs. AAAI Workshop on Deep Learning on Graphs: Methods and Applications, 2021.

[125] Chengxuan Ying, Tianle Cai, Shengjie Luo, Shuxin Zheng, Guolin Ke, Di He, Yanming Shen, and Tie-Yan Liu. Do transformers really perform badly for graph representation? In Thirty-Fifth Conference on Neural Information Processing Systems, 2021.

[126] Jinxing Zhou, Jianyuan Wang, Jiayi Zhang, Weixuan Sun, Jing Zhang, Stan Birchfield, Dan Guo, Lingpeng Kong, Meng Wang, and Yiran Zhong. Audio-visual segmentation. In Computer Vision-ECCV 2022: 17th European Conference, Tel Aviv, Israel, October 23-27, 2022, Proceedings, Part XXXVII, pages 386-403. Springer, 2022.

## 8. Appendix

## 9. A Summary

The appendix is organized as the following:

- We first validate and discuss the potential of the Meta-Transformer on more modalities (video, infrared, X-Ray, and hyperspectral images) in addition to the modalities shown in the main paper, and we provide surprising experimental results on these modalities in $\S B$
- Then we further demonstrate the performance and merits of Meta-Transformer in dealing with multi-modal tasks (involving inputs from more than one modality to perform predictions) in $\S \mathrm{C}$
- In addition, we introduce more details of experiments on text, image, point cloud, and audio in $\S \mathrm{D}$.
- Last but not least, we discuss the impact of Meta-Transformer on the machine learning and computer vision community in $\S \mathrm{E}$


## 10. B Extensibility on Single-Modality Perception

In the main body of this paper, we illustrate that Meta-Transformer can simultaneously uncover the underlying patterns of natural language, 2D images, 3D point clouds, and audio spectrograms with the same network architecture and network parameters. Furthermore, we explore its ability in perceiving other modalities, like video recognition, infrared, X-Ray, and hyperspectral image recognition. In specific, we conduct experiments on UCF101 [82] (video), RegDB [76] (infrared images), Chest X-Ray [77], and Indian Pine (hyperspectral images) datasets.

## 11. B.1 Video Recognition

For video recognition, we follow VideoMAE [111] to modify the tokenizer by replacing the $2 \mathrm{D}$ embedding layer with a 3D embedding layer to simultaneously encode the spatial-temporal information from input frames. After tokenization, by leveraging the modality-shared encoder and task-specific heads, Meta-Transformer is able to extract high-level semantic features from videos and achieve favorable performance in the action recognition task of the UCF101 dataset.

Dataset. The UCF101 [82] dataset is a common-used benchmark dataset for action recognition tasks. It is an extended version of UCF50 and contains 13,320 video clips of 101 categories. These 101 categories can be divided into 5 groups: Body motion, Human-human interactions, Human-object interactions, Playing musical instruments and Sports. All the input frames are with a resolution of $320 \times 240$ and a fixed frame rate of 25 FPS, collected from YouTube.

## 12. B.2 Infrared Image Recognition

Infrared and hyperspectral image recognition poses unique challenges due to their specific characteristics. For infrared images, the Meta-Transformer framework could be adapted to capture thermal information by encoding temperature values alongside visual features, where the tokenizer for infrared images is the same as common RGB images.

Dataset. The RegDB [76] dataset focuses on evaluating the performance of infrared recognition algorithms in unconstrained and realistic scenarios. It includes variations in pose, expression, illumination, and occlusion. We conduct experiments on the RegDB dataset to evaluate the performance of Meta-Transformer on infrared recognition.

## 13. B.3 Hyperspectral Image Recognition

Similarly, for hyperspectral images, we expect that Meta-Transformer can also handle the highdimensional spectral information by representing each spectral band in token embeddings. Compared with dealing with RGB images, the only modification is that we employ the new linear projection layer to replace the existing 2D convolution layer.

Dataset. The Indian Pine dataset is widely used in remote sensing and hyperspectral image analysis. It consists of $145 \times 145$ pixels with 145 spectral bands, which are captured in Indiana.

## 14. B.4 X-Ray Image Recognition

In addition, we explore the potential of the Meta-Transformer in medical image analysis. We leverage the tokenizer for RGB images here to encode raw medical images. Specifically, we conduct experiments regarding X-ray image analysis on the Chest X-Ray [77] dataset. It is a collection of medical images commonly used for the analysis and diagnosis of various thoracic conditions. It comprises 7,000 X-ray images of the chest. The dataset is annotated with labels indicating the presence or absence of abnormalities such as lung diseases, fractures, and heart conditions.

## 15. Extensibility on Multi-Modality Perception

Since the modalities of text, image, point cloud, and audio are all involved in this paper, we did not conduct comprehensive multi-modal experiments as common practice such as Flamingo [25], OFA [10], or BEiT-3 [3]. Instead, we conduct multi-modal experiments on a new and challenging task of Audio-Visual Segmentation [126], which is mainly focused on building an intelligent listener to align with fundamental visual tasks.

## 16. C.1 Audio-Visual Segmentation

Audio-visual segmentation [126] refers to the task of segmenting objects from different audio sources within a referring image. It aims to develop algorithms that analyze both audio and visual signals simultaneously to identify and delineate distinct sources or events. It finds applications in fields like video conferencing, surveillance, multimedia analysis, and augmented reality.

We conduct experiments on the AVSS [126] dataset, which is recently released in the field of audiovisual research. It provides a comprehensive collection of audio and visual data captured in real-world scenarios. The dataset includes synchronized audio and visual recordings, featuring various events of human actions and natural sounds. In contrast to introducing multi-modal fusion modules as existing methods, Meta-Transformer directly concatenates visual and audio embeddings after Data-toSequence tokenization. After extracting representation, we employ a simple global average pooling layer to obtain the final representations of two modalities. Table 13 illustrates the performance of

Table 13: Audio-Visual Segmentation with Meta-Transformer. We conduct experiments on the AVSS [126] dataset, we report mIou (\%) and F-score.

| Method                 |      mIou (\%)       |       F-score        |          Params           |
|:---------------------- |:--------------------:|:--------------------:|:-------------------------:|
| AVSS [126] (ResNet-50) |        20.18         |        0.252         | $\tilde{8} 0 \mathrm{M}$  |
| AVSS [126] (ASPP)      |        28.94         |          -           | $\tilde{1} 80 \mathrm{M}$ |
| AVSS [126] (PVT-v2)    |        29.77         |        0.352         | $\tilde{1} 80 \mathrm{M}$ |
| Meta-Transformer       | $\mathbf{3 1 . 3 3}$ | $\mathbf{0 . 3 8 7}$ |   $\mathbf{8 6 . 5 M}$    |

Meta-Transformer and existing methods on the AVSS dataset for audio-visual segmentation. The evaluation metrics reported in this task are mIou and F-score. In comparison, Meta-Transformer outperforms all other methods with the highest mIou of $31.33 \%$ and the highest F-score of 0.387 . It also stands out for its significantly lower parameter count, with only 86.5 million parameters compared to the approximate $80 \mathrm{M}$ to $180 \mathrm{M}$ parameters of other methods.

Meta-Transformer offers several advantages over other methods in the field.

- Unified architecture. It relieves modality-specific encoders and reduces computation by leveraging a unified encode to process both audio and images, resulting in a more efficient and streamlined process. - Faster convergence. Thanks to the unified architecture for processing both audio and images, the encoder can deeply align the two modalities instead of only at the output end, which leads to faster convergence. Meta-Transformer only needs 4 training epochs to reach $31.33 \%$ of mIou.
- Superior performance. Meta-Transformer achieves a significant improvement of $10 \%$ compared to other methods of a similar parameter scale.
- Efficiency. Despite its enhanced performance, Meta-Transformer achieves this with much fewer parameters, requiring only $1 / 3$ of the parameter amount, which makes forward and backward progress ease.

In summary, the benefits of employing the Meta-Transformer to deal with multi-modal tasks are appealing due to computational efficiency, rapid convergence, improved performance, and parameter efficiency. It reveals the significantly promising direction to apply Meta-Transformer to more multimodal tasks.

## 17. Experimental Details

Our code is built on open-source projects including MMClassification ${ }^{8}$, MMDetection ${ }^{9}$, MMsegmentatior ${ }^{10}$ OpenPoints ${ }^{11}$, Time-Series-Library ${ }^{12}$ Graphomer ${ }^{13}$

We sincerely thank their great contributions. More implementation details can be found in our source code.

## 18. E Further Impact Discussion

## 19. E.1 Modality-Free Perception

We hope that Meta-Transformer can introduce new insight into both multi-modal learning and multimodal generation fields. Meta-Transformer enables the usage of a shared encoder to encode diverse modalities, e.g. natural language, 2D images, 3D point clouds, as well as audio spectrograms., and project them into a shared representation space. This naturally reduces the modality gap across modalities and mitigates the burden of cross-modal alignment. In addition, Meta-Transformer removes the need for paired training data (such as image-text pairs), thus endowing multi-modal learning with more training flexibility.

## 20. E.2 Application Prospects

We investigate the application of Meta-Transformer on a wide range of modalities including RGB images, text, point clouds, video understanding, remote sensing (hyper-spectral images), nighttime surveillance (infrared images), and medical analysis (X-Ray images).

In video understanding, Meta-Transformer reveals the potential of enhancing the analysis and interpretation of videos by integrating information from text, audio, and image with the shared encoder. This benefits tasks such as action recognition, event detection, and video summarization. Meta-Transformer's capability to handle video-related modalities paves the way for improved video understanding applications in areas like video surveillance, video indexing, and content-based video retrieval.

In hyperspectral imaging for remote sensing, Meta-Transformer enables the analysis and understanding of hyperspectral data by extracting high-level semantic features. It enhances tasks such as

[^1] classification, target detection, and land cover mapping, improving the accuracy and efficiency of remote sensing applications. The ability to process hyperspectral images using Meta-Transformer opens doors for advancements in environmental monitoring, agriculture, urban planning, and disaster management.


In medical applications, particularly X-ray image analysis, Meta-Transformer offers a promising approach to improving diagnostic accuracy and efficiency with multi-modal information. It can effectively capture and fuse information from X-ray images, clinical data, and other modalities to aid in disease detection, anomaly identification, and treatment planning by leveraging its unified learning framework. Meta-Transformer's capability to handle multi-modal data enhances the potential for more accurate and comprehensive medical imaging analysis, leading to better patient care and outcomes.

For infrared images used in nighttime recognition and surveillance, Meta-Transformer's ability to process infrared data helps extract crucial information for object detection, tracking, and recognition in low-light conditions, which opens an avenue for advancements in nighttime surveillance, security systems, and autonomous navigation in challenging environments with the cooperation between infrared cameras with RGB cameras.

## 21. E.3 Conclusion

In summary, we think that the ability of Meta-Transformer to unify multi-modal learning comes from that neural network architectures can learn modality-invariant patterns. The architecture of Meta-Transformer illustrates the advantages of length-variable token embeddings in multi-modal learning, which provides flexible but unified forms of multi-modal semantics. Then it's time to think about designing algorithms to train networks that generalize on unseen modalities. Meanwhile, it's also intriguing to design the architecture of a unified multi-modal decoder, which can decode representations into any form of a specific modality.

Although Meta-Transformer presents a surprising performance and shows a new promising direction in multi-modal perception, we are not sure whether the proposed architectures are also effective in generative tasks. And it remains mysterious how to develop modality-invariant generative models. We hope that this can inspire future research.


[^0]:    *Equal contribution

    ${ }^{\dagger}$ Corresponding authors

    ${ }^{\ddagger}$ Project leader

[^1]:    ${ }^{8}$ https://github.com/open-mmlab/mmpretrain/tree/mmcls-1.x

    ${ }^{9}$ https://github.com/open-mmlab/mmdetection

    ${ }^{10} \mathrm{https}: / /$ github.com/open-mmlab/mmsegmentation

    ${ }^{11}$ https://github.com/guochengqian/openpoints

    ${ }^{12}$ https://github.com/thuml/Time-Series-Library

    ${ }^{13}$ https://github.com/microsoft/Graphormer
