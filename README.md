esp-open-sdk (with extensa gcc tool chian)
------------
This fork is continuation of the work of https://github.com/pfalcon/esp-open-sdk
Changes to this fork provides the support for additional achitectures "D106_micro_be_ecc" and "D106micro_be_TRAX_PROD" for xtensa GCC tool-chain. 

The complete SDK consists of:

1. Xtensa lx106/d106 architecture toolchain, based on
   following projects:
    * https://github.com/xtensatraxprodgcc/crosstool-NG
    * https://github.com/jcmvbkbc/gcc-xtensa
    * https://github.com/jcmvbkbc/newlib-xtensa
    * https://github.com/tommie/lx106-hal

The source code above originates from work done directly by Tensilica Inc.,
Cadence Design Systems, Inc, and/or their contractors.

Requirements and Dependencies
=============================

To build the standalone SDK and toolchain, you need a GNU/POSIX system
(Linux, BSD, MacOSX, Windows with Cygwin) with the standard GNU development
tools installed: bash, gcc, binutils, flex, bison, etc.

Please make sure that the machine you use to build the toolchain has at least
1G free RAM+swap (or more, which will speed up the build).

## Debian/Ubuntu

Ubuntu 14.04:
```
$ sudo apt-get install make unrar-free autoconf automake libtool gcc g++ gperf \
    flex bison texinfo gawk ncurses-dev libexpat-dev python-dev python python-serial \
    sed git unzip bash help2man wget bzip2
```

Later Debian/Ubuntu versions may require:
```
$ sudo apt-get install libtool-bin
```

## MacOS:
```bash
$ brew tap homebrew/dupes
$ brew install binutils coreutils automake wget gawk libtool help2man gperf gnu-sed --with-default-names grep
$ export PATH="/usr/local/opt/gnu-sed/libexec/gnubin:$PATH"
```

In addition to the development tools MacOS needs a case-sensitive filesystem.
You might need to create a virtual disk and build esp-open-sdk on it:
```bash
$ sudo hdiutil create ~/Documents/case-sensitive.dmg -volname "case-sensitive" -size 10g -fs "Case-sensitive HFS+"
$ sudo hdiutil mount ~/Documents/case-sensitive.dmg
$ cd /Volumes/case-sensitive
```

Building
========

Be sure to clone recursively:

```
$ git clone --recursive https://github.com/xtensatraxprodgcc/esp-open-sdk.git
```

Here, assuming that user is interested only in xtensa gcc tool chain. 

To build tool chain for "D106u_LX7_ecc_prod", run the make as given below.
```
$ make STANDALONE=n XTENSA_CORE=D106u_LX7_ecc_prod
```

To build tool chain for "D106_micro_be_ecc", run the make as given below.
```
$ make STANDALONE=n XTENSA_CORE=D106_micro_be_ecc
```

To build tool chain for "D106micro_be_TRAX_PROD", run the make as given below.
```
$ make STANDALONE=n XTENSA_CORE=D106micro_be_TRAX_PROD
```
For any other requirement please follow ReadMe page on https://github.com/pfalcon/esp-open-sdk

Using the toolchain
===================

Once you complete build process as described above, the toolchain (with
the Xtensa HAL library) will be available in the `xtensa-$XTENSA_CORE-elf/`
subdirectory. Add `xtensa-$XTENSA_CORE-elf/bin/` subdirectory to your `PATH`
environment variable to execute `xtensa-$XTENSA_CORE-elf-gcc` and other tools.
At the end of build process, the exact command to set PATH correctly
for your case will be output. You may want to save it, as you'll need
the PATH set correctly each time you compile. Here XTENSA_CORE is cpu architecture 
provided while building the complier.


Pulling updates
===============
To get updates and prepare to build a new SDK, run:

```
$ make clean
$ git pull
$ git submodule sync
$ git submodule update --init
```

If you don't issue `make clean` (which causes toolchain and SDK to be
rebuilt from scratch on next `make`), you risk getting broken/inconsistent
results.

License
=======

esp-open-sdk is in its nature merely a makefile, and is in public domain.
However, the toolchain this makefile builds consists of many components,
each having its own license. You should study and abide them all.

Quick summary: gcc is under GPL, which means that if you're distributing
a toolchain binary you must be ready to provide complete toolchain sources
on the first request.

Since version 1.1.0, vendor SDK comes under modified MIT license. Newlib,
used as C library comes with variety of BSD-like licenses. libgcc, compiler
support library, comes with a linking exception. All the above means that
for applications compiled with this toolchain, there are no specific
requirements regarding source availability of the application or toolchain.
(In other words, you can use it to build closed-source applications).
(There're however standard attribution requirements - see licences for
details).
