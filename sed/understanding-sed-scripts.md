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
with 6 spaces. And what we have now is something like:

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


`sed 'N'` will cause the pattern space to contain something like 
<code>3\nfoo</code>, then
`s/^/      /` makes the pattern space contain
<code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3\nfoo</code>, and finally, `s/
*\(.\{6,\}\)\n/` will match (starting from the end) the newline, then digit 3
and five spaces.

If the pattern space contains
<code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;974\nbar</code>,
`'s/ *\(.\{6,\}\)\n/'` will match the newline, the tree digits 9, 7 and 4, and
tree spaces. Note that in all the cases, altough <code>&#42;</code> is greedy,
only the necessary numbers of spaces are matched in <code>&nbsp;&#42;</code> so
that the entire expression finds “zero or more spaces, followed by six
characters, followed by a newline.”


