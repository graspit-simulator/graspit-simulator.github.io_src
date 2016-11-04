GraspIt! Installation - Ubuntu Linux
------------------------------------

Dependencies
~~~~~~~~~~~~

Install the following dependencies:

.. code::

  sudo apt-get install libqt4-dev
  sudo apt-get install libqt4-opengl-dev
  sudo apt-get install libqt4-sql-psql
  sudo apt-get install libcoin80-dev
  sudo apt-get install libsoqt4-dev
  sudo apt-get install libblas-dev
  sudo apt-get install liblapack-dev 
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
  
