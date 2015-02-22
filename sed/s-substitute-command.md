---
title: Sed s substitute command
description: A lot of tips and examples of Sed's s substitute command.
layout: default
---

<ul>
  <li><a href='#s-substitute-command'>s substitute commando</a></li>
  <li><a href='#regex-vs-strings'>Regex vs Strings</a></li>
</ul>

# s substitute command #


First thing ([online](https://www.gnu.org/software/sed/manual/sed.html#The-_0022s_0022-Command)):

    info sed 'The "s" Command'


Read the entire node for the `s` command just to get an idea.

The syntax for the `s` command is:

    sed 's/REGEX/REPLACEMENT/FLAGS'


The REGEX part is what we are trying to find to them replace with REPLACEMENT.

In this example, if a line starts with “Note”, it is replaced with “NOTE”.

    sed 's/^Note/NOTE/' < file.txt


We used `/` as the delimiter, but other characters could be used. Let's try
some:

    sed 's:^Note:NOTE:' < file.txt
    sed 's|^Note|NOTE|' < file.txt
    sed 's%^Note%NOTE%' < file.txt
    sed 's_^Note_NOTE_' < file.txt
    sed 's#^Note#NOTE#' < file.txt


Some seds require that you escape the first delimiter if it is not the default
one:

    sed 's\:^Note:NOTE:' < file.txt

Although GNU Sed doesn't require that escape, it works with it as well.

The most important use for a different delimiter is when the search pattern
itself contains the delimiter. Compare:

    sed 's/\/usr\/local\/bin\//\/opt\/bin\//' < myscript

vs

    sed 's:/usr/local/bin/:/opt/bin/:' < myscript


## Regex vs Strings ##

Suppose you have a shell script that asks the user for a search pattern and then
uses that pattern with Sed. What follows is a very simple script, and it could
simply be run on the command line with Sed directly, but it could be more complex.
Still, it is enough to illustrate an **important** Sed concept.


    #!/usr/bin/env bash

    echo 'Type search pattern: '
    read pattern

    sed 's/'"$pattern"'/MODIFIED/' < file.txt

Now, you run the script and you provide `opt/` as the search pattern. The shel
will have no problems with it, but when Sed tries to use `$pattern`, it will
look like

    sed 's/'"opt/"'/MODIFIED/' < file.txt

And you will get an error similar to this:

    sed: -e expression #1, char 10: unknown option to `s'


That is because the `/` in `opt/` is interpreted as the second delimiter, then
there is the third one just before the M in MODIFIED, and a fourth one at the
end, but the `s` command needs to see the delimiter exactly three times.

For simple cases, you could just change the delimiter in the script to a
character that you know will not show up in the search pattern. But in a lot of
cases, it is very unlikely that you can guarantee that a character will not be
used in the search pattern and in cases like this Sed is not the right tool
for the job.

Then we come to the topic stated at the heading of this section, “Regex vs
Strings”. **Sed has no concept of strings, and every search pattern is a
regex**. Actually, when we say that Sed is not the right tool for a certain
situation, it is because regexes are not the right tool for certain jobs and
Sed only works with regex (and not with strings, where each character is
treated literally.

For example, in PHP, you use `preg_replace()` (Perl Regular Expression Replace)
to do regex substitutions, but you can use `str_replace()` when that is
necessary. Other languages have similar functions. And Sed simply has no
concept of strings.

Therefore, `s/foo/bar/` doesn't mean “match the string 'foo' and replace it
with bar.”. Rather, it means “match 'f', immediately followed by 'o',
immediately followed by another 'o' and replace it with 'bar'.” For the
replacement text, it doesn't matter that much if we call it a string sometimes,
but for the search pattern, we must be very concious that it is always treated
as a regular expression, so, saying things like “search for the string 'foo'”
is often misleading, so, it is best to avoid it.

