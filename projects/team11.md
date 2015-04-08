---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 5 | Sentence Alignment
active_tab: homework
---
Crowd-sourcing Translation
=========================


##Introduction
Statistical Machine Translation relies heavily on the availability of bilingual training data and high quality reference translation. Large parallel corpora mean more data to estimate the parameters of the translation model, which in turn implies better translation probabilites and higher translation quality. However, finding such bilingual corpora is not always feasible and building one from scratch can be a daunting and expensive task.

Similarly, one of the essential components of Statistical Machine Translation is high quality reference translation. An example of such translations is
	* *ﻑﺭﺎﻨﺳ کی ﺖﺟﻭیﺯ کی ﺢﻣﺍیﺕ
    which translates to **France has supported the proposal**

The traditional method of obtaining high translation quality is by employing professional translators or crawling the web for bilingual sources which prove to be very expensive and time consuming. 

It is possible to decrease the cost of both of the above problems by outsourcing the translation effort to non-professionals. Following are the crowd-sourced translations for the above

*	**defending the thinking of France.**
*	**Supporting the French proposal**
*	**France suggestion was appriciatable.**
*	**France has supported the proposal.**

However, naively considering these translations would lead to lower quality, inconsistent and biased results. 

The task is to use these crowdsourced translations and the metadata associated with it to obtain near professional level translation quality. Such crowdsourced translations can serve as bilingual training data as well as high quality translations.


##Data
The data has been split up into 3 files:

1.	**LDCTranslations.tsv** - This file has the sentence id to high quality translation mapping. We currently have 4 LDC Translations per Urdu sentence
2.	**turk_translations.tsv** - This file has the sentence id, TurkTranslation and worker_id mapping. Each Urdu sentence has 4 Turk Translations, each of which are provided alongside the worker_id. This is to facilitate joining this data with the high quality reference data, and worker metadata
3.	**survey.tsv** - This file has the worker_id to metadata mapping where data regarding the country of domicile, English proficiency and Urdu proficiency have been provided
4.	**lm_probabilities.tsv** - This file has the sentence id along with bigram and trigram log probabilities for every translation

###Test and train set
We plan to use 400 of the LDC Translations as the test set which will not be provided. This will be used for scoring on the leaderboard.
The rest of the translations may be used as references for training.

##Scoring function
The scoring objective function is the BLEU similarity of a chosen translated sentence to the 4 reference (LDC) translations. We have used a modified BLEU scoring function that calculates the ngram similarity with all of the 4 reference sentences instead of just 1. The BLEU score is then accumulated over the entire translation set.

The turker translations and metadata are to be used to generate features which then can be optimized using methods like PRO and MERT by using the reference translations given in the LDC Translations.

##Default System
The starter kit would include a file *__default.py__* which is an implementation of a simple crowd-sourced translator. The default system involves naively choosing the first translation and submitting it for scoring (i.e. writing it to standard out).
 
You now have simple program that chooses a corwd-sourced translation as the final translation. Test it out using this command:

*__default.py > output__*

We can compute the bleu score of the output using the command

*__compute-bleu.py < output__*

Currently this system gives a BLEU score of 0.239.

##Baseline
The baseline system will be an implementation of PRO with a few language model features namely:

*	bigram score
*	trigram score

These scores will be obtained by using the English Gigaword corpus.


##The Challenge
Your task for this assignment is to improve the accuracy of crowdsourced translations as much as possible. Implenting PRO with simple featurs like bigram and trigram scores should be enough to cross the baseline. 

However, you can implement the following extensions to further improve the accuracy:

*	[MERT](http://mt-archive.info/ACL-2011-Zaidan.pdf)
*	[Boosted MERT](https://ssli.ee.washington.edu/people/duh/papers/acl08boost.pdf)
*	[Feature Engineering {sentence level}][Crowdsourcing Translation]
*	[Worker metadata scoring][Crowdsourcing Translation]
*	[Sentence generation][Crowdsourcing Translation]

[Crowdsourcing Translation]:(http://www.cis.upenn.edu/~ccb/publications/crowdsourcing-translation.pdf)
    

 
## Ground Rules

*	You must work **in a group having a maximum of four members** on this assignment.
*	You must turn in three things:
	1.	Your translations of turk_translations.tsv. Upload your 			results with the command turnin -c cis526 -p hw5 hw5.txt 			from any Eniac or Biglab machine. You can upload new output 		as often as you like, up until the assignment deadline. You will only be able to see your results on test data after the deadline

	2.	Your code, uploaded using the command turnin -c cis526 -p hw5-code file1 file2 .... This is due 24 hours after the leaderboard closes. You are free to extend the code we provide or write your own in whatever langugage you like, but the code should be self-contained, self-documenting, and easy to use.
    
    3.	A report describing the models you designed and experimented with, uploaded using the command turnin -c cis526 -p hw5-report hw5-report.pdf. This is due 24 hours after the leaderboard closes. Your report does not need to be long, but it should at minimum address the following points:
    *	**Motivation**: Why did you choose the models you experimented with?
    *	**Description of models or algorithms**: Describe mathematically or algorithmically what you did. Your descriptions should be clear enough that someone else in the class could implement them.
	
    *	**Results**: You most likely experimented with various settings of any models you implemented. We want to know how you decided on the final model that you submitted for us to grade. What parameters did you try, and what were the results? Most importantly: what did you learn?
    
    *	Since we have already given you a concrete problem and dataset, you do not need describe these as if you were writing a full scientific paper. Instead, you should focus on an accurate technical description of the above items.

Note: These reports will be made available via hyperlinks on the leaderboard. Therefore, **you are not required to include your real name** if you would prefer not to do so.


*	You do not need any other data than what we provide. You are free to use any code or software you like, except for those expressly intended to translate the urdu input sentences. You must write your own crowdsourcing translator. If you want to use machine learning libraries, taggers, parsers, or any other off-the-shelf resources, feel free to do so. If you aren’t sure whether something is permitted, ask us. If you want to do system combination, join forces with your classmates.You do not need any other data than what we provide. You are free to use any code or software you like, except for those expressly intended to generate or rerank translation output. You must write your own reranker. If you want to use machine learning libraries, taggers, parsers, or any other off-the-shelf resources, feel free to do so. If you aren’t sure whether something is permitted, ask us. If you want to do system combination, join forces with your classmates.

    