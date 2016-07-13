---
layout: post
title: "Pass Word Manager"
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2016-07-07 18:24:30
---

# Pass
I've decided to be grow up and use a password manager. Did I tell you I run Linux...Not interested in Lastpass, KeepassX ( hilarious name ) or other bloated GUI based managers.

[link to pass](https://www.passwordstore.org/)

I did a quick unofficial poll on the LinkedIn group for Linux users and finally got a reply why someone would use `pass`.

![LI_pass_comment](/images/pass_LI_comment.jpg){:class="img-responsive"}

Pretty hilarious IMO. I don't know why this guy thought I was hating on his favorite password manager but I did get some useful information.

## Installing Pass

Being that I run Ubuntu 14.04, install was just a `sudo apt-get install pass` away.
## Make a new GPG Key
[gpg instructions](http://www.webupd8.org/2010/01/how-to-create-your-own-gpg-key.html)

```gpg --generate-key```

Follow those instructions and I have a new 4096 bit key.
```gpg --list-keys```

While running gpg I get the messaage

```
**We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
Not enough random bytes available.  Please do some other work to give
the OS a chance to collect more entropy! (Need 210 more bytes)**
```
I tried working on email and moing the mouse around but it didn't seem to help.
Not really knowing what I was doing, I tried this:

{% highlight bash %}
for i in {1..1000000};do
    echo $RANDOM
done
{% endhighlight %}

which might have helped as the key generation was done as soon as the loop finished.

## look at the Keys:
```gpg --list-keys```

(not real output)
{% highlight bash %}
/home/user/.gnupg/pubring.gpg
-------------------------------
pub   4096R/9FFCD3ET 2016-07-08
uid                  "Daniel Cuneo" (nick name) <dpcuneo@fastmail.fm>
sub   4096R/41FH6H82 2016-07-08
{% endhighlight %}

## Initialize a new password store
[SO answer that helped me](http://unix.stackexchange.com/questions/53912/i-try-to-add-passwords-to-the-pass-password-manager-but-my-attempts-fail-with)

```pass init 9FFCD3ET```

## Git
Pass can be used with Git.
```git pass init```

In fact the git commands are used the same as usual with the exception that the prefix "pass" is added in front of each command. I thus added a new repository to my Bitbucket account and followed the usual instructions.

{% highlight bash %}
pass git remote add origin https://foo.git
pass git push -u origin --all
{% endhighlight %}

## Make Some Entries
I'm going to start with my email since it should be the easiest.
Pass uses plain ascii text so you can actually put anything you want in your password file. The standard thing to so is:

* password
* username
* url
* other, such as answers to questions

I'll start with my Stack Exchange password and put it in a directory `forums`:
```pass generate forums/stackexchange.com```

To add more lines to the randomly generated password:

```pass edit forums/stackexchange```

VI is opend and I add another line with my username.

It appears to have worked...I can get access to my encrypted password by asking for it from ```pass```.

```pass show forums/stackexchange```

I'm prompted for my gpg key password, and out comes the new randomly generated password for StackExchange.

I pushed all this to BitBucket for use on a work machine.
```pass push origin master```

