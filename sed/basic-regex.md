---
title: Sed ^ anchor
description: How to use the beginning of line/pattern space anchor in Sed, examples and gotchas
layout: default
---

# ^ doesn't mean “beginning of line” #

The `^` anchor generally means “match the null position at the beginning of a
line”, although you can generally think like that in Sed as well, sometimes
that leads to wrong assumptions.

The input file contains:

    foo
    bar
    jedi

If you do

    sed '=' file.txt

the output is

    1
    foo
    2
    bar
    3
    jedi

Now you do:

    sed '=' | sed 'N; s/^/      /' file.txt

and the output, suprisingly, is:

        1
    foo
        2
    bar
        3
    jedi

As you see, only the odd output lines were indented with 6 spaces, the
even ones were left untouched, even though we used `s/^/      /'. Why isn't
Sed adding spaces in the beginning of all lines? Read further along to discover.

When we do `N`, we add a newline to the pattern space, and then the next line
of input. That means that `sed '=' | sed 'N'` causes the pattern space to hold

    1\nfoo

then

    2\nbar

then

    3\njedi

We think, for instance, that “2” is one line, and “bar” is another line, and
they actually are each one a separate line. The pattern space even contains a
`\n` between them, and that means the pattern space really contains two lines.

Here comes the gotcha: in Sed, `^` means “match the null position at the
beginning of the pattern space” (and not at the beginning of a line).

That means that even though when the pattern space contains `3\njedi` (and
that means we have two lines in the pattern space), `s/^/      /` will only
add 6 spaces in front of “3”, leaving the “jedi” part alone. It will add 6
spaces at the beginning of the pattern space, and not at the beginning of
the line (or lines).

Remember this: When it comes to regex, `^` generally means “the null position
at the beginning of a line”, but in Sed it really means “the null position
at the beginning of the pattern space.”

