---
layout: post
title: "Organizing Data in Scientific Programming"
modified:
categories:
excerpt:
tags: []
image:
feature:
date: 2016-10-09 12:29:12
---

# Programming in a research laboratory
I started my scientific work experience in a lab that studies brain diseases
utilizing MRI and other modalities. I then spent sometime in industry learning
to work at a faster pace and with newer technology.

Now I'm back in research and I'm  finding myself using a whole new set tools
and principals.

<figure>
    <img src='/images/t1t2flairbrain.jpg' alt="Anatomical MRI modalities, T1, T2 and FLAIR">
    <figcaption>http://casemed.case.edu/clerkships/neurology/Web%20Neurorad/MRI%20Basics.htm</figcaption>
</figure>

I'm going to focus this writing on organizing inputs to processing work flows,
and in my cases that's brain MRI scans. Typically a study will have subjects,
which undergo sessions, that have runs. The MRI Scanners produce files in the
medical format known as DICOM. The meta data stored as date-times in
the file name and some DICOM header tags.

The files are brought into a researchers file system in a flat format. Then
someone puts them ins some order that makes sense to humans.

* Subject
   * Session
      * Modality
         * runs


# Why not just keep the files where they are ?
I've seen a lab where symbolic links where used to
organize the data without having to copy it around. That was highly annoying
when I was allowed access to those files for a data pull. Follow links in
`rsync` would only make sense if I recreated the entire file structure. Those
data weren't in directories with names like, "time\_point\_n". If I'm going to
write logic to organize files by date in chronological order, then I'll rewrite
to new directory names.

But again...why ? After all the logic is done, I could just leave the DICOMs in
place and use a database.And the good news is, handing date-times is pretty
painless with Python and Pandas. That logic will also be reusable for the most
part.

Perfect, so I'm back in research and I'm going to lean on Python / Pandas and
work with databases instead of walking the file tree.

This concept can scale using Hadoop as well. The inventory is mapped and the
since order of the inventory is not important we just reduce to a flat file. In
fact using shell scripts can use useful in that one doesn't need to have a
MapReduce machine setup with any additional tools than a shell.

# Post Doc Programmers
In most research field the programming is done my non-experts, people who may
not have take a single programming class and learning from other post docs or
and they likely have not been bitched out for their typically very poor
programming habits and style.

The biggest offender for me, are MATLAB scripts which build paths to data in
puts. Complex monolithic programs that required a complex and opaque data
structure that itself required a complex and opaque script to get working.

Often times the researchers are unclear what data they, where it is located and
if it's homogeneous or not. Especially with research tools which do not
typically  enforce a standard just merely allow it, or not. For example, I work
with medical DICOM images and there are fields in the header for helpful meta
data such as the type of modality, or perhaps the "slice timing", which are
sometimes left blank.

# A Better Approach: A Data Base
The most organized lab I worked in and got my start, used a well thought out
system to store meta data, a SQL database. This database was curated and
maintained carefully. It was the life line of the lab which processed and
stored several terabyte of imaging data for research.

I also learned during my short stint as a consultant, that generating an
invoice for goods delivered and received is pretty  crucial. When given a data
set, it's wise to inventory it and send a receipt for what was given. Especially
if you suspect there will be issues regarding the quality or meta data
inconsistencies.

I have written a handful of inventory programs which work with any type or
group of files and a few particularly for medical imaging storage, e.g. DICOM
and NiFTI1.

I'm a Python programmer at heart, coming from MATLAB. I enjoy brushing up my C
whenever the job warrant's it, and very comfortable with Linux shell scripting.
I know that shell scripting can be abused. It's hard to read, doesn't have the
greatest functionality with error handling. But it also runs pretty fast,
especially when the heavy lifting is done by compiled C programs. It's also easy
to exploit GNU Parallel to produce a csv inventory which can then be loaded
into SQLITE.

# Inventory Work flow for Data Inputs and Outputs Instead of writing annoying
dense and hard to ready MATLAB scripts to build paths and configurations,
simply use a database. Use SQLITE if it's small that's fine. Also, you don't
want to depend on a DBA for basic work flow access.

{% highlight bash %}
{% raw %}
#!/bin/bash

function get\\_file\\_ext
{
    input="$1"
    base="${input##*/}"
    #base=$(basename "$input")
    test\_=$(echo "$base" | grep -c "\.")

    if [ $test\_ = 0 ];then
        echo "NONE"
        else
            ext="${base##.*}"
            #ext=$(echo "$base" | rev | cut -d. -f 1 | rev)
    fi
    echo ${ext}
}

function file\_type
{
    input="$1"
    type\_=$(file "$input" | cut -d: -f 2 | sed 's/\W/ /g')
    echo $type\_
}

function get\_byte\_size
{
    input="$1" size=$(du -b "$input" | cut -f 1)
    echo "${size}"
}


file\_="$1"
ext=$(get\_file\_ext "${file\_}")
type\_=$(file\_type "${file\_}")
size=$(get\_byte\_size "${file\_}")

printf "\"${file\_}\",\"${ext}\",\"${type\_}\",\"${size}\""
{% endraw %}
{% endhighlight %}

We want to call this row builder once for every file in a field system or
directory. This is a perfect job for GNU Parallel.

{% highlight bash %}
{% raw %}
find /somedirectory -type f | parallel --jobs 10
'inventory.sh {}' >> output.txt
{% endraw %}
{% endhighlight %}

# Short Cuts for Schema I hate writing out SQL create table syntax, and
<figure>
    <img src='/images/pandas_logo.png' alt="Pandas Logo">
    <figcaption>Pandas is great for apply a regex for rows to build new categorical columnes.</figcaption>
</figure>

luckily there are solutions. One is to use CSV-KIT which uses Python to provide
nice command line programs. Another way is just to use Pandas to create a data
frame, which will right away help debug your inventory csv generation as it
will fail to load if the rows are not homogeneous.

Then use Sqlalchemy to connect to SQLITE and finally push the data frame to a
SQLITE database file.

{% highlight bash %}
ipython import pandas as  pd from sqlalchemy import
create\_engine

df = pd.read\_csv("inventory\_file.csv") # if a line fails the exception will
be specific about which line which is great engine=
create\_engine("sqlite:////path/to/inventory\_database.db")
df.to\_sql("inventory", engine)
{% endhighlight %}

## SQLITE
I mentioned SQLITE above. I'm somewhat new to SQL and databases and I've found
that SQLITE3 is very easy to get going. The DB is a file that can be checked
into source control and / or shared with co-workers. No need to wait on a DBA
for prototyping.

I like SqliteStudio I like to use a GUI studio for SQLITE, SqliteStudio is
great;[http://sqlitestudio.pl/](SqliteStudio) It's not in the Debian or RHEL
repos, but it is a standalone executable so there's no installation, but untar
it and run.

The first thing you'll want to do is to rebuild some of the structure that was
lost going from a directory structure to a flat inventory file.

Here's some steps I typically always perform.

{% highlight sql %}
SELECT COUNT(`fullpath`),
CASE
    WHEN `fullpath` LIKE '%/subA/%' THEN `SubA`
    WHEN `fullpath` LIKE '%/subB/%' THEN `SubB`
    ELSE `Neither` END AS `Sub`
FROM `inventory`
    GROUP BY `Sub`
{% endhighlight %}

Screen shot of SqliteStudio.

<figure>
    <img src='/images/sql_studio_query_table.png' alt="Screen shot of SqlStudio">
    <figcaption>Exploring data for sanity checks.</figcaption>
</figure>

I typically prefer to make a categorical column to repeat this procedure easily
since it's a simple and necessary sanity check.

{% highlight sql %}
ALTER TABLE `inventory` ADD COLUMN `Sub`

INSERT INTO inventory(Sub)
    SELECT `fullpath`,
        CASE
            WHEN `fullpath` LIKE '%/subA/%' THEN `SubA`
            WHEN `fullpath` LIKE '%/subB/%' THEN `SubB`
            ELSE `Neither` END AS `Sub`
    FROM `inventory`
{% endhighlight %}

Now out basic sanity check is easier.

{% highlight sql %}
SELECT COUNT(`fullpath`) AS CNT, `Sub`
    FROM `inventory`
        GROUP BY `Sub`
            ORDER BY CNT
{% endhighlight %}


# Conclusion
So far no one has complained about this system. They are learning a few SQL
commands to select their data. Overall it's a lot fewer lines of code than the
MATLAB they would write.

Using my this system, sanity checking is built into the processing of asking
for data.
