---
layout: default
img: dore_babel
img_link: http://en.wikipedia.org/wiki/Confusion_of_tongues
caption: Gustav Dore, The Confusion of Tongues
title: Term Project
active_tab: project
---

<div class="alert alert-info">
  Each team's revised project write-up is available at the bottom of the page.
  Vote on your favorites using the indicated Piazza poll by 11:59 pm on April 16.
</div>

Term Project <span class="text-muted">The Ultimate Challenge</span>
=============================================================

Your term project is to design a homework assignment similar to the ones you have completed
in this class. Your project will consist of the following components:

1. A description of the problem, in the style and format of the homework write-ups.
1. Training and evaluation data.
1. A commented implementation of the simplest possible solution to the problem.
   This will be the default program provided to students. Name this file `default`.
1. A commented implementation of a baseline published in the literature, along with
   skeleton code obtained by removing the parts that students should implement.
   Name these files `baseline-solution` and `baseline`, respectively.
1. One extension per team member that attempts to improve on the baseline, along
   with a brief (one- to three-paragraph) accompanying write-up for each extension
   describing the general approach and whether it worked. Name these `extension-1`,
   `extension-2`, etc.
1. A scoring script implementing an objective function that can be used to score
   submissions on the class leaderboard. Naturally, the output of all model
   implementations should be gradeable with this program. Name this file `grade`.

## Implementation Specification

All implementations should be runnable using the following command:

    ./program | ./grade

More specifically, each implementation should write its results to `stdout`, and
the grading program should accept its input via `stdin`. Any logging information
should be printed to `stderr`. If any of your programs require command-line arguments,
such as paths to the data files, parameters for algorithms, etc., sensible defaults
should be provided in case they are omitted.

If you are writing your code in a scripting language like Python, be sure to
include the appropriate [hash-bang](http://en.wikipedia.org/wiki/Shebang_%28Unix%29)
line at the top of each file, such as `#!/usr/bin/env python`, so that your programs
can be executed directly.

If you are writing your code in a compiled language like Java, include a `Makefile`
so that your code can be compiled into the above form by calling `make`.

## Topics

You should identify what topic you would like to work on, and then email the instructor or schedule a meeting to go over the details.  Here are some ideas of topics that you could use for your final project: 

* Sentence alignment
* Language identification
* Transliteration
* Feature engineering for discriminative word alignment
* Language modeling 
* Predict the right translation given a context
* Phrase pair extraction
* SCFG rule extraction
* Implement the CKY algorithm for decoding SCFGs
* System combination
* Domain adaptation
* Lexicalized re-ordering
* Clause restructuring for better re-ordering
* Mining parallel documents from Common Crawl
* Extracting parallel sentences from Wikipedia
* Monolingual text-to-text generation 
* Predicting the goodness of translation without references
* Crowdsourcing translation
* Minimum Bayes risk re-ranking
* Predict the features in WALS for a language, given a subset of its features

You are welcome to choose your own topic, provided that it is related to machine translation.

## Final Submission
Your final submission should consist of the following in a compressed archive:
<pre>
README.md
data/
test-data/
default
baseline
extension-*
grade
</pre>
Submit the archive via turnin before the deadline.

## Milestones and Due Dates
* February 27 - term project assigned
* During Spring break (March 7-15) - Select your term project topic and email your instructor to say what you've selected
* March 17 - draft write-up is due.  Your write-up should include 
1. A problem definition
1. A pointer to or more more papers or sections textbook that describes the problem
1. What objective functions you are considering
1. What type of data you will need to evaluate
* March 24 - your data must be collected and submitted
* March 26 HW4 due
* March 31 - objective function must be implemented (it is OK to use an existing implementation if you need to), and your default system is due
* April 2 - revised write-up is due, taking into account any feedback from instructor and TA.  This will be published on the course web site.
* April 7 - your baseline system is due
* April 9 - Language Research "Core Requirements" are due
* April 14 - no late days allowed for this deadline - your completed project is due (final writeup, data, objective fn, default system, baseline system, and extensions).  It will be published on the class web site, and used as the final homework for other students.
* April 15-16 - Read over the other students' term projects, and vote on the ones that you think should be included. 
* April 28 - Final HW due: turn in your implementation of another student's term project
* April 28 - The language research project is also due.
* final exam date (May 12th) - *extra credit* implement one or more extensions beyond your own baseline, or do one or more of the new student-written HWs.

## Revised Project Write-ups
<p>
{% for i in (1...16) %}
<a href="{{site.baseurl}}/projects/team{{i}}.html">Team {{i}}</a>{% if i < 16 %},{% endif %}
{% endfor %}
</p>
