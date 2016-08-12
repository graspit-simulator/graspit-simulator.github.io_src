GraspIt! Installation - Ubuntu Linux with ROS
---------------------------------------------

GraspIt! is also available through a ROS stack, allowing you to either
the GraspIt! binaries or compile the source code through the ROS
installation mechanism. The ROS stack also provides examples of how to
use ROS and GraspIt! together. For more details, please see the relevant
stack documentation on the ROS wiki:

In order to use newer GraspIt! versions use the graspit-ros package:

https://github.com/graspit-simulator/graspit-ros

.. code::

  cd ros_ws/src
  git clone https://github.com/graspit-simulator/graspit-ros
  cd ..
  catkin_make

This packages pulls in graspit as a git submodule.  You can then 
write your own ros package to interact with graspIt!. 

For example:

https://github.com/curg/graspit_interface
