.. _tutorial_visual_studio_2013:
==========================================================
How to create a new opendnp3 project in Visual Studio 2013
==========================================================
 
This is a tutorial on how to create a new visual studio 2013 project using the opendnp3 library. During the tutorial I'll be using Visual Studio 2013, but it should be very similar for older (or even newer) versions of this IDE. 

First of all I'll assume that you have a copy of opendnp3 source files and you have installed the opendnp3 libraries and header files on your computer. If not, please read this (link!) before.
I'll also assume that the following environment variable is set on your system.

.. code-block:: bash
	
	%OPENDNP3_DIR% 
	
Now, let's create our new project. If you already know how to do it, you can skip a few steps. 

First, we go to File - New - Project

.. image:: /img/tutorials/new-project-visual-studio-13/01a.new-project.png
   :align: center

Then we pick a name and a destination folder. Let's choose opendnp3-test as an example.

.. image:: /img/tutorials/new-project-visual-studio-13/01b.name.png
   :align: center
   :width: 600px
   
We hit continue...

.. image:: /img/tutorials/new-project-visual-studio-13/01c.continue.png
   :align: center
   :width: 600px

Then check the options as shown in the next image. Check the "empty project" solution in order to have a clean project to start from.

.. image:: /img/tutorials/new-project-visual-studio-13/01d.settings.png
   :align: center
   :width: 600px

Then we copy the demo files from the opendnp3 source code folder to our newly created folder.

.. image:: /img/tutorials/new-project-visual-studio-13/02.copy.png
   :align: center
   :width: 600px
   
Now, the files are correctly placed but won't appear on the solution. We first go to 

.. image:: /img/tutorials/new-project-visual-studio-13/03.show.png
   :align: center
   
We now need to include all those files to our project. So select all the files, right click and then "Include in project". 

.. image:: /img/tutorials/new-project-visual-studio-13/03.include.png
   :align: center

Congratulations! Your project is *almost* ready to build. 

We just need to tell the compiler and the linker where did we save our opendnp3 library files.

The one-step solution (with Property Sheets)
============================================
Property sheets are a very handy way of re-using configuration files. You can create a property file with all the necessary compiler and linker options and share it among different projects. You can read more `here <http://msdn.microsoft.com/en-us/library/669zx6zc.aspx>`_.

First, create a new text file and rename it to opendnp3_properties.props. 
Now, open the file with an ordinary text editor (like notepad++) and add the following lines:

.. code-block:: xml

   <?xml version="1.0" encoding="utf-8"?>
   <Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
     <ImportGroup Label="PropertySheets" />
     <PropertyGroup Label="UserMacros" />
     <PropertyGroup />
     <ItemDefinitionGroup>
       <ClCompile>
         <AdditionalIncludeDirectories>$(ASIO_HOME);$(OPENDNP3_DIR)\opendnp3-include;%(AdditionalIncludeDirectories)</AdditionalIncludeDirectories>
         <PreprocessorDefinitions>ASIO_STANDALONE;ASIO_HAS_STD_SYSTEM_ERROR;_WIN32_WINNT=0x0501;%(PreprocessorDefinitions)</PreprocessorDefinitions>
       </ClCompile>
       <Link>
         <AdditionalDependencies>asiodnp3.lib;asiopal.lib;opendnp3.lib;openpal.lib;%(AdditionalDependencies)</AdditionalDependencies>
         <AdditionalLibraryDirectories>$(OPENDNP3_DIR)\opendnp3-lib</AdditionalLibraryDirectories>
       </Link>
     </ItemDefinitionGroup>
     <ItemGroup />
   </Project>

Now go back to the project and go to View > Property Manager and click on the "Add Existing Property Sheet" 

   
.. image:: /img/tutorials/new-project-visual-studio-13/07a.prop_sheets.png
   :align: center

Select the recently created file and accept. If you want to have a better insight on what the .xml file is doing, you can continue reading.
   
The classical solution (without Property sheets)
================================================


Configuring the compiler
------------------------
Right click on the project name, then Properties.
On the left side, go to Configuration Properties > C/C++ > General
and edit the "Additional Include Directories" property with these values:

.. code-block:: bash

   $(ASIO_HOME)
   $(OPENDNP3_DIR)\opendnp3-include
   
   
.. image:: /img/tutorials/new-project-visual-studio-13/04a.compiler.png
   :align: center
   :width: 600px
   
.. image:: /img/tutorials/new-project-visual-studio-13/04b.compiler.png
   :align: center  

It is very important to add a preprocessor instruction to tell the compiler to use the header-only version of ASIO. In fact, if we don't declare this options, the compiler will try to use the built version of boost library and it will fail if it doesn't find it in our system.
To modify this property we go to the "Preprocessor" option under the C/C++ section.
Add the following values to the "Preprocessor Definitions" variable:

.. code-block:: bash
   
   ASIO_STANDALONE
   ASIO_HAS_STD_SYSTEM_ERROR
   

.. image:: /img/tutorials/new-project-visual-studio-13/06a.preprocessor.png
   :align: center
   :width: 600px
   
.. image:: /img/tutorials/new-project-visual-studio-13/06b.preprocessor.png
   :align: center  
   
Configuring the linker
----------------------

Right click on the project name, then Properties.
On the left side, go to ``Configuration Properties > Linker > General``
and edit the ``Additional Include Directories`` property with these values: 

.. code-block:: bash

   $(OPENDNP3_DIR)\opendnp3-lib  
   
.. image:: /img/tutorials/new-project-visual-studio-13/05a.linker.png
   :align: center
   :width: 600px
   
.. image:: /img/tutorials/new-project-visual-studio-13/05b.linker.png
   :align: center
   

Now edit input > Additional dependencies  as follows:

.. code-block:: bash

   asiodnp3.lib
   asiopal.lib
   opendnp3.lib
   openpal.lib
  
.. image:: /img/tutorials/new-project-visual-studio-13/05d.linker.png
   :align: center
   
.. image:: /img/tutorials/new-project-visual-studio-13/05c.linker.png
   :align: center