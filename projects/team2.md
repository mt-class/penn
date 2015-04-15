---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 5 | POS Tagging
active_tab: homework
---
# Homework X: POS Tagging

Part-of-speech taggers (POS taggers) improve translation by assigning tags to words in a sentence based on the syntactic context of the word. This can create a better translation because it helps eliminate ambiguity and align the words more accurately. Unfortunately, POS taggers are generally language specific and their creation takes a great deal of time and resources. Fortunately, it is possible to use preexisting treebanks to tag sentences in different languages. A treebank is a database of parsed syntactic trees created with POS tagged data. Language independent POS taggers use dependency trees from those treebanks in order to parse the sentence, align the target language with the tagged input language and project the dependency tree on to the previously POS-less language. Your mission is to create a language-independent part-of-speech tagger that projects an English POS dependency tree onto a word-aligned German translation.

### Getting Started
To begin, download our files. You will see a Python program called baseline, which contains the beginnings of a basic language-independent part-of-speech tagger.
Run the program the command:
./baseline > output
You will notice nothing has happened. This is because the baseline is basically empty. However, if the baseline were to produce something you would be able to grade it using the following command:
./grade-dev < output
This will return the percentage of correct tags.

### The Challenge
Your task is to write the entire program. First write an aligner that outputs the same sentences from the input appended with ::: and the correct parts of speech. This alone should be enough to match or beat our baseline and way more than enough to beat our default, which just assigns POS’s randomly. We suggest you look at the papers “Breaking the Resource Bottleneck for Multilingual Parsing” (http://www.umiacs.umd.edu/users/weinberg/hrw02-3.pdf) and “A Universal Part-of-Speech Tagset” (http://petrovi.de/data/universal.pdf) for advice and to learn more about the problem.
Now your job is to improve it! You see, a basic alignment system does not produce perfect results because one-to-many and many-to-one alignments can result in faulty or absent tags. To get full credit you must experiment, and here are some ideas:
... Identify certain non-word tokens in an independent manner and use that to guide their tagging
... Analyze the German language for usable POS patterns
... Consider the relationship between English words and their German translations for other usable POS patterns
... Use probability training to estimate the most likely POS tags for incorrectly tagged words. Either use this as a guide for words that failed to be tagged, or replace all tags with the most common one for their words

But the sky’s the limit! You can try anything you want, as long as you follow the ground rules.

### Ground Rules
You must work independently on this assignment.
You should submit each of the following:
1. An POS-tagged version of the entire dataset.
2. Your code. This is due either on April 28th or 24 hours afterwards, depending on what Professor Callison-Burch says on the subject. You are free to extend the code we provide or write your own in whatever language you like, but the code should be self-contained, self-documenting, nonsentient, and easy to use.
3. A report describing your code, including comprehensive explanations of the following:
Motivation: Why did you choose the models you experimented with?
Description of models or algorithms: Describe mathematically or algorithmically what you did. Your descriptions should be clear enough that someone else in the class could implement them

### Results
You most likely experimented with various approaches. Explain how you decided on the final model that you submitted for us to grade. Please include actual values from the provided grading program. State what you learned as well.
You may only use data or code outside of what is provided with advance permission. We will ask you to make your resources available to everyone. You must write your own code.
