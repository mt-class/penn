\
<span> — Tuesday, March 17</span>\
<span>Due Tuesday, April 28, 11:59pm online</span>

Introduction {#introduction .unnumbered}
============

The goal of previous homework assignments was to take a given data set
and optimize something, whether it was model alignment, model decoding,
an evaluation metric, or translation ranking. On this homework
assignment, the data are what we are optimizing! Lots of classic machine
translation datasets are curated, but that does not scale as well as
harvesting translations organically. A common place to find them is by
crawling the Internet; many websites are translated into multiple
languages. We call these \`\`parallel" websites. The question of this
assignment is:\

**How can you algorithmically determine which websites are parallel?**\

We will study a fragment of the [Common
Crawl](http://commoncrawl.org/the-data/get-started/) dataset, a
collection of *petabytes* of website content crawled over the last seven
or so years. You will algorithmically determine which websites are
English-French parallel and feed those into a provided aligner and
decoder. Thus, you are trying to optimize model score via pairing
websites! We will use BLEU as an evaluation of model quality.

Getting Started {#getting-started .unnumbered}
===============

To begin, download the Homework 5 starter kit. You may either choose to
develop locally or on Penn servers. For the latter, we recommend using
the Biglab machines, whose memory and runtime restrictions are much less
stringent than those on Eniac. The Biglab servers can be accessed
directly using the command `ssh PENNKEY@biglab.seas.upenn.edu`, or from
Eniac using the command `ssh biglab`.\

In the downloaded directory you will find a Python program called
<span>mine</span>, which crawls through the provided sandbox of sites
and selects pairs simply based on URLs. Run the program with the command
<span>./mine \> output</span> in your terminal.\

This process is implemented following a simplified version of the
[MapReduce](http://en.wikipedia.org/wiki/MapReduce) parallel
architecture, and is based on the first step, *Candidate Pair
Selection*, of the STRAND algorithm (Resnik and Smith, 2003).\

The mapper first scans the sites for those with a URL that contains one
of the following terms:

-   <span>english</span>

-   <span>french</span>

-   <span>en</span>

-   <span>fr</span>

The latter two should be more common, as they are valid
[ISO-639](http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) codes.
If such code is found and is surrounded by non-alphanumeric characters,
the URL is identified as a potential match. The mapper then outputs a
key-value pair, where the key is the URL with the code replaced by \*,
and the value is a tuple containing the original URL, the language name,
and the full HTML markup of the page. For example, if the URL
<span>www.mt-class.org/fr/</span> is found, the following key-value pair
is output:

-   *Key*: <span>www.mt-class.org/\*/</span>

-   *Value*: <span>www.mt-class.org/fr/</span>, French, (full
    corresponding HTML page)

The reducer then receives all of the values mapped to the same
generalized, language-independent URL, and searches the values for the
existence of both English and French versions of the site. If we do have
the same site in both languages, then the corresponding pairs are
generated and fed into the data as parallel English and French
documents.\

This URL-based matching is a simple and inexpensive solution to the
problem of mining candidate document pairs of English and French
documents. The algorithm is relatively fast, as it only looks at the
URLs of the sites and ignores the HTML markup. The nature of the web
suggests that sites with URLs differing only by a short language code
can act as good translations of each other, so this mining program gives
our translation models solid data. To see how the provided translation
models performed given your mined document pairs, simply run
<span>./grade \< output</span>. This `grade` program takes your data and
runs IBM Model 1 with it to generate a translational model. It then
takes the model created and attempts to make alignments on our test set
of French sentences. The accuracy of the alignments compared to
human-made alignments is used for your score. In a general sense, more
data and better data should correlate with a higher alignment score.\

This model, though efficient, likely misses many good potential
candidates. In fact, for many mining algorithms, it’s only the first
step in finding parallel documents.

The Challenge {#the-challenge .unnumbered}
=============

Your task for this assignment is to **algorithmically identify pairs of
parallel English-French websites**. As mentioned in the previous
section, we have provided you with a default Python program called
`mine` which identifies pairs based on URLs based on particular shared
terms and the existence of English and French versions of a particular
site.\

The baseline that you must beat was achieved by [automatically
identifying URL pair
patterns](http://personal.cityu.edu.hk/~ctckit/papers/Kit-Ng_URLpairing-PID483174.pdf).
This paper is a more nuanced implementation of the default heuristic.\

You might consider implementing the following approaches as inspiration
in identifying parallel websites:

-   [STRAND Algorithm](http://www.aclweb.org/anthology/J03-3002.pdf)

-   [BITS
    Algorithm](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.75.2378&rep=rep1&type=pdf)

-   [An Iterative Link-based Method for Parallel Web Page
    Mining](http://nlp.cs.rpi.edu/paper/yuemnlp2014.pdf)

-   [PTMiner
    Algorithm](http://citeseerx.ist.psu.edu/viewdoc/download;jsessionid=7DF7F8E5C85636EAFE29EE7BD13FE09E?doi=10.1.1.25.7941&rep=rep1&type=pdf)

-   Create your own algorithm (be sure to document in report)

Ground Rules {#ground-rules .unnumbered}
============

-   You must work **independently** on this assignment.

-   You should submit each of the following:

    1.  Your parallel pairings uploaded from any Eniac or Biglab machine
        using the command `turnin -c cis526 -p hw5 hw5.txt`. You may
        submit new results as often as you like, up until the assignment
        deadline. The output will be evaluated using `grade` program.
        The top few positions on the leaderboard will receive bonus
        points on this assignment.\

    2.  Your code, uploaded using the command
        `turnin -c cis526 -p hw5-code file1 file2 ...`. This is due 24
        hours after the leaderboard closes. You are free to extend the
        code we provide or write your own in whatever language you like,
        but the code should be self-contained, self-documenting, and
        easy to use.\

    3.  A report describing the models you designed and experimented
        with, uploaded using the command
        `turnin -c cis526 -p hw2-report hw2-report.pdf`. This is due 24
        hours after the leaderboard closes. Your report does not need to
        be long, but it should at minimum address the following points:

        -   **Motivation**: Why did you choose the models you
            experimented with?

        -   **Description of models or algorithms**: Describe
            mathematically or algorithmically what you did. Your
            descriptions should be clear enough that someone else in the
            class could implement them.

        -   **Results**: You most likely experimented with various
            settings of any models you implemented. We want to know how
            you decided on the final model that you submitted for us to
            grade. What parameters did you try, and what were the
            results? Most importantly: what did you learn?

        Since we have already given you a concrete problem and dataset,
        you do not need describe these as if you were writing a full
        scientific paper. Instead, you should focus on an accurate
        technical description of the above items.\

        Note: These reports will be made available via hyperlinks on the
        leaderboard. Therefore, **you are not required to include your
        real name if you would prefer not to do so.**

-   You do not need any other data than what is provided. You should
    feel free to use additional codebases and libraries **except for
    those expressly intended to do parallel detection for you**. You are
    free to use tools for DOM parsing, such as TagSoup, JTidy, etc. You
    are also free to use third-party tools to classify languages based
    on content. This may be useful if the website does not contain any
    metadata on what language it is in.


