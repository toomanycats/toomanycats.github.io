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

I made up the next lines...

```/home/someone.gnupg/pubring.gpg
----------------------------------
pub    4096R/8923jJ6K 20150-09-12
```

## Initialize a new password store
[SO answer that helped me](http://unix.stackexchange.com/questions/53912/i-try-to-add-passwords-to-the-pass-password-manager-but-my-attempts-fail-with)

```pass init 8923jJ6K```

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

{% highlight bash %}
insert [ --echo, -e | --multiline, -m ] [ --force, -f ] pass-name Insert a
new password into the password store called pass-name. This will read the new
password from standard in. If --echo or -e is not specified, disable keyboard
echo when the password is entered and confirm the password by asking for it
twice. If --multiline or -m is specified, lines will be read until EOF or
Ctrl+D is reached. Otherwise, only a single line from standard in is read.
Prompt before overwriting an existing password, unless --force or -f is spec‐
ified.
{% endhighlight %}

I'll start with my Stack Exchange password:
```pass generate forums/stackexchange.com```

It appears to have worked...I can get access to my encrypted password by asking for it from ```pass```.

```pass show forums/stackexchange```

I'm prompted for my gpg key password, and out comes the new randomly generated password for StackExchange.

I pushed all this to BitBucket for use on a work machine.


