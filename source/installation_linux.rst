GraspIt! Installation - Ubuntu Linux
------------------------------------

Qt
~~

You will need the following libraries in order to compile GraspIt!:

-  Qt 4

-  Coin 3

-  SoQt

-  Blas and Lapack

-  QHull

On Ubuntu, you should be able to get all these from the package manager.
It should also possible to compile them all from code, but since that
tends to be system-specific we no longer provide instructions or
guidelines for that.

Use ``sudo apt-get install`` to install the following packages:

-  ``libqt4-dev``

-  ``libqt4-opengl-dev``

-  ``libqt4-sql-psql``

-  ``libcoin60-dev``

-  ``libsoqt4-dev``

-  ``libblas-dev``

-  ``liblapack-dev``

-  ``libqhull-dev``

GraspIt!
~~~~~~~~

Download and unzip the GraspIt! code itself. Set the following
environment variable:

-  ``GRASPIT`` - the directory where you unzipped GraspIt!

Create the GraspIt! Makefile from the Qt project file. Edit
``$GRASPIT/graspit.pro`` to suit your system and installation needs.
Then type ``qmake graspit.pro``.

Build and go. From ``$GRASPIT/`` type ``make``.
