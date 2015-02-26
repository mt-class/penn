---
layout: default
img: tower_of_babel
caption: Tower of Babel by Pieter Bruegel the Elder (1563)
title: Language Research
active_tab: lin10
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
1. If the language uses a script other than the Roman alphabet, what script does it use?  Are there any major publications that are written in the language?  What faction of the speakers of the language can write in it?  Are there any impediments (e.g. no keyboards for the script)?  
1. Do any online machine translation systems exist for the language?  If so, give an example of an English translation of a paragraph from a Wikipedia article written in that language.  Have there been any machine translation research papers that examine the language?  The [Machine Translation Archive](http://www.mt-archive.info/) is a good place to look for research papers.


Additional requirements that vary by group size.  Please pick N of these, where N is the size of your group:

1. Harvest monolingual data written in that language.  Good sources for this might be: 
- [Wikipedia database dumps](http://en.wikipedia.org/wiki/Wikipedia:Database_download#Other_languages)
- Web crawling international news services like the [Voice of America](http://www.voanews.com/#), the [BBC World Service](http://www.bbc.co.uk/ws/languages), [Deutsche Welle](http://www.dw.de), [Global Voices](http://globalvoicesonline.org/#)
- *Save the text in Unicode format.  Include all the raw text in a single file or in a tarball with multiple files.  Include a section in your writeup that gives the sources of your data, and describes how much data you were able to collect.  If you had a clever way of finding more data, describe your method.*
1. Try to find bilingual data for the language.  Potential sources for this are:
- The Bible, The Universal Declaration of Human Rights, [Global Voices](http://globalvoicesonline.org/#)
- Inter-linear glosses from grammar books about the language
- Finding a bilingual speaker of the language of the language and asking him or her to translate some sentences for you
- *Save the bitext in Unicode format.  Either include two files (one in English and one in your language) that are sentence-aligned with one sentence per line, or a tarball with parallel documents.  Include a section in your writeup that gives the sources of your data, and describes how much data you were able to collect.*



Here are some useful resources for researching languages:

* Check out books on the language from the library. 
* [World Atlas of Language Structures](http://wals.info/)
* [Ethnologue](http://www.ethnologue.com/)
* [Omniglot](http://www.omniglot.com/)
* [About World Languages](http://www.aboutworldlanguages.com/)
* [Machine Translation Archive](http://www.mt-archive.info/)



