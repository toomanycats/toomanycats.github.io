---
layout: post
title: "Outline Tool for VIM"
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2016-09-22 00:11:36
---
Since I started a new job, I've needed to outline some processes. Since I
often work in a Linux environment, I might not feel like switching to my
Windows machine. Plus I just really like VIM.

[Github page](https://github.com/vimoutliner/vimoutliner)

# 'Vim outline' Google returned "Vimoutliner"
I use Vundle so installation was trivial.

Here's the Vundle line in my `.vimrc`: `Plugin
'https://github.com/vimoutliner/vimoutliner.git'`

Vimoutliner is really just about indenting and folding.

{% highlight shell %}
Title of the Outline
: author <,,t> <..d>

First Topic
    sub topic
    sub topic 2
        sub sub topic

Second Topic

{% endhighlight %}

## HTML Output

<figure>
    <img src="/images/vimoutliner_html.png" alt="rendered html from outline">
    <figcaption>Rendered HTML from otl file</figcaption>
</figure>

# Folding
Folding is easy in VIM:

* zR  unfold all
* zM  fold all
* zA  Toggle: (un)fold a section recursively
* za  Toggle: fold top level

There's more but those are my main commands I use. `:help fold` to read
more about it VIM.

There's also so called, "comma comma commands", which are used in
command mode. `,,t` puts a time stamp at the cursor and `,,d` the date.


# Convert to Markup The tool also comes with some handy utility scripts
which converts the `otl` file into something portable:

* fs2otl
* MediaWiki2otl
* otl2html.py
* otl2ooimpress.py
* otl2table.py
* otl2tags.py
* otlhead
 * shell script with generating a summary of level (n)

and more.

I tend to use the html creator which is useful with email where HTML is
enabled or to enable in a Wiki page.

# Give it a try...
