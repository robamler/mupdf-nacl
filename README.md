MuPDF for Google Chrome Native Client
=====================================

This repository is a fork of [MuPDF](http://mupdf.com/) that adds compilation rules to cross-compile with the Google Chrome Native Client toolchain.
Compiling this code creates a static library "libmupdf.a" that can be used in Native Client modules.

MuPDF is free software: you can redistribute it and/or modify it under the terms of the [GNU Affero General Public License](http://www.gnu.org/licenses/agpl-3.0.html) as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

The original source code for MuPDF is Copyright 2006-2015 Artifex Software, Inc.
The owner of this repository is not affiliated with Artifex Software, Inc. or with Google, Inc.


Prerequisites
-------------

You need the following tools to compile the MuPDF library with the Native Client toolchain:

1. Some standard build tools; on Debian/Ubuntu:<br>
   ```
   sudo apt-get install autoconf automake cmake gettext libtool pkg-config xsltproc
   ```

2. The [Google Chrome browser](https://www.google.de/chrome/browser/desktop/) (if you want to execute any Native Client modules).

3. The Native Client SDK:
   [Download](https://developer.chrome.com/native-client/sdk/download) the zip file and extract it to (e.g.) `~/nacl_sdk`.
   Then find out your version of the Chrome browser and check out the corresponding version of the NaCl SDK.
   E.g., if you have Chrome version 44, then run
   ```
   cd ~/nacl_sdk
   ./naclsdk update pepper_44
   export NACL_SDK_ROOT=`pwd`/pepper_44
   ```
   You may want to set `$NACL_SDK_ROOT` in your `~/.bashrc`.

4. Install `gclient` as described [here](http://dev.chromium.org/developers/how-tos/install-depot-tools):
   ```
   cd ~
   git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
   export PATH=`pwd`/depot_tools:"$PATH"
   ```
   Again, you may want to add a line to your your `~/.bashrc` that sets `$PATH` permanently.

5. The [naclports](https://code.google.com/p/naclports/) project on Google Code facilitates the compilation of some commonly used libraries for Native Client. Install it as follows:
   ```
   mkdir ~/naclports
   cd ~/naclports
   gclient config --name=src https://chromium.googlesource.com/external/naclports.git
   gclient sync
   cd src
   git checkout -b pepper_44 origin/pepper_44  # should match your version of the NaCl SDK
   gclient sync # ignore error message, don't use --force
   ```
   The last command may produce an error message asking you to use the `--force` command line option.
   *Don't* do that and just ignore the error message.
   To make sure that you're on the right branch, try `git branch`.
   It should indicate "pepper_XX" as your current branch (where XX corresponds to the version you chose).

6. Check out the code for this repository and its subrepositories:
   ```
   cd ~
   git clone https://github.com/robamler/mupdf-nacl.git
   cd mupdf-nacl
   git submodule update --init --recursive
   ```


Compilation
-----------

1. Go to the "naclports" directory you created in step 5 under "Prerequisites" above and compile some required libraries:
   ```
   cd ~/naclports/src
   NACL_ARCH=pnacl make zlib libpng freetype jpeg8d bzip2
   ```

2. Generate files that are needed for the build process:
   ```
   cd ~/mupdf-nacl
   make generate
   ```

3. Cross-compile the library:
   ```
   make OS=pnacl-cross build=release install-nacl-libs
   ```
   The installation copies the libraries "libmupdf.a", "libmujs.a", "libjbig2dec.a", and "libopenjpeg.a" into your `$NACL_SDK_ROOT`.
   The third-party libraries "libjpeg.a", "libz.a", and "libfreetype.a" are not installed, since better-tested versions of these are already provided by naclports.


Usage
-----

To use the MuPDF library in your Native Client module, link with `-lmupdf -ljbig2dec -lfreetype -lopenjpeg -lpng -ljpeg -lz -lbz2`.
For an example Native Client module that uses MuPDF, see [k2pdfopt-nacl](https://github.com/robamler/k2pdfopt-nacl).
