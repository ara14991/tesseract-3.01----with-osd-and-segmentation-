tesseract-3.01----with-osd-and-segmentation-
============================================
=================================================
 Building Tesseract-3.01 with Visual Studio 2008 
=================================================

November 6, 2011

This readme discusses the contents of tesseract-vs2008-3.01.zip which
contains files that let you build tesseract-3.01 using Visual Studio
2008.


Installation
============

First create an empty directory where you will unpack all the required
downloads. Assume you call this directory C:\BuildFolder.

1. Download the Leptonica 1.68 pre-built binary package
   (leptonica-1.68-win32-lib-include-dirs.zip) from:

      http://code.google.com/p/leptonica/downloads/detail?name=leptonica-1.68-win32-lib-include-dirs.zip

   and unpack it to C:\BuildFolder.

2. Download the tesseract 3.01 Visual Studio 2008 source files
   (tesseract-vs2008-3.01.zip) from:

      <<<PUT ACTUAL DOWNLOAD URL HERE>>>

   and unpack it to C:\BuildFolder

You should now have the following directory structure:

   C:\BuildFolder

     include\
        leptonica\
        tesseract\

        leptonica_versionnumbers.vsprops
        tesseract_versionnumbers.vsprops
        stdint.h
        
     lib\

     tesseract-3.01\
        vs2008\
           cntraining\
           combine_tessdata\
           libtesseract\
              libtesseract.vcproj
           mftraining\
           port\
           tesseract\
              tesseract.vcproj
           unicharset_extractor\
           wordlist2dawg\

           tesseract.sln

3. Download the tesseract 3,01 source files (tesseract-3.01.tar.gz)
   from:

      http://code.google.com/p/tesseract-ocr/downloads/detail?name=tesseract-3.01.tar.gz

   and unpack it to C:\BuildFolder.

This will add a bunch of directories to your already existing
C:\BuildFolder\tesseract-3.01 directory. You should now have the
following directory structure:

   C:\BuildFolder

     include\
        leptonica\
     lib\
     tesseract-3.01\
        api\
        ccmain\
        ccstruct\
        ccutil\
        classify\
        config\
        contrib\
        cube\
        cutil\
        dict\
        doc\
        image\
        java\
        image\
        neural_networks\
        tessdata\
        testing\
        textord\
        training\
        viewer\
        vs2008\
        wordrec\


Building
========

1. Open C:\BuildFolder\tesseract-3.01\vs2008\tesseract.sln in Visual
   Studio 2008.

You'll see the following projects in the Solution Explorer:

   cntraining
   combine_tessdata
   libtesseract301
   mftraining
   tesseract
   unicharset_extractor
   wordlist2dawg

2. Select the build configuration you'd like to use from the "Solution
   Configurations" dropdown. It lists the following configurations:

      DLL_Debug
      DLL_Release
      LIB_Debug
      LIB_Release

The DLL_ configurations build the DLL version of libtesseract-3.01 (and
link with the DLL version of Leptonica 1.68). The LIB_ configurations
build the static library version of libtesseract-3.01 (and link with the
static version of Leptonica 1.68 and the required image libraries).

3. Build libtesseract by right-clicking the libtesseract301 project and
   choosing "Build" from the pop-up menu.

The resultant library will be written to the
C:\BuildFolder\tesseract-3.01\vs2008\ConfigurationName directory where
ConfigurationName is the same as the build configuration you selected
earlier. It is also copied to the C:\BuildFolder\lib folder to make it
easy to link your own applications to libtesseract.

The library is named as follows:

   static libraries:

      libtesseract301-static.lib
      libtesseract301-static-debug.lib

   DLLs:

      libtesseract301.lib  (import library)
      libtesseract301.dll
      libtesseract301d.lib (import library)
      libtesseract301d.dll


4. Build the main tesseract ocr application by right-clicking the
   tesseract project. 

The resultant executable will be written to the
C:\BuildFolder\tesseract-3.01\vs2008\ConfigurationName directory where
ConfigurationName is the same as the build configuration you selected
earlier. It is named as follows:

  LIB_Release: tesseract.exe
  LIB_Debug:   tesseractd.exe
  DLL_Release: tesseract-dll.exe
  DLL_Debug:   tesseract-dlld.exe
   

Testing
=======

It's usually better to make a separate directory to test tesseract. To
run tesseract, you either need to make sure your test directory contains
the tessdata tesseract language data folder or you set the
TESSDATA_PREFIX environment variable to point to it. See
http://code.google.com/p/tesseract-ocr/wiki/ReadMe for important
details.

For example, you can use the following directory structure:

   C:\BuildFolder
     include\
     lib\
     tesseract-3.01\
     testing\
        tessdata\

Copy your executable to C:\BuildFolder\testing. If you built a DLL
version then be sure to also copy the required DLLs to the same
directory (or add C:\BuildFolder to your PATH -- However, this isn't
really recommended).

For example, if you are trying to run tesseractd.exe then you'll need to
also copy the following to C:\BuildFolder\testing:

   liblept168d.dll
   libtesseract301d.dll

Copy a few test images to C:\BuildFolder\testing just to make it easy to
run test commands.

Test tesseract by doing something like the following:

   tesseractd.exe eurotext.tif eurotext

This will create a file called eurotext.txt that will contain the result
of ocring eurotext.tif.

(BTW, instead of using Windows' plain old Command Prompt window I highly
recommend using the free "TCC/LE - Windows CMD Replacement Command
Console" by jpsoftware which is available at
http://jpsoft.com/tccle_cmd_replacement.html).


Building the Training Applications
==================================

The training related applications are built using the following
projects:

   cntraining
   combine_tessdata
   mftraining
   unicharset_extractor
   wordlist2dawg

Note that currently these applications can ONLY be built with the
LIB_Debug and LIB_Release configurations. If you try to use a DLL
configuration you'll get "undefined external symbol" errors.

See http://code.google.com/p/tesseract-ocr/wiki/TrainingTesseract3 for
more information on using these applications.


Programming with libtesseract
=============================

<<<NEEDS WORK>>>

To use libtesseract in your own application you need to include
baseapi.h and resultiterator.h. There doesn't seem to be any
documentation on baseapi.h so you currently need to use
api\tesseractmain.cpp as an example.

Add the following Preprocessor definitions when compiling any files that
include baseapi.h:

   __MSW32__;USE_STD_NAMESPACE;TESSDLL_IMPORTS;CCUTIL_IMPORTS;LIBLEPT_IMPORTS

Be sure to add the following to your include path:

   C:\BuildFolder\include
   C:\BuildFolder\include\leptonica
   C:\BuildFolder\include\tesseract (and all its sub-directories)

Add C:\BuildFolder\lib to your "Additional Library Directories".

In the C:\BuildFolder\include directory are two Visual Studio Property
Sheet files:

   tesseract_versionnumbers.vsprops
   leptonica_versionnumbers.vsprops

Using tesseract_versionnumbers.vsprops (which automatically inherits
leptonica_versionnumbers.vsprops) can make it easier to specify the
libraries you need to import. For example, when creating a staticly
linked executable you can say:

   zlib$(ZLIB_VERSION)-static-mtdll-debug.lib
   libpng$(LIBPNG_VERSION)-static-mtdll-debug.lib
   libjpeg$(LIBJPEG_VERSION)-static-mtdll-debug.lib
   giflib$(GIFLIB_VERSION)-static-mtdll-debug.lib
   libtiff$(LIBTIFF_VERSION)-static-mtdll-debug.lib
   liblept$(LIBLEPT_VERSION)-static-mtdll-debug.lib

to make your application less dependent on library version numbers.

See the tesseract project for a good example of which compiler and
linker settings you need for various build configurations. However, note
that VS2008 automatically adds libraries built from Project Dependencies
to the linker input. In your case you'll have to explicitly add
something like libtesseract$(LIBTESS_VERSION)-static-debug.lib.

<<<Need to add the URL of the zip file that contains include & lib
directory contents for those people who don't want to build libtesseract
themselves>>>

<<<Also need to supply a Python script that copies all header files
mentioned in libtesseract.vcproj to a specified directory (normally
C:\BuildFolder\include\tesseract) while preserving the original
tesseract-3.01 directory structure.>>>


Debugging libtesseract
----------------------

Before debugging programs written with libtesseract, you should first
download the Leptonica sources (leptonica-1.68.tar.gz) and VS2008 source
package (vs2008-1.68.zip) from:

   http://code.google.com/p/leptonica/downloads/detail?name=leptonica-1.68.tar.gz
   http://code.google.com/p/leptonica/downloads/detail?name=vs2008-1.68.zip

Unpack them to C:\BuildFolder to get the following directory structure:

   C:\BuildFolder
     include\
     lib\
     leptonica-1.68\
        vs2008\
     tesseract-3.01\
        vs2008\
     testing\
        tessdata\

(see
http://tpgit.github.com/UnOfficialLeptDocs/vs2008/building-liblept.html
for more information)

Tesseract uses Leptonica "under the hood" for all (most?) of its image
processing operations. Having the source available (and compiling it in
debug mode) will make it easier to see what's really going on.

You might want to add
C:\BuildFolder\leptonica-1.68\vs2008\leptonica.vcproj and
C:\BuildFolder\tesseract-3.01\vs2008\libtesseract\libtesseract.vcproj as
"Existing Projects" to your solution. This sometimes seem to make VS2008
better able to find source files while debugging.


Visual Studio 2010
==================

There currently isn't an analogous VS2010 source package. However, you
should be able to adapt the instructions at
http://tpgit.github.com/UnOfficialLeptDocs/vs2008/vs2010-notes.html to
convert the VS2008 solution.

..         
   Local Variables:
   coding: utf-8
   mode: rst
   indent-tabs-mode: nil
   sentence-end-double-space: t
   fill-column: 72
   mode: auto-fill
   standard-indent: 3
   tab-stop-list: (3 6 9 12 15 18 21 24 27 30 33 36 39 42 45 48 51 54 57 60)
   End:
