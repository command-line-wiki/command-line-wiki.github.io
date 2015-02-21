---
title: Compiling and Running Java Programs With javac and java Command Line Tools
description: Here you will learn how to compile and run Java programs on the command line, including .class and .jar files in your application.
layout: default
---

You have lots of `*.jar` files in

    WEB-INF/lib/<lots of .jar files here>

and some `.java` files in

    WEB-INF/src/mypkg/<some .java files here>

This is how to compile the application which requires `.jar` files from one
directory and `.java` files from another. The generated `.class` files are placed in
`WEB-INF/classes/`. Rhe compiler creates the packages/directories accordingly.

The class that contains the `public static void main` method is `Main` inside
the package `mypkg`. To run the application, it is necessary to load the
`.class` files and all those `.jar` files.

Compile:

    javac -classpath 'WEB-INF/lib/*' WEB-INF/src/mypkg/*.java -d WEB-INF/classes/

Run:

    java -classpath 'WEB-INF/lib/*:WEB-INF/classes/' mypkg.Main


To compile, and this is the most important part, note the single quotes to
prevent \* from being expanded by the shell. `javac` needs itself to interpret
the glob (*). You *must prevent the shell from expanding the \** and let `javac`
take care of the glob the way it pleases.


To run the application, note that we specified the `.jar` files path and the
`.class` files path using the `-classpath` option and separated both directories
using `:`. On windows it would be `;`.



The next command line should run a java application, but it is wrong and won't
work. Assume a \*nix environment.

    java -classpath '/path/to/jarfiles/*:/path/to/classfiles/somepkg/*' somepkg/Main

`javac` only needs to know the base directory for class files, and the package
hierarchy should not be specified at the command line. `javac` handles the
package automatically. We are using

    /path/to/classfiles/somepkg/

but it should simply be:

    /path/to/classfiles/

The other problem is the glob in `/path/to/classfiles/*`. That is used only to
specify jar files. Again, for class files, `javac` just needs to know their base
directory. It then *automatically* looks for packages and filenames ending with
`.class` inside that base directory. It recurses through deeper directories as
necessary.

A third proplem is `somepkg/Main`, which should be `somepkg.Main`.

The correct command is:

    java -classpath '/path/to/jarfiles/*:/path/to/classfiles/' somepkg.Main


The directory strcucture is really /proj/somepkg/Main.class, but when running
java applications packages (which are directories on the filesystem) are
specified using dot notation, not path separator notation. So, if you have

    ~/proj/pkg1/subpkg/Main.class

You specify this to run the program using the `java` command:

    java pkg1.subpkg.Main


Or, if using other `.jar` and `.class` files:

    java -classpath 'WEB-INF/lib/*:WEB-INF/classes/' pkg1.subpkg.Main
