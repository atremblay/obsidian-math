---
title: "BertNet: Harvesting Knowledge Graphs with Arbitrary Relations from
  Pretrained Language Models"
authors:
  - "Shibo Hao"
  - "Bowen Tan"
  - "Kaiwen Tang"
  - "Bin Ni"
  - "Xiyan Shao"
  - "Hengzhe Zhang"
  - "Eric P. Xing"
  - "Zhiting Hu"
published: "2022-06-28"
arXiv: "http://arxiv.org/abs/2206.14268v3"
paper: "http://arxiv.org/pdf/2206.14268v3"
---

#### Abstract

It is crucial to automatically construct knowledge graphs (KGs) of diverse new relations to support knowledge discovery and broad applications. Previous KG construction methods, based on either crowdsourcing or text mining, are often limited to a small predefined set of relations due to manual cost or restrictions in text corpus. Recent research proposed to use pretrained language models (LMs) as implicit knowledge bases that accept knowledge queries with prompts. Yet, the implicit knowledge lacks many desirable properties of a full-scale symbolic KG, such as easy access, navigation, editing, and quality assurance. In this paper, we propose a new approach of harvesting massive KGs of arbitrary relations from pretrained LMs. With minimal input of a relation definition (a prompt and a few shot of example entity pairs), the approach efficiently searches in the vast entity pair space to extract diverse accurate knowledge of the desired relation. We develop an effective search-and-rescore mechanism for improved efficiency and accuracy. We deploy the approach to harvest KGs of over 400 new relations from different LMs. Extensive human and automatic evaluations show our approach manages to extract diverse accurate knowledge, including tuples of complex relations (e.g., "A is capable of but not good at B"). The resulting KGs as a symbolic interpretation of the source LMs also reveal new insights into the LMs' knowledge capacities.
		