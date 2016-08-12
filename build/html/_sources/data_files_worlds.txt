.. role:: math(raw)
   :format: html latex
..

Worlds
------

In GraspIt!, robots and bodies populate a simulation world. This
document describes how these elements can be added or deleted from a
world and describes the format of a world file, which stores the current
state of the world.

When GraspIt! begins the world is empty. The user may either load a
previously saved world by choosing File->Open, or
populate the new world. To import an obstacle (a static body) or an
object (a dynamic body), use File->Import Obstacle or
File->Import Object, and then choose the Body file
(see the previous section on bodies). Note that any Body file
(regardless of whether it’s meant for a static or dynamic body) can be
loaded as an obstacle (GraspIt! will just ignore the dynamic
parameters). However, when a body file is imported as an Object,
GraspIt! will automatically instantiate it as a dynamic body. It will
also try to find the dynamic parameters in the body file and, if it can
not find them, assign default values. Be aware that the default values
occasionally have unpredictable results.

To import a robot, use File->Import Robot, open the
correct robot folder, and select the robot configuration (.xml) file.

To delete a body, select it, and then press the ``<DELETE>`` key. To
remove a robot, first select the entire robot (by double-clicking one of
the links when the selection tool is active) and press the ``<DELETE>``
key.

Note: newly imported bodies or robots always appear at the world origin.
You can move existing bodies out of the way before importing a new one.
If you do not, than the newly imported body will overlap with an old
one, and you will have to temporarily toggle collisions in order to move
one of them out of the way.

When the user selects “Save” in the file menu, GraspIt! saves the
current world state in an world file using an XML-compatible format.
This file can contain the following tags:

-  ``<obstacle>`` - a body to be loaded as obstacle. Contains the
   following sub-tags:

   -  ``<filename>`` - a pointer to the file containing the body to be
      loaded as an obstacle. The path is relative to ``$GRASPIT``.

   -  ``<transform>`` - the position and orientation of the obstacle in
      the simulation world. As always, a ``<transform>`` tag can contain
      multiple sub-tags, each specifying a translation, rotation or
      both. Usually, in World files, transforms are specified with a
      single sub-tag, of the type ``<fullTransform>``, which contains a
      complete transform encoded as a Quaternion.

-  ``<graspableBody>`` - a body to be loaded as a Graspable Body. It is
   identical to the Obstacle tag. The only difference is that GraspIt!
   will load the specified Body as a GraspableBody, initialize its
   dynamic properties, and make it part of the dynamic computations.

-  ``<robot>`` - a Robot to be loaded into this world. Contains the
   following sub-tags:

   -  ``<filename>`` - a pointer to the Robot XML file. The path is
      relative to ``$GRASPIT``.

   -  ``<dofValues>`` - a string containing the saved values of all
      degrees of freedom of the robot. Note that this might mean a
      single number per DOF, or more information, depending on the DOF
      type.

   -  ``<transform>`` - the position and orientation of the Robot in the
      world, as described in the Obstacle case.

-  ``<connection>`` - indicates a connection between two robots. This
   means that one Robot is attached to the end of a kinematic chain of
   another Robot, such as a hand attached to a robotic arm. Contains the
   following sub-tags:

   -  ``<parentRobot>`` - the index of the parent robot in the world,
      which is given by the order in which ``<robot>`` tags appear in
      the World file.

   -  ``<parentChain>`` - the kinematic chain number on the parent robot
      that the other robot is attached to.

   -  ``<childRobot>`` - the index of the child robot in the world,
      which is given by the order in which ``<robot>`` tags appear in
      the World file.

   -  ``<mountFilename>`` (optional) - specifies a body that is
      optionally used as a mount piece between the two robots.

   -  ``<transform>`` the constant offset transform between the last
      link of the parent’s kinematic chain and the base link of the
      child robot.

-  ``<camera>`` (optional) - specifies the world position, orientation
   and focal point of the virtual camera.

For an example, take a look at the ``barrettGlassDyn.xml`` file supplied
with this GraspIt! distribution.

Data files from previous versions of GraspIt!
---------------------------------------------

Starting with version 2.1, GraspIt! loads all of its data (Bodies,
Robots and Worlds) from a new, XML-compatible format. The new version is
not backwards-compatible, meaning that the old data files will no longer
work.

To help with the transition we have included a stand-alone converter
that will convert your old data files to the new format. This converter
is included in ``$GRASPIT/xmlconverter``.

You will need to build the converter separately, starting from the Qt
project file ``xmlconverter.pro``. Here are the steps:

-  Windows

   -  run ``qmake -t vcapp xmlconverter.pro``

   -  load ``xmlconverter.vcproj`` in MS Visual Studio and build.

-  Linux

   -  run ``qmake xmlconverter.pro``

   -  ``make``

Once you have the executable, you can use it to convert any of the old
GraspIt! data files. Just type ``xmlconverter [filename]``. It will
automatically figure out the type of the file being converted based on
its extension. You can convert the following files:

-  Body files (``.iv`` or ``.wrl``). It will create the XML file that
   needs to go with the geometry file, and leave the old geometry file
   unchanged.

-  Robot files (``.cfg``). It will create your new XML robot
   configuration file. It will also attempt to find all the link body
   files that the Robot file references and convert them as well.

-  World files (``.wld``). It will create your new XML World files, but
   it will not attempt to convert any of the Body or Robot files that
   are references therein. We recommend leaving World files for last,
   after you have converted your Body and Robot files.
