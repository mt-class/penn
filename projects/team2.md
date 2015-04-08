---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 5 | Sentence Alignment
active_tab: homework
---
Ayla Taylor
Brian Sladek
Casey Davis
Harry Okun

Topic:

Part-of-speech taggers (POS taggers) improve translation by assigning tags to words in a sentence based on the syntactic context of the word. This can create a more accurate translation because it helps eliminate ambiguity and align the words more accurately. Unfortunately, POS taggers are generally language specific and are difficult, time consuming and require a lot of resources to create. Languages such as English have been extensively tagged and there are many treebanks created for it. A treebank is a database of parsed syntactic trees created with a POS tagged data. POS tagging for languages with fewer resources is much more difficult but no less important. This is why language independant POS taggers are interesting to us. Language independant POS taggers use dependency trees in order to parse the sentence, align the target language with the tagged input language and project the dependency tree on to the aligned target language. 

Project Description:

The goal of the project is to create a language-independent part-of-speech tagger that uses a direct projection algorithm (Hwa, Resnik, Weinberg) to project an English POS dependency tree onto a word-aligned French translation. The output will be the POS tags assigned to the French translations. 

Our default system will select a random POS tag for each word.

Our baseline system will use the direct projection algorithm in “Breaking the Resource Bottleneck for Multilingual Parsing (Hwa, Resnik, Weinberg).

Academic Papers:

Breaking the Resource Bottleneck for Multilingual Parsing
http://www.umiacs.umd.edu/users/weinberg/hrw02-3.pdf

A Universal Part-of-Speech Tagset
http://petrovi.de/data/universal.pdf

Scoring Functions:

For the scoring, we will compare the output to a French Gold Standard POS-tagged text and judge it based on the similarity.


Training/Evaluation Data:

This project requires three resources. First, it requires a dependency parser to be used on the source language. In our case, we have chosen to use Stanford dependency parser. It also requires a word aligner to align the words in the source language to the words in the target language. Finally, it requires a parallel sentence aligned English-French corpus. In the evaluation phase, we plan to demonstrate the effectiveness of the model by comparing its output to a preexisting French POS-tagged text.
