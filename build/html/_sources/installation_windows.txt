GraspIt! Installation - Windows
-------------------------------

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
