---
layout: post
title: "Hosting Git"
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2016-07-23 12:45:48
---
## Private Git Repository
I wanted a private secure git repository for my encrypted password storage.
I haven't worked in a laboratory while with access to Linux servers in a while. That would be my first choice. They are typically managed rather well and have git installed and no one bothers my stuff.

## Get Used To Not Having SUDO Privaledges
Even though I lack my customary access to lab servers at the moment, I do still have access to my University Linux account that is granted to the science majors.

I don't feel too guilty using this resource since I give the department regular donations.

The machine I will use runs CentOS and the trick will be to install git without access to yum. That means building [git](https://github. com/git/git) from source. Getting the source is easy and there's decent [install](https://github. com/git/git/blob/master/INSTALL) instructions as well.

* Get the source [git](https://github. com/git/git)
    * Obviously we can't `git clone`
    * Download the zip version from Github
        * scp to the future host
* use configure to set the install directory
* Turn off any "fancy" features that will require more dependancies
    * According to the INSTALL doc
        * NO_PERL (had an error and it's optional)
        * NO_OPENSSL b/c my `curl -V < 7. 34. 0`
        * NO_EXPAT seems purely optional

Assuming you have downloaded the zip version of the source and you have uploaded the archive to your home directory on the target machine:

{% highlight bash %}
unzip git-master. zip
cd git-master. zip

make configure
./configure --prefix=/home/user/bin
make NO_PERL=YesPlease NO_OPENSSL=YesPlease NO_EXPAT=YesPlease
make install
{% endhighlight %}

## Testing

I initiated a new empty GIT repo in my home directory, `git_password_store`.

{% highlight bash %}
mkdir git_password_store
cd git_password_store
git init
{% endhighlight %}

I already have a private repo on Bitbucket.com for my encrypted passwords and I’m not removing it until this new repo is has worked for a few months.THerefore I’m adding another remote URL.

{% highlight bash %}
cd .password_store
git remote add other-repo username@IP:/home/user/git_password_store
it remote -v

git push other-repo master
{% endhighlight %}

## Gotcha
You cannot have the same branch checked out on the server.
I make a branch called `other-branch` as a work around, and switch to that.

Here's the error:
{% highlight bash %}
! [remote rejected] master -> master (branch is currently checked out)
error: failed to push some refs to 'user@IP:/home/users/username/git_password_store'
{% endhighlight %}

My work around is to simply create another branch on the new git server and switch to that. You can read some other ideas on [SO](http://stackoverflow.com/questions/2816369/git-push-error-remote-rejected-master-master-branch-is-currently-checked)

My GIT repo is working on my old school account and so far no one has complained or taken it down. I use an SSH key to login as well. Hopefully if I take precautions then the admin will me alone.
