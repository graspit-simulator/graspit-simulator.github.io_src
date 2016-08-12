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

Install the following dependencies:

.. code::

  sudo apt-get install libqt4-dev
  sudo apt-get install libqt4-opengl-dev
  sudo apt-get install libqt4-sql-psql
  sudo apt-get install libcoin80-dev
  sudo apt-get install libsoqt4-dev
  sudo apt-get install libblas-dev
  sudo apt-get install libqhull-dev


GraspIt!
~~~~~~~~

Download GraspIt!

.. code::

  git clone git@github.com:graspit-simulator/graspit.git
  
Build GraspIt!

.. code::

  cd graspit
  export GRASPIT=$PWD
  mkdir build 
  cd build 
  cmake ..
  make -j5
  
