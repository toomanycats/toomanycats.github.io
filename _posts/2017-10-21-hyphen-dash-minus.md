---
layout: post
title: "Hyphen-Dash-Minus"
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2017-10-21 07:28:30
---

# Describing Command Line Syntax  to a Novice: Dash, Hyphen, Minus

<figure> <img src="/images/dashes.gif"> </figure>

I'm often helping people fairly new to Linux command line usage. Once they have
some experience, I can say:

> el ess option el tee are

and expect to see them type:

{% highlight console %}
ls -ltr
{% endhighlight %}

However in the painful beginning, it's more like this:

> el ess space minus el no space tee are enter

I hated saying "minus" so I switched to "dash". That took people a moment to
figure out but I didn't care, because it's a painful process anyways.

However, I was wrong. The "dash" is really a "hyphen" and "minus" is actually
correct.

## Looking It Up
I was visiting my friend Paul Lovinger, the author the _The Penguin Dictionary of
American English Usage and Style_,  and asked him about my
doubts with "minus" vs. "hyphen". He directed me to read the chapter of his book
on punctuation. So I did.

On page 330:

> 4. Dash and hyphen A. _The difference_ The public often confuses the _dash_
>    (--) and the _hyphen_ (-). Occasionally the press mixes them up too. Both
>    punctuation marks are horizontal lines, but a hyphen is very short; a
>    hash is longer. ...
> They have largely opposite functions. A dash separates words. A hyphen unites
> words, although it may separate syllables of a word.

It's not clear whether command line arguments are being separated from the
command, or we are joining a list of arguments. I don't think this definition
is going to provide much guidance on whether to call the "-" a _hyphen_ or a
_dash_.

Consider for a moment "long options" typically denoted like so:

{% highlight console %}
cp --relative $src $tar
{% endhighlight %}

<figure> <img src="/images/penguin_dictionary.jpg">
<figcaption>source:https://www.amazon.com/Penguin-Dictionary-American-English-Usage/dp/0142000469</figcaption>
</figure>

The style guide does provide some background for a choice here.

> When a typewriter or word processor is used, one strike of the key to the
> right of the zero (unshifted)  makes a hyphen. Two strokes of that key make
> what represents a dash.

Assuming we call the `-` a _hyphen_ we could then call the long option a
_dash_.

When teaching beginners, I think I'll still be saying something along the
lines of,

> see pee minus minus relative

<figure> <img src="/images/hyphen.jpg">
<figcaption>source:http://www.home-designing.com/2010/03/a-post-targeted-to-our-non-web-savvy-audience</figcaption>
</figure>

## Wikipedia to the Rescue

Now armed with the facts about traditional English language usage of dashes and
hyphens, I tried Google. Not surprising, I  landed on a very appropriate
Wikipedia Page; [Wikipedia Article](https://en.wikipedia.org/wiki/Hyphen-minus).

> The use of a single character for both hyphen and minus was a compromise made
> in the early days of fixed-width typewriters and computer displays.[2] However,
> in proper typesetting and graphic design, there are distinct characters for
> hyphens, dashes, and the minus sign. Usage of the hyphen-minus nonetheless
> persists in many contexts, as it is well-known, easy to enter on keyboards, and
> in the same location in all common character sets.

# How I Verbalize Options Now

Most people know what a `minus` sign is versus how to explain that a _dash_ and
_hyphen_ are.

Therefore I've fallen back on a practice that I once disliked that now feels
OK, since it's technically correct in the context of computer keyboards.

When describing how to write a command in a Linux terminal I use "minus" for
hyphen (-) or "minus minus" for dash (--). If I want to grill a newbie, I can
always fallback on the _hyphen_ and _dash_.
