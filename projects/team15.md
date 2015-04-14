---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 5 | Sentence Alignment
active_tab: homework
---
# Implement the CKY algorithm for decoding SCFGs


The task is to translate from Urdu to English using existing grammar rules and language models. 


##Getting Started

To begin, download the starter kit. You may either choose to develop locally or on Penn servers. For the latter, we recommend using the Biglab machines, whose memory and runtime restrictions are much less stringent than those on Eniac. The Biglab servers can be accessed directly using the command ssh PENNKEY@biglab.seas.upenn.edu, or from Eniac using the command ssh biglab.

You can run the default system using the command:

```python
./default > 1-best.en
```

The default system uses a very simple method to generate a translation based on word-to-word translation and ignores complex grammars and language models. 
	
	word_e = argmax_P(word_e|word_f) word_e

To test the result, use the command:

```python
	./grade < 1-best.en
```
The program will evaluate the translation using BLEU score, and print the BLEU stats features as well as the BLEU score.

Your task is to improve the score as much as possible.


####Data
-----
_The './data' including following parts:_

1. grammar

  * The format of grammar is as follows:
  
      [S] ||| [X,1] ||| [X,1] ||| features ||| alignment

      for example
  
      [X] ||| " " جالبین " بھی ||| " " jalibeen " " also ||| 8.93287 6.92926 1 1.00000 0 0 ||| 0-0 1-1 2-2 4-5

  * The features are:
  
      p(e|f) p(f|e) Lex(e|f) Lex(f|e) rarity phrase-penalty

  * Alignment is:
  
      source_word-target_word

2. lm

  lm contains 1-grams, 2-grams and 3-grams language model. 

3. train data, tune data

  tune data are for students to tune the CKY model. 
  You may want to train your grammar model by yourself. Then you can use the train data to extract the SCFG rules.
  test data includes the test sentences you need to translate.

_The './test-data' including the sentence to be translated:_



##The Challenge
----
Your work is to increase the BLEU score as much as you can. Implementing CKY algorithm and integrating language model with some simple feature engineering will help you beat the baseline. Specific algorithm can be found on textbook chapter 11. To improve:
  * Adding LM, TM, Alignment feature to choose rules
  * Using MERT/PRO to tune the weight of the features
  * Increase the number of candidates hypothesis for original language intervals 
  * [Details about CKY](http://pages.cs.wisc.edu/~agorenst/cyk.pdf)  
  * [pseudo code for CKY](http://pages.cs.wisc.edu/~agorenst/cyk.pdf)
  * [An efficient version of CKY](http://www.petrovi.de/data/iwpt11.pdf)
  
##Ground rules
------
1. You must work independently on this assignment.

2. You should submit each of the following:

	An alignment of the entire dataset, uploaded from any Eniac or Biglab machine using the command turnin -c cis526 -p hw1 hw1.txt. You may submit new results as often as you like, up until the assignment deadline. The output will be evaluated using a secret metric, but the grade program will give you a good idea of how well you’re doing. The top few positions on the leaderboard will receive bonus points on this assignment.

	Your code, uploaded using the command turnin -c cis526 -p hw1-code file1 file2 .... This is due 24 hours after the leaderboard closes. You are free to extend the code we provide or write your own in whatever langugage you like, but the code should be self-contained, self-documenting, and easy to use.

	A report describing the models you designed and experimented with, uploaded using the command turnin -c cis526 -p hw1-report hw1-report.pdf. This is due 24 hours after the leaderboard closes. Your report does not need to be long, but it should at minimum address the following points:

	* **Motivation**: Why did you choose the models you experimented with?
	* **Description of models or algorithms**: Describe mathematically or algorithmically what you did. Your descriptions should be clear enough that someone else in the class could implement them. What are your models? How did you optimize them? How did you align with them? What were the values of any fixed parameters you used?
	* **Results**: You most likely experimented with various settings of any models you implemented. We want to know how you decided on the final model that you submitted for us to grade. What parameters did you try, and what were the results? If you evaluated any qualities of the results other than AER, even if you evaluated them qualitatively, how did you do it? Most importantly: what did you learn?

	Since we have already given you a concrete problem and dataset, you do not need describe these as if you were writing a full scientific paper. Instead, you should focus on an accurate technical description of the above items.

	Note: These reports will be made available via hyperlinks on the leaderboard. Therefore, you are not required to include your real name if you would prefer not to do so.


