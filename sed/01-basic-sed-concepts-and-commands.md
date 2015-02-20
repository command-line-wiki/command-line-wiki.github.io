---
title: Basic Sed Concepts and Commands
layout: default
---

<ul class='toc'>
 <li><a href='#introduction'>Introduction</a></li>
 <li><a href='#a-note-about-shells'>A Note About Shells</a></li>
 <li><a href='#getting-started'>Getting Started</a></li>
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

## A Note About Shells ##

Sed is used from a shell, or in combination with some other command line tool
or shell feature. Most of the examples were tested on Bash because it is the
default on the majority of Linux systems. We assume that most examples will
also work on Zsh, Ksh, and other modern shells or can be easily modified to
work on those shells (at least after a quick web search, asking on the shell's
mailing list or their IRC channel). BSD users are generally savvy enough when
it comes to shell and command line so we'll assume they can tweak the
examples to make them work on those systems ans shells as well.


## Getting Started ##

Read (or [online here](https://www.gnu.org/software/sed/manual/sed.html#Introduction)):

    info sed 'Introduction'

And also read the few paragraphs before the description of command line options
(or [online here](https://www.gnu.org/software/sed/manual/sed.html#Invoking-sed)):

    info sed 'Invoking sed'

So, the (most basic) way to invoke Sed is:

    sed SCRIPT INPUTFILE...


`SCRIPT` is some sort of Sed command (or commands). They can be created “on the
fly” on the command line, or written in a text file and then passed to Sed. In
this basic approach, however, let's concentrate on the command line script
only.

`INPUTFILE...` means that you can pass one or more input files to Sed. The
three dots in man and info pages mean “one or more of the previous item/atom.”

The Sed `SCRIPT` can often be a complex beast, but let's exemplify the basic
syntax of Sed's invocation with a script that replaces `foo` with `bar` on
the entire file.

    sed 's/foo/bar/' file.txt

The part `'s/foo/bar/'` is means substitute (`s`) `foo` with `bar`. The `/`
character is the delimiter. The delimiter is the character used to *delimit*
the text that is matched text and the replacement text. In our example, `foo`
is what we want to match, and `bar` is what we want to replace `foo` with. If
`file.txt` contains the string `foo` on any of its lines, it will be replaced
with `bar`. No matter whether the each line contained the `foo` and the
substitution occurred or not, all lines are finally printed to *stdout*.


As for the INPUTFILE, there are several ways to feed and *input file* to Sed.
Behold!

The most common case:

    sed 's/foo/bar/' file.txt

This common case easily accepts more than one filename:

    sed 's/foo/bar/' file1.txt file2.txt etc.txt

Using an explicit redirection:

    sed 's/foo/bar' < file.txt

Piping:

    echo 'this is foo' | sed 's/foo/bar/'
    printf %s 'this is foo' | sed 's/foo/bar/'
    cat file.txt | sed 's/foo/bar/'


Redirecting and piping:

    < file.txt | sed 's/foo/bar/'


Using a here string:

    sed 's/foo/bar/' <<< 'this is foo'


Using a here document:

    $ sed 's/foo/bar/' <<EOF
    > this is foo
    > EOF
    this is bar

In this last example, the “$” symbol is Bash's PS1, and the “&gt;” symbol is
Bash's PS2. You don't type those characters yourself. They are shown just to
make it clear how the entire command looks like. Let's describe how to type
the last command step by step, just in case.

The shell is happily waiting for you to type a command, and you see the symbol
“$”. You then type `sed 's/foo/bar' <<EOF` and hit &lt;Enter&gt;. Now the
prompt changes to a “&gt;” to show you that it understood that the command is
not finished and it is waiting for you to type more instructions. Then you
enter `this is foo` followed by another &lt;Enter&gt;. Finally you type
is `EOF` again and type &lt;Enter&gt; one last time. The shell then sees that
you finished typing your command and run the entire script, showing `this is
bar`.

Read more about Here Documents and Here Strings
[here](http://mywiki.wooledge.org/HereDocument) and
[here](http://tldp.org/LDP/abs/html/here-docs.html). They are also described
in your shell's man page. Make sure there is not spaces or tabs after the
closign EOF delimiter or the shell will not understand that you are done typing
the command. As a rule of thumb, it is generally a good idea no to leave
traling spaces on any lines and on any programming language. There are several
reasons why that is a good thing but that our topic here. Configure your editor
to show trailing whitespace. I'll show how to do it in Vim:

    set listchars=:trail-
    set list

Now, Vim will show a `-` for every stray traling whitespace at the
end of lines (`:help listchars` and `:help list`).




