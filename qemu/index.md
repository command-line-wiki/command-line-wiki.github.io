---
title: QEMU Common and Useful Command Line Options
description: Some very commonly used and useful QEMU examples, how to start it for installing a system and run an installed system, SSH into guest system, and more.
layout: default
---

## Creating the hard disk image ##


qcow2 is that "increase HD as needed" thing, but it causes OpenBSD to keep
giving "not enough space" errors. It happens on VirtualBox as well.

DON'T USE THIS:

    qemu-img create -f qcow2 obsd1 20G

But a fixed size HD works. `-f raw` creates a fixed-size disc.

USE THIS:

    qemu-img create -f raw obsd1 20G


## Installing ##

Start install:

    qemu-system-i386 -cdrom ~/Downloads/install56.iso -boot order=d obsd1


`-boot order=d` causes the boot order to try the cdrom first.


## Starting the Installed System ##

Start Installed System:

    qemu-system-i386 -boot order=c obsd1 -k pt-br

`-k` is for the keyboard language. The example used a Brazilian Portuguese
keyboard layout.

`-boot order=c` tells that the boot process should start directly from the
hard drive (and not from cdrom or floppy).


## SSH Into Guest OpenBSD ##

ssh to FreeBSD run in Qemu (worked on OpenBSD as well):

  http://daemonforums.org/showthread.php?t=308


Start the system with the `-redir tcp:9000::22` option:

    qemu-system-i386 -boot order=c obsd1 -k pt-br -redir tcp:9191::22


And then invoke SSH like this:

    ssh devel@localhost -p 9191


## OpenBSD Keyboard ##

https://demoncyber.wordpress.com/2012/01/28/configurando-teclado-abnt2-no-openbsd/

Make sure you start qemu system with the `-k pt-br` command line option and then
run this from inside the running OpenBSD guest:

    vi /etc/wsconsctl.conf

and add:

    keyboard.encoding=br

Or from the command line:

    wsconsctl -w keyboard.encoding=br


More info:

Short how to (the commands above):

  https://demoncyber.wordpress.com/2012/01/28/configurando-teclado-abnt2-no-openbsd/

More detailed stuff about a bug:

  http://blog.nielshorn.net/2011/03/qemu-and-brazilian-keyboards/





