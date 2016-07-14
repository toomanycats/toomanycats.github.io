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
# Jekyll
This is the second Jekyll theme I've tried, SO Simple Theme and so far it's working for me. I probably should also consider Octopress.

## Rakefile
I'm also using a Rakefile from [gummesson](https://github.com/gummesson/jekyll-rake-boilerplate.git).

A quick note about the rake solution I'm using, I didn't get it working with `ZSH`, I have to drop into `BASH`. So far an inspection of the source has not revealed to me the issue but for now it's not a high priority. This works:

{% highlight shell %}
bash
rake post["New Blog: First Post"]
{% endhighlight %}

## Serving the blog locally
To use the automatic sitemap generator and other features, setting the `_config.yml` file's `url` key is important and probably just the right thing to do in general.

This makes testing out the rendering on my laptop require a second config file where the `url` is set to `127.0.0.1:4000` or leaving it blank. I followed SO advice linked [here](http://stackoverflow.com/questions/27386169/change-site-url-to-localhost-during-jekyll-local-development)

## Why Jekyll
I chose to use Jekyll for a few reasons

* Learn more about Web standards and models
   * CSS
   * HTML
   * Javascript
* Learn some Ruby
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

