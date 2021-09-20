---
layout: post
title:  "Notes on torchscript"
date:   2020-06-28 18:30:15 -0800
categories: jekyll update
---

As I am learning more about `torchscript`, these are the questions I find asking myself. Note that the answers
I may have found so far might be not entirely accurate. 

### The questions
1. Why `torch.jit`? Roughly because of portability, wanted to separate out the code from the model. `torch.jit` compiles
the code into a lower level representation of the model.
2. What is _eager mode_?
This enables easier debugging. For instance, PyTorch has eager mode by default, whereas for Tensorflow you have to activate it
(see [this reference](https://ai.googleblog.com/2017/10/eager-execution-imperative-define-by.html#:~:text=Eager%20execution%20is%20an%20imperative,research%20and%20development%20more%20intuitive.).
But there is a tradeoff: while having an interpreter output things as and when the instructions are run, is great for debugging, 
this might not be good for optimization. For optimization, it is usually better to have a pan-view of the entire code so as to 
know what optimizations might be applicable. And this is precisely why: script mode! 
3. 


{% highlight python %}
def sample_function(input):
    return input
{% endhighlight %}


### References.
1. [Blog on Torchscript](https://towardsdatascience.com/a-first-look-at-pytorch-1-0-8d3cce20b3ee)

