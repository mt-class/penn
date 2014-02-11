---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 2 | Decoding
active_tab: homework
---

<div class="alert alert-info">
This homework is due Thursday February 20th, 11:59pm.
</div>

Decoding:  <span class="text-muted">Challenge Problem 2</span>
=============================================================

Decoding is process of taking input that looks like this:

*honorables sénateurs , que se est - il passé ici , mardi dernier ?*

...And turning into output that looks like this:

*honourable senators , what happened here on Tuesday ?*


In order to decode we need a probability model over pairs
of English and French sentences. You did most of the work of creating
such a model in [Challenge 1](hw1.html). In this assignment,
we will give you a fixed model consisting of a (phrase-based) translation
model and a language model. *Your challenge is to find the most 
probable translation.*

## Getting Started


Under the <tt>decode</tt> directory, we have provided you with a very 
simple decoder written in Python (v.2.6-2.7; older versions will not work!) 
The decoder translates monotonically &mdash; that is, without reordering the
English phrases &mdash; and by default it also uses very strict pruning limits, 
which you can vary on the command line.
There is also a data directory containing a translation model, a language 
model, and a set of input sentences to translate. Run the decoder using this 
command:


<tt>decode &gt; output</tt>

This loads the models and decodes the input sentences, storing the
result in <tt>output</tt>. You can see the translations simply by looking
at the file. To calculate their true model score, run the command:


<tt>grade &lt; output</tt>

This command computes the probability of the output sentences
according to the model. It works by summing over all possible ways that
the model could have generated the English from the French. In general this
is intractable, but because the phrase dictionary is fixed and sparse, the
specific instances here can be computed in a few minutes. It is still
easier to do this exactly than it is to find the optimal translation.
In fact, if you look at the grade script you may get some hints about 
how to do the assignment!

Improving the search algorithm in the decoder &mdash; for instance by enabling
it to search over permutations of English phrases &mdash; should permit you 
to find more probable translations
of the input French sentences than the ones found by the baseline system. 
This assignment differs from Homework 1, in that there
is no hidden evaluation measure.
The <tt>grade</tt> program will tell you the probability of your output, and
whoever finds the most probable output will receive the most points.

## The Challenge

Your task for this assignment is to <b>find the English sentence
with the highest possible probability</b>.
Formally, this means your goal is to solve the problem:
\( \mathop{\arg\,\max}\limits_e~ p(f|e) \times p(e) \), where \(f\) is a
French sentence and \(e\) is an English sentence. In the model we have 
provided you, \( p(e) = p(e_1|START) \times \prod_{j=2}^J p(e_j|e_{j-1}e_{j-2}) \times p(END|e_J,e_{J-1}) \)
and \( p(f|e) = p(segmentation) \times p(reordering) \times p(phrase~translation) \).
We will make the simplifying assumption that segmentation and reordering
probabilities are uniform across all sentences, and hence constant. This results
in a model whose probability density function does not sum to one. But from a
practical perspective, it slightly simplifies the implementation without
substantially harming empirical accuracy. This means that you only need
consider the product of the phrase translation probabilities 
\( p(f|e) = \prod_{\langle i,i',j,j' \rangle \in a} p(f_{\langle i,i' \rangle}|e_{\langle j,j' \rangle}) \)
where \( \langle i,i' \rangle \) and \( \langle j,j' \rangle \) index phrases
in \(f\) and \(e\), respectively.

Unfortunately, even with all of these simplifications, finding the most
probable English sentence is completely intractable! To compute it
exactly, for each English sentence you would need to compute \( p(f|e) \) 
as a sum over all possible alignments with the French sentence:
\( p(f|e) = \sum_a p(f,a|e) \). A nearly universal approximation is to
instead search for the English string together with a single alignment, 
\(\mathop{\arg\,\max}\limits_{e,a}~ p(f,a|e) \times p(e) \).
This is the approach taken by the monotone baseline decoder.

Since this involves multiplying together many small probabilities, it is 
helpful to work in logspace to avoid numerical underflow. We instead solve for
\(e,a\) that maximizes:
\( \log p(f,a|e) + \log p(e) = 
\log p(e_1|START) + \sum_{j=2}^J \log p(e_j|e_{j-1}e_{j-2}) + \log p(END|e_J) +
\sum_{\langle i,i',j,j' \rangle \in a} \log p(f_{\langle i,i' \rangle}|e_{\langle j,j' \rangle})
\). 
The baseline decoder already works with log probabilities, so it is 
not necessary for you to perform any additional conversion; you can simply work
with the sum of the scores that the model provides for you. Note that
since probabilities are always less than or equal to one, their equivalent 
values in logspace will always be negative or zero, respectively (you may
notice that <tt>grade</tt> sometimes reports positive translation model 
scores; this is because the sum it computes does not include 
the large negative constant associated 
with the log probabilities of segmentation and reordering). Hence your
translations will always have negative scores, and you will be looking for
the one with the <b>smallest absolute value</b>. In other words, we have
transformed the problem of finding the most probable translation into a 
problem of finding the shortest path through a large graph of possible
outputs.

Under the phrase-based model we've given you, the goal is to 
find a phrase segmentation, translation of each resulting phrase,
and permutation of those phrases such that the product of the phrase
translation probabilities and the language model score of the resulting
sentence is as high as possible. Arbitrary permutations of the English 
phrases are allowed, provided that the phrase translations are one-to-one and
exactly cover the input sentence. Even with all of the simplifications we
have made, this problem is <i>still</i>
NP-Complete, so we recommend that you solve it using an approximate
method like stack decoding, discussed in Chapter 6 of the textbook. You 
can trade efficiency for search effectiveness
by implementing histogram pruning or threshold pruning, or by playing around
with reordering limits as described in the textbook. Or, you might
consider implementing other approaches to solving the search problem:

<ul>
  <li><a href="http://www.iro.umontreal.ca/~felipe/bib2webV0.81/cv/papers/paper-tmi-2007.pdf">Implement a greedy decoder</a>.</li>
  <li><a href="http://aclweb.org/anthology-new/P/P09/P09-1038.pdf">Reduce the problem to a traveling salesman problem (TSP) and decode using an off-the-shelf TSP solver</a>.</li>
  <li><a href="http://mi.eng.cam.ac.uk/~wjb31/ppubs/ttmjnle.pdf">Reformulate the problem as a shortest-path problem through a finite-state lattice and decode using finite-state devices</a>.</li>
  <li><a href="http://aclweb.org/anthology-new/D/D11/D11-1003.pdf">Decode using Lagrangian relaxation</a> (often finds exact solutions!)</li>
</ul>

Several techniques used for the IBM Models (which have very similar 
search problems as phrase-based models) could also be adapted:
<ul>
  <li><a href="http://aclweb.org/anthology-new/W/W01/W01-1408.pdf">Implement A* Search</a>.</li>
  <li><a href="http://aclweb.org/anthology-new/C/C02/C02-1050.pdf">Implement a bidirectional decoder</a>.</li>
  <li><a href="http://aclweb.org/anthology-new/N/N09/N09-2002.pdf">Decode using an integer linear programming (ILP) solver</a>.</li>
</ul>

But the sky's the limit! There are many, many ways to try to solve the decoding
problem, and you can try anything you want as long as you follow the ground rules:


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
  Your translations of the entire dataset, uploaded to BASE_URL/assignment2.txt
  following the <a href="assignment0.html">Assignment 0 instructions</a>. 
  You can upload new output as often as you like. There is no hidden metric
  for this assignment; the <tt>grade</tt> program will tell you exactly
  how probable your output it. Whoever has
  the most probable output at the deadline will receive the most bonus points.
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
  What did you learn? What did you think about the quality of the translations?
  Do you think that more probable translations are better? Please post your 
  response within two days of submitting your final solution to the 
  leaderboard; we will withold your grade until we receive it.
  </li>
  </ol>
</li>
<li>
   You do not need any other data than what is provided. You should feel 
   free to use additional codebases and libraries <b>except for those
   expressly intended to decode machine translation models</b>. 
   You must write your
   own decoder. If you would like to base your solution on finite-state
   toolkits or generic solvers for traveling salesman problems or
   integer linear programming, that is fine. 
   But machine translation software including (but not limited to)
   Moses, cdec, Joshua, or phrasal is off-limits. You may of course inspect 
   these systems if you want to understand how they work. But be warned: they are
   generally quite complicated because they provide a great deal of other
   functionality that is not the focus of this assignment.
   It is possible to complete the assignment with a quite modest amount
   of python code.
   If you aren't sure whether 
   something is permitted, ask us.
</li>
</ul>
If you have any questions or you're confused about anything,
<a href="https://piazza.com/upenn/spring2014/cis526/home">just ask</a>.

