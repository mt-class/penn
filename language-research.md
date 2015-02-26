---
layout: default
img: tower_of_babel
caption: Tower of Babel by Pieter Bruegel the Elder (1563)
title: Language Research
active_tab: language-research
---


So far in the class we have mainly focused on the mathematics and algorithms that underly machine translation systems,  In this project we will examine properties of languages. Some of these properties make machine translation more challenging.   You will form a team and research a language.  You will:

* collect information about the syntax and morphology of the language
* investigate where it is spoken, and what other languages its speakers are exposed to
* describe its writing system
* gather monolingual and bilingual data for the language
* create your own natural language processing tools for the language

For this exercise, we are going to target *low resource languages* which are languages that are not handled well by statistical machine translation, usually because they have an insufficient amount of training data.  Here is a [list of languages that you can choose from](list-of-languages.html).  If you would like to pick a different language, please ask for permission from your instructor. This project is inspired by the upcoming [DARPA Low Resource Languages for Emergent Incidents (LORELEI) Program](http://www.darpa.mil/Our_Work/I2O/Programs/Low_Resource_Languages_for_Emergent_Incidents_(LORELEI).aspx).

This is a group project, and you can work in teams of 2-6 people (the amount of work that we expect from you will vary depending on the size of your team).


Core requirements for all teams regardless of size:

1. Write a 1 page description about the syntax and morphology of the language that includes information like the following:
	- What is the canonical order of the elements of the sentence (like Subject/Object/Verb, and  Adjective/Noun)?  
	- Does the language have free word order, or does the order tend to be fixed?
	- What sort of inflectional morphology does the language use?  Do verbs inflect for person, number, tense? 
	- How many cases and/or genders does the language have?
	- What pronouns does the language have? Are they differentiated by levels of politeness?
	- *[The World Atlas of Language Structures online](http://wals.info) is a great source of this information. If possible, your writeup should give examples of sentences in the language that illustrate these properties, along with [interlinear glosses](http://en.wikipedia.org/wiki/Interlinear_gloss) into English.  You can often find these in the grammar books that WALS cites as evidence for the features.  Grammar books are often available in the Penn library, and are sometimes available for purchase from Google.*
1. Give about a half page description of who speaks the language.  How many speakers of the language are there?  Where is the language spoken?  What other languages are people in that area exposed to?  Are most people bilingual?  Include a map showing where the language is spoken.
1. If the language uses a script other than the Roman alphabet, what script does it use?  Is it written left-to-right like English or right-to-left like Arabic?  Are there any major publications that are written in the language?  What faction of the speakers of the language can write in it?  Are there any impediments (e.g. no keyboards for the script)?  
1. Do any online machine translation systems exist for the language?  If so, give an example of an English translation of a paragraph from a Wikipedia article written in that language.  Have there been any machine translation research papers that examine the language?  The [Machine Translation Archive](http://www.mt-archive.info/) is a good place to look for research papers.  Can you find any other existing NLP tools (for instance, parser or treebanks or morphological analyzers)?


Additional requirements that vary by group size.  Please pick N of these, where N is the size of your group:

1. Harvest monolingual data written in that language.  Good sources for this might be: 
- [Wikipedia database dumps](http://en.wikipedia.org/wiki/Wikipedia:Database_download#Other_languages)
- Web crawling international news services like the [Voice of America](http://www.voanews.com/#), the [BBC World Service](http://www.bbc.co.uk/ws/languages), [Deutsche Welle](http://www.dw.de), [Global Voices](http://globalvoicesonline.org/#)
- After you have found some samples of the language's writing, try searching for some of its words using Google.  Can you find web pages written in the language that way?
- *Save the text in Unicode format.  Include all the raw text in a single file or in a tarball with multiple files.  Include a section in your writeup that gives the sources of your data, and describes how much data you were able to collect.  If you had a clever way of finding more data, describe your method.*
1. Try to find bilingual data for the language.  Potential sources for this are:
- The Bible, The Universal Declaration of Human Rights, [Global Voices](http://globalvoicesonline.org/#)
- Inter-linear glosses from grammar books about the language
- Finding a bilingual speaker of the language of the language and asking him or her to translate some sentences for you
- *Save the bitext in Unicode format.  Either include two files (one in English and one in your language) that are sentence-aligned with one sentence per line, or a tarball with parallel documents.  Include a section in your writeup that gives the sources of your data, and describes how much data you were able to collect.*
1. Collect all interlinear glosses from one or more grammar books about the language.   Produce a unicode CSV file with the following columns:
- The foreign sentence
- The interlinear gloss (with an equal number of words as the foreign sentence)
- A translation into English
- A label for what linguistic phenomena this sentence illustrates (this could come from the chapter heading from the grammar book, or it can be one of the labels from WALS.info).
1. If the language is written in a non-Roman script, build a transliteration system for it.  Transliteration is the process of writing out how a word sounds in another script.  In MT is is especially useful for out of vocabulary words which are often names that never occurred in the bilingual training data. 
- The process of transliteration can be handled with the machine translation mechanisms (including using the word aligner to align characters across scripts, and the decoder to find the best transliteration).  It is an easier problem since it doesn't require reordering.  
- Read this paper on [transliterating from all languages](http://www.cis.upenn.edu/~ccb/publications/transliterating-from-all-languages.pdf) by my former PhD student to see how she did it.  
- Use your word aligner and decoder to do transliteration, or use an existing MT system like Moses or Joshua to do transliteration. You can [download the transliterated name pairs](http://www.cis.upenn.edu/~ccb/data/transliteration/wikipedia_names.gz) described the paper and use them as training data.
- Add a section to your writeup saying how your transliteration system works, and do an informal evaluation of its quality.
1. Build a [language identification system](http://en.wikipedia.org/wiki/Language_identification) that is able to predict whether a sample of text is written in the language or not.
- Add a section in your writeup that describes what training data you used, how your model works, and how you evaluated its accuracy.  
- You can build a model using language-model style features, or you could design a discriminative model that takes into account additional features like unicode ranges.
1. Does the language have a presence on Twitter?  Use the --location-query of this  [Twitter stream scraper](https://github.com/inactivist/twitter-streamer) to harvest tweets from the regions that speak the language.   You can find latitude and longitude coordinates for a geographic region using this [bounding box tool](http://boundingbox.klokantech.com).  
- How many tweets were you able to capture from that region?
- Label the language using in the tweets.  This can be a binary label that indicates whether or not a tweet is written in your language of interest.
- What faction of the tweets in that region were written in the language? 
- If you were not able to locate any tweets in that geographic region, try querying Twitter for some high frequency words in the language. 
- Add a section to your write-up describing your effort to collect tweets.  Save a file with the tweets that you collected (including their metadata), and a file with your labeled tweets.
1. Build a [named entity tagger](http://en.wikipedia.org/wiki/Named-entity_recognition) for the language.  To train the system, you can use an existing piece of software like  [Stanford's CoreNLP](http://nlp.stanford.edu/software/crf-faq.shtml) or [OpenNLP](http://opennlp.apache.org/documentation/1.5.3/manual/opennlp.html#tools.namefind.training).  The key to this project will be to create annotated training data in the language that labels names of people, and possibly of locations and organizations. 
- Train a named entity recognizer for your language
- Perform an informal evaluation.  Does your NER system label any words that did not occur in your training data?  Do those appear to be names?  Alternately, hold out some of the names from your training data.  Did you system spot them?
- Add a section to your writeup describing what you did.
1. Collect a bilingual dictionary for the language.  You can do this by finding a native language informant to translate words for you, or you can find an online dictionary and harvest entries from it.  Your dictionary should be in a CSV file with the following fields:
- The foreign word
- Its English translation
- the source of the translation
- (optional) an example sentence that uses the word
- (optional) its part of speech 
- (optional) a definition
1. Find a language informant at Penn who knows both English and the language that you are researching.  Have the language informant assemble bilingual sentence pairs and create manual word alignments for the language.
- Here is [a word alignment interface](word-alignment-HIT-template.html) that you could use.  It is designed to be used with Amazon Mechanical Turk.
- Describe what you did in your writeup.  Save the data somewhere.
1. Try to locate language informants on a crowdsourcing platform like Mechanical Turk or CrowdFlower.  How can you ensure that they actually speak the language?  Design a test to ensure that the crowdworkers know the language:
- Try implementing [a language proficiency test for the language](https://tacl2013.cs.columbia.edu/ojs/index.php/tacl/article/download/414/88) or show them images paired with captions in the language (which you could draw from Wikipedia) and ask them to select the correct caption for the image.  
- Gather some info about the people who participate, like what their native language is, how many years they have spoken English, how many years they have spoken your language, what country were born in, and what country  they live in now.
- In your writeup give a description of what language tests you implemented, how many crowd workers attempted it, and how many passed.  
1. Create tables of [inflectional paradigms](http://en.wikipedia.org/wiki/Inflection) for some of the words in your language. 
- What types of words inflect?  Verbs? Nouns? What are the different things that they inflect for? 
- Add a description to your writeup that shows how words inflect in the language.  Give your tables, and carefully label the columns to show what features it reflects (things like singular versus plural, masculine versus feminine versus neuter, etc).



Here are some useful resources for researching languages:

* Check out books on the language from the library. 
* Wikipedia
* [World Atlas of Language Structures](http://wals.info/)
* [Ethnologue](http://www.ethnologue.com/)
* [Omniglot](http://www.omniglot.com/)
* [About World Languages](http://www.aboutworldlanguages.com/)
* [Machine Translation Archive](http://www.mt-archive.info/)



