---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 5 | Sentence Alignment
active_tab: homework
---
# Identifying Multilingual Documents

In previous assignments you have primarily worked with data containing only one language. In practice
a document may contain multiple languages which may degrade the performance of machine translation systems. Identifying the language(s) present in a document has several
uses such as acting as a preprocessing step for machine translation as well as for acquiring bilingual texts from
mining web resources. __Your task will be to classify the proportion of languages present in a document.__

## Getting Started

To begin, download the starter kit for the assignment. You may either choose to develop locally or on Penn's servers. 
For the latter, we recommend using the Biglab machines, whose memory and runtime restrictions are much less stringent 
than those on Eniac. The Biglab servers can be accessed directly using the command `ssh PENNKEY@biglab.seas.upenn.edu`, 
or from Eniac using the command `ssh biglab`.

In the downloaded directory, you will find a Python program called "predict.py", which is an implementation of a simple language classifier.
This can be run as follows:

    ./predict.py > predictions

The classifier works by computing the $$k$$ most common byte n-grams over the monolingual training data for each language. Given a new
document to evaluate, the classifier indicates that a language is present if the document contains a n-gram that appeared in that
language's $$k$$ most common n-grams (in the training data). After computing the languages present, it simply outputs a probability distribution where each
language has equal proportion in the document. By default the classifier uses the settings $$k = 1000$$ and $$n = 5$$.

The output of the classifier can be scored with the following command:

    ./score.py < predictions

The objective function computed is the mean cosine similarity between the predicted distribution of languages and the true language distribution across the evaluated documents.
The cosine similarity between two vectors A and B is defined as:

$$\cos(A, B) = \frac{A \cdot B}{||A||\,||B||}$$

In the directory you will also find the training and blind test data for the assignment. The training data is split into monolingual and multilingual documents with a maximum of 5 languages in a single document. Each training data directory has a metadata file which describes the languages contained in each documents as well as the number of bytes for each language. The files in the testing data have been randomly assigned names and it is unspecified how many languages are contained in each document. For more details, see the README file in the main directory.

## The Challenge

Your task for this assignment is to __predict the distribution of languages as accurately as possible.__ We will make the assumption that the only languages
available are those specified in `selected_langs`, so it isn't possible for a document to contain a language not present in the training data. Your performance
will be evaluated against a blind test set which may contain any number of languages.

To start out, you might want to treat the problem as a binary classification where a language can be either present or not present within a document. Given a binary
vector, you can simply divide by the number of languages guessed to get a probability distribution. Later you can make your predictions more granular by guessing
the proportion of languages in the documents.

Some strategies you might find effective are:

 * Use character based n-grams
 * Calculate the relative n-gram probabilities per language
 * Consider unicode range based features
 * Train a naive-Bayes classifier
 * Segment the documents into monolingual sections and perform language identification
 * [Use a tf-idf based model (LINGUINI)](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.107.2022)
 * [Use a probabilistic mixture model](http://www.transacl.org/wp-content/uploads/2014/02/38.pdf)
 * [Correct for the token emission rate per language](http://www.transacl.org/wp-content/uploads/2014/02/38.pdf)

A classifier that uses relative n-gram probabilities and corrects for token emission rate should be sufficient to beat the baseline. (Note: this is subject to change)

But the sky's the limit! You can try anything you want, as long as you follow the ground rules.

## Ground Rules

* You must work independently on this assignment.
* You should submit each of the following:
    1. Your predictions of each of the language distributions for the documents in test. Upload your results with the command `turnin -c cis526 -p hwX hwX.txt` from any Eniac or Biglab machine. You can upload new output as often as you like, up until the assignment deadline.

    2. Your code, uploaded using the command `turnin -c cis526 -p hwX-code file1 file2 ....` This is due 24 hours after the leaderboard closes. You are free to extend the code we provide or write your own in whatever langugage you like, but the code should be self-contained, self-documenting, and easy to use.

    3. A report describing the models you designed and experimented with, uploaded using the command `turnin -c cis526 -p hwX-report hwX-report.pdf`. This is due 24 hours after the leaderboard closes. Your report does not need to be long, but it should at minimum address the following points:
           
        * __Motivation__: Why did you choose the models you experimented with?

        * __Description of models or algorithms__: Describe mathematically or algorithmically what you did. Your descriptions should be clear enough that someone else in the class could implement them.

        * __Results__: You most likely experimented with various settings of any models you implemented. We want to know how you decided on the final model that you submitted for us to grade. What parameters did you try, and what were the results? Most importantly: what did you learn?

         Since we have already given you a concrete problem and dataset, you do not need describe these as if you were writing a full scientific paper. Instead, you should focus on an accurate technical description of the above items.

          Note: These reports will be made available via hyperlinks on the leaderboard. Therefore, __you are not required to include your real name__ if you would prefer not to do so.

* You do not need any other data than what we provide. You are free to use any code or software you like, __except for those expressly intended to perform language detection__. You may examine how existing systems work, but you must write your own language detection code. If you want to use machine learning libraries or any other off-the-shelf resources, feel free to do so. If you aren't sure whether something is permitted, ask us.
