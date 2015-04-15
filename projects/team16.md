---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 5 | Paraphrase Complexity
active_tab: homework
---
Paraphrase Complexity Differentiation
--------------------------------

Sentence simplification is the process by which an algorithm will take a sentence and compress it into a shorter and simpler form while maintaining as much of the meaning of the original sentence as possible. This is one interesting application of Monolingual Text-to-Text translation. This problem has many interesting facets, such as how to simplify the sentence by choosing one paraphrasing over another. This problem is important in modern internet applications where different audiences that understand different levels of complexity must be accounted for.

Paraphrases can have different scores in a complexity range, which allows us to compare them to each other. For example we can compare the two n-grams: *father* and *dad*. In this example, we would like an algorithm to determine that we score *father* with a higher complexity than *dad*. This assignment will have you take a corpus of phrase pairs and train a comparator to decide which phrase of each pair is simpler.

Getting Started
---------------

To begin, download the [Complexity Homework](http://urlhere.com ) starter kit. You may either choose to develop locally or on Penn’s servers. For the latter, we recommend using the Biglab machines, whose memory and runtime restrictions are much less stringent than those on Eniac. The Biglab servers can be accessed directly using the command `ssh PENNKEY@biglab.seas.upenn.edu`, or from Eniac using the command `ssh biglab`.

In the downloaded directory you will find a Python program called `default`, which contains a complete but very simple complexity decision algorithm based on the length of phrases. For each line in the data file, it compares the length of each tab-separated phrase and chooses the shorter of the two.

Run the basic program using the command: 

    `./default > default.out`. 

This will produce a file containing a phrase choice for each pair in the file. 

You can produce a score for these choices by running the command: 

    `./grade-dev < default.out` 

This score is based on a reference file which contains human decisions about the complexity of phrases. These human decisions, retrieved using a Mechanical Turk task, are assumed to be law for the purposes of this assignment.

Do you think that determining based on length is a good strategy? Run the following command and compare the result against the expected accuracy of randomly guessing (50%). To see the result more rapidly, you can directly run and grade the model using the command:

    `./default | ./grade-dev`


The Challenge
-------------

Your task is to **improve the accuracy of the automatic phrase complexity differentiator as much as possible**. Begin by taking a look at this paper written by Ellie Pavlick on [Lexical Style Properties in Paraphrases](http://www.seas.upenn.edu/~epavlick/papers/style_for_paraphrasing.pdf). The paper describes a method for determining a complexity score utilizing parallel corpora data that convey the same meaning in different lexical styles (in our case, complexity vs. simplicity). 

We use parallel articles from Wikipedia (http://en.wikipedia.org/wiki/) and simplified Wikipedia (https://simple.wikipedia.org/wiki/) as examples of complex and simple language respectively. For instance, a sentence in normal Wikipedia might read:

*Buxton is within the sphere of influence of Greater Manchester due to its close proximity to the area .*

Meanwhile, the simplified Wikipedia version of this sentence reads:

*Buxton is also close to Manchester .*

Given examples of language at each end of the complexity dimension, we score a phrase by the log ratio of the probability of observing the word in the *complex* corpus (COMP) to observing it in the combined corpora (ALL). More specifically, the reference corpus is normal Wikipedia and the combined data is normal and simplified Wikipedia together. The phrase complexity model should be able to map a phrase to *w* to a complexity score:

$$complexity(w) = log(\frac{P(w | COMP)}{P(w|ALL)})$$

There is no limitation to the number of words in a phrase. The baseline model recognizes up to tri-grams, but you can train your model to recognize phrases that contain more than three words. Moreover, phrases which do not occur at all in COMP are treated as though they occurred once. This score can be used as a feature, along with others like phrase length. These features can be combined using a weight vector that you tune yourself.

While this strategy may get you up to the baseline score, it is encouraged to explore other approaches that will allow you to more accurately compare each phrase. Some possible extensions might be:

* Gather additional data for simple and complex phrases
* Implement a smoothing function to handle phrases that do not occur in data
* Train a translation model that will map known paraphrases to new ones

You can do whatever you want, the sky’s the limit!

Ground Rules
------------

* You must work **independently** on this assignment.

* You should submit each of the following:

    1.  A choice for each phrase in the dataset, uploaded from any Eniac or Biglab machine
        using the command `turnin -c cis526 -p complexity-hw complexity-hw.txt`.
        You may submit new results as often as you like, up until the assignment deadline.
        The output will be evaluated using a secret metric,
        but the `grade` program will give you a good idea of how well you're doing.
        The top few positions on the leaderboard will receive bonus points on this assignment.

    2.  Your code, uploaded using the command `turnin -c cis526 -p complexity-code file1 file2 ...`.
        This is due 24 hours after the leaderboard closes.
        You are free to extend the code we provide or write your own in whatever
        langugage you like, but the code should be self-contained, 
        self-documenting, and easy to use.

    3.  A report describing the models you designed and experimented with, uploaded
        using the command `turnin -c cis526 -p complexity-report complexity-report.pdf`. This is
        due 24 hours after the leaderboard closes. Your report does not need to be
        long, but it should at minimum address the following points:

        * **Motivation**: Why did you choose the models you experimented with?

        * **Description of models or algorithms**: Describe mathematically or algorithmically what you did.
          Your descriptions should be clear enough that someone else in the class could implement them.

        * **Results**: You most likely experimented with various settings of any models you implemented.
          We want to know how you decided on the final model that you submitted for us to grade.
          Most importantly: what did you learn?

        Since we have already given you a concrete problem and dataset, you do not
        need describe these as if you were writing a full scientific paper. Instead,
        you should focus on an accurate technical description of the above items.

        Note: These reports will be made available via hyperlinks on the leaderboard.
        Therefore, **you are not required to include your real name** if you would prefer not
        to do so.

* You may only use data or code outside of what is provided
  _with advance permission_. We will ask you to make 
  your resources available to everyone. You must write your
  own code.

Any questions should be be posted on the
[course Piazza page](https://piazza.com/upenn/spring2015/cis526).

*Credits: This assignment is adapted from research done and data collected by 
[Ellie Pavlick](http://www.seas.upenn.edu/~epavlick/).*


