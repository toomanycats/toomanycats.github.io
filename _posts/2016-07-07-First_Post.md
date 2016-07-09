---
layout: post
title: "New Blog: First Post"
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2016-07-07 13:17:40
---
# Testing Out My New Blog
This is the second Jekyll theme I've tried, SO Simple Theme and so far it's working for me. I probably should also consider Octopress.

I'm also using (trying to use) a Rakefile from <a href=https://github.com/gummesson/jekyll-rake-boilerplate.git>Link</a>.

A quick note about the rake solution I'm using, I didn't get it working with `ZSH`, I have to drop into `BASH`. So far an inspection of the source has not revealed to me the issue but for now it's not a high priority. This works:

{% highlight shell %}
bash
rake post["New Blog: First Post"]
{% endhighlight %}

## Why Jekyll
I chose to use Jekyll for a few reasons

* Learn more about Web standards and models
   * CSS
   * HTML
   * Javascript
* Learn Ruby
* Code snippets
* Free hosting on Github

## Code Snippet Test

{% highlight python linenos %}
import numpy as np
from numba import jit

@jit
def iter_mean(data):
    """
    calculate a mean using iterative method.
    `data` is an array like object.
    """

    mu = np.float64(mu)

    for i in range(len(data)):
       mu += 1.0 / (i + 1) * (mu - data[i])

    return mu
{% endhighlight %}

