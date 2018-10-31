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

# Ranting About CSV Files

<figure>
<img width="500" src="https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/76/18/metablogapi/2055.HSG09091001_71EB0EE6.jpg">
</figure>

If you've been around the business world and Excel, you may have had to export your work as a CSV, Comma-separated-variable text file. Or you're a researcher, a self taught programmer and have realized that text file outputs are easily portable and a good choice for small to medium sized data files. Text files are easy to sanity check, run terminal tools against and open in alomost any program.

A common problem I've had with other people's CSV files, is that they suck... The main reason is because CSV is just not a great format. However, they can made better or worse and this post outlines a few simple ways to make they suck less.

Text is a bulky format for data. Each ASCII character takes 2 bytes to store and if you encode in Unicode, you're using at least 2 bytes per character. That adds up in file size and makes sending files in email a PITA.

You shouldn't be sending files via email you should use Box, Dropbox, or source control. However, there are reasons to email files to people and we all do it.

##  Causes of Pain
**Data types are not preserved**
    - currency, date, time, integer or a decimal, that info is lost
    - Excel does have a way to encode data types but it obviously doesn't port well
        - [https://superuser.com/questions/318420/formatting-a-comma-delimited-csv-to-force-excel-to-interpret-value-as-a-string](superuser question)
**Font information**
**No standard format for CSV**
    - "c" stands for comma, but CSV's can be delimited by other characters
**Large file size for big data sets**
    - makes it hard to send files in email
    - long load time is a PITA to iterate with.

## How to Make CSV's Better
Try to use ASCII when possible to save your files. It's more portable when the receiving side is working in a terminal. Yes, terminals support Unicode now and VI and Emacs do as well. Sometimes Unicode files can cause someone pain and you want them to get work done ASAP.

### Not All Carriage Returns Are the Same
<figure>
<img width="350" src="https://i.stack.imgur.com/e4xm6.jpg">
<figcaption>https://i.stack.imgur.com/e4xm6.jpg</figcaption>
</figure>

CSV files are text. Windows, OSX and Linux, treat the newline differently. With PC's, the newline is denoted by the codes `\n\r`, which is line-feed and a carriage return. Whereas Mac's use just the carriage return, `\r` and Linux text files will have only a line-feed, `\n`.

This is annoying if you don't expect it. Keep this in mind when you see "extra" blank lines in a text file where you didn't expect it.

Being a Linux user, I'd say you should always use the Linux newline format. It plays nicer with version control systems and slightly easier to convert to and from the other two platforms, OSX and Windows.

If you work with CSV files a lot, find a good conversion tool to convert the text into either DOS mode or Linux. Terminal users both Linux and DOS have a program called `dos2unix` and `unix2dos` which handles those transformations easily and quickly.

### Enclose Fields with Double Quotes
<figure>
<img width="320" src="http://webdesignledger.com/wp-content/uploads/2010/12/apostrophe-quotation-marks-01.gif">
<figcaption>http://webdesignledger.com/wp-content/uploads/2010/12/apostrophe-quotation-marks-01.gif</figcaption>
</figure>

Since the column delimiter is a comma, it would be a PITA if your data happened to have commas is in it. It happens a lot if you are European or if you are storing quirky file names. Believe or not, there are file names out there that use commas in the name.

The solution is to use double quotes around each field. There is an option in Excel and other programs, to enclose all columns with quotes and I suggest the double quote, not a single, because they are prettier.

### Choice of Delimiter
<figure>
<img width="300" src="https://image.slidesharecdn.com/regexfinalsatyavvd-110719054116-phpapp02/95/regular-expressions-12-728.jpg?cb=1311054689">
<figcaption>
Remember Yahoo!, me neither, https://image.slidesharecdn.com/regexfinalsatyavvd-110719054116-phpapp02/95/regular-expressions-12-728.jpg?cb=1311054689
</figcaption>
</figure>

There is a reason to use tabs as a delimiter. It's not a horrible practice and some people prefer it. I think it's a PITA, because those files typically do not display on a terminal very well. If you're working in a terminal in Unix like environment and like tab spaced fields, then look up the utility program "column".

Use commas, it's a CSV file.

## Use A Header
<figure>
<img width="260" src="https://cdn.shopify.com/s/files/1/0578/5357/products/Pic-5.jpeg?v=1469731081### Use A Header Dammit">
<figcaption>Cool header, https://cdn.shopify.com/s/files/1/0578/5357/products/Pic-5.jpeg?v=1469731081</figcaption>
</figure>

Use a header on the file even if it's obvious what the data is. Some programs load a CSV file and automatically pick off the header and it helps the next person. It's really sucks to get a file in email from someone asking me to help them and there's no header.

# Glossary
**field** Same as a column, the data will be columns, made of rows.

**row** Rows go down in a column and left to right across is a row of data.

**delimiter** The symbol that separates the fields.

**newline character(s)** The special characters that signify where the line break is. These are not visible in most programs which display text. However, they will be visable if the program expects a particular symbol for a newline and another is currently used.

**text file** A type of file that is strictly text data as opposed to binary data. A Word doc is binary. If you open one is a text editor will likely see a bunch of symbols. Rich Text Format (RTF) is an exception, being pure text it is very portable.

**header** The first row of a CSV file that contains the names of all the fields. It is formatted the same, double quoted and comma separated.
