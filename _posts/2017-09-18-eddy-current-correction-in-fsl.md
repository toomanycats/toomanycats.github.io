---
layout: post
title: "Eddy Current Correction in FSL"
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2017-09-18 19:22:39
---

# DTI processing: Eddy Current Correction
I'm working on my first Diffusion Tensor Imaging project at work. There's new
gotcha's I've encountered and I thought I should do a small write up about one
that irked me today.

<figure>
<img src="http://4.bp.blogspot.com/-pKxLdBQVDeI/Vg1B1q-I_gI/AAAAAAAADS0/Fs-KalhMqk8/s1600/before_after_hcp_v4.gif">
<figcaption>_image source:https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/eddy_</figcaption>
</figure>

The topic is eddy current correction. You might have read the FSL wiki page
about it:

[FSL eddy_current](200~https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FDT/UserGuide#Eddy_Current_Correction).

So I should start using the new `eddy` program but for now I'm not. The lab has
been using the older style correction for some time and I can't just throw in a
new method on an existing project.

## Hidden Dependencies
FSL is a pretty good set of tools. Most of it is written in C++ and runs rather
quick. However, while fighting with `eddy_current` today I learned that Python
2 is a dependency.

{% highlight bash %}
cat eddy_current
fslroi $input ${output}_ref $ref 1

fslsplit $input ${output}_tmp
full_list=`${FSLDIR}/bin/imglob ${output}_tmp????.*`

for i in $full_list ; do
    echo processing $i
    echo processing $i >> ${output}.ecclog
    ${FSLDIR}/bin/flirt -in $i -ref ${output}_ref -nosearch -interp ${interpm} -o $i -paddingsize 1 >> ${output}.ecclog
done

fslmerge -t $output $full_list
/bin/rm ${output}_tmp????.* ${output}_ref*
{% endhighlight %}

This program is really a script wrapper and what is does is really simple.

   * Strips off the first volume to use as the reference volume
   * separates the rest of the 4D file into volumes.
   * Then registers the volumes to the first volume.
   * Registered volumes are merged back into a 4D file
   * cleans up

That's it. Although, along the way, temp files are created, and a Python helper
program is called to create a string for `fsl_merge`.

How silly is that ??!!

To create a string of files, we could just use `find` with a regular
expression. That would avoid a whole dependency on another language.

# Even Better
Don't use `eddy_current` because you don't need to. I learned during a
presentation some years ago, that the correction for eddy currents turns out to
be a power series solution ( be afraid, very afraid) and that in the end, the
12 degrees of freedom affine registration actually is the solution !!

## Simpler Code
So now that we are armed with the facts, we can just use `mcflirt` with some options:

{% highlight bash %}
mcflirt <input 4D> <output 4D> -cost normcorr -refvol 0 -dof 12
{% endhighlight %}

That's it. No Python 2 to worry about and no extra weirdness.

# How this bad programming bit me in the ass
I write my pipelines in Python3. That's the future and it's a good one. When I
ran `eddy_current`, I did not get an error code greater than 0, or stderror
message, just a failure with an obscure error message.

TBH, I guessed pretty quickly that there was a Python 2 vs Python 3 issue
because the error mentioned something about `print` not having parenthesis.

There's a helper program called `imglob` that's written in Python2.There's an
interpreter directive that says to use:

{% highlight bash %}
#!/usr/bin/env python2
{% endhighlight %}

Ok, but my IT folks don't use the virtual environments and I use Anaconda. So,
the directive was ignored and the Python on my path was used, Python 3.5.

## It gets worse: No Error Codes Returned
The call to `imglob` doesn't check for the return code, so you don't know if it
fails.

They have already written a decent shell script, why didn't they just add it ?

{% highlight bash %}
cat eddy_current
fslroi $input ${output}_ref $ref 1

fslsplit $input ${output}_tmp

full_list=`${FSLDIR}/bin/imglob ${output}_tmp????.*`
########################
### check for errors ###
########################
if [[ $? -ne 0 || -z $fill_list ]]; then
    >&2 echo "failed to return string for fslmerge: see imglob"
    exit 2
fi

for i in $full_list ; do
    echo processing $i
    echo processing $i >> ${output}.ecclog
    ${FSLDIR}/bin/flirt -in $i -ref ${output}_ref -nosearch -interp ${interpm} -o $i -paddingsize 1 >> ${output}.ecclog
done

fslmerge -t $output $full_list
/bin/rm ${output}_tmp????.* ${output}_ref*
{% endhighlight %}

### Let's fix it up more assuming we don't use `mcflirt` as a solution.
A better way to handle temp files, is to make use of `/tmp`, and `trap`.
Also, `find` can build strings with a lot of flexibility. I'm showing the use of globbing
with `-name` but you could use `--regex`.

{% highlight bash %}
temp=$(mktemp -d)

log="${output}.ecclog"

function clean_up
{
    rm -rf $temp
}

trap clean_up ERR SIGINT

fslroi $input ${temp}/ref $ref 1

fslsplit $input ${temp}/tmp

full_list=$(find ${temp} -type f -name "tmp*" -print0 | sort -z | tr '\0' ' ')

if [ -z $fill_list ]; then
    >&2 echo "Failed to return string for fslmerge. "
    exit 2
fi

for i in $full_list ; do
    echo processing $i
    echo processing $i >> ${log}
    ${FSLDIR}/bin/flirt -in $i -ref ${temp}/ref.nii.gz -nosearch -interp ${interpm} -o $i -paddingsize 1 >> "${log}"
done

fslmerge -t $output $full_list
{% endhighlight %}


# Another Example
I work with FreeSurfer a lot as well, and in 5.3 there's an annoying dependency
on FSL. If you wish to use the T2 weighted image to improve the regular FS
processing, you can include the T2 on the command line.

When you include the T2 image, it needs to be registered to the T1. So instead
of just learning how to use `mri_convert` to do the registration, someone
leaned on what they knew and used FSL's `flirt`.

Thankfully in FreeSurfer 6 the need for `flirt` was removed and the annoying
Perl deprecation warning was fixed.

# Conclusion
Don't add a dependency until after you've decided there's no other choice. If
the software suite is large like FreeSurfer or FSL, there's bound to be a
better way that is native to the platform.
