---
title: 'Rename files from the command line'
description: 'Rename files from the command line either using the plain rename command, perl-rename or shell tricks'
layout: default
---

# Rename files from the command line #

Rename files from the command line either using the plain rename command, perl-rename or shell tricks.

## Rename ##

First things first:

    rename --help
    man rename


##Using Bash ##

Rename directories. Just give ascending numeric names, starting from 21 (pks from a db):

    $ ls
    2958/  3157/  3158/  3303/  3390/  4005/  4006/  4007/  4754/  4814/  4918/  4996/  5367/  6726/

    $ c=21; for d in */; do printf -v name '%03d' $c; mv "$d" "$name"; ((c++)); done

    $ ls
    021/  022/  023/  024/  025/  026/  027/  028/  029/  030/  031/  032/  033/  034/


Recursively renamve `blah..blah.txt` to `blah.blah.txt` (this is the plain rename command,
not the perl-rename one):

    find . -exec rename '..' '.' {} \;

Lowercase every filename:

    for f in *[[:upper:]]*; do mv -- "$f" "${f,,*}"; done

Replace whitespace with underscores recursively:

    find . -name '*.txt' -exec perl-rename.pl 's/ /_/g' {} \;


