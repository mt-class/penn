Translation Quality Estimation
===

One extremely common problem in machine translation is automatic evaluation.  When given a set of hypothesis translations and a single reference translation, we often need to develop a system that can automatically judge which of the hypothesis translations is most appropriate.  However, suppose that we are not supplied with a reference translation, but only the original sentence and a single hypothesis translation.  How could we develop a system to automatically judge quality of the hypothesis translation?

Suppose that we have the following original sentence: _Norway's_ _five_ _million_ _people_ _enjoy_ _one_ _of_ _the_ _highest_ _standards_ _of_ _living,_ _not_ _just_ _in_ _Europe,_ _but_ _in_ _the_ _world._

And assume that we have some translation system that gives the following translation of the original sentence: _Los_ _cinco_ _millones_ _de_ _habitantes_ _de_ _Noruega_ _gozan_ _de_ _uno_ _de_ _los_ _mayores_ _niveles_ _de_ _vida,_ _no_ _sólo_ _de_ _Europa_ _sino_ _del_ _mundo._

For each (sentence, translation) pair produced, your goal is to automatically estimate the quality of the translation by scoring it on a scale from 1-3 based on the amount of post-editing necessary to perfect the translation.  The meaning of each score is explained below.

- 1 = perfect translation, no post-editing needed at all
- 2 = near miss translation: translation contains maximum of 2-3 errors, and possibly additional errors that can be easily fixed (capitalisation, punctuation)
- 3 = very low quality translation, cannot be easily fixed

In order to help you score translations, you will be provided with a set of features corresponding to each (sentence, translation) pair.  The baseline features that are provided are as follows:

1. Average source token length	
2. LM probability of source sentence	
3. LM probability of target sentence	
4. Number of occurrences of the target word within the target hypothesis (averaged for all words in The hypothesis - type/token ratio)	
5. Average number of translations per source word in the sentence (as given by IBM 1 table thresholded such that prob(t|s) > 0.2)	
6. Average number of translations per source word in the sentence (as given by IBM 1 table thresholded such that prob(t|s) > 0.01) weighted by the inverse frequency of each word in the source corpus	
7. Percentage of unigrams in quartile 1 of frequency (lower frequency words) in a corpus of the source language (SMT training corpus)	
8. Percentage of unigrams in quartile 4 of frequency (higher frequency words) in a corpus of the source language
9. Percentage of bigrams in quartile 1 of frequency of source words in a corpus of the source language
10. Percentage of bigrams in quartile 4 of frequency of source words in a corpus of the source language
11. Percentage of trigrams in quartile 1 of frequency of source words in a corpus of the source language
12. Percentage of trigrams in quartile 4 of frequency of source words in a corpus of the source language
13. Percentage of unigrams in the source sentence seen in a corpus (SMT training corpus)

**Your task is to develop a quality estimation system that assigns these scores automatically using these baseline features along with any additional features that you care to implement.**

Getting Started
---

To begin, please download the [Quality Estimation starter kit](PLACE_LINK_ADDRESS_HERE).  You may either choose to develop locally or on Penn’s servers. For the latter, we recommend using the Biglab machines, whose memory and runtime restrictions are much less stringent than those on Eniac. The Biglab servers can be accessed directly using the command `ssh PENNKEY@biglab.seas.upenn.edu`, or from Eniac using the command `ssh biglab`.

In the downloaded directory you will find a Python program called `estimate`, which is an implementation of a simple quality estimator.  The estimator reads every (sentence, translation) pair in the dataset and scores the translation on the scale from 1-3 described above.

To run the quality estimator from the command line, using the following command:

	./estimate > output

This produces the file `output`, which will contain the scorings for each of the translations in the dataset.  You are then able to score your quality estimator using the following command:

	./grade < output

Your estimations will be scored by calculating the Mean Absolute Error rate against a hidden file of hand-annotated scores.

The Challenge
---

Currently, the default system is using all 13 of the provided baseline features to train a SVM regression model with grid search and cross validation to optimize the hyperparameters. (done via [SCIKit Learn Library](http://scikit-learn.org/0.13/auto_examples/grid_search_digits.html)).

One potential first step in improving your quality estimator would be to think of more features that might be useful in deciding the quality of a given translation.  Here are just a few ideas:

- Length of original sentence *
- Length of translation *
- Number of punctuation marks in original sentence *
- Number of punctuation marks in translation *
- Number of untranslated words in the translation sentence

Note: including the starred (*) features will be sufficient to get a score near the baseline system.

The default system is currently viewing each (sentence, translation) pair and attempting to evaluate the translation based off of only this information.  Since the dataset is actually ranking four different translations for each original sentence, you might group all of the translations together to make more educated estimations as to each of their quality. A possible feature could be to calculate average edit rate distance (Using TER metric), of a candidate translation, from the other translations, as a bad translation is likely not to be very similar to other translations.

Finally, you might also consult any of the following papers for more ideas:

- [Findings of the 2014 Workshop on Statistical Machine Translation](http://statmt.org/wmt14/pdf/W14-3302.pdf)
- [Ninth Workshop on Statistical Machine Translation](http://statmt.org/wmt14/book.pdf)
- [Estimating the Sentence-Level Quality of Machine Translation Systems](http://mt-archive.info/EAMT-2009-Specia.pdf)
- [Training a Sentence-Level Machine Translation Confidence Measure](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.187.4893&rep=rep1&type=pdf)
- Anything else that you find!

But the sky’s the limit! There are many, many ways to try to solve the decoding problem, and you can try anything you want as long as you follow the ground rules:

Ground Rules
---

- You must work **independently** on this assignment.

- You should submit each of the following:

	1. Your scorings of each (sentence, translation) pair **for the test dataset**, uploaded from any Eniac or Biglab machine using the command `turnin -c cis526 -p hwX hwX.txt`.  Note that you are unable to grade these scorings yourself, as you are not provided with the gold-standard scorings for the test data. You may submit new results as often as you like, up until the assignment deadline. The output will be evaluated using grade program. The top few positions on the leaderboard will receive bonus points on this assignment.

	2. Your code, uploaded using the command `turnin -c cis526 -p hwX-code file1 file2 ...`. This is due 24 hours after the leaderboard closes. You are free to extend the code we provide or write your own in whatever langugage you like, but the code should be self-contained, self-documenting, and easy to use.

	3. A report describing the models you designed and experimented with, uploaded using the command `turnin -c cis526 -p hwX-report hwX-report.pdf. This is due 24 hours after the leaderboard closes. Your report does not need to be long, but it should at minimum address the following points:

		- **Motivation:** Why did you choose the models you experimented with?

		- **Description of models or algorithms:** Describe mathematically or algorithmically what you did. Your descriptions should be clear enough that someone else in the class could implement them.

		- **Results:** You most likely experimented with various settings of any models you implemented. We want to know how you decided on the final model that you submitted for us to grade. What parameters did you try, and what were the results? Most importantly: what did you learn?

Since we have already given you a concrete problem and dataset, you do not need describe these as if you were writing a full scientific paper. Instead, you should focus on an accurate technical description of the above items.

Note: These reports will be made available via hyperlinks on the leaderboard. Therefore, **you are not required to include your real name** if you would prefer not to do so.