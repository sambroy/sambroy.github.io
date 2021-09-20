---
layout: post
title:  "wordpiece related papers"
date:   2020-10-10 18:30:15 -0800
categories: jekyll update
---

notes on wordpiece

**bpe-dropout paper**

1. bpe is deterministic. given corpus, find frequent byte pairs. bpe breaks segment into
unique sequences. We stop the bpe process according to the vocab size we want.

The good:
* simple, easy to understand segmentation, [easy to implement](http://ethen8181.github.io/machine-learning/deep_learning/subword/bpe.html)

The not so good:
* we would ideally like to give an option to the training algorithm - since we are working
at the level of subwords, that is already a bit nebulous. Are we absolutely confident about
the subword segmentation that our algorithm has chosen, and use that to train our LM?
**Bottomline**: we would like options for the segmentation.

How is this mitigated in practice?
* after the bpe paper came out in nmt, the subword regularization paper came out.
This proposes a way (sentencepiece) that makes it possible to get multiple segmentations
for a given word sequence.
However, this is a bit complicated.
* the bpe-dropout paper makes a simple tweak on the bpe idea, introducing a randomness
step (a _dropout_ step) and this again makes it possible to have multiple segmentations
for a given word sequence.
  * in the bpe algorithm, you _merge_ byte pairs in the text. Now, for dropout, you randomly
  pick some of these _merges_ to actually execute.
  * this is what gives multiple alternatives for segmentations for a word sequence.

Interesting side-effects:
* bpe-dropout is robust towards mis-spellings. This point is brought out in this [blog](https://jlibovicky.github.io/2019/11/07/MT-Weekly-BPE-dropout.html).
* Normally, we use a decoder to search through the possibilities for the output text.
Typically we use a greedy decoder or a beam search tree. However, bpe regularization provides another viewpoint:
  * we can utilize the different segmentations of the context according to bpe-dropout (or sentencepiece)
  and use the different segmentations to generate suggestions (in an auto-completion scenario).
  * This gives us options instead of relying on beam search to provide multiple options.

### sequence to read
1. [wordpiece blog](https://paperswithcode.com/method/wordpiece)
2. [google paper on wordpiece](https://arxiv.org/pdf/1609.08144v2.pdf)
