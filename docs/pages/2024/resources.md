---
title: Resources
permalink: /2024/resources
layout: "2024"
---

# Linguistic resources

For the eHealth-KD 2024 edition, systems will be assessed on a complicated NERC task based on medical text. The challenge will use a brand new corpus created from the abstracts of Web of Science documents. This corpus is entirely different from the ones used in previous eHealth-KD challenges. All texts in the corpus are in English and are divided into training, development, and test sets. The annotations were generated with the help of sci-spacy and a UMLS component and then manually reviewed for errors and improvements. The annotations were also adjusted to align with the 40-category list.

Due to the imbalanced distribution of annotations, the text was divided into sentences to create a balanced dataset. This was done only for the training split. Moreover, splitting the text into sentences ensured that all categories in all datasets had some annotations.

All resources related to eHealth-KD 2024 will be available in the [eHealthKD corpora repository](https://github.com/ehealthkd/corpora) according to the official schedule.

## Corpus data

The corpus will be divided into three parts. Participants will be provided with training and development sets and baseline implementations to help them train and fine-tune their systems. These sets will be `.csv` files containing plain text input (sentences) and the expected output (spans of entities).

It's important to note that the correct outputs of the test set won't be accessible to the participants until the challenge ends. Once the challenge ends, the entire corpus, including the solutions for the test set, will be made available to the research community under a suitable license.

## Testing data

The testing data (for evaluation) is available in the `eval/testing` folder. Additionally, fully annotated testing data, separated by source, is now available in the `ref/testing` folder.

The folder `scenario1-main` contains `3000` sentences. The first `2700` are in Spanish and the last `300` are in English. Of these, only `50` in each language will be used for evaluation (the rest are provided to discourage manual annotation), but you must output annotations for all of the sentences, since the reference sentences are shuffled.

Teams that prefer to participate only in one language can ignore the remaining sentences and not output annotations for them. **However**, make sure to respect character positions to maintain the alignment with the reference annotations. For example, if you are ignoring the Spanish sentences, then your first entity annotation should start around char 195,700 which is roughly where English sentences begin.

Folders `scenario2-taskA` and `scenario3-taskB` contains `50` Spanish sentences and `50` English sentences in that order.

In respect of the ethics of the competition, we kindly ask participants **not** to manually review the testing output, beyond the minimum necessary to guarantee there are no implementation errors. Especially, please do not manually annotate any of the test-set sentences, evaluate your predicted annotations, or make any design decision based on perceived performance of the test set. Doing so would incur in overfitting the testing data and diminish the value of the challenge for all participants.

### Download links:

- [🏋️ Training data](https://github.com/ehealthkd/corpora/tree/master/2024/ref/training)
- [🏋️ Develop data](https://github.com/ehealthkd/corpora/tree/master/2024/ref/develop)
<!-- - [🏋️ Testing data (for evaluation)](https://github.com/ehealthkd/corpora/tree/master/2021/eval/testing)
- [🏋️ Testing data (reference)](https://github.com/ehealthkd/corpora/tree/master/2021/ref/testing) -->

## Evaluation and utility scripts

Evaluation scripts will be provided so that participants can test their systems offline with respect to the same metrics used in the challenge. Since participants will not have access to the test gold annotations, their offline performance will need to be evaluated in the development set. This metric will not be exactly the same as the one obtained in the test set, but it should serve for participants to compare different strategies and perform hyper-parameter tunning.

A [utility script](https://github.com/ehealthkd/corpora/tree/master/scripts) to load, manipulate, and save ANN files is provided. Instructions for its use are available at that location.

### **Download links**:

- [🔧 Tools for loading and manipulating ANN files](https://github.com/ehealthkd/corpora/tree/master/scripts/anntools.py)

## Baselines

A simple baseline is released along with the corpus. The baseline source code is freely available as well. The baselines performance on the development and the test set are now published on [a training server](https://competitions.codalab.org/competitions/30333).

Check out [the submission section](/2021/submission) for details on running the baseline and scoring scripts.

### **Download links**:

- [🔧 Baseline implementation](https://github.com/ehealthkd/corpora/tree/master/scripts/baseline.py)
- [🔧 Scoring script](https://github.com/ehealthkd/corpora/tree/master/scripts/score.py)

# Additional resources

Participants may freely use any additional resources they consider necessary to improve their systems, from other corpora (annotated or not), to external knowledge either explicitly (i.e., using knowledge bases) or implicitly (i.e., captured in word embeddings). For the purpose of sharing the results we expect participants to fully disclose everything they use.
However, participants may not manually annotate the test set, since doing so would be in violation of the ethics of the competition. Furthermore, we expect participants to perform all the fine tuning using only the training and development, and then perform one single run in the test set for submission, so that no accidental overfitting occurs in the test set.
