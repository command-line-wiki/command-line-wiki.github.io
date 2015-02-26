---
title: Often Used Sed Commands
description: This page describes and provides examples on the most used sed commands.
layout: default
---

<ul class='toc'>
 <li><a href='#getting-started'>Getting Started</a></li>
 <li><a href='#comment-command'># comment command</a></li>
 <li><a href='#q-quit-command'>q quit command</a></li>
 <li><a href='#d-delete-command'>d delete command</a></li>
 <li><a href='#p-print--command'>p print command</a></li>
</ul>


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


## \# comment command ##

Read ([online](https://www.gnu.org/software/sed/manual/sed.html#Common-Commands)):

    info sed Common\ Commands

Technically, `#` is also a command, and its use is the same as for programming
languages: to describe and document what specific parts of code does, add
licencing information, and even curse when projects requisites keep changing.

Then, beware of that `#n` thing at the very first line of a script, as
mentioned in the info page.


## q quit command ##

Read ([online](https://www.gnu.org/software/sed/manual/sed.html#Common-Commands)):

    info sed Common\ Commands


One example of the use of `q` is to do something only with the first occurrence
of a pattern. If no addresses are specified, Sed will try to find a match in all
lines of a file. That means that if you are trying to replace “Intro” with
“INTRO” only the first time “Intro” appears on a file, you have to use `q`.

    sed '/Intro/ { s/Intro/INTRO/; q };


We haven't either talked about using regex as addresses yet, neither about using
`{` and `}` to group commands, but that above command says, “look for a line
that contains 'Intro' (and this is a regex, not a string), then, apply the
following group of commands when that regex matches a line.” Inside the group
we have the `s/Intro/INTRO/` command, then the command separator `;`, then the
last command, the `q`.


## d delete command ##

TODO


## p print command ##


TODO



## n command ##


From `info sed 'Common Commands'`:

<blockquote>
If auto-print is not disabled, print the pattern space, then, regardless,
replace the pattern space with the next line of input. If there is no more
input then sed exits without processing any more commands.
</blockquote>

This is file.txt:

    $ cat file.txt
    foo
    bar
    jedi

Let's understand what the following Sed command does:

    sed 'n' < file.txt

Sed reads the first line of input, “foo” and places it in the PS. The command
is `n`, so, Sed prints the PS and replaces PS with the next line, “bar”. Again,
the `n` command is applied, which makes Sed print PS (which is now “bar”), and
the last line of input is read. Now, “jedi” is in PS,`n` is executed, which
causes PS to be once more printed to STDOUT. There are not more input lines, so
sed just exits. At the end, this command just prints

    foo
    bar
    jedi

just like the `cat` command would do. As we see, the `n` command is not very
useful on its own, but at least we now know how it works.

Let's try an example that makes a little more sense. The `n` command is useful
in situations where you want to deal with “every other line”, that is, you skip
one line, and do something with the other. For example, you have this file
where each country name is written in en\_US in odd lines, and in pt\_BR in
even lines:

    Japan
    Japão
    Brazil
    Brasil
    United States
    Estados Unidos

You want to add “(pt\_BR)” after each line where the name is indeed in pt\_BR.

    $ sed 'n; s/.*/& (pt_BR)/' < countries.txt
    Japan
    Japão (pt_BR)
    Brazil
    Brasil (pt_BR)
    United States
    Estados Unidos (pt_BR)


And if you want to add “en\_US” to the respective lines as well, we do:

    $ sed 's/.*/& (en_US)/; n; s/.*/& (pt_BR)/' < countries.txt
    Japan (en_US)
    Japão (pt_BR)
    Brazil (en_US)
    Brasil (pt_BR)
    United States (en_US)
    Estados Unidos (pt_BR)


Let's explain `sed 'n; s/.*/& (pt_BR)/'` first.

Sed reads “Japan” into PS. The command is `n`, so, Sed just prints it and reads
the next line of input. Note that Sed doesn't run the `s` command on “Japan”
(because of the `n` command). Now “Japão” is in PS (again because of `n`) and
the `s` command is run, causing “Japão” to become “Japão (pt\_BR)”, and PS is
printed. There are no more commands, so, Sed just restarts the cycle again.

“Brazil” is read into PS (which automatically replaces whatever was
there). `n` is the command to be run, which causes PS to be printed and the
next line of input to be read into PS, which causes PS now contain “Brasil”,
which is the text that the `s` command operates on this time, turning “Brasil”
into “Brasil (pt\_BR)”. PS is once more printed.

The cycle starts once more for “United States”, which is simple printed, “Estados
Unidos” is read into PS and operated on by the `s` command and then printed.

Note that every time sed reads a line into the PS, it will run any commands on
the contents of the PS and then automatically print the PS unless the `-n`
command line option is used.

As for `'s/.*/& (en_US)/; n; s/.*/& (pt_BR)/'`, the process is very similar.
First, “Japan” is read into PS. The command is `s`, which turns “Japan”
into “Japão (en\_US)”. Then `n` causes PS to be printed, and the next
line of input to placed into PS (the old content of PS is gone by now). The
current command is the second `s`, which replaces “Japão” with “Japão
(pt\_BR). PS is printed and the cycle starts again for “Brazil” and
finally for “United States”.

