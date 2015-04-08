# cis526
##MT term project
> The task is to translate from Urdu to English using existing grammar rules and language models.

> In the main directory, use the command to run the default system:

>	python one2one.py

> This program will generate a file called '1-best.en'. It uses a very simple method to generate a translation based  on word-to-word translation and ignores complex grammars and language models. 
	
	word_e = argmax_P(word_e|word_f) word_e

> To test the result, use the command:

	python compute-bleu.py < 1-best.en
	The program will evaluate the translation using BLEU score, and print the BLEU stats features as well as the BLEU score.
	The stats are a 10-element tuple, in the form of:
	(
		word count in hypothesis, 
		effective word count in reference set, 
		1-gram matches in hypothesis, 
		1-gram count in hypothesis, 
		2-gram matches in hypothesis,
		2-gram count in hypothesis, 
		3-gram matches in hypothesis, 
		3-gram count in hypothesis, 
		4-gram matches in hypothesis, 
		4-gram count in hypothesis
	)

	And the BLEU score is counted using the formula:
	
	score = exp(min(0,(1 - refcount/hypcount))+ln(grammatch_1...grammatch_4/gramcount_1..gramcount_4)
	
The student's task is to improve the score as much as possible.


###Data
-----
_The './data' including following parts:_

1. grammar

  * The format of grammar is as follows:
  
      [S] ||| [X,1] ||| [X,1] ||| features ||| alignment

      for example
  
      [X] ||| " " جالبین " بھی ||| " " jalibeen " " also ||| 8.93287 6.92926 1 1.00000 0 0 ||| 0-0 1-1 2-2 4-5

  * The features are:
  
      p(e|f) p(f|e) Lex(e|f) Lex(f|e) rarity phrase-penalty

  * Alignment is:
  
      source_word-target_word


2. lm

  lm contains 1-grams, 2-grams and 3-grams language model. 

  The format is ARPA backoff N-gram models, as follows:

    * \data\
     
      ngram 1=n1
      
      ngram 2=n2
        ...
        
      ngram N=nN

      + \1-grams:

        `p`	w		[bow]
          
          ...

      + \2-grams:

       ` p`	w1 w2		[bow]
          
          ...

      + \N-grams:
        
        `p`	w1 ... wN
        
        ...

    * \end\

    > `p` is the logarithm (base 10) of conditional probability of that N-gram
    `w1 ... wN` are the words making up the N-gram.
    [bow] are optional logarithm (base 10) of the backoff weight for the N-gram

3. devtest.ur

  > The data to be translated

4. train data, tune data

  > tune data are for students to tune the CKY model. 
  You may want to train your grammar model by yourself. Then you can use the train data to extract the SCFG rules.
  test data includes the test sentences you need to translate.



###Default System:
----
> 'one2one.py' generates a default output translation '1-best.en' based on word to word translation, ignoring language models.



###Objective function:
----
> 'compute-bleu.py' computes the BLEU score from stdin. 

> We can use
	python compute-bleu.py < 1-best.en 
to see the result
