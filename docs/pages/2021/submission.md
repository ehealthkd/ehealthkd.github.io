---
title: Submission
permalink: /2021/submission
layout: "2021"
---

# Submission details

The eHealth-KD 2021 challenge will be evaluated in [Codalab](https://competitions.codalab.org/competitions/30333). We have prepared a baseline implementation to guide you through the submission process. 

## Getting the training data

The first step will be to download the eHealth-KD 2021 corpus, which is stored in a Github repository. If you are using `git`, you can download it directly by cloning the repository:

```bash
$ git clone https://github.com/ehealthkd/corpora.git
```

Alternatively you can navigate to <https://github.com/ehealthkd/corpora>, and you'll find a link to download the whole project as a zip file. Unzip it and you're ready to go.

At this point you should have a `corpora` folder with the following structure:

```bash
$ ls -lh corpora

drwxrwxr-x 4 user user 4,0K mar 26 15:58 2021
-rw-rw-r-- 1 user user 1,1K mar  6 09:35 LICENSE
-rw-rw-r-- 1 user user   90 mar  6 09:35 README.md
drwxrwxr-x 3 user user 4,0K mar  6 09:51 scripts
```

All the data for the 2021 edition is on the `2021` folder. Future editions will be stored in this repository as well in their corresponding folders.

You should see the following folder structure inside `2021`:

```
$ ls -lh corpora/2021

drwxrwxr-x 5 apiad apiad 4,0K mar 26 20:50 eval
-rw-rw-r-- 1 apiad apiad  237 mar  6 09:36 README.md
drwxrwxr-x 5 apiad apiad 4,0K mar 27 07:48 ref
drwxrwxr-x 2 apiad apiad 4,0K mar 27 07:54 submissions
```

The `submissions` folder will contain all submissions once the challenge is over, and is where  we recommend you to store yours. Right now it contains only the baseline submission.

The `ref` folder contains `training`, `develop` and `testing` subfolders, each of which contains (or will contain) the annotated data used in each stage. At this point only the `ref/training` folder has any content.

Inside the `eval` folder you'll find three equally named subfolders (`eval/training`, `eval/develop`, and `eval/testing`) that contain (or will contain) the input and output of the evaluation data separated in the three scenarios described [in the tasks section](/2021/tasks):

```
ls -lh corpora/eval/training

drwxrwxr-x 2 user user 4,0K mar 26 16:15 scenario1-main
drwxrwxr-x 2 user user 4,0K mar 26 16:16 scenario2-taskA
drwxrwxr-x 2 user user 4,0K mar 26 16:17 scenario3-taskB
```

Inside each of these folders you'll find two pairs of files: `input.txt`/`input.ann` and `output.txt`/`output.ann`. The first two correspond to the input for your system in the corresponding scenario (e.g., text only for scenario 1 and 2, but also annotated entities for scenario 3), and the remaining two correspond to the expected outputs.

### Expected workflow

To develop your system, we recommend that you first train on the training data at `ref/training`, and evaluate on `eval/training`. This is a random subset of the same training data, so your system should be able to overfit it.

Afterwards, you can cross-validate on `eval/develop` (once the develop data is released) to reduce overfitting and fine-tune hyperparameters.

Finally, you will submit up to three different runs on the `eval/testing` data, for which you won't have access to the reference output during the competition. Hence, this submission is blind.

> 💡 On your final submission, you are free to train on both `ref/training` and `ref/develop`, and mix and match these two sources as you see fit. We recommend using the `ref/training` for parameter fitting and the `ref/develop` for model selection and hyper-parameter tuning, but you are free to split the data in any way you see fit.

## Running the baseline

Now that we understand the structure of the dataset, let's turn to the `scripts` folder. There you will find a `baseline.py` script that contains a very simple implementation of a entity-relation extraction system. You can use this implementation as starting point to develop your own system, or simply to compare with it.

> 📝 This implementation does no learning at all, it just stores all the annotations seen during training and reproduces them verbatim if it finds the exact same keyphrases during evaluation.
>
> Hence, it should be an easy baseline to surpass when developing your own solution. Furthermore, if you decide not to participate in any given scenario, you can always reuse the baseline implementation for that case.

As a first step we will train the baseline on the `ref/training` data and evaluate in on the `eval/training` data. The baseline implementation is already tailored for the structure of the data and will correctly detect the input and output files.

If you are inside the `corpora` folder, you can run the baseline with:

```
$ python3 scripts/baseline.py \
    --ref 2021/ref/training \
    --eval 2021/eval/training \
    --submit 2021/submissions/baseline/training/run1

Loaded 1500 sentences for fitting.
Training completed: Stored 4635 keyphrases and 8535 relation pairs.
Evaluating on 2021/eval/training/scenario1-main.
Loaded 100 input sentences.
Writing output to 2021/submissions/baseline/training/run1/scenario1-main
Evaluating on 2021/eval/training/scenario2-taskA.
Loaded 100 input sentences.
Writing output to 2021/submissions/baseline/training/run1/scenario2-taskA
Evaluating on 2021/eval/training/scenario3-taskB.
Loaded 100 input sentences.
Writing output to 2021/submissions/baseline/training/run1/scenario3-taskB
```

As the script says, we have loaded 1500 sentences from the training data and stored all the keyphrases and relations there. Then, we have run the inference part on each of three scenarios, and writen the output to the corresponding folders.

Now let's check the output. In the `2021/submissions` folder you'll find a `baseline` folder. Inside there is a `training` folder, and inside a `run1` folder. This means we have created one run for the `eval/training` dataset. Similarly, if you run on the `eval/develop` or `eval/testing` datasets, you should place the output in `2021/submissions/<team>/develop` and `2021/submissions/<team>/testing` respectively. 

Inside you can have **up to three different runs**, `run1`, `run2` and `run3`. Each of these should contain the corresponding `scenario1-main`, `scenario2-taskA` and `scenario3-taskB` folders, with the corresponding `output.txt` and `output.ann` just as the baseline script does. When in doubt, check `2021/submissions/baseline` for reference. You can use the three different runs to submit different systems or different versions of the same system. This is optional, and you can only submit `run1`. The best run for each scenario will be selected during scoring.

## Checking the score

The `score.py` script implements the scoring function used in the challenge. You can run it offline to test your results on the training and/or develop datasets, but keep in mind that the official results are evaluated on the testing datasets, whose reference annotations won't be available until after the challenge is over.

Continuing with our baseline implementation, let's check the score in the `eval/training` dataset.

```
$ python3 scripts/score.py --gold 2021/eval/training --submit 2021/submissions/baseline/training

Scoring scenario 1 on run 1:

correct_A: 644
incorrect_A: 49
partial_A: 0
spurious_A: 626
missing_A: 10
correct_B: 541
spurious_B: 186
missing_B: 109
--------------------
recall: 0.8758
precision: 0.5792
f1: 0.6973

Scoring scenario 2 on run 1:

correct_A: 684
incorrect_A: 40
partial_A: 2
spurious_A: 690
missing_A: 5
--------------------
recall: 0.9371
precision: 0.4838
f1: 0.6381

Scoring scenario 3 on run 1:

correct_B: 674
spurious_B: 90
missing_B: 12
--------------------
recall: 0.9825
precision: 0.8822
f1: 0.9297

Run 2 not found!
Run 3 not found!

scenario1_f1:0.6972639011473962
scenario1_precision:0.5791788856304986
scenario1_recall:0.8758314855875832
scenario1_best:run1
scenario2_f1:0.6380996739636703
scenario2_precision:0.4837570621468927
scenario2_recall:0.9370725034199726
scenario2_best:run1
scenario3_f1:0.929655172413793
scenario3_precision:0.8821989528795812
scenario3_recall:0.9825072886297376
scenario3_best:run1
```

As you can see, we'll get a detailed score for each run and each scenario, plus aggregated metrics. If you want to see even more details, you can add `--verbose`, `--scenarios`, and `--runs` to see specifically which mistakes were made in one particular scenario and run.

```
$ python3 scripts/score.py --gold 2021/eval/training/ --submit 2021/submissions/baseline/training/ --runs 1 --scenarios 3 --verbose

Scoring scenario 3 on run 1:

correct_B: 674
spurious_B: 90
missing_B: 12

===================  SPURIOUS_B  ===================

Relation(from='manchas', to='piel', label='in-place')
Relation(from='tratamiento', to='tratamiento', label='is-a')
Relation(from='problemas', to='problemas', label='is-a')
Relation(from='usted', to='usted', label='same-as')
Relation(from='respiración', to='hijo', label='subject')
...

===================  MISSING_B   ===================

Relation(from='respiración', to='hijo', label='target')
Relation(from='interior', to='cuerpo', label='arg')
Relation(from='órganos', to='interior', label='part-of')
Relation(from='moverse', to='persona', label='subject')
Relation(from='algunas', to='Este', label='causes')
...

--------------------
recall: 0.9825
precision: 0.8822
f1: 0.9297

...
```

> ⚠️ Keep in mind we are training and evaluating on the same set, hence the artificially high results!

## Runing on training and evaluating on develop

Now that the development set is released, you can train your system on the `ref/training` data and evaluate it instead on the `eval/develop`. This should give you a more reasonable idea of the actual performance on an unknown dataset. To test the baseline in this workflow, we just have to change the `--eval` parameter, and `--submit` parameter to output our predictions in a different folder:

```bash
$ python3 scripts/baseline.py \
    --ref 2021/ref/training \
    --eval 2021/eval/develop \
    --submit 2021/submissions/baseline/develop/run1

Loaded 1500 sentences for fitting.
Training completed: Stored 4635 keyphrases and 8535 relation pairs.
Evaluating on 2021/eval/develop/scenario1-main.
Loaded 100 input sentences.
Writing output to 2021/submissions/baseline/develop/run1/scenario1-main
Evaluating on 2021/eval/develop/scenario2-taskA.
Loaded 100 input sentences.
Writing output to 2021/submissions/baseline/develop/run1/scenario2-taskA
Evaluating on 2021/eval/develop/scenario3-taskB.
Loaded 100 input sentences.
Writing output to 2021/submissions/baseline/develop/run1/scenario3-taskB
```

And now we can compute the score on this new set of sentences:

```bash
$ python3 scripts/score.py --gold 2021/eval/develop --submit 2021/submissions/baseline/develop

Scoring scenario 1 on run 1:

correct_A: 211
incorrect_A: 33
partial_A: 37
spurious_A: 394
missing_A: 630
correct_B: 7
spurious_B: 90
missing_B: 844
--------------------
recall: 0.1342
precision: 0.3063
f1: 0.1867

Scoring scenario 2 on run 1:

correct_A: 211
incorrect_A: 33
partial_A: 37
spurious_A: 394
missing_A: 630
--------------------
recall: 0.2519
precision: 0.34
f1: 0.2894

Scoring scenario 3 on run 1:

correct_B: 7
spurious_B: 24
missing_B: 844
--------------------
recall: 0.008226
precision: 0.2258
f1: 0.01587

Run 2 not found!
Run 3 not found!

scenario1_f1:0.1866614048934491
scenario1_precision:0.30634715025906734
scenario1_recall:0.13422247446083996
scenario1_best:run1
scenario2_f1:0.28940731399747793
scenario2_precision:0.34
scenario2_recall:0.2519209659714599
scenario2_best:run1
scenario3_f1:0.015873015873015872
scenario3_precision:0.22580645161290322
scenario3_recall:0.008225616921269096
scenario3_best:run1
```

> 👉 As you might expect, the results from the baseline are significantly worse in new data than on training data.

## Runing on training and evaluating on the test set


Now that the development set is released, you can train your system on the `ref/training` and evaluate it  on the `eval/testing` dataset to perform an official submission. We recommend you to run the baseline first (and even submit it to Codalab) to get an idea for the whole process. Notice that you won't be able to see score until after the competition is finished, but you will get error messages if something is wrong with the submission format. Since the testing scenario is somewhat large, the baseline will take a couple of minutes to finish.

```bash
$ python3 scripts/baseline.py \
    --ref 2021/ref/training \
    --eval 2021/eval/testing \
    --submit 2021/submissions/baseline/testing/run1

Loaded 1500 sentences for fitting.
Training completed: Stored 4635 keyphrases and 8535 relation pairs.
Evaluating on 2021/eval/testing/scenario1-main.
Loaded 3000 input sentences.
Writing output to 2021/submissions/baseline/testing/run1/scenario1-main
Evaluating on 2021/eval/testing/scenario2-taskA.
Loaded 100 input sentences.
Writing output to 2021/submissions/baseline/testing/run1/scenario2-taskA
Evaluating on 2021/eval/testing/scenario3-taskB.
Loaded 100 input sentences.
Writing output to 2021/submissions/baseline/testing/run1/scenario3-taskB
```

## Submitting the results

Once you've checked your results, you'll want to submit them to Codalab to appear in the leaderboard. During the whole challenge we will running a training server where you can test the whole workflow. This training server will evaluate your score on the `eval/training` and `eval/develop` datasets.

> 📝 Once the test set is released, we will open a new server for you to submit the final output that you run on the `eval/testing` dataset.

Navigate to [**eHealth-KD 2021 Development Server**](https://competitions.codalab.org/competitions/30333). You will need to sign-in to Codalab (or register if you haven't). Once in the competition page, look for the "Participate" section, and then the "Submit / View results" subsection. You'll see a big "Submit" button that will prompt you for a .zip file.

To create this zip file, `cd` into your submission folder, and zip its content. For example:

```
cd corpora/2021/submissions/baseline
zip -r submission.zip .
```

Double check that when you open your zip file, the root of the archive contains the `training`, and potentially `develop` and `testing` folders, inside of which you have at least one `run1` folder (or up to three), and inside of these the `scenario*` folders. 

> ⚠️ The root of the archive **must not** contain a single `<team>` folder, but directly the `training` folder and so on. Otherwise the evaluation script will fail. When in doubt, check the `corpora/submissions/baseline/submission.zip` file in the original repository.

Upload this .zip file to Codalab and wait a few seconds. You might need to hit "Refresh" a couple of times.

When you're confident, you can submit to the [**Official Server**](https://competitions.codalab.org/competitions/30830) instead of the development server. 

> ⚠️ **NOTE**: In the **Official Server** you don't need to provide the `training` or `develop` submissions (but you are free to do so as well, the scoring script won't complaint). Only the output inside of `testing` will be considered for the competition.
