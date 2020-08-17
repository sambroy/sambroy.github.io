---
layout: post
title:  "Controlled Text Generation"
date:   2020-02-23 18:30:15 -0800
categories: jekyll update
---

In the following, we summarize some of the blogs/articles on the topic of controlled text generation.

*[Neural Degeneration paper](https://arxiv.org/pdf/1904.09751.pdf)*
- Overall, the strategy for beam search as well as other strategies is: given the pool of possible extensions, the tokens, figure out which ones to retain. The only quantity that we have, to play with, are the probabilities of the tokens. Given these probabilities, design
a cut-off strategy.
- Problem: The probabilities of the tokens are (often) heavy tailed.
  * This means that even low likelihood tokens (as a group) has a reasonable probability of being chosen. Thus, beam search or greedy
  might go astray.
  * And once you choose a low likelihood token, this continues - more and more low likelihood tokens get chosen because we are
now conditioning on this low likelihood token too. Akin to one's thought being derailed. Other ways of thinking about this is that
"errors being compounded". Related keyphrases: teacher forcing, etc.
  * thus, we have to prevent sampling from the tail.
- Solutions:
  * **temperature sampling**. motivated by thermodynamics. higher temperature = more energy in the system, more unstable. More unstable
  means that low likelihood states will be reached more often. So in order to retain confidence in only the high likelihood tokens,
  lower the temperature.
  * **top-k sampling**. Given all the tokens with their individual probabilities, keep only the top k (top according to their probabilities)
  tokens. This may not necessarily be good - why? For some contexts, the words/tokens might have a broad/heavy tailed distribution, while for other contexts, the distribution might be quite spiky/almost dirac delta like. This is what the paper calls broad/narrow distributions. Hence the following strategy.
  * **top-p sampling/nucleus sampling**. Here "p" stands for the probability. We have to declare a cutoff for the tokens. Instead of selecting a fixed k in the top k, keep only the top few tokens that account for the most of the probability mass (i.e. go for
  quantiles instead of mean - this is clearly more robust).



**Terms**

_Exposure Bias_
When we are generating text, we generate a new word according to the context, then add this word to the context, and use this modified
context to generate a new word, and so on.
However, now what happens is that the new word/set of words are coming from a distribution that has not been seen by the training data.
(For instance, the n-gram generated might not be at all in the set of training n-grams). This leads to a bias and is called the
"exposure bias".

** Other Ideas **



# References:
### Papers
1. [The Curious Case of Neural Text Degeneration](https://arxiv.org/pdf/1904.09751.pdf)
2. [Generalization in Generation: A closer look at Exposure Bias](https://arxiv.org/pdf/1910.00292.pdf)
3. [On the Importance of the Kullback-Leibler Divergence Term in Variational Autoencoders for Text Generation](https://arxiv.org/pdf/1909.13668.pdf)
4. [Plug and Play Models]()

### Blog articles
1. [Medium blog article](https://medium.com/phrasee/neural-text-generation-generating-text-using-conditional-language-models-a37b69c7cd4b)
2. [Evaluating BERT text generation](Bertscore: Evaluating text generation with BERT.)
3. [MoverScore paper](https://public.ukp.informatik.tu-darmstadt.de/UKP_Webpage/publications/2019/2019_EMNLP_WZ_MoverScore.pdf)
4. [Blog explaining the neural degeneration paper](https://towardsdatascience.com/how-to-sample-from-language-models-682bceb97277)
