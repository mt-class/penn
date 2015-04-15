---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 5 | Crowd-Sourced
active_tab: homework
---
# Ranking Crowd-Sourced Translations

The invention and popularization of crowdsourcing platforms like Amazon's Mechanical Turk and CrowdFlower has led to a revolution in the creation of and analysis content for all academic disciplines. In the case of Machine Translation, crowdsourcing is commonly used to generate translations for languages with small samples of parallel corpora and evaluate translation quality. The value in doing this comes from the fact that crowdsourced tasks are completed by humans, not computers. That said, having untrained workers half a world work on translate documents into languages they might be unfamiliar with can lead to a variety of problems. One of these is picking the best translation from a group of crowdsourced translations. Sometimes you get lucky and get experts, other times you get people who don't even know english. In this project you'll write a program that ranks a group of crowdsourced translations from best to worst given one of three reference translations.

## Relevant Papers and Textbook Sections

See below for a collection of resources that we used to come up with our ideas for this project.
- Textbook Sections:
  - Chapter 8.1: Manual Evaluation
  - Chapter 8.2: Automatic Evaluation
  - Chapter 8.4: Task-Oriented Evaluation
- Lecture Slides:
  - 2/12/15 - Evaluating Translation Quality Part 1
  - 2/17/15 - Evaluating Translation Quality Part 2
- Papers:
  - Workshop on Creating Speech and Language Data with Amazon’s Mechanical Turk: Proceedings of the Workshop by Chris Callison-Burch and Marke Dredze (link: http://bit.ly/18Vh81X)
  - Crowdsourcing Translation: Professional Quality from Non-Professionals by Omar F. Zaidan and Chris Callison-Burch (link: http://bit.ly/1wTKqJU)
  - Fast, Cheap, and Creative: Evaluating Translation Quality Using Amazon’s Mechanical Turk by Chris Callison Burch (link: http://bit.ly/1BLCB89)

## Getting Started

To begin, download the starter kit for the assignment. You may either choose to develop locally or on Penn's servers.
For the latter, we recommend using the Biglab machines, whose memory and runtime restrictions are much less stringent
than those on Eniac. The Biglab servers can be accessed directly using the command `ssh PENNKEY@biglab.seas.upenn.edu`,
or from Eniac using the command `ssh biglab`.

In the downloaded directory, you will find a Python program called "default", which is an implementation of a simple translation ranker. It ranks the translations in the order they were given in turker_translations.tsv. It can be run as follows:
```
./default > default.out
```

The grading program we have provided, "grade", compares the rankings of the translations in an output file to rankings of the translations ranked according to BLEU score when given the entire set of reference translations. The number output can be considered a percentage of the scores that were correct. The grading program can be run as follows:
```
./grade < _____.out
```

## The Challenge

Your challenge for this assignment is to rank the translation as accurately as possible. You haven't been given the entire set of reference translations so it's probably impossible that you get a score of 1.000... However, implementing a system that makes use of the entire reference corpus should easily put you above the baseline. We've listed some ideas for how you could rank the translations below:
- Un-stemmed Levenshtein Distance
- Stemmed Levenshtein Distance
- Sentence-level BLEU
- Sentence-level n-gram count
- Corpus-level n-gram count
- Corpus-level bleu

## Ground Rules

- You must work on this assignment **alone**.
- You must turn in three things:
  1.  Your ranking of `turker_translations.tsv` Upload your results with the command `turnin -c cis526 -p hw5 hw5.txt` from any Eniac or Biglab machine. You can upload new output as often as you like, up until the assignment deadline.

  2.  Your code, uploaded using the command `turnin -c cis526 -p hw5-code file1 file2 ....` This is due 24 hours after the leaderboard closes. You are free to extend the code we provide or write your own in whatever langugage you like, but the code should be self-contained, self-documenting, and easy to use.

  3.  A report describing the models you designed and experimented with, uploaded using the command `turnin -c cis526 -p hw5-report hw5-report.pdf`. This is due 24 hours after the leaderboard closes. Your report does not need to be long, but it should at minimum address the following points:
    - **Motivation**: How and why did you choose the models you experimented with?
    - **Description of models or algorithms**: Describe mathematically or algorithmically what you did. Your descriptions should be clear enough that someone else in the class could implement them.

    - **Results**: You most likely experimented with various settings of any models you implemented. We want to know how you decided on the final model that you submitted for us to grade. What parameters did you try, and what were the results? Most importantly: what did you learn?

  Since we have already given you a concrete problem and dataset, you do not need describe these as if you were writing a full scientific paper. Instead, you should focus on an accurate technical description of the above items.

  Note: These reports will be made available via hyperlinks on the leaderboard. Therefore, **you are not required to include your real name** if you would prefer not to do so.

This document was modeled after the homework assignments displayed on the CIS 526 homepage. That page was originally authored by Chris Callison-Burch, Mitchell Stern, and Justin Chiu.
