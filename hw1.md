---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 1 | Alignment
active_tab: homework
---

<div class="alert alert-info">
  Leaderboard submission due Tuesday, February 2nd, 11:59pm.
  Write-up due Wednesday, February 3rd, 11:59pm.
</div>

Alignment <span class="text-muted">Homework 1</span>
=============================================================

Word alignment is a fundamental task in statistical machine translation.
This homework will give you an opportunity to try your hand at developing solutions to this challenging and interesting problem.

Getting Started
---------------

You must have git and python (2.7) on your system to run the assignments.
Once you've confirmed this, run this command:

    git clone https://github.com/callison-burch/dreamt.git

In the `aligner` directory you will find a python program called
`align`, which contains a complete but very simple alignment algorithm.

For every word, it computes the set of sentences that the word appears in.
This aligner uses **_set similarity_** to determine which words are aligned to each
other in a corpus of parallel sentences. The set similarity measure we use is
[Dice's coefficient](http://en.wikipedia.org/wiki/Dice's_coefficient/), defined 
in terms of sets $$X$$ and $$Y$$ as follows:

<p>$$D(X,Y) = \frac{2 \times |X \cap Y|}{|X| + |Y|}$$</p>

Dice's coefficient ranges in value from 0 to 1.

How do we use set similarity to align words? The intuition is that if you look at the set of
sentence pairs from a parallel corpus that contain an English word $$e$$, and that set is similar
to the set of sentence pairs that contain a French word $$g$$, then these words are likely to be
translations of each other.

Formally, every pair of word types $$(e,g)$$ in the parallel corpus receives a Dice “score” $$\delta(e,f)$$.
The alignment algorithm then goes through all pairs of sentences $$(\textbf{e},\textbf{f})$$ and predicts that English
word $$e_i$$ is aligned to French word $$f_j$$ if $$\delta(e_i,f_j) > \tau$$.
By making $$\tau$$ closer to 1, fewer points are aligned but with higher precision; by making it closer to 0,
more points are aligned, probably improving recall.
By default, the aligner code we have provided you uses $$\tau=0.5$$ as its threshold.

Run the baseline heuristic model 1,000 sentences using the command:

    python align -n 1000 > dice.a

This runs the aligner and stores the output in `dice.a`. To display the alignments visually and score the alignments,
run this command (use the `-n N` option to display verbose output for only the first $$N$$ sentences):

    python score-alignments < dice.a

This command scores the alignment quality by comparing the output alignments against a
set of human alignment annotations using a metric called the alignment error rate (AER),
which balances the precision and recall of the guessed alignments (see Section 6 of
[this paper](http://aclweb.org/anthology-new/P/P00/P00-1056.pdf)). Look at the terrible
output of this heuristic model -- it’s better than chance, but not any good.
Try training on 10,000 sentences instead of 1,000, by specifying the change on the command line:

    python align -n 10000 | python score-alignments 

Performance should improve since the degree of association between words will be estimated from more sentence pairs.
Another experiment that you can do is change the threshold criterion $$\tau$$ used by the aligner using the `-t X` option.
How does this affect alignment error rate?

The Challenge
-------------

Your task is to **improve the alignment error rate as much as possible** (lower is better).
It shouldn't be hard: you've probably noticed that thresholding a Dice coefficient is a bad
idea because alignments don't compete against one another.
A good way to correct this is with a model where alignments compete against each other, such as IBM Model 1.
It forces all of the words you are conditioning on to compete to explain each word that is generated.

IBM Model 1 is a simple probabilistic translation model we talked about in class. A source sentence
$$\textbf{g}= \left < \epsilon,g_1,g_2,\ldots,g_n \right >$$ (where $$\epsilon$$ represents a null token present in every sentence)
and a desired target sentence length $$m$$ are given, and conditioned on these, Model 1 defines a distribution
over translations of length $$m$$ into the target language using the following process:

\\begin{align}
  \textrm{For each } i &\in [1,2,\ldots,m]\\\\
  a_i &\sim \textrm{Uniform}(0,1,\ldots,n)\\\\
  e_i &\sim \textrm{Categorical}(\theta_{g_{a_i}})
\\end{align}

The random variables $$\textbf{a} = \left < a_1, a_2, \ldots, a_m  \right >$$ are the _alignments_
that pick out a source word to translate at each position in the target sentence.
A source word $$g_j$$ may be translated any number of times (0,1,2, etc.), but each word in the target
language $$e_i$$ that is generated is generated exactly one time by exactly one source word.

The marginal (marginalizing over all possible alignments) likelihood of a sentence
$$\textbf{e} = \left < e_1, e_2, \ldots, e_m \right >$$ given $$\textbf{g}$$ and $$m$$ is:

<p>$$P({\bf e}|{\bf f}) = \prod_i \sum_j P(a_i = j | |{\bf e}|)$$</p>

The iterative EM update for this model is straightforward. At each iteration,
for every pair of an English word type $$e$$ and a French word type $$f$$,
you count up the expected (fractional) number of times tokens $$f$$ are aligned to tokens of $$e$$
and divide by the expected number of times that g was chosen as a translation source.
That will give you a new estimate of the translation probabilities $$p(g∣e)$$,
which leads to new alignment expectations, and so on. We recommend developing on a small data set
(1,000 sentences) and a few iterations of EM (in practice, Model 1 needs only 4 or 5 iterations to give good results).
When you are finished, you should see both qualitative and quantitative improvements in the alignments.

Developing a Model 1 aligner should be enough to beat our baseline system
and earn a passing grade. But alignment isn't a solved problem, and the goal of
this assignment isn't for you to just implement a well-known algorithm. To 
get full credit you **must** experiment with at least one additional
model of your choice and document your work. Here are some ideas:

* Implement [a model that prefers to align words close to the diagonal](http://aclweb.org/anthology/N/N13/N13-1073.pdf).
* Implement a [hidden Markov model (HMM) alignment model](http://aclweb.org/anthology-new/C/C96/C96-2141.pdf).
* Implement [a morphologically-aware alignment model](http://aclweb.org/anthology/N/N13/N13-1140.pdf).
* [Use *maximum a posteriori* inference under a Bayesian prior](http://aclweb.org/anthology/P/P11/P11-2032.pdf).
* Train a French-English model and an English-French model and [combine their predictions](http://aclweb.org/anthology-new/N/N06/N06-1014.pdf).
* Train a [supervised discriminative alignment model](http://aclweb.org/anthology-new/P/P06/P06-1009.pdf) on the annotated development set.
* Be Bayesian about it ([variational](http://www.cs.rochester.edu/~gildea/pubs/riley-gildea-acl12.pdf) or [Gibbs](http://aclweb.org/anthology/P/P11/P11-2032.pdf)).
* Train an [unsupervised discriminative alignment model](http://aclweb.org/anthology-new/P/P11/P11-1042.pdf).
* Seek out additional [inspiration](http://scholar.google.com/scholar?q=word+alignment).

But the sky's the limit! You are welcome to design your own model, as long 
as you follow the ground rules:

Ground Rules
------------

* You must work independently on this assignment.
* You must turn in three things:
  1. An alignment of the entire dataset, uploaded to the [leaderboard submission site](http://jhumtclass.appspot.com) according to <a href="assignment0.html">the Assignment 0 instructions</a>. You can upload new output as often
     as you like, up until the assignment deadline. The output will be evaluated 
     using a secret metric, but the `grade` program will give you a good
     idea of how well you're doing, and you can use the `check` program
     to see whether your output is formatted correctly. Whoever has
     the highest score at the deadline will receive the most bonus points.

     *Note*. The upload site will reject files larger than 1 MB, so please reduce your file to only the first 1,000 lines before uploading, e.g.,

          python align | head -n 1000 > output.txt

  1. Your code. Send us a URL from which we can get the code and git revision
     history (a link to a tarball will suffice, but you're free to send us a 
     github link if you don't mind making your code public). This is due at the
     deadline: when you upload your final answer, send us the code.
     You are free to extend the code we provide or roll your own in whatever
     langugage you like, but the code should be self-contained, 
     self-documenting, and easy to use. 
  1. A clear, mathematical description of your algorithm and its motivation
     written in scientific style. This needn't be long, but it should be
     clear enough that one of your fellow students could re-implement it 
     exactly. We will review examples in class before the due date.
* You may only use data or code resources other than the ones we
  provide _with advance permission_. We will ask you to make 
  your resources available to everyone. If you have a cool idea 
  using the Berkeley parser, or a French-English dictionary, that's 
  great. But we want everyone to have access to the same resources, 
  so we'll ask you to share the parses. This kind of 
  constrained data condition is common in real-world evaluations of AI 
  systems, to make evaluations fair. A few things are off-limits:
  Giza++, the Berkeley Aligner, or anything else that
  already does the alignment for you. You must write your
  own code.

If you have any questions or you're confused about anything, just ask.

*Credits: This assignment is adapted from one originally developed by 
[Philipp Koehn](http://homepages.inf.ed.ac.uk/pkoehn/)
and later modified by [John DeNero](http://www.denero.org/). It
incorporates some ideas from [Chris Dyer](http://www.cs.cmu.edu/~cdyer).*
