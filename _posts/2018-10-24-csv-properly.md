---
layout: post
title: "CSV Properly"
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2018-10-24 20:16:16
---

# Intermediate Guide to CSV Files
  and the aweful csv's I've seen and been handed...

<figure><img src="https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/76/18/metablogapi/2055.HSG09091001_71EB0EE6.jpg"></figure>

If you've been around the business world and Excel, you may have had to export your work as a Comma-separated-variable text file. Or you're a reseracher and a self taught programer and have realized that text file outputs are easily portable and a good choice for small to medium sized data files.

A common problem I've had with other people's CSV files, is that they suck...That is CSV is not a great format.

Some of the reasons CSV file are hard to work with are not solvable and not anyone's fault. Text is a bulky format for data. Each ASCII character takes 2 bytes to store and if you encode in Unicode like UTF-8, then it's at least 2 bytes per character. That adds up in file size and can make sending files in email more of a PITA.

Now you really shouldn't be sending files via email when there's Box, Dropbox, and source control. But if you work in a business setting and using Excel often, then email would be a normal thing to want to do...although you shouldn't, use Box or Dropbox.

##  Sources of PITA from CSV
1. Data types are not preserved
..* currency, date, time, integer or a decimal??
2. Font information
..* not often important but for sciencetific work can come up
3. No standard format for CSV
..* "c" stands for comma, but CSV's can be delimited by other characters
4. Large file size for big data sets
..* makes it hard to send files in email

## How to CSV's Better
Try to use ASCII when possible to save your files. It's more portable when the recieveing side is working in a terminal. Yes, terminals support Unicode now and VI and Emacs do as well, but sometimes non-ascii files can cause someone a PITA and you want them to get your work done ASAP.

Real programmers probably won't care but this post is about making CSV's better and if you're passing your CSV on to do data analysis, in my experience ASCII helps.

### Not All Carriage Returns Are the Same
CSV files are text. Windows, OSX and Linux, treat the newline differently. With PC's, the newline is denoted by the codes "\n\r", which is line-feed and a carriage return. Whereas Mac's use just the carriage return, "\r" and Linux text files will have only a line-feed, "\n".

This is annoying if you don't expect it. Keep this in mind when you see "extra" blank lines in a text file where you didn't expect it.

Being a Linux user, I'd say you should always use the Linux newline format. It plays nicer with version control systems and slighly easier to convert to and from the other two platforms, OSX and Windows.

### Data Can Have Commas Embedded In It
#### Why you should enclose all columns with double quotes
Since the column delimiter is a comma, it would be a PITA if your data happened to have commas is in it. It happens a lot if you're a European or if you are storing file names. Believe or not, there are file names out there that use commas in the name !

The solution is to use double quotes around each field. There is an option in Excel and other programs, to enclose all columns with quotes and I suggest the double quote, not a single, because they are prettier.

## Use Header, or Else
Use a header on the file even if it's obvious what the data is. Some programs load a CSV file and automatically pick off the header and it helps the next person. It's really sucks to get a file in email from someone asking me to help them and there's no header.

## Choice of Delimiter
There is a reason to use tabs as a delimiter. It's not a horrible practice and some people prefer it. I think it's a PITA, because the file typically doens't display on a terminal very well. If you're learning to use the terminal more in Unix like environment, then look up the utility program "column".

I say, use commas, it's a CSV file right, why make people guess the delimiter ??

# Glossary
**field** Same as a column, the data will be columns, made of rows.
**row** Rows go down in a column and left to right across is a row of data.
**delimiter** The symbol that separates the fields.
**newline character(s)** The special characters that signify where the line break is. These are not visible in most programs which display text. However, they will be visable if the program expects a particular symbol for a newline and another iscurrently used.
**text file** A type of file that is strictly text data as opposed to binary data. A Word doc is binary. If you open one is a text editor will likely see a bunch of symbols. Rich Text Format (RTF) is an exception, being pure text it is very portable.
