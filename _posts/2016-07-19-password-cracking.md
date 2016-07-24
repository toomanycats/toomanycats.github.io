---
layout: post
title: "Password Cracking"
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2016-07-19 12:46:41
---
I recently discovered the Youtube channel, [Computerphile](https://www.youtube.com/results?search_query=computerphile). It's a great channel you can learn a lot from. This [episode](https://www.youtube.com/watch?v=7U-RbOKanYs) with Dr. Mike Pound examines how passwords are hacked.

In the video Dr. Pound uses a program called Hashcat, which runs on GPU's. He uses a cluster at his work to crack some hashed passwords.  Hashcat is open source and can be found on [Github](https://github.com/hashcat/hashcat).

My machine is a System76 using the Intel Haswell graphics card which is not supported in the newest version of Hashcat. However, the earlier versions which use only the CPU, are still available and called [hashcat-legacy](https://github.com/hashcat/hashcat-legacy). Hashcat has a nice website with a Wiki and documentation for the legacy version is [here](https://hashcat.net/wiki/doku.php?id=hashcat-legacy).

## Password Dictionary Attacks
Before watching Dr. Pound's presentation, I didn't know what a dictionary attack meant. I figured it used a list of real world words, but didn't understand that it also means a list of words from previously hacked accounts. A real world sample of what use as passwords. Attacks then use these real world examples with permutations to get crack a password.

## You Too Can Get A Dictionary
There's a really cool repository on Github full of dictionaryies, [Seclist](https://github.com/danielmiessler/SecLists).

## My Own Passwords
I'mnow using a password manager and randomly generated passwiths stored encrypted. However, I didn't always do this and I thought I had better test out my old passwords.

I've been having some trouble getting hascat (legacy) to work. It built fine and appears to run, but hasn't actually cracked any examples yet.

## Quick And Dirty
While I figure out what I'm doing wrong with `hashcat`, I decided to just check the dictionaries from Seclist, for anything that looks like my old passwords.

Being a Miyazaki fan, I based this old password example on my favorite Miyazaki film, "Nausicaa of the Valley of the Wind".

{% highlight bash %}
git clone https://github.com/danielmiessler/SecLists.git
cd Seclist/Passwords

grep -i "nausicaa" \*.txt
{% endhighlight %}

{% highlight bash %}
ashley_Madison.txt:nausicaa
bt4-password.txt:nausicaa
darkc0de.txt:nausicaa
md5decryptor.uk.txt:nausicaa78
md5decryptor.uk.txt:nausicaag
md5decryptor.uk.txt:nausicaa1979
md5decryptor.uk.txt:nausicaa
openwall_all.txt:nausicaa
rockyou.txt:nausicaa
rockyou.txt:nausicaa6
rockyou.txt:00nausicaa04
{% endhighlight %}

So even without running `hashcat` yet,it's clear, any password based in the name"nausicca" will not secure.


*to be continued*
