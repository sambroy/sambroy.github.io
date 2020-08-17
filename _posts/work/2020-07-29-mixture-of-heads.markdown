---
layout: post
title:  "Mixture of h-1 heads vs. h heads - ACL 2020"
date:   2020-07-28 18:30:15 -0800
categories: jekyll update
---

What follows are some short notes on the paper on the [mixture of h-1 heads vs. h heads](https://www.aclweb.org/anthology/2020.acl-main.587/).

**Overall theme**

Usually in the transformer architecture we use multiple heads. Are all of these
necessary?

Earlier papers have indicated that the architecture is overparametrized, not all of
the heads might be necessary.

This paper keeps all the heads, but instead of mixing the heads uniformly, inserts a
layer that learns the right _mixture_ of the heads.

What is mostly important to follow in this paper is the transition from the usual
aggregation of heads in a transformer to what this paper does.

**Usual aggregation**:
Given the head outputs $$H_1, H_2, ..., H_h$$ the vanilla transformer uses a matrix $$W$$
to aggregate these head outputs. Thus this corresponds to a _input-independent combination_
of the head values. Of course the attention heads do incorporate the inputs, so the
aggregation does take the input into account.

Next, the authors motivate the addition of an extra component (the MAE component - _mixture of
  attentive experts_) to the existing transformer architecture.

Whenever such components are added on to an existing architecture, two questions arise:
1. Is it possible to capture the new component in the existing framework? i.e. is the new
component really new?
2. Is the new component useful?

**The new component - Mixture of experts**:
The starting point is the realisation mentioned above: in the current transformer architecture, the
different heads are aggregated according to the weight matrix $$W$$; this is equivalent to the
_same aggregation_ irrespective of the inputs (and the input matrices $$H_i$$).

We may think of the heads as being experts, and thereby we would want to learn the
correct _mixture of experts_, i.e. the correct weighting of the experts.  


### Main questions
* Surprise 1: we would have made each individual head $$H_i$$ as an expert. Instead here,
each expert corresponds to $$h-1$$ heads, so that every two experts "share" $$h-2$$ heads.
Why is this?
* Surprise 2: The sampling step in the F-step. We use the relative importances/weights of the
experts (the $$g$$ values) and then we _sample_ using these weights. We choose _only_ the
expert chosen by this process to update weights in this round.
Is this why we choose an expert to be a selection of $$h-1$$ heads? So that across
different experts (and their updates) there is some sharing of weights; so that in
different rounds, we are not updating entirely different sets of weights.
Is this done so that we have faster convergence?
* Surprise 3: The authors refer to an earlier paper on mixture models - as to how such models
fixate to a degenerate set of weights. This is why they use a block coordinate descent style
update instead of jointly updating the F and the G vectors in the paper (their NoBCD model).
* Surprise 4: Connections to dropout




## References:
1. [h-1 heads, ACL 2020](https://www.aclweb.org/anthology/2020.acl-main.587/)
2. [programmersought blog article](https://www.programmersought.com/article/85534594552/)
