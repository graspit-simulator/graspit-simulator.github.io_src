.. role:: math(raw)
   :format: html latex
..

The Dynamics Engine
===================

GraspIt! has two main modes of operation. In the first one, which has
been assumed in all of the previous chapters, the user directly
interacts with the simulation world by moving objects around or moving
robot joint draggers. The collision detection system disallows any
movement that would bring two bodies in collision, and stops any such
movement at the point of contact. We refer to this operation mode as
“static operation”.

In real life however, objects move around as a result of contact and
external forces. In GraspIt! these phenomena are taken into account when
using the dynamics engine. This engine computes the contact forces that
prevent interpenetration and the velocities and accelerations of all the
bodies in response to contact forces, joint constraints and external
forces. The resulting motion of the bodies is integrated over time in a
time-stepping procedure resulting in a full simulation of a grasping
process (as opposed to just assessing the quality of a final grasping
posture, as we do in static operation).

User interaction in dynamics mode
---------------------------------

The key difference between static and dynamic operation is in the way
the user interacts with the simulation world. In statics, the user
directly moves bodies around, and sets robot joint values. None of these
are allowed in statics. The only way the user can affect the simulation
world is by setting desired values for the robot DOF’s. Built-in
controllers for the robot DOF’s take care of the rest, simulating true
dynamic operation. DOF controllers generate motor forces; as a result of
these robot joints move and potentially touch other objects. Bodies are
also affected by gravity, and will fall off the world if they are not
supported by an obstacle (such as a floor or table). Obstacles are only
part of the dynamics engine in the sense that they provide contacts for
other bodies. They never move in dynamics.

Use the ``Dynamics start`` button in the toolbar to start or pause the
dynamics engine. While the engine is running, you should see the
dynamics timer advancing. During this time, you will see bodies move as
a result of gravity, or DOF motor forces if the desired DOF values have
been set. For a quick demo, see the Getting started chapter of this
manual.

Currently, the only way for a user to specify a desired position for a
robot in dynamics mode is to use the AutoGrasp feature. Use the Grasp->Auto Grasp menu just as you would in statics. When
dynamics are on, this will set the robot desired DOF values at either
the DOF max or min, depending on the default velocity specified in the
robot configuration file. In the future, we intend to provide an
interface for a user to set desired robot DOF values during dynamics
execution.

DOF controllers
---------------

The simulated DOF controllers have proven to be very difficult to
calibrate and tune. Currently, we are implementing PD controllers and
attempt to generate trajectories with smooth acceleration and velocity
profiles from current DOF values to desired DOF values. See the
``DOF::PDPositionController(...)`` function for details. These
controllers are **very** sensitive to the proportional and derivative
gains used, and we have no better method of setting these than empirical
tuning. Even like this, for complex dynamic simulations, we often see
erratic behavior. We are hoping to improve the DOF control mechanism in
the future.

Implementation
--------------

For all the details on the dynamics engine implementation, see the
chapter. Here we provide just a quick overview.

The dynamics engine works in time steps. At each time step, two main
tasks are performed:

-  move all dynamic bodies according to the velocities computed at the
   previous time step. If any collision occurs, interpolate back in time
   until the exact moment of contact. Mark all the contacts in the
   world.

-  formulate solve a Linear Complementarity Problem (LCP) that gives us:

   -  the contact forces that prevent interpenetrations

   -  the joint forces that keep the joints in place

   -  the velocities of all the bodies in the world in response to the
      forces above, as well as external forces (motors, gravity, etc.)

-  see the dynamics-related functions in the ``World`` class for more
   details.

The core of the dynamics engine, and also its most important
computational effort, is the LCP solver. We have implemented a version
of Lemke’s algorithm for this, found in the ``dynamics.cpp`` file.

Under development: dynamics improvements
----------------------------------------

Unfortunately, the dynamics engine exhibits occasional instability,
which we would like to address in the future. These might be caused by
the LCP solver, the time step integration, or both. It has been
suggested by experts in the field that the LCP solver is the primary
suspect. We are hoping at some point to replace the current solver with
a better one, based on newer methods proposed in the literature.

We would also like to cut down on computation time by providing the LCP
with a “warm start” using the solution from the previous time step. This
is made more difficult by the fact that contacts are always broken and
then re-computed between time steps, making it harder to keep track of
previous time step solutions. We have put in place a mechanism for
“inheriting” contact parameters between time steps, you will find this
in the ``Contact`` class. However, with the current LCP solver, we have
not been able to obtain an improvement using this information. Contact
“inheritance” though has been left in place (although disabled for now)
in the hope that future LCP solvers might be able to take advantage of
it.


Bullet Dynamics
----------------------------------------
In order to use Bullet 2.83.7 with GraspIt! do the following:

1) Install Bullet

.. code::
   git clone git@github.com:bulletphysics/bullet3.git
   cd bullet3 
   git checkout 2.83.7
   mkdir build
   cd build
   ccmake ..

Set BUILD_SHARED_LIBS to ON

.. code::
   cmake ..
   make -j5
   sudo make install


2) Configure GraspIt!

.. code::
   cd graspit/build
   ccmake ..

Set DYNAMICS_ENGINE to BULLET_DYNAMICS
3) Build GraspIt!

.. code::
   cd graspit/build
   cmake ..
   make -j5


4) Run GraspIt!

