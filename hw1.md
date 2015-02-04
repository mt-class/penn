---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 1 | Alignment
active_tab: homework
---

<div class="alert alert-info">
  Leaderboard submission due 11:59 pm, Tuesday, February 3rd.
  Code and report due 11:59 pm, Wednesday, February 4th.
</div>

Alignment <span class="text-muted">Homework 1</span>
=============================================================

Aligning words is a key task in machine translation. We start with
a large _parallel corpus_ of aligned sentences. For example, we might
have the following sentence pair from the proceedings of the bilingual
Canadian parliament:

*le droit de permis passe donc de <span>$</span> 25 à <span>$</span> 500*.

*we see the licence fee going up from <span>$</span> 25 to <span>$</span> 500*.

Getting documents aligned at the _sentence_ level like this is
relatively easy: we can use paragraph boundaries and cues
like the length and order of each sentence. But to learn a translation
model we need alignments at the _word_ level. That's where you come
in. **Your challenge is to write a program that aligns words
automatically.** For example, given the sentence above, your program
would ideally output these pairs:

*le -- the,
droit -- fee,
permis -- license,
passe -- going,
passe -- up,
donc -- from,
<span>$</span> -- <span>$</span>,
25 -- 25,
à -- to,
<span>$</span> -- <span>$</span>,
50 -- 50*

Your program can leave words unaligned (e.g. *we* and *see*) or
multiply aligned (e.g. *passe* aligned to *going up*). It will be
faced with difficult choices. Suppose it sees this sentence pair:

*I want to make it clear that we have to let this issue come to a vote today*.

*il est donc essentiel que cette question fasse le objet de un vote aujourd' hui .*

Your program must make a choice about how to align the words of the non-literal
translations *I want to make it clear* and *il est donc essentiel*. Even
experienced bilinguals will disagree on examples like this. So word alignment
does not capture every nuance, but it is still very useful.

Getting Started
---------------

To begin, download the [Homework 1 starter kit](http://seas.upenn.edu/~cis526/hw1.zip).
You may either choose to develop locally or on Penn's servers. For the latter, we
recommend using the Biglab machines, whose memory and runtime restrictions are much
less stringent than those on Eniac. The Biglab servers can be accessed directly
using the command `ssh PENNKEY@biglab.seas.upenn.edu`, or from Eniac using the
command `ssh biglab`.

In the downloaded directory you will find a Python program called
`align`, which contains a complete but very simple alignment algorithm.

For every word, it computes the set of sentences that the word appears in.
This aligner uses **_set similarity_** to determine which words are aligned to each
other in a corpus of parallel sentences. The set similarity measure we use is
[Dice's coefficient](http://en.wikipedia.org/wiki/S%C3%B8rensen%E2%80%93Dice_coefficient),
defined in terms of sets $$X$$ and $$Y$$ as follows:

$$D(X,Y) = \frac{2 \times |X \cap Y|}{|X| + |Y|}$$

Dice's coefficient ranges in value from 0 to 1.

How do we use set similarity to align words? The intuition is that if you look at the set of
sentence pairs from a parallel corpus that contain an English word $$e$$, and that set is similar
to the set of sentence pairs that contain a French word $$g$$, then these words are likely to be
translations of each other.

Formally, every pair of word types $$(e,f)$$ in the parallel corpus receives a Dice “score” $$\delta(e,f)$$.
The alignment algorithm then goes through all pairs of sentences
$$(\textbf{e},\textbf{f}) = (\langle e_1, e_2, \ldots, e_m \rangle, \langle f_1, f_2, \ldots, f_n \rangle)$$
and predicts that English word $$e_i$$ is aligned to French word $$f_j$$ if $$\delta(e_i,f_j) > \tau$$.
By making $$\tau$$ closer to 1, fewer points are aligned but with higher precision; by making it closer to 0,
more points are aligned, probably improving recall.
By default, the aligner code we have provided you with uses $$\tau=0.5$$ as its threshold.

Run the baseline heuristic model 1,000 sentences using the command:

    ./align -n 1000 > dice.a

This runs the aligner and stores the output in `dice.a`. To display the alignments visually and score the alignments,
run this command (use the `-n N` option to display verbose output for only the first $$N$$ sentences):

    ./grade < dice.a

This command scores the alignment quality by comparing the output alignments against a
set of human alignment annotations using a metric called the alignment error rate (AER),
which balances the precision and recall of the guessed alignments (see Section 6 of
[this paper](http://aclweb.org/anthology-new/P/P00/P00-1056.pdf)). Look at the terrible
output of this heuristic model -- it’s better than chance, but not any good.
Try training on 10,000 sentences instead of 1,000, by specifying the change on the command line:

    ./align -n 10000 | ./grade

Performance should improve since the degree of association between words will be estimated from more sentence pairs.
Another experiment that you can do is change the threshold criterion $$\tau$$ used by the aligner using the `-t threshold` option.
How does this affect alignment error rate?

The Challenge
-------------

Your task is to **improve the alignment error rate as much as possible** (lower is better).
It shouldn't be hard: you've probably noticed that thresholding a Dice coefficient is a bad
idea because alignments don't compete against one another.
A good way to correct this is with a model where alignments compete against each other, such as IBM Model 1.
It forces all of the words you are conditioning on to compete to explain each word that is generated.

IBM Model 1 is a simple probabilistic translation model we talked about in class. A source sentence
<span>$$\textbf{f}= \langle \epsilon,f_1,f_2,\ldots,f_n \rangle$$</span> (where $$\epsilon$$ represents a null token present in every sentence)
and a desired target sentence length $$m$$ are given, and conditioned on these, Model 1 defines a distribution
over translations of length $$m$$ into the target language using the following process:

$$
\begin{align*}
  \textrm{For each } i &\in [1,2,\ldots,m]\\
  a_i &\sim \textrm{Uniform}(0,1,\ldots,n)\\
  e_i &\sim \textrm{Categorical}(\theta_{f_{a_i}})
\end{align*}
$$

The random variables $$\textbf{a} = \langle a_1, a_2, \ldots, a_m  \rangle$$ are the **_alignments_**
that pick out a source word to translate at each position in the target sentence.
A source word $$f_j$$ may be translated any number of times (0,1,2, etc.), but each word in the target
language $$e_i$$ that is generated is generated exactly one time by exactly one source word.

The marginal (marginalizing over all possible alignments) likelihood of a sentence
$$\textbf{e} = \langle e_1, e_2, \ldots, e_m \rangle$$ given $$\textbf{f}$$ and $$m$$ is:

$$ P({\bf e} \mid {\bf f}, m) = \prod_{i=1}^m \sum_{j=0}^n p(a_i = j) \times p(e_i \mid f_j) $$

The iterative EM update for this model is straightforward. At each iteration,
for every pair of an English word type $$e$$ and a French word type $$f$$,
you count up the expected (fractional) number of times tokens $$f$$ are aligned to tokens of $$e$$
and divide by the expected number of times that $$f$$ was chosen as a translation source.
That will give you a new estimate of the translation probabilities $$p(f∣e)$$,
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
* Use [*maximum a posteriori* (MAP) inference under a Bayesian prior](http://aclweb.org/anthology/P/P11/P11-2032.pdf).
* Train a French-English model and an English-French model and [combine their predictions](http://aclweb.org/anthology-new/N/N06/N06-1014.pdf).
* Train a [supervised discriminative alignment model](http://aclweb.org/anthology-new/P/P06/P06-1009.pdf) on the annotated development set.
* Be Bayesian about it ([variational](http://www.cs.rochester.edu/~gildea/pubs/riley-gildea-acl12.pdf) or [Gibbs](http://aclweb.org/anthology/P/P11/P11-2032.pdf)).
* Train an [unsupervised discriminative alignment model](http://aclweb.org/anthology-new/P/P11/P11-1042.pdf).
* Seek out additional [inspiration](http://scholar.google.com/scholar?q=word+alignment).

But the sky's the limit! You are welcome to design your own model, as long 
as you follow the ground rules laid out below.

Ground Rules
------------

* You must work **independently** on this assignment.

* You should submit each of the following:

    1.  An alignment of the entire dataset, uploaded from any Eniac or Biglab machine
        using the command `turnin -c cis526 -p hw1 hw1.txt`.
        You may submit new results as often as you like, up until the assignment deadline.
        The output will be evaluated using a secret metric,
        but the `grade` program will give you a good idea of how well you're doing.
        The top few positions on the leaderboard will receive bonus points on this assignment.

    2.  Your code, uploaded using the command `turnin -c cis526 -p hw1-code file1 file2 ...`.
        This is due 24 hours after the leaderboard closes.
        You are free to extend the code we provide or write your own in whatever
        langugage you like, but the code should be self-contained, 
        self-documenting, and easy to use.

    3.  A report describing the models you designed and experimented with, uploaded
        using the command `turnin -c cis526 -p hw1-report hw1-report.pdf`. This is
        due 24 hours after the leaderboard closes. Your report does not need to be
        long, but it should at minimum address the following points:

        * **Motivation**: Why did you choose the model you experimented with?

        * **Description of model or algorithm**: Describe mathematically or algorithmically what you did.
          Your description should be clear enough that someone else in the class could implement it.
          What is your model? How did you optimize it? How did you align with it?
          What were the values of any fixed parameters you used?

        * **Results**: You most likely experimented with various settings of any models you implemented.
          We want to know how you decided on the final model that you submitted for us to grade.
          What parameters did you try, and what were the results?
          If you evaluated any qualities of the results other than AER, even if
          you evaluated them qualitatively, how did you do it?
          Most importantly: what did you learn?

        Since we have already given you a concrete problem and dataset, you do not
        need describe these as if you were writing a full scientific paper. Instead,
        you should focus on an accurate technical description of the above items.

        **Note:** These reports will be made available via hyperlinks on the leaderboard.
        Therefore, you are not required to include your real name if you would prefer not
        to do so.

* You may only use data or code outside of what is provided
  _with advance permission_. We will ask you to make 
  your resources available to everyone. If you have a cool idea 
  using the Berkeley parser, or a French-English dictionary, that's 
  great. But we want everyone to have access to the same resources, 
  so we'll ask you to share the parses. This kind of 
  constrained data condition is common in real-world evaluations of AI 
  systems, to make evaluations fair. A few things are off-limits:
  Giza++, the Berkeley Aligner, or anything else that
  already does the alignment for you. You must write your
  own code.

Any questions should be be posted on the
[course Piazza page](https://piazza.com/upenn/spring2015/cis526).

*Credits: This assignment is adapted from one originally developed by 
[Philipp Koehn](http://homepages.inf.ed.ac.uk/pkoehn/)
and later modified by [John DeNero](http://www.denero.org/). It
incorporates some ideas from [Chris Dyer](http://www.cs.cmu.edu/~cdyer).*
