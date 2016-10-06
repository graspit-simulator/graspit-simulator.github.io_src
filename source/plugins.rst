GraspIt! Plugins
------------------------------------

When using GraspIt! for a particular task, rather than directly modifing the GraspIt! source code, it is often cleaner to place the code for the particular task in a GraspIt! plugin. Example plugins can be seen in the plugins directory: 

https://github.com/graspit-simulator/graspit/tree/master/plugins

In order to start GraspIt! with a plugin:

.. code::

  export GRASPIT_PLUGIN_DIR=[Path to compiled plugins]
  graspit -p libmyplugin


For example:

.. code::

  cd ~/graspit/plugins/helloworld
  qmake helloWorldPlugin.pro
  make -j5
  export GRASPIT_PLUGIN_DIR=~/graspit/plugins/helloworld/lib
  graspit -p libhelloWorldPlugin
  
GraspIt! should start up and spew "hello world" repeatedly on the terminal.
