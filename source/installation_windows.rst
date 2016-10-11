GraspIt! Installation - Windows
-------------------------------

~~~~~~~~~~~~~
Visual Studio
~~~~~~~~~~~~~

We assume you are using the Microsoft Visual Studio compiler. The test
we have done is on a Windows7 32-bit computer.

Qt
~~

Download Qt for C++ from the the .

| GraspIt! is currently tested using Qt 4.7.4. On the Qt website, choose
“Qt Library” section, then “Qt libraries 4.7.4 for Windows (VS 2008, 228
MB)” and wait until it begins to download automatically. If not, you can
click the link on the page to begin download. Currently, the recommended
download link that you will arrive at is
| .
| However, this will change as new versions are added to the website.
Unzip the archive to a directory of your choice.

Set the following environment variables:

-  ``QTDIR`` - the directory where you unzipped Qt (e.g.
   ``C:/Qt/4.7.4``)

-  ``PATH`` - add ``$QTDIR/bin`` to your ``PATH``

WARNING: Other programs use Qt binaries, and often include the Qt dll’s
(such as ``QtGui4.dll``) in their distribution. If a path to some other
version of a Qt dll exists in you ``PATH`` environment variable
**before** ``$QTDIR/bin``, the system will link GraspIt against the
wrong version, often causing failure to run. To be sure that you are
linking against the right dll’s, put ``$QTDIR/bin`` at the beginning of
your ``PATH``.

Coin3D
~~~~~~

Download Coin3d for your C++ compiler from the .

We have tested the installation of GraspIt! using Coin 3.1.3.

Goto build/msvc9/coin3.sln and compile the project.

Set the following environment variables:

-  ``COINDIR`` - the directory where you unzipped Coin (e.g.
   ``C:/Coin3D/3.1.3``).

-  ``PATH`` - add the path to ``coin3.dll`` (e.g.
   ``C:/Coin3D/3.1.3/bin``).

SoQt
~~~~

Download SoQt from the .

We tested SoQt 1.5.0.

You will need to build SoQt yourself from source code. The source code
comes with MS Visual Studio solution files, choose the appropriate one
depending on your compiler. Again, we recommend building both debug and
release versions. After the build completes, take a look in
``$COINDIR/bin`` to make sure that the appropriate SoQt dlls
(``soqt1.dll`` and/or ``soqt1d.dll``) have been built.

Lapack
~~~~~~

You will need to link GraspIt! against your favorite implementation of
Lapack (cLapack or MKL). This usually involves two steps: using a
“wrapper” file so you can call Fortran functions from C code and linking
against the correct library.

We have tested GraspIt! with cLapack from:.

To install it:

-  Goto

-  Follow the instruction under section “Easy Windows Build” to build
   the libraries

-  set the environment variable ``CLAPACKDIR`` to the root of the
   respective Lapack library installation

Unfortunately, it is possible that you might have to change these
settings to get the installation to work on your system, or you might be
using a different version of Lapack altogether. Due to the large number
of possible configurations, we can not provide more in-depth guidelines.
If linking against Lapack fails, please see the Windows-specific
GraspIt! project file (``graspit-lib-WINDOWS.pro``), find the
blas/lapack block, and try to adapt it to your settings.

| You can also try using Intel Math Kernel Library (MKL), available from
|  (free trial available). To switch between MKL and cLapack, just go to
graspit.pro and change the flag.

Dlfcn
~~~~~

Using Graspit! in Windows , we need to get a copy of dlfcn for plugin.
One version is from Google code. You can download it from here: .

Please download the source and unzip it locally. Set the following
environment variable:

-  ``DLFCN`` - the directory where you unzipped dlfcn.

GraspIt!
~~~~~~~~

Download and unzip the GraspIt! code itself. Set the following
environment variable:

-  ``GRASPIT`` - the directory where you unzipped GraspIt! (e.g.
   ``C:/Documents and Settings/your_username/My Documents/Graspit``).

Build QHull. Open and build the MS Visual Studio solution
``$GRASPIT/qhull/windows/qhull.sln``. Once again, we recommend building
both debug and release versions. Make sure ``qhull.lib`` has been
installed in the appropriate directory (``$GRASPIT/qhull/windows/Debug``
and/or ``$GRASPIT/qhull/windows/Release``).

Edit ``$GRASPIT/graspit.pro`` to suit your particular installation.
Among other options, you can choose which version of Lapack to use.
Then, create the GraspIt! MS Visual Studio project file from the Qt
project file. Execute the following command from inside ``$GRASPIT`` in
a command prompt:

``qmake -t vcapp -o graspit.vcproj graspit.pro``

This will create your GraspIt! MS Visual Studio project file. Open
``graspit.vcproj`` in MS Visual Studio. Build GraspIt! and run.

**IMPORTANT**: based on the choice of Debug vs. Release made in the
``graspit.pro`` file, make sure the appropriate configuration (Debug or
Release) is **also** selected in MS Visual Studio. This ensures linking
against the correct Qt libraries.

**WARNING**: it is not enough to switch between Debug and Release builds
using only the option in in MS Visual Studio. This will link against the
appropriate Qt libraries but NOT against the appropriate Coin, SoQt and
QHull libraries! The correct way of choosing a Debug or Release build is
to also edit the ``graspit.pro`` file.



~~~~~~~~~~~~~~
MSys / MingGW
~~~~~~~~~~~~~~

Prerequisite: This guide is for advanced users who already have at least some experience with using cmake and the bash shell (or similar).

Step 1. Install MinGW, MSYS, Qt and CMake
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Download the `MinGW toolchain`_ (from the MinGW-w64 project) which is already set up for Qt and extract it, e.g. to C:/MinGW. See also the `MinGW Qt wiki`_ page.

2. Download **Qt 4** and run the installer (default destination is C:/Qt). We recommend to use `Qt4 download from qt.io`_.
  During installation, you have to locate the MinGW files previously extracted. See also this useful resource: `Installing Qt for windows`_.

3. Add the **MinGW** *bin* directory to your PATH in Windows.

4. Download and install `CMake for Windows`_.

5. Install **MSYS2**
    - Download MSYS2 [from here](https://msys2.github.io/) and execute the installer.
    - Open a MSYS shell and add the MinGW *bin* directory to PATH, eg. ``echo "export PATH=${PATH}:<path-to-minGW-bin>" >> .bashrc``
    - *(optional)*: Install vim for MSYS shell with ``pacman -S vim``

    
.. _MingGW toolchain: http://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/4.8.2/threads-posix/dwarf/i686-4.8.2-release-posix-dwarf-rt_v3-rev3.7z/download
.. _MingGW Qt wiki: https://wiki.qt.io/MinGW.
.. _Qt4 download from qt.io: https://download.qt.io/archive/qt/4.8/4.8.6/
.. _Installing Qt for windows: https://github.com/iat-cener/tonatiuh/wiki/Installing%20Qt%20For%20Windows
.. _CMake for Windows: https://cmake.org/download/

**General notes for compiling with cmake**

CMake and MSYS do not get along that well. When you use cmake from within
the MSYS command line, it will complain about sh.exe being in your PATH.
So in general, the best is to use the cmake graphical interface for Windows
to generate the makefiles, and then use the MSYS shell to compile.


**If** you explicitly want to try and run cmake from within a MSYS shell, you should set it to generate MinGW makefiles:    
``
cd build
cmake -G "MinGW Makefiles" ..
``
      
*Note about the error with sh.exe*    

This error will go away after re-running cmake.
Be careful though, because this may lead to the wrong compilers being chosen, ie. using the MSYS
gcc/g++ instead of the MinGW ones. You could try to explicitly set them with CMAKE_CXX_COMPILER and CMAKE_C_COMPILER.
In this guide however, only instructions for using the Windows CMake GUI are given.

*Compiling*        

For convenience, you may want to create a softlink to *ming32-make.exe* and call it *make.exe*. Alternatively, just run the mingw *make* explicitly:

``mingw32-make``
    

Step 2. Install basic dependencies
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. lapack/blas: Install from source. See also `LAPACK for Windows`_.
    You can use the `original LAPACK source`_.
2. qhull: Compile from source cloning the `qhull repository`_, and install.

.. _LAPACK for Windows: http://icl.cs.utk.edu/lapack-for-windows/lapack/ 
.. _original LAPACK source: http://netlib.org/lapack/lapack.tgz
.. _qhull repository: https://github.com/qhull/qhull

Alternatively, try pacman to install the libraries.     
``pacman -Ss lapack``       
will list the specific name of the package.
Then install with   
``pacman -S <name>``

For qhull you may try the same: find out package name with ``pacman -Ss qhull`` and install with ``-S``

Step 3. Install Coin and SoQt
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A good set of instructions can be found on this github wiki: `SoQT for Windows`_.
Please refer to the instructions given there for installation.

`Binaries for Coin and SoQt`_ exist as well.
However they are not the most recent version any more.

.. _SoQT for Windows: https://github.com/iat-cener/tonatiuh/wiki/Installing-SoQt-For-Windows
.. _Binaries for Coin and SoQt: http://ascend4.org/Building_Coin3d_and_SoQt_on_MinGW.

A few notes about the installation:

*Coin*    
-  Use the extra configure flag  ``--build=x86_64-w64-mingw32``, or if you are not on x86_64, get the output of ``gcc -dumpmachine``.
   Careful though not to use the MSYS build (it has to be the MinGW build).
- As of May 2016, the source code had to be edited as suggested in the instructions linked above, except the change in *freetype.cpp* which was not necessary.
- You also may need to install diffutils: ``pacman -S diffutils``.
- Add the *bin* directory of the resulting Coin install to your PATH environment variable.

*SoQt*    
- The environment variable QTDIR has to be set to the directory where all the Qt includes can be found.

Step 4. Install bullet
~~~~~~~~~~~~~~~~~~~~~~

This is very straight forward with cmake. Clone `the bullet repository`_ and build with cmake.

.. _the bullet repository: https://github.com/bulletphysics/bullet3

###Step 5. Compile graspit

For compiling with CMake, the path to the **MSYS bash.exe** needs to be passed to cmake.
This is required for cmake to be able to call *soqt-config* (which is not provided as Windows .exe file and needs to be run via bash).     
So you will need to pass the path to bash.exe (probably this is ``<your-msys-install>/usr/bin``) with the CMAKE_PREFIX_PATH.

Use the **CMake GUI** to generate the make files, as running cmake from the MSYS shell may be problematic (see also building with cmake explained in Step 1).

1. Start CMake GUI.
2. Select source and build directory, then click "Configure".
3. Select "MinGW Makefiles", and you may first try to stick to the default compilers.   
    *If* it does not compile, or the default compilers causes other issues, try to re-run cmake and this time select "Specify native compilers",
    choosing the fortran.exe/gcc.exe/g++.exe *from your MinGW install* (not MSYS).
4. The first configuration process may be unsuccessful because not all directories are included.
    Either way, you still need to add the following to your CMAKE_PREFIX_PATH:
    - the path to bash.exe
    - the path to the Qt files (QTDIR), and
    - any other dependency directories.      
    You can add the paths by clicking on "New Entry" and add a variable named CMAKE_PREFIX_PATH, in which you add all paths, separated by semicolons.
5. Click "Configure" again. It should be printing something like **"Result of soqt_config: ..." followed by a list of libraries**.
    *If it does not print the libraries, it did not find bash.exe, and the makefiles have not been generated properly!!*
6. After configuration was successful, click on "Generate".

After the cmake files were generated in the GUI,
go to the MSYS shell, change to the build directory, and type

``mingw32-make``

That's all, you should be ready to go, try the **graspit_simulator.exe** in your build directory!
