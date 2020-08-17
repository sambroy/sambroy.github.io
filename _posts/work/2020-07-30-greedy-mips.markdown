---
layout: post
title:  "Greedy MIPS"
date:   2020-07-28 18:30:15 -0800
categories: jekyll update
---

Contains short notes on the [Greedy MIPS paper, NeurIPS 2017](https://papers.nips.cc/paper/7129-a-greedy-approach-for-budgeted-maximum-inner-product-search.pdf). I did not make any effort to beautify the diagrams here; rather the intention is to have a quick understanding of the underpinnings of the paper.


### Goal
This paper considers the _Maximum Inner Product Search_ (MIPS) problem, stated as follows:

Given a vector $$q$$, and a collection
of candidate vectors $$h_1, h_2, \cdots , h_n$$, find out the candidates (top-k) that have the largest
inner products $$q \circ h_i$$.

#### Why is this problem interesting?

As one of many examples, from NLP, a common step is one where we need to choose one (or a fixed number, say 10) out of all the tokens in a vocabulary (say there are 40000 terms in the vocab). In order to do this, we "hit" the embeddings of the vocab tokens with a "query"
vector (say, a learned weight vector) and take the top few inner products. This is clearly captured by the framework of the MIPS problem.

Back to the MIPS problem, the important dimensions are:
* $$n$$ vectors
* each vector (including the query $$q$$) has dimension $$k$$.

Thus, a naive linear search will use $$n\cdot k$$  operations to compute the inner products, followed by a sorting operation that takes
$$n \log n$$ time.

The overall goal is to improve on this time; however the tradeoff is that we may instead yield an approximation to the MIPS problem.

In the paper, the authors actually consider the
_budgeted_ MIPS problem: we have to pick up the $$B$$ vectors $$h_i$$ such that these have the top $$B$$ values in the list of inner products $$ q\circ h_i$$.

### Related Literature
Given the cardinal position, that the MIPS problem enjoys in many naturally occurring computations in
deep networks, the problem has seen a recent surge of interest, with a variety of solutions

* Some solutions involve a _reduction_ to
the _Nearest Neighbor Search_ (NNS) problem,
* Others involve _sampling_ based approaches.

We will talk about these in other short notes, so we do not discuss those here.

### Starting off
However, note that in the sampling approach,
the idea is to _sample_ the $$j^{th}$$ candidate with probability $$P(j) \sim q \circ h_j$$.
More precisely, sample $$(j, t)$$ pairs according to $$q_t h_{j, t}$$ (where $$t$$ ranges over the
  coordinates of each vector, say $$q$$ or $$h_j$$).

Suppose we have 3 vectors, each of 2 dimensions as follows:

  |vector |dim0 | dim1 |
  |--| --- | ----------- |
  |h_1| 4 | 5 |
  |h_2| 3 | 1 |
  |h_3|2|7|



### References

* [greedy mips paper, neurips 2017](https://papers.nips.cc/paper/7129-a-greedy-approach-for-budgeted-maximum-inner-product-search.pdf)
  * [code by rofuyu](https://github.com/rofuyu/exp-gmips-nips17)
