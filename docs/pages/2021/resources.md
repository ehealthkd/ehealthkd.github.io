---
title: Resources
permalink: /2021/resources

---

# Linguistic resources

For the eHealth-KD 2021 edition, systems will be evaluated in a **cross-domain** and **multi-language corpus**. Both the training, development, and test collections will contain sentences extracted from MedlinePlus, Wikinews, and the CORD-19 corpus, all of them related with health topics, but showing a significant variety in terms of format and structure. Furthermore, the majority of sentences are in Spanish, but a small set of English sentences is also included, to evaluate generalization across domains.

In the training collection, each sentence will be labeled with its corresponding domain and language (Spanish or English), so that participants can potentially fine-tune different models for each domain/language and learn to identify them in the text. In the development collection, sentences will be mixed from all different sources and no labelling will be provided, so that participants can evaluate their systems in a similar environment to the testing set.

Participants can opt-out of the cross-domain / multi-language challenge at evaluation time, in which case only the metrics from the main domain (MedlinePlus) and language (Spanish) will be used.

> **NOTE:** The resulting documents and output files are distributed along with the Task. There is no need for participants to download extra data from MedlinePlus servers, since all the input is already distributed.

## Corpus data

The corpus will be divided into three sections. Training and development sets will be published along with baseline implementations, for participants to train and fine-tune their systems. These files will consist of both plain text input and the expected outputs for both subtasks. Afterward, a small test set will be released, with plain text only, further divided into 3 sub-sets, one for each scenario. Participants are expected to submit the corresponding output files.

In no case, participants will be able to access the correct output files for the test set before the challenge ends. Afterward, the full corpus, including Brat-annotated files will be freely available under a suitable license for the research community.

### Download links:

> Will be provided shortly.

## Evaluation scripts

Evaluation scripts will be provided so that participants can test offline their systems with respect to the same metrics used in the challenge. Since participants will not have access to the test gold annotations, their offline performance will need to be evaluated in the development set. This metric will not be exactly the same as the one obtained in the test set, but it should serve for participants to compare different strategies and perform hyper-parameter tunning.

### **Download links**:

> Will be provided shortly.

## Baselines

A simple baseline will be released along with the corpus. The baseline source code will be freely available as well. The baselines performance on the development and the test set will be published.

### **Download links**:

> Will be provided shortly.

# Additional resources

Participants may freely use any additional resources they consider necessary to improve their systems, from other corpora (annotated or not), to external knowledge either explicitly (i.e., using knowledge bases) or implicitly (i.e., captured in word embeddings). For the purpose of sharing the results we expect participants to fully disclose everything they use.
However, participants may not manually annotate the test set, since doing so would be in violation of the ethics of the competition. Furthermore, we expect participants to perform all the fine tuning using only the training and development, and then perform one single run in the test set for submission, so that no accidental overfitting occurs in the test set.
