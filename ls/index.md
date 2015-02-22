---
title:  'ls command line tutorial'
description: 'This is a tutorial about the LS command (Linux, BSD, MacOS and other Unix-like OSs). Lots of examples.'
layout: default
---
# ls command tutorial #

This is a tutorial about the LS command (Linux, BSD, MacOS and other Unix-like OSs). Lots of examples.

Basic:

    type ls
    info ls
    man ls
    ls --help | less


If a directory is not specified, `ls` lists the current directory by default. Hence,
the next two commands do the same thing:

    ls
    ls ./


List all files except hidden ones:

    ls


List everything, even hidden files:

    ls -a
    ls --all


List files permission, owner, group, size in bytes, etc...:

    ls -l


List the contents of `/etc/profile.d/`:

    ls -l /etc/profile.d/


List the contents of `/etc/skel/` (which should contain only hidden files):

    ls /etc/skell
    ls --all /etc/skell

List all the contents of your home directory and pipe the output to the pager `less`:

    ls -al ~/ | less


## Globs and Patterns ##

List all `.txt` files:

    ls *.txt


List all `.png` files:

    ls *.png

List all `.ico` and `.jpg` files:

    ls *.ico *.jpg


List all files that have an extension of four characters:

    ls *.????

List all files that have an extension of three or two characters:

    ls *.??? *.??

List all files that start with four characters, followed by `-plans.txt`:

    ls ????-plans.txt

List all files that start with `2014`, followed by anything, ending in `.txt`:

    ls 2014*.txt


List and pipe to the pager `less` with **colors**:

    ls ~/ | less -r

