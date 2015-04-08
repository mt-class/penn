---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 5 | Sentence Alignment
active_tab: homework
---

Sentence Alignment | Homework 5
=============================================================


Given a parallel corpus of sentences with foreign language and the translated language. 
To convey the information, it is important that the sentences are aligned.
The paragraphs are aligned correctly most of the times however, the sentences within the paragraph are not.
The task is to align the sentences correctly like below,

*Una delle vittime pi˘ recenti Ë stato Kumar Ponnambalam , che qualche mese fa era venuto in visita qui al Parlamento europeo*.

*One of the people assassinated very recently in Sri Lanka was Mr Kumar Ponnambalam , who had visited the European Parliament just a few months ago*.

This is represented in the output file as, 1st sentence in foreign language (Italian) is equivalent to 1st sentence in translated language (English)

**1 <==> 1**

We wrote a program that aligns words
automatically as a part of Homework1 where our program
would ideally output pairs of foreign and translated language. In this homework, we use cues
like the length and order of each sentence. 

Your program can align multiple sentences in the foreign language (Italian) to a single sentence in the 
translated language (English) like below,

*Signora Presidente , un richiamo al Regolamento.*

*Gradirei avere il suo parere riguardo all ' articolo 143 sull ' inammissibilit‡ .*

*Madam President , on a point of order , 
I would like your advice about Rule 143 concerning inadmissibility .*

This is represented in the output file as, 1st and 2nd sentence in foreign language (Italian) is equivalent to 1st sentence in translated language (English)

**1 <==> 1**

**2 <==> 1**

The other possibility could be a single sentence in foreign language can become multiple sentences in translated language like below,

*Dichiaro ripresa la sessione del Parlamento europeo , interrotta venerdÏ 17 dicembre e rinnovo a tutti i miei migliori auguri nella speranza che abbiate trascorso delle buone vacanze .*

*I declare resumed the session of the European Parliament adjourned on Friday 17 December 1999.*

*And I would like once again to wish you a happy new year in the hope that you enjoyed a pleasant festive period .*

This is represented in the output file as, 1st sentence in foreign language (Italian) is equivalent to 1st and 2nd sentence in translated language (English)

**1 <==> 1,2**

Your program must make a choice about 
    how to align the sentences as there are multiple possibilities of alignment with each sentence.

Getting Started
---------------

To begin, download the [Homework starter kit]().
You may either choose to develop locally or on Penn's servers. For the latter, we
recommend using the Biglab machines, whose memory and runtime restrictions are much
less stringent than those on Eniac. The Biglab servers can be accessed directly
using the command `ssh PENNKEY@biglab.seas.upenn.edu`, or from Eniac using the
command `ssh biglab`.

In the downloaded directory you will find a Python program called
`program`, which contains a complete but very simple sentence alignment algorithm.

By default, we are aligning each foreign sentence with English sentence in the order 
of the line number.

Run the baseline sentence alignment model for 100 sentences using the command:

    ./program -n 100 > output.txt

This runs the alignment program on first 100 sentences and stores the output in `output.txt`. To score the alignments,
run this command:

    ./grade < output.txt

This command scores the alignment quality by comparing the output alignments against a
set of gold sentence alignment references using a metric called accuracy. Try training on all sentences instead of 100 by specifying the change on the command line:

    ./program | ./grade

The Challenge
-------------

Your task is to **improve the accuracy as much as possible** (higher is better).

The default system is just aligning sentences directly according to line number. A simple way to improve accuracy would be to consider length of sentences and align sentences accordingly. This will at least help detecting sentences where one
foreign language sentence is translated to English language.

Other than considering only the sentence length, you can consider Names, numbers and punctuations to improve accuracy.

Also to compare each sentence in foreign language against the entire translated corpus, one can use dynamic programming. An illustration of how dynamic programming works for edit distance is present [here](http://nlp.stanford.edu/IR-book/html/htmledition/edit-distance-1.html). 


Here are some ideas that may help you to improve your sentence alignment accuracy:

* [A Program for Aligning Sentences in Bilingual Corpora](http://www.aclweb.org/anthology/J93-1004.pdf).
* [Aligning Sentences in Bilingual Corpora using Lexical Information](http://www.ee.columbia.edu/~stanchen/papers/aclrev.pdf).
* [Fast and Accurate Sentence Alignment of Bilingual Corpora](http://msr-waypoint.com/pubs/68886/sent-align2-amta-final.pdf).

You are welcome to design your own model, as long 
as you follow the ground rules laid out below.

Ground Rules
------------

* You must work **independently** on this assignment.

* You should submit each of the following:

    1.  An alignment of the entire dataset, uploaded from any Eniac or Biglab machine
        using the command `turnin -c cis526 -p hw5 hw5.txt`.
        You may submit new results as often as you like, up until the assignment deadline.
        The output will be evaluated using a secret metric,
        but the `grade` program will give you a good idea of how well you're doing.
        The top few positions on the leaderboard will receive bonus points on this assignment.

    2.  Your code, uploaded using the command `turnin -c cis526 -p hw5-code file1 file2 ...`.
        This is due 24 hours after the leaderboard closes.
        You are free to extend the code we provide or write your own in whatever
        language you like, but the code should be self-contained, 
        self-documenting, and easy to use.

    3.  A report describing the models you designed and experimented with, uploaded
        using the command `turnin -c cis526 -p hw5-report hw5-report.pdf`. This is
        due 24 hours after the leaderboard closes. Your report does not need to be
        long, but it should at minimum address the following points:

        * **Motivation**: Why did you choose the models you experimented with?

        * **Description of models or algorithms**: Describe mathematically or algorithmically what you did.
          Your descriptions should be clear enough that someone else in the class could implement them.
          What are your models? How did you optimize them? How did you align with them?
          What were the values of any fixed parameters you used?

        * **Results**: You most likely experimented with various settings of any models you implemented.
          We want to know how you decided on the final model that you submitted for us to grade.
          What parameters did you try, and what were the results?
          If you evaluated any qualities of the results other than accuracy, even if
          you evaluated them qualitatively, how did you do it?
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

Any questions should be posted on the
[course Piazza page](https://piazza.com/upenn/spring2015/cis526).