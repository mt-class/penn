---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 5 | Quality Estimation
active_tab: homework
---
#Quality Estimation: Final Project
Quality estimation is a topic of increasing interest in Machine Translation. It aims at providing a quality indicator for unseen translated sentences at various granularity levels. In this assignment, we will focus on sentence-level estimation. Different from Machine Translation evaluation, quality estimation systems do not rely on reference translations and are generally addressed using machine learning techniques to predict quality scores.

Some interesting uses of sentence-level quality estimation are the following:

1. Decide whether a given translation is good enough for publishing as is
2. Inform readers of the target language only whether or not they can rely on a translation
3. Filter out sentences that are not good enough for post-editing by professional translators
4. Select the best translation among options from multiple MT and/or translation memory systems

In this assignment we will give you Spanish source sentences and their English translations, which can either be human translations or two versions of machine translations. **Your mission, should you choose to accept it, is to accurately score a sentence.** The scores are $$1, 2$$ or $$3$$.

+ 1 means a perfect translation, no post-editing needed at all
+ 2 means a near miss translation: translation contains maximum of two-three errors, and possibly additional errors that can be easily fixed (capitalization, punctuation)
+ 3 means a very low quality translation, which cannot be easily fixed


##Getting Started
To begin, download the project starter kit. You may either choose to develop locally or on Penn’s servers. For the latter, we recommend using the Biglab machines, whose memory and runtime restrictions are much less stringent than those on Eniac. The Biglab servers can be accessed directly using the command `ssh PENNKEY@biglab.seas.upenn.edu`, or from Eniac using the command `ssh biglab`.

In the downloaded directory, you have a program that evaluates how good a machine translation system output is. Test it out using this command:

```
./default > output
```
Every translation $$e$$ of an input sentence $$f$$ has an associated feature vector $$h(e, f)$$. The `default` takes a parameter vector $$\theta$$ whose length is equal to that of $$h(e, f)$$. By default, $$\theta = [1, 1, \ldots]$$. For each sentence in the training data, we obtain a weighted sum of the features. Then, for each score we compute the average sum. Then, for each sentence in the test set, we compute a sum of the features and compare it with the average sum. We assign a score based on the minimum difference to the average of each score.

<br />
To evaluate, use this command:

```
./grade < output
```
The scoring is the accuracy measured against human annotated labels.

##Baseline Method
For the baseline implementation we are planning to try out a few different methods to see which is feasible. They include:

1. K Nearest Neighbors
2. Clustering
3. An algorithm for learning new weights 

##The Challenge
You can improve the parameter vector by using effective learning algorithms that optimize $$\theta$$. You can also improve the values assigned to the scores by using a function other than the naive average.

Implement a version of a learning algorithm and some feature engineering to improve accuracy.  Here are some ideas to improve the accuracy:

1. Add new features
2. You could try training different classifers.
3. You could also develop systems based on referential translation machines and parallel feature decay algorithms. 

But the sky’s the limit! You can try anything you want, as long as you follow the ground rules.

## Ground Rules

* You must work **independently** on this assignment.

* You should submit each of the following:

    1.  Your translations of the entire dataset, uploaded from any Eniac or Biglab machine
        using the command `TBD`.
        You may submit new results as often as you like, up until the assignment deadline.
        The output will be evaluated using `grade` program.
        The top few positions on the leaderboard will receive bonus points on this assignment.

    2.  Your code, uploaded using the command `TBD`. This is due 24 hours after the leaderboard closes. You are free to extend the code we provide or write your own in whatever language you like, but the code should be self-contained, self-documenting, and easy to use.
  
    3.  A report describing the models you designed and experimented with, uploaded
        using the command `TBD`. This is due 24 hours after the leaderboard closes. Your report does not need to be
        long, but it should at minimum address the following points:

        * **Motivation**: Why did you choose the models you experimented with?

        * **Description of models or algorithms**: Describe mathematically or algorithmically what you did. Your descriptions should be clear enough that someone else in the class could implement them.

        * **Results**: You most likely experimented with various settings of any models you implemented. We want to know how you decided on the final model that you submitted for us to grade. What parameters did you try, and what were the results? Most importantly: what did you learn?

        Since we have already given you a concrete problem and dataset, you do not
        need describe these as if you were writing a full scientific paper. Instead,
        you should focus on an accurate technical description of the above items.

        Note: These reports will be made available via hyperlinks on the leaderboard.
        Therefore, **you are not required to include your real name** if you would prefer not
        to do so.
  
<br />
*Credits: This assignment was developed by Anwesha Das, Deepti Panuganti, Dhruvil Shah, and Aryeh Stiefel.*