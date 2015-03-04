---
title: Understanding Sed Scripts
description: You'll follow step by step explanations on several and interesting Sed scripts, and how they really work.
layout: default
---

## Number Lines, Right-Aligned Numbers ##



This is the script:

    sed '=' file.txt | sed 'N; s/^/      /; s/ *\(.\{6,\}\)\n/\1 /'

But first, we'll concentrate on this slightly simpler version:

    sed '=' file.txt | sed 'N; s/^/      /; s/\n/ /'

We changed `s/ *\(.\{6,\}\)\n/\1 /` with `s/\n/ /`.


If you run the command above, you'll get a result similar to this:

    1 foo
    2 bar
    ...
    9 consetetur
    10 sadipscing
    11 elitr,
    ...
    99 lorem
    100 ipsum
    101 dolor


As you see, the numbers are on the left, left-aligned.

    sed '=' file.txt

just produces:

    1
    foo
    2
    bar
    ...

Then, we pipe that output to

    sed 'N; s/^/      /; s/ *\(.\{6,\}\)\n/\1 /'

Let's break that further down.


    N

will just cause the pattern space to contain `1\nfoo`, then `2\nbar`, etc.

Then comes the next command:

    s/^/      /

That will substitute the
[beginning of the pattern space](basic-regex.html#beginning-of-pattern-space-anchor)
with 6 spaces. And what we have know is something like:

        1
    foo
        2
    bar
    ...
        999
    lorem

Note that the numbers are still on the left, left-aligned, and not on the
left but right-aligned, which is our final goal.

That is, they look like

    n
    nn
    nnn
    nnnn

but we want them to look like this

       n
      nn
     nnn
    nnnn

Finally, we have `s/\n/ /`. This is what causes the line number to appear in
the same line as the line of text itself.

Now, we go back and replace the simpler `s/\n/ /` with the trickier
`s/ *\(.\{6,\}\)\n/\1 /`. The advantage of this version is that the line numbers
are still on the left, but right-alinged:

      1 foo
      2 bar
        ...
      9 consetetur
     10 sadipscing
     11 elitr,
        ...
     99 lorem
    100 ipsum
    101 dolor


Just to make it clear, the full command now is

    sed '=' file.txt | sed 'N; s/^/      /; s/ *\(.\{6,\}\)\n/\1 /'


That will find zero or more spaces, followed by six or more characters (any
chars) followed by a newline. Those “six or more chars” are remembered because
of the parenthesis. Whe then place the contents of what was remembered back
with the `\1` notation and add a space after it. That is what causes both the
numbers to be on the same line as the line of text itself, and the right-alighment
of the numbers.

But how does it really right-align the numbers? Suppose we have

        n
    foo

And let's look at it in reverse order. `/ *\(.\{6,\}\)\n/` will match the
newline, then it will match at least 6 characters, and remember then, plus
any other white space on the far left, if necessary. Let's represent spaces
with a `-` character.

    -----n foo

becomes


Now,

       nn
    bar

becomes

    ----nn bar


and

     nnn
    lorem

becomes

    ---nnn lorem

That is

    -----n foo
    ----nn bar
    ---nnn lorem

The `\1` puts back some characters, possibly two or three numbers, and the rest
is filled with leading spaces. The newline is discareded (it was not matched
inside the group) and a single white space is added after the numbering.


Suppose you have a file with this content:

         9
    foo
         99
    bar
         999
    jedi

That is actually the output of

    sed = file.txt | sed 'N; s/^/      /'

But it is nice so we can isolate main part of the script to try to understand it:

    sed 's/ *\(.\{6,\}\)\n/\1 /' < file.txt


Sed will try to match 6 chars, and will use the <code>&nbsp;&#42;</code> part
of the regex to match leading spaces only if needed to make the match happen.
So, if we have three digits, and `.\{6,\}` will match those three digits, three
leading spaces just before the digits, and the <code>&nbsp;&#42;</code> part
will match any other white space before those if needed to cause the regex to
match.





