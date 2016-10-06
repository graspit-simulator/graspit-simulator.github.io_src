GraspIt! Installation - Ubuntu Linux with ROS
---------------------------------------------

GraspIt! can be integrated into a ros workspace via the graspit-ros package:

https://github.com/graspit-simulator/graspit-ros

.. code::

  cd ros_ws/src
  git clone https://github.com/graspit-simulator/graspit-ros --recursive
  cd ..
  catkin_make

This packages pulls in GraspIt! as a git-submodule.  You can then 
write your own code which will be both its own ros package and a GraspIt! plugin to allow interaction with GraspIt!. For examples of how to write a plugin look at the GraspIt! Plugins page from this user manual. 

For an example of a plugin exposing GraspIt! functionality via ROS services and action servers:

https://github.com/curg/graspit_interface
