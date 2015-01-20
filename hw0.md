---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 0 | Setup
active_tab: homework
---

<div class="alert alert-info">
  Submissions will close at 11:59 pm on Tuesday, January 27.
</div>

Alien Translation & Leaderboard Setup
=============================================================

## Leaderboard

In this course, the coding portion of each assignment will be graded automatically
in real-time, with results being made available on the website leaderboard upon
submission. You may submit as many times as you like before the deadline, and only
your final result will be used when assigning an overall grade for the assignment.

## Homework 0

[This handout](handout1.pdf) shows a parallel corpus between two alien languages.
You'll use this data to translate some alien words and sentences.

1. Try to draw lines between the words that are translations of each
other. Start out by finding words that occur in multiple source sentences,
like "ghirok", and see if you can find words that appear in all of the
corresponding target sentences. Do you notice any patterns in how the
words translate? Do you think that the prefix "ok-" corresponds to something?
*You do not have to turn anything in for this step.*

2. [This file](hw0_input.txt) contains some words and sentences. Write a text
file named hw0.txt with your translations, one per line, and submit it on Biglab
using the following instructions.

## Submission Instructions

<b>Note: Submissions for this assignment will be accepted starting at 11:59 pm on
January 20, 2015.</b>

1. Create two additional text files of one line each with your desired leaderboard
alias (the name by which you will be identified on the leaderboard) and preferred
email address. Name them alias.txt and email.txt, respectively.

2. Log in to Biglab, either directly using the command
<pre>
  ssh PENNKEY@biglab.seas.upenn.edu
</pre>
or by first logging into Eniac then logging into Biglab using the command sequence
<pre>
  ssh PENNKEY@eniac.seas.upenn.edu
  ssh biglab
</pre>

3. From Biglab, submit your alias, email address, and translation files using the
following command:
<pre>
  turnin -c cis526 -p hw0 alias.txt email.txt hw0.txt
</pre>
It is crucial that you submit from Biglab, as this is the server on which the
leaderboard monitor script will be running. If you submit from another location,
your submission will be recorded, but your score will not be updated on the
leaderboard.

Any questions should be be posted on the
[course Piazza page](https://piazza.com/upenn/spring2015/cis526).
