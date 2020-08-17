---
layout: post
title:  "An antipattern: Modifying a list inside enumerate"
date:   2020-06-28 18:30:15 -0800
categories: jekyll update
---
In this post, we will understand what happens 
when we modify a list inside a `for` loop, where
the `for` loop essentially loops over the list 
itself. 

To be clear, this is very much an anti-pattern: 
[this SO link](https://stackoverflow.com/questions/4081217/how-to-modify-list-entries-during-for-loop)
provides some discussion as to why so.

The problem that we will tackle is the following:
Suppose you are given a list of string tokens. Now, some 
of the string tokens may include _apostrophes_: and in such a case
we want to merge those apostrophes with the tokens 
appearing immediately before. 

As an example, a list of tokens `["I", "don", "'t", "want", "to", "go"]`
should be converted to `["I", "don't", "want", "to", "go"]`.

We abstract out the essence of this problem as the following:

**Problem**:
We are given a `special` string (call it `x`), and a list of
strings. Traverse the list of strings; if the special
string appears at a position, then merge it with the 
string at the _previous_ position. Output this
modified list of strings.

There are a couple of assumptions we will make here:
1. One key assumption that we will make (as is also 
reasonable to make for the original setting of the problem)
is that the appearances of the special string in the list of
string is _non-consecutive_.
2. The other assumption is that the strings in the list are
all _non-empty_. 

Given this, here is a sample implementation in Python:

{% highlight python %}
def process_list(special, input_list):
    rev_input_list = input_list[::-1]

    for idx, item in enumerate(rev_input_list):
        if item == special:
            if idx + 1 < len(rev_input_list):
                rev_input_list[idx+1] = rev_input_list[idx + 1] + special
                rev_input_list.pop(idx)
                
    output_list = rev_input_list[::-1]
    return output_list
{% endhighlight %}

A sample run:

{% highlight python %}
special = "b"
input_list = ["a", "bc", "b", "d",  "c",  "b"]

output_list = ['a', 'bcb', 'd', 'cb']
assert process_list(special, input_list) == output_list
{% endhighlight %}

One of the reasons why modifying the list inside a `for`
loop that iterates over that very list is because it becomes
hard to argue pre- and post-conditions as to what happens
with the elements of the list. 

In this case, however, we can actually reason in this regard. 
Here is an informal invariant:

If an element of the list is `pop`ped from the list, then 
that index is taken by the merged list entry. Also, 
we do not consider the merged entry in the next iteration of 
the `for` loop.

It is very surprising that the above program works at all; 
it is easy to assume that there is only one evaluation of the 
call to `enumerate(rev_input_list)` - but as it turns out
that is not the case at all. It is evaluated at every 
step, with the index being updated (by 1) at every step, 
and whatever item appears at that index in the updated list.

### References.
1. [The enumerate built-in function](https://www.python.org/dev/peps/pep-0279/)
