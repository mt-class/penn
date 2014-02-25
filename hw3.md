---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 3 | Evaluation
active_tab: homework
---

<div class="alert alert-info">
This homework is due Thursday March 6th, 11:59pm.
</div>

Evaluation:  <span class="text-muted">Challenge Problem 3</span>
=============================================================

Machine translation systems are typically evaluated through relative
ranking. For instance, given the following German sentences:

*Die Prager Börse stürzt gegen Geschäftsschluss ins Minus.*

*Nach dem steilen Abfall am Morgen konnte die Prager Börse die Verluste korrigieren.*

...One machine translation system produces this output:

*The Prague stock exchange throws into the minus against closing time.*

*After the steep drop in the morning the Prague stock exchange could correct the losses.*

...And a second machine translation system produces this output:

*The Prague stock exchange risks geschäftsschluss against the down.*

*After the steilen waste on the stock market Prague, was aez almost half of the normal tagesgeschäfts.*

A plausible ranking of the translation systems would place the first
system higher than the second. While neither translation is perfect, the 
first one is clearly easier to understand and conveys more of the original
meaning. *Your challenge is to write a program that ranks the systems in 
the same order that a human evaluator would.*

There is great need to evaluate translation systems: to 
decide whether to purchase one system or another, or to assess incremental
changes to a system --- since, as you saw in the first two assignments,
there are many choices in the design of a system that naturally lead to
different translations. Ideally such comparisons between systems should be 
done by humans, but human rankings are slow and costly to obtain, making 
them less feasible when comparisons are must be done frequently or between 
large numbers of systems. Furthermore, as you saw in class, it is possible
to use machine learning techniques to directly optimize machine translation 
towards an objective function. If we could devise a function that 
correctly ranked systems, we could, in principle achieve better translation
through optimization. Hence automatic evaluation is a topic of intense study.
 
## Getting Started

Under the <tt>evaluate</tt> directory, we have provided you with a very 
simple evaluation program written in Python. There is also a directory containing
a development dataset and a test dataset. Each dataset consists of 
a human translation and many machine translations of some German documents.
The evaluator compares each machine translation to a human 
*reference translation* sentence by sentence, computing how many words 
they have in common. It then ranks the machine translation systems according 
to the percentage of words that also appear in the reference. Note that while we
collect statistics for each sentence, and the best system on each sentence
will vary, the final ranking is at the system level.
Run the evaluator on the development data using this command:

<tt>evaluate &gt; output</tt>

This runs the evaluation and stores the final ranking in 
<tt>output</tt>. You can see the rank order of the systems simply by looking
at the file &mdash; the first line is the best system, the second is second
best, and so on. To calculate the correlation between this ranking and a 
human ranking of the same set of systems, run the command:


<tt>grade &lt; output</tt>

This command computes 
<a href="http://en.wikipedia.org/wiki/Spearman%27s_rank_correlation_coefficient">Spearman's rank correlation coefficient</a>
(rho) between the automatic ranking
and a human ranking of the systems (see Section 4 of 
<a href="http://aclweb.org/anthology-new/W/W11/W11-2103.pdf">this paper</a> for an explanation
of how the human rankings were obtained). A rho of 1 means that the rankings
are identical; a rank of zero means that they are uncorrelated; and a negative
rank means that they are inversely correlated.

You should also rank the test data, for which we did not provide human
rankings. To do this, run the command:

<tt>evaluate -d data/test &gt; output</tt>

You can confirm that the output is a valid ranking of the test data 
using the check command:

<tt>check &lt; output</tt>

Your ranking should be a total ordering of the systems &mdash; ties are not allowed.

## The Challenge

Improving the evaluation algorithm should cause rho
to increase. Your task for this assignment is to <b>obtain a
Spearman's rank correlation coefficient that is as high as possible on the 
test data.</b> Whoever obtains the highest rho will receive the most 
points. 


One way to improve over the default system is to implement the 
well-known <a href="http://aclweb.org/anthology-new/P/P02/P02-1040.pdf">BLEU</a>
metric. You may find it useful to experiment with BLEU's parameters,
or to retokenize the data in some way. However, there are many, many 
alternatives to BLEU &mdash; the topic of evaluation is so popular that
Yorick Wilks, a well-known researcher, once remarked that  
*more has been written about machine translation evaluation than about
machine translation itself*. Some of the techniques people have tried
may result in stronger correlation with human judgement, including:

<ul>
  <li><a href="http://aclweb.org/anthology-new/W/W11/W11-2105.pdf">Incorporating recall statistics into the metric.</a></li>
  <li><a href="http://aclweb.org/anthology-new/W/W07/W07-0734.pdf">Stemming the words, or counting synonyms as matches.</a></li>
  <li><a href="http://aclweb.org/anthology-new/W/W11/W11-2112.pdf">Analyzing predicate-argument structure and distributional semantics of the translations</a>.</li>
  <li><a href="http://aclweb.org/anthology-new/W/W11/W11-2106.pdf">Combining many of these features with machine learning techniques</a> (you could train on the development data).</li>
  <li><a href="http://aclweb.org/anthology-new/W/W11/W11-2113.pdf">Combining only simple features with machine learning</a>.</li>
</ul>

But the sky's the limit! There are many, many ways to automatically evaluate
machine translation systems, and
you can try anything you want as long as you follow the ground rules:

## Ground Rules

<ul>
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
  <ol>
  <li>
  Your ranking of the systems in the test dataset, uploaded to BASE_URL/assignment3.txt
  following the <a href="assignment0.html">Assignment 0 instructions</a>. 
  You can upload new output as often as you like. Your rankings will be
  evaluated using a hidden metric. However, the 
  <tt>grade</tt> program will give you a good indication of how you're doing.
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
  What did you learn? Do you think that this is a reasonable way to evaluate
  machine translation systems? Please post your 
  response within two days of submitting your final solution to the 
  leaderboard; we will withold your grade until we receive it.
  </li>
  </ol>
</li>
<li>
   You should feel free to use additional data resources such as thesauruses, 
   WordNet, or parallel data. You are also free to use additional codebases and libraries 
   <b>except for those expressly intended to evaluate machine translation 
   systems</b>. You must write your own evaluation metric. However, if you 
   want your evaluation to depend on lemmatizers, stemmers, automatic parsers,
   or part-of-speech taggers, or you would like to <i>learn</i> a metric using 
   a general machine learning toolkit, that is fine. But translation metrics including 
   (but not limited too) available implementations of BLEU, METEOR, TER, 
   NIST, and others are not permitted. You may of course inspect these systems
   if you want to understand how they work, although they tend to include
   other functionality that is not the focus of this assignment.
   It is possible to complete the assignment with a very modest amount
   of python code. If you aren't sure whether something is permitted, ask us.
</li>
</ul>

If you have any questions or you're confused about anything,
<a href="https://piazza.com/upenn/spring2014/cis526/home">just ask</a>.

