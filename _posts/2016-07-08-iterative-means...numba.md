---
layout: post
title: "Python JIT Numba: Computing a Mean"
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2016-07-08 15:40:20
---
<figure>
    <img width="512px" src="/images/numbat_resized_median.jpg" alt="image of numbat">
    <figcaption>A numbat found accidently while searching google for numba logo.</figcaption>http://www.johndcook.com/blog/standard_deviation/
</figure>

## Numba and Computing Means
I needed a reason to visit Python JIT compilers, and in particular Numba.

## Iterative Means
When I was working in a neuroscience laboratory, we needed to compute the mean and standard deviation for signal in each brain parcel. This was accomplished in C++ using the ITK library.

The first iteration of the program I wrote the means didn't match a previously caculuated number, they were off my a small amount $\approx 0.001%$.

My boss was not having it. It didn't matter if the discrepancy was trivial and smaller than the uncertainty of the signal numbers, that still needed fixing. He found this great article by a professional statistician, [John Cook](http://www.johndcook.com).

## Large Numbers and Floating Point Rounding Error
<figure>
    <img src="/images/float_schematic.png" alt="schematic of a 32 bit float">
    <figcaption>source: wikipedia</figcaption>
</figure>

I'm not going to attempt to explain the fine points of floating point numbers and how they are defined. You can read that on [Wikipedia](https://en.wikipedia.org/wiki/Single-precision_floating-point_format#Single-precision_examples).

It a nut shell, we need to know that a 32 bit floating points number has 24 significant bits which is about 7 decimal digits. A 64 bit floating point number has 15 decimals of precision[SO Answer nicely summarizes](http://stackoverflow.com/questions/13542944/how-many-significant-digits-have-floats-and-doubles-in-java).

To get a better feel for how a number is represented in floating point notation watch this 4 minute [video](https://www.youtube.com/watch?v=n-XozGu1viM).

If we are calculating the standard deviation of large numbers on the order of magnitude of 1e9, when we perform the substraction the rounding error can cause a negaitve number where we'd expect and require a positive number. JC's blog post has more detail and better explanation; [link](http://www.johndcook.com/blog/2008/09/26/comparing-three-methods-of-computing-standard-deviation/).

## Numba
So I use Python in my day to day programming and analysis work. The numpy `mean` method is not special in any way other than being vectorized and optimzed. Which is usually enough for my needs but not always. Note, for weighted averages use the nump.average method.

To compute the iterative standard deviation we also need an iterative mean.

{% highlight python %}
from numba import jit

def iter_mean_1d(data):
    """
    Compute iterative mean for an array-like object.
    """
    mu = np.float64(0)
    n = len(data)

    for i in range(n):
        mu += 1.0 / (i + 1) * (data[i] - mu)

    return mu

@jit
def jit_iter_mean_1d(data):
    """
    Compute iterative mean for an array-like object.
    """
    mu = np.float64(0)
    n = len(data)

    for i in range(n):
        mu += 1.0 / (i + 1) * (data[i] - mu)

    return mu

# in ipython terminal
data = np.random.random(1e6) + 1e9
print( data.mean() )
#Out[19]: 1000000000.5002171

mu = jit_iter_mean_1d(data)
print mu
#Out[20]: 1000000000.5002156
{% endhighlight python %}

OK they are a tiny bit different. Before moving on to the full standard deviation calculation I want to time the executions.

{% highlight python %}
timeit jit_iter_mean_1d(data)
100 loops, best of 3: 9.3 ms per loop

# without Numba
timeit iter_mean_1d(data)
1 loops, best of 3: 303 ms per loop
{% endhighlight %}

Good, Numba is faster, I must be doing it somewhat correctly.


_to be continued_


