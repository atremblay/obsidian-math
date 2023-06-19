###### Abstract

Labeling training data is one of the most costly bottlenecks in developing machine learning-based applications. We present a first-of-its-kind study showing how existing knowledge resources from across an organization can be used as weak supervision in order to bring development time and cost down by an order of magnitude, and introduce Snorkel DryBell, a new weak supervision management system for this setting. Snorkel DryBell builds on the Snorkel framework, extending it in three critical aspects: flexible, template-based ingestion of diverse organizational knowledge, cross-feature production serving, and scalable, sampling-free execution. On three classification tasks at Google, we find that Snorkel DryBell creates classifiers of comparable quality to ones trained with tens of thousands of hand-labeled examples, converts non-servable organizational resources to servable models for an average 52% performance improvement, and executes over millions of data points in tens of minutes.

[Paper](https://arxiv.org/pdf/1812.00417)

status: #to/read