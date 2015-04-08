---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 5 | Sentence Alignment
active_tab: homework
---
Reranking Crowdsourced Translation: <span class="text-muted">Homework 5</span>
=============================================================

Crowdsourcing is the process of obtaining services or aggregating ideas or content from a large group of people, especially from an online community.  Amazon Mechanical Turk is a crowdsourcing platform that allows people (called Requesters) to create Human Intelligence Tasks (HITs) that others (called Workers) then complete for a micropayment. 

Currently, expensive professional translations are needed to evaluate the output of machine translation models. However, it is possible that the results from crowdsourcing translation could be used as the benchmark against which machine translation outputs are compared. 

Crowdsourcing Translation uses crowds comprised of non-professional translators to  generate translations. However, naively collecting translations by crowdsourcing the task to non-professional translators yields disfluent, low-quality results if no quality control is exercised (Zaidan, Callison-Burch 2010). 

Your challenge is to automatically select the best sentence translations from the dataset based on sentence-level, worker-level, or ranking features.

Getting Started
---------------
To begin, download the Homework 5 starter kit. You may either choose to develop locally or on Penn’s servers. For the latter, we recommend using the Biglab machines, whose memory and runtime restrictions are much less stringent than those on Eniac. The Biglab servers can be accessed directly using the command `ssh PENNKEY@biglab.seas.upenn.edu`, or from Eniac using the command `ssh biglab`.

In the downloaded directory, you have a simple program that chooses a worker translation for a given source sentence. Test it out using this command:

    python program > output

This defauly program chooses the the first worker generated translation of the source sentence. To evaluate the chosen worker translations in this assignment, we will be using BLEU. To compute BLEU score against the reference translations, use the followine command: 

    python grade < output

The Challenge
-------------
Your task for this assignment is to automatically select the best translations for each source sentence using worker-level, sentence-level, and ranking features. In the files provided, you will find various data that will be useful in creating a system that does this. In the 'translations' file, you will find the four worker translations for a given sentence, along with the worker IDs of the workers who generated these translatons. In the 'survey' file, you will find various demographic information for all of these workers. 

Implementing a system that chooses the best translation using only worker-level and ranking feaures, such as worker language ability, location, and the average rank of a particular translation should be able to beat the baseline. However, there will be substantial room for improvement. One possibilty is to implement [a system that uses various sentence level features](http://www.cis.upenn.edu/~ccb/publications/crowdsourcing-translation.pdf) such as:


* Sentence-length
* N-gram match
* String edit distance
* Web n-gram match percentage
* Web n-gram geometric average

But the sky's the limit! You are welcome to design your own model, as long 
as you follow the ground rules laid out below.

Ground Rules
------------

* You must work **independently** on this assignment.

* You should submit each of the following:

    1.  The best worker translation for each of the Urdu sentence in the dataset,
        uploaded from any Eniac or Biglab machine
        using the command `turnin -c cis526 -p hw5 hw5.txt`.
        You may submit new results as often as you like, up until the assignment deadline.
        The output will be evaluated using a secret metric,
        but the `grade` program will give you a good idea of how well you're doing.
        The top few positions on the leaderboard will receive bonus points on this assignment.

    2.  Your code, uploaded using the command `turnin -c cis526 -p hw5-code file1 file2 ...`.
        This is due 24 hours after the leaderboard closes.
        You are free to extend the code we provide or write your own in whatever
        langugage you like, but the code should be self-contained, 
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
          What parameters did you try, and what were the results? Most importantly: what did you learn?

        Since we have already given you a concrete problem and dataset, you do not
        need describe these as if you were writing a full scientific paper. Instead,
        you should focus on an accurate technical description of the above items.

        Note: These reports will be made available via hyperlinks on the leaderboard.
        Therefore, **you are not required to include your real name** if you would prefer not
        to do so.

* You do not need any other data than what we provide. You are free to use any code or software you like, except for those expressly intended to generate or rerank translation output. You must write your own reranker. If you want to use machine learning libraries, taggers, parsers, or any other off-the-shelf resources, feel free to do so. If you aren’t sure whether something is permitted, ask us.

Any questions should be be posted on the
[course Piazza page](https://piazza.com/upenn/spring2015/cis526).
