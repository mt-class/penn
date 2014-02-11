---
layout: default
img: rosetta
img_url: http://www.flickr.com/photos/calotype46/6683293633/
caption: Rosetta stone (credit&#59; calotype46)
title: Homework 1 | Alignment
active_tab: homework
---

Homework & Leaderboard setup
=============================================================


In this course, there will be five homework assignments (not counting this one).  These assignments will be automatically graded and their results will be uploaded to a leaderboard, which will show assignment-level and cumulative leaders for each assignment.

## Do this now

In order to setup the homework environment, please do the following:

1. Sign up for our [Piazza course](http://www.piazza.com/upenn/spring2014/cis526).

2. Email <a href="mailto:jonny@cs.jhu.edu">Jonny Weese</a> with the following information:
* Your name
* Your preferred email address
* A handle (this will identify you on the leader board)
* A base URL where we can download your assignments (see below)
* Upload your "assignment0.txt" file to your designated location.  This file should contain a single number.

## Base URL

The leaderboard will score and rank your assignments, both on a
per-assignment and cumulative basis.  In order to automate this
process, we need to know where to find your files.  Each assignment in
the course will be submitted by making available the output of your
  assignment at some set location.  This location will be:

<pre>
  BASE_URL/assignment0.txt
  BASE_URL/assignment1.txt
  BASE_URL/assignment2.txt
  BASE_URL/assignment3.txt
  BASE_URL/assignment4.txt
  BASE_URL/assignment5.txt
</pre>

If you have web space (say on the CS servers), your BASE_URL could be
there (e.g., <code>cs.jhu.edu/~post/mt-class</code>).  If you don't
have access, you can used a service
like <a href="http://db.tt/OH6VZxS">Dropbox</a>.  Dropbox, in
particular, allows you to create a folder in your Public folder and
then share the URL with us (it will look something
like <code>http://dl.dropbox.com/u/23493939/mt-class/assignment0.txt</code>).

If you have questions about this or problems producing a BASE_URL,
please post them to Piazza.


## Assigment 0

[This handout](handout1.pdf) shows a parallel corpus
between two alien languages. You'll use this data to translate some
alien words and sentences.

1. Try to draw lines between the words that are translations of each
other. Start out by finding words that occur in multiple source sentences,
like "ghirok", and see if you can find words that appear in all of the
corresponding target sentences. Do you notice any patterns in how the
words translate? Do you think that the prefix "ok-" corresponds to something?
*You do not have to turn anything in for this step.*

2. [This file](hw0_input.txt) contains some words and sentences,
one per line. Write a text file with your translations. Name this file
assignment0.txt and upload it to the base URL that you sent to Jonny.

## Leaderboards

The leaderboards will be posted as soon as there is data.  The board
will work as follows: as soon as a homework is posted, we will look
for assignment solutions for every student every 5 minutes.
These files will be scored and used to update the leader board.  You
can change this file as often as you'd like; we will use the latest
results.  Your official project turn-in will be whatever we download
at project due time, which will typically be 11:59 PM EDT.
