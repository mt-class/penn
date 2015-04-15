---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 5 | Crowd-Sourced
active_tab: homework
---
# Term Project Writeup

CIS 526: Machine Translation (Spring 2015)

Christian Barcenas, David Liao, Yesha Ouyang, Fifi Yeung

## Ranking Crowd-Sourced Translations: Homework 5

### Problem Definition
Given a set of crowd-sourced translations for a corpus, how do you
automatically choose the best translations?

### Motivation
Crowdsourcing is the process of procuring services and content by subdividing
and contracting a project out to a large, diverse group of people, rather than
from traditional employees or organizations. The rise of global online
communities and the creation of platforms such as Amazon Mechanical Turk has
played a major role in the increasing popularity of crowdsourcing tasks that
are economically infeasible or computationally intractable to automate.

Compared to hiring professional translators, crowdsourcing is a very
economically-friendly method of producing a set of translations. Depending on
the target language and the degree of redundancy necessary, the cost of
generating parallel corpora can often be an order of magnitude or lower using
Mechanical Turk.

However, Amazon offers little opportunity to screen workers before they are
selected for the task. Without external quality control, naive crowdsourcing
results in disfluent, low-quality results that do not differ significantly
from machine output. Some people will attempt to game the system (e.g. by using
an existing machine translation system like Google Translate), and others will
simply be insufficiently qualified to produce translations.

As a result of these concerns, the evaluations that subsequently provide
reference tagged data primarily come from human experts. These evaluations can
be used for a variety of purposes, including training and evaluating SMT
models. If good-quality crowd-sourced translations could be extracted, then
this provides a new avenue for cheaply producing reference data.

In homework 3, you have learned of metrics to objectively measure the quality
of machine-produced translations. Armed with this knowledge, you will be given
sentences in Urdu and a set of Turker-translated English sentences, as well as
feature data from the Turkers themselves. Your tasks is to create a system to
automatically filter out the garbage (i.e. low-quality and/or bad-faith)
translations and choose the best individual sentences.

### Getting Started
To begin, download the Homework 5 starter kit. You may either choose to develop
locally or on Penn’s servers. For the latter, we recommend using the Biglab
machines, whose memory and runtime restrictions are much less stringent than
those on Eniac. The Biglab servers can be accessed directly using the command
`ssh PENNKEY@biglab.seas.upenn.edu`, or from Eniac using the command
`ssh biglab`. You will also require the SciKit as a dependency.

In the starter kit, you will find three raw data files: `reference.p`, `translations.p`, and `evaluator_features.p`. These pickle files contain the crowdsourced and reference translations, the crowdsourced evaluations of these translations, and other identifying features about the evaluators such as geographic data. You may use this data to generate features for your own models.  Additionally, you will find a text file `translator_features.tsv` containing feature data about the translators. Please note that `data-train` and `data-dev` are folders with the same exact data.

Specifically, each pickle data file contains a dictionary keyed by `seg_id`, the unique string identifier for an Urdu sentence:

  * The value of each key in `reference.p` is a dictionary of:
    * ‘`f`’: foreign sentence
    * ‘`e`’: a list of the four reference english translations

  * The value of each key in `translations.p` is a dictionary of:
    * ‘`inputs`’: a 4-best list of the candidate translations for which you will be choosing the best single candidate
    * ‘`input_translators`’: a list of the uids of the Turkers that translated the sentences in `inputs`
    * ‘`votes`’: a list of (`evaluator_uid`, `best_translator_uid`) tuples
    * The `evaluator_uid` is the uid of the Turker that evaluated the given 4-best list of input sentences, and the `best_translator_uid` is the uid that the evaluator Turker decided as the best translator, i.e. one of the four uids in `input_translators`.

  * The value of each key in `evaluator_features.p` is a dictionary of:
    * ‘`time_taken`’: the time taken by the evaluator in seconds
    * ‘`country`’: the evaluator’s country of origin
    * ‘`display_language`’: the evaluator’s display language

We have provided a default program that chooses a translation for each sentence
from a list of candidates.

    ./default > english.out

This ranker simply picks the first translation out of each of the candidates.
To evaluate these translations on the development set, you can compute the BLEU
score against their reference translations. As you know, the BLEU score is a
metric for evaluating the quality of translated from one language to another,
where quality is considered the correlation between the candidate output and
the output of a human reference.

For this assignment, we include four human references in the training data, and
the grade function that you will be scored on outputs the weighted average of
the four BLEU scores.

    ./grade < english.out

What’s the best you could you do by picking other sentences from the list? To give you an idea, we’ve given you an oracle for the development data. Using knowledge of the reference translation, it chooses candidate sentences that maximize the BLEU score.

    ./oracle | ./grade

The oracle should convince you that it is possible to do much better than the default reranker. Maybe you can improve it by by learning some features from the crowdsourced evaluations
of the translations. Try a few different settings. How close can you get to the oracle BLEU score?


### The Challenge
Your task is to select the best sentences that *improve the corpus level BLEU
score as much as possible* (higher is better).

For baseline, you can implement a weighted majority vote to choose the best
translation. You could choose the majority vote vectors by hand, or you could
employ machine learning techniques you have studied in the class. Features on
the translator should be enough to help learn the weight vector.

Consider using features such as the time it took the evaluator to complete the
task, default language, and region. Implementing this learning algorithm should
be enough to beat baseline, however there is certainly room for improvement.

Here are some ideas:

  - Use sentence level features we have studied throughout the class to
    discriminate good translations from bad translations (1)

  - For each sentence-level feature, compute a corresponding feature over all
    of the respective Turker’s translations (1)

  - Use a binary classifier to best train weights for the feature vector. Some ideas include Powell    method, Hill climb, and Gradient ascent, but feel free to use any other method you are familiar with.

  - Remove or penalize translations that may have originated from statistical
    translation systems like Google Translate (2)

  - Prioritize translators whose translations generally correlate with expert
    translations (3)

  - Incorporate ranking data from a separate Mechanical Turk task (1)

(1) [Crowdsourcing Translation: Professional Quality from Non-Professionals](http://aclweb.org/anthology/P/P11/P11-1122.pdf)

(2) [The Language Demographics of Amazon Mechanical Turk](http://www.cis.upenn.edu/~ccb/publications/language-demographics-of-mechanical-turk.pdf)

(3) [Fast, Cheap, and Creative: Evaluating Translation Quality Using Amazon’s Mechanical Turk](http://aclweb.org/anthology//D/D09/D09-1030.pdf)

### Addendum: Data
Students will be given two data files, taken from
[Zaidan and CCB’s paper](http://aclweb.org/anthology/P/P11/P11-1122.pdf). The
`formatted.txt` file stores the raw evaluation features and Mechanical
Turk metadata that students will parse. The second file,
`translations_train.tsv`, contains the reference translations from the
LDC, and the original sentences in Urdu. The LDC translations are for the
students’ reference for grade, and consist of 700 lines, only covering roughly
half of the corpus.

On the grading side, we have `translations_test.tsv`, which contains
all 1456 lines of LDC translations so we can evaluate the students’ performances
on the leaderboard. They will only be graded on the last 746 lines. This is to
prevent them from overfitting or cheating if they use machine learning methods
or train against the BLEU score.

#### Ground Rules
  - You must work independently on this assignment.
  - You must turn in three things:
    - Your best translations selected from translations_train.tsv. Upload
      your results with the command `turnin -c cis526 -p hw5 hw5.txt`
      from any Eniac or Biglab machine. You can upload new output as often
      as you like, up until the assignment deadline. You will only be able to
      see your results on test data after the deadline.
    - Your code, uploaded using the command `turnin -c cis526 -p hw5-code file1 file2 ....`
      This is due 24 hours after the leaderboard closes. You are free to extend
      the code we provide or write your own in whatever language you like, but
      the code should be self-contained, self-documenting, and easy to use.
    - A report describing the models you designed and experimented with,
      uploaded using the command `turnin -c cis526 -p hw5-report hw5-report.pdf`.
      This is due 24 hours after the leaderboard closes. Your report does not
      need to be long, but it should at minimum address the following points:
        - *Motivation*: Why did you choose the models you experimented with?
        - *Description of models or algorithms*: Describe mathematically or
          algorithmically what you did. Your descriptions should be clear
          enough that someone else in the class could implement them.
        - *Results*: You most likely experimented with various settings of
          any models you implemented. We want to know how you decided on the
          final model that you submitted for us to grade. What parameters did
          you try, and what were the results? Most importantly: what did you
          learn?
    - Since we have already given you a concrete problem and dataset, you do
      not need describe these as if you were writing a full scientific paper.
      Instead, you should focus on an accurate technical description of the
      above items.
    - Note: These reports will be made available via hyperlinks on the
      leaderboard. Therefore, *you are not required to include your real name*
      if you would prefer not to do so.
  - You do not need any other data than what we provide. You are free to use
    any code or software you like, *except for those expressly intended to
    generate or rerank translation output*. You must write your own reranker.
    If you want to use machine learning libraries, taggers, parsers, or any
    other off-the-shelf resources, feel free to do so. If you aren’t sure
    whether something is permitted, ask us. If you want to do system
    combination, join forces with your classmates.


