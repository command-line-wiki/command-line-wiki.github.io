---
title: 'mv command line examples'
description: 'Lots of examples on how to use the mv command on unix-like systems'
layout: default
---

# mv command examples #



Renames my-script.c to my-program.c:

    mv my-{script,program}.c

Renames script-V1.pl to script-v1.pl:

    mv script-{V,v}1.pl

Renames script1.pl to script-v1.pl:

    mv script-{,v}1.pl

Renames messate.txt to original-message.txt:

    mv {,original-}message.txt

Renames original-message.txt to message.txt:

    mv {original-,}message.txt


NOTE: Those command line ideas work with the `mv` command as well.
