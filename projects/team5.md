# Team members
Quanze Chen, Brendan Callahan, Chenyang Lei

# Extracting parallel sentences from Wikipedia
In the traditional approach to creating bilingual corpora, first parallel documents are created/discovered. This is followed by the alignment of parallel sentences within those parallel documents. Lastly, this is proceeded by aligning the words in each of the already aligned sentences.

When using a data source such as wikipedia, the traditional approach falls down. The traditional approach expects near-identical parallel documents, however using Wikpedia as our data source, what we actually have are comparable documents. Once aligned via mechanisms such as interwiki links or other annotations, we know that these documents are about the same subject, however there can be a large variance in content. (Do we want to list any papers here, like http://msr-waypoint.com/pubs/68886/sent-align2-amta-final.pdf?)

For example, we may have an English document about the French Alps that has 15 sentences, but French version might have 150 sentences. Even in that relatively extreme example, we could still have the opposite scenario where the English document contains sentences that the French one does not. Also there are possible cases where one sentence may align to multiple sentences in the other language. The hard part is identifying as many such pairs of sentences as we can so that we can fully use the dataset.

# Getting started

To begin, download the Term project starter kit. You may either choose to develop locally or on Penn’s servers. For the latter, we recommend using the Biglab machines, whose memory and runtime restrictions are much less stringent than those on Eniac. The Biglab servers can be accessed directly using the command ssh PENNKEY@biglab.seas.upenn.edu, or from Eniac using the command ssh biglab.

For training data, in the downloaded directory and under the data sub-directory, you will find several files. The files named `orig.enu.snt` and `orig.esn.snt` contain the original wikpedia articles, aligned, and split so that there is one sentence per line. The next set of files `pairs.enu.snt` and `pairs.esn.snt` contain just the parallel sentences that were found and are to be used as training data. The final file `pairs.label` contains a small sample of output data that the `grade` script will use to locally grade your system. You can also find a `dict.es` file which contains a Spanish-English dictionary.

In the downloaded directory you will find a Python program called align, which performs alignment based on the coverage of the Spansh translations onto the English sentence. For each pair of sentences $$e \in E$$ and $$s \in S$$ for a matching document pair $$(E, S)$$, we define coverage as follows:

<center>
$$C(e, s) = \frac{|e \cap T(s)|}{|e|}$$
</center>

Where $$T(s)$$ is the set of English words that correspond to some Spanish word in $$s$$ according to a dictionary. The default system will only pick a sentence pair if this coverage is over a cutoff of $$c = 0.6$$. To pick out sentence pairs, we maximize the coverage for each pair. Observing that sentence have weaker relations when they are farther away from each other, we impose a distance penalty when ordering the scores $$p^{|i - j|}$$ for $$E_i, S_j$$ two sentences with indices $$i$$ and $$j$$.

The default system, under optimal parameters, may already give you some relatively good matches, but they are far from complete and only match sentences already with high translation agreement.
    

### Scoring function
To score your results, we will be using a test set and calculating your results' [F1-Score](http://en.wikipedia.org/wiki/F1_score) against the sample of the test data. We will test the rest of the test data on the server to grade.
F1 is a combination of metrics to test for both correctness and a sufficient volume of correct sentences. Systems only with high precision (easily done by only matching very confident sentences) will also be penalized. It considers both the precision $$p$$ and the recall $$r$$ of the test to compute the score: $$p$$ is the number of correct positive results divided by the number of all positive results, and $$r$$ is the number of correct positive results divided by the number of positive results that should have been returned. The F1 score can be interpreted as a weighted average of the precision and recall.

<center>
$$F_1 = 2\cdot \frac{p\cdot r}{p+r}$$
</center>

# The challenge
Your challenge is to devise your own algorithm to generate as many reliable pairs of matches as possible. One such algorithm is described in this paper http://research.microsoft.com/pubs/140708/N10-1063.pdf and will be used as our baseline system. You are free to experiment with other systems as well.

You're also welcome to experiment with possible extensions to the baseline:

- Come up with sentence level translation models, language models and adapt an IBM model for word alignment.
- Implement feature engineering such as matching proper nouns
- Improve on the dictionary translation sentence level word set matching. Think of better ways to evaluate what a matching sentence is (instead of just naively matching words)
- Train a classifier on the given test and development sentences
- Crowdsourcing alignments and training on them

# Ground rules

* You must work **independently** on this assignment.

* You should submit each of the following:

    1.  An alignment of the entire dataset, uploaded from any Eniac or Biglab machine
        using the command `turnin -c cis526 -p hw1 hw1.txt`.
        You may submit new results as often as you like, up until the assignment deadline.
        The output will be evaluated using a secret metric,
        but the `grade` program will give you a good idea of how well you're doing.
        The top few positions on the leaderboard will receive bonus points on this assignment.

    2.  Your code, uploaded using the command `turnin -c cis526 -p hw1-code file1 file2 ...`.
        This is due 24 hours after the leaderboard closes.
        You are free to extend the code we provide or write your own in whatever
        langugage you like, but the code should be self-contained, 
        self-documenting, and easy to use.

    3.  A report describing the models you designed and experimented with, uploaded
        using the command `turnin -c cis526 -p hw1-report hw1-report.pdf`. This is
        due 24 hours after the leaderboard closes. Your report does not need to be
        long, but it should at minimum address the following points:

        * **Motivation**: Why did you choose the models you experimented with?

        * **Description of models or algorithms**: Describe mathematically or algorithmically what you did.
          Your descriptions should be clear enough that someone else in the class could implement them.
          What are your models? How did you optimize them? How did you align with them?
          What were the values of any fixed parameters you used?

        * **Results**: You most likely experimented with various settings of any models you implemented.
          We want to know how you decided on the final model that you submitted for us to grade.
          What parameters did you try, and what were the results?
          If you evaluated any qualities of the results other than AER, even if
          you evaluated them qualitatively, how did you do it?
          Most importantly: what did you learn?

        Since we have already given you a concrete problem and dataset, you do not
        need describe these as if you were writing a full scientific paper. Instead,
        you should focus on an accurate technical description of the above items.

        Note: These reports will be made available via hyperlinks on the leaderboard.
        Therefore, **you are not required to include your real name** if you would prefer not
        to do so.
