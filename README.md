MuPDF for Google Chrome Native Client
=====================================

This repository is a port of the [MuPDF](http://mupdf.com/) library to Google Chrome Native Client (NaCl).
If you are writing your own NaCl or PNaCl module, or you are porting some other program to a PNaCl module, then you can link against this library.
PNaCl modules are (almost) binaries that can be executed by the Google Chrome web browser inside a sandbox without requiring any extra permissions from the user.
Currently, this project only provides a port of the core muPDF library, i.e., there is no visual front-end.
I am linking against this library from a different project of mine, [k2pdfopt-nacl](https://github.com/robamler/k2pdfopt-nacl/).
See there for compilation instructions.

Please refer to the file named "README" in this directory for copyright and licensing issues and for information about the original code of muPDF.

If you are looking for an easy way to display PDF documents in a web-app, you may want to have a look at Mozilla's [pdf.js](https://mozilla.github.io/pdf.js/).

(c) 2016 -- Robert Bamler
