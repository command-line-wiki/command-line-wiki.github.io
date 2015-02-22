---
title: Other Useful Commands
description: Some other useful command line examples that don't fit a particular category.
layout: default
---


# Some Other Useful Command Line Examples #


Lowercase all text recursively creating a bkp file:

    find . -name '*.txt' -print0 | xargs -0 sed -i.bkp '' -e 's/./\l&/g'


Capitalize every first word of files recursively:

    find . -name '*.txt' -print0 | xargs -0 sed -i.bkp2 '' -e 's/\b\(.\)/\u\1/g'


Add new line to end of lines:

    find . -name '*.txt' -exec sed -i -e '$a\' {} \;
