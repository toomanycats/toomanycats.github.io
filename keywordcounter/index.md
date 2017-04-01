---
layout: page
title: Keyword Counter Project
excerpt: Discovering keywords in job openings
modified:
---
<center><a  href="http://www.careerwords.zone">Keyword Counter</a>

<figure>
    <img  src="/images/keyword_counter_logo.png" alt="keyword counter logo">
    <figcaption>Keyword Counter Logo</figcaption>
</figure>
</center>

I wanted to learn more about creating a dashboard with D3.js or some other type of tool. I settled on the Python wrapper for D3 called Bokeh.

In addition to learning basics about dasboarding, I also got my feel wet with Flask, and learned some basic Javascript via JQuery and other random HTML protocol that I hadn't learned in passing before.

he main work horse in KW Counter is Scikit-Learn's text count vectorizer, nothing fancy. I do use a grammar based approach via NTLK as well to sift through the entire text.

It's also nice to have a reason to work with Google analytics and Adwords.

## Hosting
I'm using Red Hat free hosting tier at Openshift. I like it because of the ready to use "cartridge" system. MySQL and Python setup was trivial. The terminal interface functionality is similar to AWS but simpler and has worked well for my needs.

In the future I think using AWS makes sense to gain more control and because it's a professional norm. That's on my TODO list.

## TODO
* Optimize (more) for mobile
    * Replace current gallery
        * Use screen shots of the new vertical outputs
    * Check button placement
* Replace count vectorizer
    * Mutual information matrix
* SEO
    * Ask for links
* Add "leave feedback" or "comments" feature
* Migrate to AWS
    * NGINX

