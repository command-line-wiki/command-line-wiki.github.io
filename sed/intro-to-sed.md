---
title: Introduction to Sed, The Stream Editor
description: How to get started learning and using Sed on unix-like systems (Linux, BSD).
layout: default
---

<ul class='toc'>
 <li><a href='#introduction'>Introduction</a></li>
 <li><a href='#a-note-about-shells'>A Note About Shells</a></li>
</ul>

## Introduction ##

We will be most concerned with GNU Sed (it seems to be version that offers more
features (GNU extensions vs POSIX). Still, we'll try, whenever possible, to
offer versions of the examples that work in a more portable way.



Read Sed's info and man page. The info page is much more verbose, while the man
page is more concise. In my opinion, although the info page is much more
beginner friendly (and I dare to say that the man page even lacks important
information), both are hard for a Sed neophyte to get started.

Info page:

    info sed

The info page can also be read online
[here](https://www.gnu.org/software/sed/manual/sed.html).

Man page:

    man 1 sed


It is a good idea to read the man and info pages entirely at least once
to get a feel for what commands, options and features are available. Don't
worry if you don't understand some or most of the parts. We are going to
refer back to specific documentation as we go along when necessary.

In OpenBSD, both the info page actually opens the man page. As mentioned
earlier, the info page is more detailed. Keep in mind, however, that OpenBSD
is famous for its precise, correct and up to date documentation (among other
qualities, of course). Sed's online manual page is found
[here](http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man1/sed.1?query=sed).



## A Note About Shells ##

Sed is used from a shell, or in combination with some other command line tool
or shell feature. Most of the examples were tested on Bash because it is the
default on the majority of Linux systems. We assume that most examples will
also work on Zsh, Ksh, and other modern shells or can be easily modified to
work on those shells (at least after a quick web search, asking on the shell's
mailing list or their IRC channel). BSD users are generally savvy enough when
it comes to shell and command line so we'll assume they can tweak the
examples to make them work on those systems ans shells as well.

