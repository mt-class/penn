---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 3 | Evaluation
active_tab: homework
---

<div class="alert alert-info">
  Leaderboard submission due 11:59 pm, Thursday, March 5th.
  Code and report due 11:59 pm, Friday, March 6th.
</div>

Evaluation:  <span class="text-muted">Homework 3</span>
=============================================================

Automatic evaluation is a key problem in machine translation. Suppose that we have two
machine translation systems. On one sentence, system A outputs:

*This type of zápisníku was very ceněn writers and cestovateli*.

And system B outputs:

*This type of notebook was very prized by writers and travellers*.

We suspect that system B is better, though we don’t necessarily know that its translations
of the words *zápisníku*, *ceněn*, and *cestovateli* are correct. But suppose that we also
have access to the following reference translation.

*This type of notebook is said to be highly prized by writers and travellers*.

We can easily judge that system B is better. **Your challenge is to write a program that
makes this judgement automatically**.
 
## Getting Started

To begin, download the [Homework 3 starter kit](http://seas.upenn.edu/~cis526/hw3.zip).
You may either choose to develop locally or on Penn's servers. For the latter, we
recommend using the Biglab machines, whose memory and runtime restrictions are much
less stringent than those on Eniac. The Biglab servers can be accessed directly
using the command `ssh PENNKEY@biglab.seas.upenn.edu`, or from Eniac using the
command `ssh biglab`.

In the downloaded directory, you now have simple program that decides which of two
machine translation outputs is better. Test it out using this command:

<pre>evaluate &gt; output</pre>

This program uses a very simple evaluation method.
Given machine translations $$h_1$$ and $$h_2$$ and reference translation $$e$$,
it computes $$f(h_1,h_2,e)$$ as follows, where $$\ell(h,e)$$ is the count of words in $$h$$
that are also in $$e$$.

$$f(h1,h2,e)=\left\{\begin{array}{ll}
  1 & \mbox{if } \ell(h_1,e) > \ell(h_2,e)\\
  0 & \mbox{if } \ell(h_1,e) = \ell(h_2,e)\\
  -1 & \mbox{if } \ell(h_1,e) < \ell(h_2,e)  
\end{array}\right.$$

We can compare the results of this function with those of human annotator who rated
the same translations.

<pre>grade &lt; output</pre>

## The Challenge

Your task for this assignment is to **improve the accuracy of automatic evaluation as
much as possible**.  Improving the metric to use the simple
[METEOR](http://aclweb.org/anthology/W/W07/W07-0734.pdf)
([Wikipedia](http://en.wikipedia.org/wiki/METEOR) also has a nice description) metric with
the chunking penalty in place of $$\ell(h,e)$$
is sufficient to pass. METEOR computes the harmonic mean of precision and recall, penalized by the number of chunks. That is:

$$\ell(h,e) = \left(1 - \gamma \left(\frac{c}{m}\right)^\beta\right)\frac{P(h,e) \cdot R(h,e)}{(1-\alpha)R(h,e)+\alpha P(h,e)}$$

where $$P$$ and $$R$$ are precision and recall, defined as:

$$R(h,e) = \frac{|h\cap e|}{|e|} \qquad \mbox{and} \qquad P(h,e) = \frac{|h\cap e|}{|h|},$$

$$\beta,\gamma$$ are tunable parameters, $$c$$ is the number of chunks and $$m$$ is the number of
matched unigrams.

Be sure to tune the parameter $$\alpha$$ that balances precision and recall.
This is a very simple baseline to implement. However, evaluation is not solved,
and the goal of this assignment is for you to experiment with methods that yield
improved predictions of relative translation accuracy. Some things that you might try:

* [Learn a classifier from the training data.](http://aclweb.org/anthology//W/W11/W11-2113.pdf)
* [Use WordNet to match synonyms.](http://wordnet.princeton.edu/)
* [Compute string similarity using string subsequence kernels.](http://jmlr.org/papers/volume2/lodhi02a/lodhi02a.pdf)
* Use an n-gram language model to better assess fluency.
* [Develop a single-sentence variant of BLEU.](http://aclweb.org/anthology//P/P02/P02-1040.pdf)
* [Use a dependency parser to assess syntactic well-formedness.](http://ssli.ee.washington.edu/people/jgk/dist/metaweb/mtjournal.pdf)
* Develop a method to automatically assess semantic similarity.
* [See what evaluation measures other people have implemented.](http://www.statmt.org/wmt10/pdf/wmt10-overview.pdf)

But the sky’s the limit! Automatic evaluation is far from solved, and there are many different
solutions you might invent. You can try anything you want as long as you follow the ground rules:

## Ground Rules

* You must work **independently** on this assignment.

* You should submit each of the following:

    1.  Your translations of the entire dataset, uploaded from any Eniac or Biglab machine
        using the command `turnin -c cis526 -p hw3 hw3.txt`.
        You may submit new results as often as you like, up until the assignment deadline.
        The output will be evaluated using `grade` program.
        The top few positions on the leaderboard will receive bonus points on this assignment.

    2.  Your code, uploaded using the command `turnin -c cis526 -p hw3-code file1 file2 ...`.
        This is due 24 hours after the leaderboard closes.
        You are free to extend the code we provide or write your own in whatever
        langugage you like, but the code should be self-contained,
        self-documenting, and easy to use.
  
    3.  A report describing the models you designed and experimented with, uploaded
        using the command `turnin -c cis526 -p hw3-report hw3-report.pdf`. This is
        due 24 hours after the leaderboard closes. Your report does not need to be
        long, but it should at minimum address the following points:

        * **Motivation**: Why did you choose the models you experimented with?

        * **Description of models or algorithms**: Describe mathematically or algorithmically
	  what you did.
          Your descriptions should be clear enough that someone else in the class could
	  implement them.

        * **Results**: You most likely experimented with various settings of any models
	  you implemented.
          We want to know how you decided on the final model that you submitted for us to grade.
          What parameters did you try, and what were the results?
          Most importantly: what did you learn?

        Since we have already given you a concrete problem and dataset, you do not
        need describe these as if you were writing a full scientific paper. Instead,
        you should focus on an accurate technical description of the above items.

        Note: These reports will be made available via hyperlinks on the leaderboard.
        Therefore, **you are not required to include your real name** if you would prefer not
        to do so.

* You should feel free to use additional data resources such as thesauruses,
  WordNet, or parallel data. You are also free to use additional codebases and libraries
  <b>except for those expressly intended to evaluate machine translation
  systems</b>. You must write your own evaluation metric. However, if you
  want your evaluation to depend on lemmatizers, stemmers, automatic parsers,
  or part-of-speech taggers, or you would like to *learn* a metric using
  a general machine learning toolkit, that is fine. But translation metrics including
  (but not limited too) available implementations of BLEU, METEOR, TER,
  NIST, and others are not permitted. You may of course inspect these systems
  if you want to understand how they work, although they tend to include
  other functionality that is not the focus of this assignment.
  It is possible to complete the assignment with a very modest amount
  of python code. If you aren't sure whether something is permitted, ask us.
				    
Any questions should be be posted on the
[course Piazza page](https://piazza.com/upenn/spring2015/cis526).

*Credits: This assignment is adapted from one originally developed by 
[Adam Lopez](https://alopez.github.io).  It is based on the shared task for evaluation metrics that is run at the annual [Workshop on Statistical Machine Translation](http://statmt.org/wmt15). The task was introduced by [Chris Callison-Burch](http://www.cis.upenn.edu/~ccb/).*
