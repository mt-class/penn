---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 4 | Reranking
active_tab: homework
---

<div class="alert alert-info">
This homework is due Thursday March 27th, 11:59pm.
</div>

Reranking:  <span class="text-muted">Challenge Problem 4</span>
=============================================================

In [Assignment 2](hw2.html) you saw that search could find
more probable translations, but this is most useful when you have a good model.
In [Assignment 3](hw3.html) you saw that it was possible
to create a metric that correlated (at least somewhat) with human assessments
of machine translation systems. Armed with such a metric, you are now in
a position to quantitatively evaluate machine translation output, which is 
an important step to creating a better model. In this assignment,
you will be given a set of N-best candidate translations for each sentence
of a French test set, and you will create a model to choose the best one.
*Your goal is to improve the translation quality by choosing better
translations.*

## Getting Started

<div class="alert alert-info">
The original data for this homework is unfair: the nbest lists are in the order
that the MT system output them, which is a strong signal for quality.
See Section 6 of <a href="http://www.transacl.org/wp-content/uploads/2013/05/paper165.pdf">this paper</a>.

<b>Do not use the given nbest lists; download the shuffled lists
<a href="hw4-shuffled-nbest.tgz">here</a>.</b>
</div>

Under the <tt>rerank</tt> directory, we have provided you with a very 
simple reranking program written in Python and a few utility programs. 
There is also a directory containing training and test datasets. Each dataset 
consists of many alternative machine translations for each sentence of the 
input data. To see the available translations of the 500 French sentences in
<tt>data/test.fr</tt>, take a look at the file
<tt>data/test.nbest.shuffled</tt>. Each line contains a sentence number, a translation,
and a set of feature values for that translation that were provided by a
state-of-the-art translation model. The goal of the reranking program is to choose the
best translation from among the many alternatives in the file. By default, 
the reranker weights all of the feature values uniformly and scores each 
candidate by their weighted sum. It then outputs the candidate with the 
minimum cost. To run it, type:

<tt>rerank &gt; output</tt>

This runs the reranker and stores the set of sentences that it deems to be
best in the file named <tt>output</tt>. We have additionally provided you with
a program that calculates the BLEU score of the first 250 sentences of the 
test set from a set of human reference translations in <tt>data/test.en</tt>. 
Run this command:

<tt>grade &lt; output</tt>

It is possible to do much better than the 
default reranker! To see this, run the command:

    oracle | grade

The oracle uses the human reference translations to choose sentences from
the N-best list that yield the highest BLEU score. The algorithm that does
this is approximate, because actually finding the highest BLEU score is 
intractable (See footnote 8 of 
<a href="http://www.mt-archive.info/AMTA-2006-Lopez.pdf">this paper</a>
for an explanation). But even though this is not a true upper bound on the BLEU
score, you can see that there is quite a lot of room for improvement.

Of course, you will not be able to submit oracle translations for the
assignment, because you don't have reference translations for the last 250 
sentences of the test set. So you will need to develop a way to choose 
better sentences from the N-best list without access to a reference. To help
you in this task, we have provided you with some training data (which includes
both N-best lists and reference sentences) and a very simple implementation
of the *pairwise ranking optimization* (PRO) algorithm, using a
<a href="http://en.wikipedia.org/wiki/Perceptron">perceptron</a> as its underlying classifier.
PRO attempts to optimize translation quality towards BLEU
score, the metric on which we evaluate. It produces a vector of weights for the
features on the translations in the N-best list. The reranker can read this
weight vector with the <tt>-w</tt> option. It takes the dot
product of the weight vector and the feature values to rank the candidate 
translations, choosing the best one for each sentence. To run this
process, type:

    learn | rerank -w - | grade

You should observe that the BLEU score improves slightly. The <tt>learn</tt>
program contains several parameters that you can optionally vary, which may
improve accuracy.

## The Challenge

Improving the model should cause the BLEU score to increase even more. 
Your task for this assignment is to *improve the BLEU score
as much as possible, subject to the constraint that your translations
must come from the N-best list.*
 While it is possible to do even better by considering candidate
translations that are not in the N-best list, for this task you should 
focus on simply choosing the best translation from among the ones we
have given you, rather than attempting to generate new candidates.
Whoever obtains the highest BLEU score will receive the most points.

There are various ways that you might improve on the default system.
It is quite likely that simply varying some of the parameters to the PRO
algorithm will yield an improvement. However, there are many other ways
that you might improve the reranker. These might include:

<ul class="real">
<li><a href="http://aclweb.org/anthology-new/W/W08/W08-0302.pdf">Adding new, informative features!</a></li>
<li><a href="http://aclweb.org/anthology-new/P/P03/P03-1021.pdf">
Implementing Och's minimum error rate algorithm</a> 
(Note: there are many alternative descriptions of this algorithm, 
e.g. Algorithm 1 of 
<a href="http://www.cs.jhu.edu/~alopez/papers/survey.pdf">this survey paper</a>).
</li>
<li>...Or one of its <a href="http://aclweb.org/anthology-new/D/D11/D11-1004.pdf">many</a> 
<a href="http://aclweb.org/anthology-new/W/W08/W08-0304.pdf">variants</a>
</li>
<li>Replacing the underlying classifier used by PRO.</li>
</ul>

But the sky's the limit! There are many, many ways that you can improve
the performance of the baseline translation system, and you can try anything 
you want as long as you follow the ground rules:

## Ground Rules

<ul class="real">
<li>
   You may work in independently or in groups of any size, under these 
   conditions: 
   <ol>
   <li>
   You must notify us by posting a public note to piazza.
   </li>
   <li>
   Everyone in the group will receive the same grade on the assignment. 
   </li>
   <li>
   You can add people or merge groups at any time before you post your
   final submission. HOWEVER, you cannot drop people from your group once 
   you've added them. Collaboration is fine with us.
   </li>
  </ol>
</li>
<li> You must turn in three things:
  <ol class="real">
  <li>
  Your translations of the test data (selected from the candidates in the 
  N-best lists we provided), uploaded to BASE_URL/assignment4.txt
  following the <a href="assignment0.html">Assignment 0 instructions</a>. 
  You can upload new output as often as you like. Your translations will be
  evaluated using a hidden metric. However, the 
  <tt>grade</tt> program will give you a good indication of how you're doing,
  and the <tt>check</tt> program will verify that your output is correctly
  formatted for our hidden evaluation.
  Whoever has the highest score on the leaderboard at the assignment 
  deadline will receive the most bonus points.
  </li>
  <li>
  Your code. Send us a URL from which we can get the code and git revision
  history (a link to a tarball will suffice, but you're free to send us a 
  github link if you don't mind making your code public). This is due at the
  deadline: when you upload your final answer, send us the code.
  You are free to extend the code we provide or roll your own in whatever
  langugage you like, but the code should be self-contained, 
  self-documenting, and easy to use. 
  </li>
  <li>
  A description of your algorithm, posted to piazza. 
  Brevity is encouraged, as long as it is clear what you did; a paragraph or 
  even bullet points is fine. You should tell us not only about your final 
  algorithm, but also things that you tried that didn't work, experiments that
  you did, or other interesting things that you observed while working on it.
  What did you learn? Please post your 
  response within two days of submitting your final solution to the 
  leaderboard; we will withold your grade until we receive it.
  </li>
  </ol>
</li>
<li>
   You should feel free to use additional data and utilities that you think
   will be useful. If you want to experiment with features based on parsers
   and taggers, go ahead. If you want to use different machine learning 
   algorithms based on available toolkits, feel free. 
   It is possible to complete the assignment with a very modest amount
   of python code. If you aren't sure whether something is permitted, ask us.
</li>
</ul>
If you have any questions or you're confused about anything, 
<a href="https://piazza.com/upenn/spring2014/cis526/home">just ask</a>.

