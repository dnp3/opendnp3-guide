Copyright (c) 2014 Automatak LLC


## Notice

This is a work in progress for the upcoming 2.0.x  release. Don't mistake this as complete documentation.


## About

This is the official documentation source to the [opendnp3 project](www.automatak.com/opendnp3).

It uses the [Sphinx](http://sphinx-doc.org/) documentation platform.

## Setup and Building

To build the documentation you'll need a working version of Sphinx. Follow the installation direction [here](http://sphinx-doc.org/latest/install.html) for your platform.

It also uses the [graphviz](http://www.graphviz.org/) Sphinx extension. The "dot" program needs to be on your PATH so Sphinx can run it. 
On Windows, the graphviz bin directory must be manually added to the PATH.

With Sphinx and graphviz installed:
		
```
make html
```
		
Documentation will be written to the /build folder.


### Building the pdf file

Additionally to sphinx and graphviz, you'll need to install rst2pdf.

#### Installing rst2pdf on windows
The basic installation guid was [here] (http://khuntronak.blogspot.in/2013/04/how-to-install-rst2pdf-tool-on-windows.html)

* Download rst2pdf source from [Google Code] (https://code.google.com/p/rst2pdf/downloads/list)

* Unzip the source and copy this folder into C:\ location.

* Goto rst2pdf source directory which contains setup.py file.

* While running setup.py for package installations, Python 2.7 searches for an installed Visual Studio 2008. You can trick Python to use a newer Visual Studio by setting the correct path in VS90COMNTOOLS environment variable before calling setup.py.
	- If you have Visual Studio 2010 installed, execute
		`SET VS90COMNTOOLS=%VS100COMNTOOLS%`
		
	- or with Visual Studio 2012 installed (Visual Studio Version 11)
		`SET VS90COMNTOOLS=%VS110COMNTOOLS%`
		
	- or with Visual Studio 2013 installed (Visual Studio Version 12)
		`SET VS90COMNTOOLS=%VS120COMNTOOLS%`
		
* Run python setup.py install command and it will be installed. 

* Install the fonts located in the `/fonts` folder. If you don't install them, you will get some errors at the make stage. rst2pdf will output a message telling that the not found fonts were replaced. I recommend installing the fonts to get a consistent output between the different operative systems. Beware that the fonts found on  the [DejaVu website] (http://dejavu-fonts.org/wiki/Main_Page) do not contain spaces in the name. I personally renamed them (added the missing space) so I suggest using the fonts modified by me.

Then in the command line type

```
make pdf
```

the output will be located in 

```
/build/pdf
```

All the configuration for the pdf output has to be done in the `conf.py` (scroll down to the *Options for PDF* output section.
