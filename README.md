Copyright (c) 2010, 2011 Green Energy Corp.
Copyright (c) 2013 Automatak LLC

Licensed under the terms of the Apache Public License v2.0.


Definitive Guide to opendnp3
=====================

NOTICE
------
This is a work in progress for the upcoming 2.0.x  release. Don't mistake this as complete documentation.


About
-----
This the first port of the wiki files present in the Automatak DNP3 project to the Sphinx 
documentation platform.

All the content was taken from this project https://github.com/automatak/dnp3/wiki and adapted to 
fit into the structure of the Sphinx software. 

For more information on the Sphinx project please visit: 

http://sphinx-doc.org/


To clone the repository type: 

      git clone https://github.com/pgiu/AutomatakHelp-2.0.git


To build the documentation you'll need: 

- a working version of Sphinx. You can grab a copy from :  http://sphinx-doc.org/latest/install.html and follow the installation instructions. 
	
- after installing Sphinx just type:
		
		make html
		
with that instruction you'll see files in the /build folder updated.


Important notes on how to port from github wiki to Sphinx:
----------------------------------------------------------

Code is no highlighted using the same syntax. The syntax for code highlighting in Sphinx is something like this: 

´´´
.. code-block:: bash

   $ cd java
   $ mvn test
´´´

Please keep on mind that you should leave a blank line between the 'code-block' line and the beginning of the code itself.
It is also important to keep a blank space between 'code-block::' and the name of the language (bash, in the example)

On the left panel of every sphinx page, there is a link that says: 'show source'. If you like some feature of the page you're seeing you can just copy that line of code ! 

		
