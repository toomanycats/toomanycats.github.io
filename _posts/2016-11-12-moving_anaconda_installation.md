---
layout: post
title: Moving an Anaconda Installation
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2016-11-12 21:41:19
---
My home directory was approaching the disk quota and my source wasn't being seen by 
the Grid reliably....it was time to move to the code base and that included my `anaconda` installation. 

# Google Doesn't Help this Time
If you google for "moving anaconda installation" you don't find a lot of ideas. I'm sure people do it but they
don't ask on SO.  

I copied my files over before I remembered that it wasn't so simple. When i tried to step into an Ipython terminal and 
load some code to experiment with, stuff broke and I wasn't optimistic. However, reading the errors  and knowing that I had
just moved my main programming language's binary path, gave me a good clue as to where to start looking.

# Grep
I just installed the Silver Surfer `ag` and had the perfect reason to try it; `ag -l "home" *.py` turned up many files; .
nthe orer oder thousands as i recall. It was just a matter of `ag -l foo *.py | sed 's/$old/$new/g'`, hopefully.
I went for broke, and it worked.

