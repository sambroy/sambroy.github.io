---
layout: post
title:  "multi task gpt3"
date:   2020-07-28 18:30:15 -0800
categories: jekyll update
---

Contains short notes on the paper [Measuring Massive Multitask Language Understanding](https://arxiv.org/abs/2009.03300).

### Goal


### dimensions
* kind of model (t5 finetuned? gpt3?)
* size of the model
* zero shot or few shot

### findings/curiosities
1. for gpt-3 (D for DaVinci - the x-large model), 
zero shot and few shot accuracies are not too far apart. 
zero shot = context contains no examples, accuracy around 37%. 
5-shot = context contains 5 examples, then a prompt, accuracy around 43%. 
However, the smaller gpt3 models (A, B, C) do not show much better than random performance. 

All of the questions are multiple-choice, with 4 choices. 
Random performance - 4 options, so 25%. 

2. importance of model size. Among the gpt-3 family of models, the large model wins out by a large
margin. 
however, the story is slightly more nuanced here: the authors get interesting results when evaluating
unifiedQA in addition to gpt-3.
unifiedQA is the T5 model finetuned on QA (with the finetuning tasks being QA). 
How does it perform?
size of unifiedQA is 3 billion params (so like the A=Ada model of gpt-3). 
However, accuracy of unifiedQA (fewshot setting) is 38.5% which is much better than Ada, 
in fact quite close to the much larger gpt-3 model. 

3. lopsided performance across different disciplines.
the authors say that "GPT-3 learns about topics in a pedagogically unusual order." 
and performance on disciplines vary: college medicine > college mathematics > elementary mathematics. 
The paper points out that elementary mathematics is more calculation heavy. 
So, if we take the viewpoint that GPT-3 is "memorizing" subwords and their order, for calculation 
heavy material, they have to memorize more smaller subwords and stitch them together. 

4. calibration. 
the authors present results indicating that gpt-3 is uncalibrated.







### References

* [multitask LU paper](https://arxiv.org/abs/2009.03300)
    * [code](https://github.com/hendrycks/test)
