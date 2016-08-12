.. role:: math(raw)
   :format: html latex
..

Grasp Planning II - Eigengrasp planning
=======================================

This chapter described the grasp planners that run in Eigengrasp space.
Unfortunately, they do not share the same framework as the
Primitive-based planner described earlier, although from a software
engineering viewpoint they probably should, as they are still rooted in
the same try-many-grasps concept.

Most of the ideas behind these planners are explained in a lot of detail
in the . We strongly recommend reading the Eigengrasp related papers
that can be found there before proceeding through this chapter, they
will make most of the concepts shown here much clearer.

Grasp planning in Eigengrasp space
----------------------------------

In this family of planners, the task of grasp planning is seen as a
search over multiple variables, or as an optimization problem for a
high-dimensional function. The variables define the grasp and the
optimized function is the quality of the grasp. In general, in order to
define a grasp we need two sets of variables: the intrinsic variables
(to define hand DOF’s) and the extrinsic variables, to define the
position of the hand relative to the target object.

For dexterous hands, if we consider one variable for each DOF, the space
is too high-dimensional to search efficiently. That is why most of these
algorithms only work well in eigengrasp space, where the intrinsic
variables are the eigengrasp amplitudes. However, there is nothing in
the structure of these planners that prevents them from running in DOF
space. In fact, if you load a hand model that does not have Eigengrasps
defined, GraspIt! will automatically define the “identity” eigengrasp
set, with one eigengrasp corresponding to each DOF. You can then run all
Eigengrasp-based planners exactly as you would if “real” Eigengrasps
were defined.

An Eigengrasp-based planner is composed of three things:

-  a set of variables to search over

-  a quality function to optimize over these variables (usually related
   in some way to grasp quality)

-  an optimization method (usually Simulated Annealing)

To start, load a hand and an object model, then use the Grasp->EigenGrasp Planner menu. The Eigengrasp Planning
dialog window appears. The left side of the window is dedicated to the
variables that are searched. The right hand side is dedicated to the
planning process itself, allowing you to choose from multiple types of
planners, see details about the current planning execution, etc.

Variables
---------

All variables that are being searched are shown in the left panel of the
dialog. Variables serve two main purposes: they define hand position and
hand posture.

-  **hand posture** is defined using Eigengrasp amplitudes. As stated
   above, if the hand you are currently using does not have other
   Eigengrasps defined, the planner will just use the “identity”
   eigengrasp set. All Eigengrasp variables are shown in the dialog with
   the prefix ``EG``.

-  **hand position** can be parameterized in many different ways. The
   drop down list ``Type`` offers a number of different parameterization
   options. When the selection in this drop-down list changes, you will
   see the list of variables change as well. For complete exhaustive
   searches, the default option is ``Axis-angle``. In this case hand
   position is parameterized using 6 variables: 3 for translation and 3
   for rotation. The other ways of specifying hand position are more
   specialized; see code and for more details.

In the GraspIt! code, a state comprising all the variables in search is
contained in the ``SearchState`` class. Please consult the documentation
of this class for many details about these variables, different ways to
encode hand position, etc.

Next to each variable, there is an ``On`` checkbox, which can be used to
enable / disable search over a particular variable. If a variable is
disabled (its box is unchecked), it is no longer part of the search, and
will maintain its current value in all generated solutions. For each
variable, there are also other options marked ``Input``, ``Target`` and
``Confidence``. These options are used for a very specialized planning
operation, using external input. This type of planning is only described
in one of the papers in the Publications section, and not in this
manual. For more information, please contact us. In general, just ignore
these options and leave them in the default configuration.

Search energy
-------------

The search energy is a way of assessing how good a state encountered
during a search really is. It can also be considered a function of the
search variables that needs to be optimized. The most straightforward
search energy is the one that we have already seen used by the
Primitive-based Grasp Planner: use a grasp Quality Measure. However, a
traditional QM is only defined when the hand is actually in contact with
the object. When planning in Eigengrasp space, many hand states will be
very close, but not in perfect contact with the object. One possibility
would be to use the AutoGrasp function and close the hand, and compute a
QM for the resulting grasp. However, that is a slow process, and we
would like to be able to evaluate more hand postures quickly. As a
result, we have tried to define a number of implementations of the
“Search Energy” concept that do not require hand-object contacts.

The default energy function (pre-selected in the ``Energy`` formulation
drop-down list) is ``Hand Contacts``. This formulation attempts to bring
all links of the hand as close as possible to the target object. The
computation of this energy function can be greatly sped-up by
pre-specifying “desired” contact locations on each link (see for
details). This is the role of the checkbox ``Preset contacts``. Such
pre-defined contacts can be loaded from a file. For the Human and
Barrett hands, pre-specified contact files are supplied with the
distribution. If no contact file has already been loaded, when the
``Preset contacts`` checkbox is checked the system will ask the user to
select a file to be loaded. Note that it is also possible to specify a
pre-defined contacts file in a robot configuration file, so that it is
always loaded together with the Robot. From a code standpoint, a
pre-defined contact location is loaded into an instance of the
``VirtualContact`` class; see that class and its documentation for
details.

Other formulations for the search energy are also available, but many of
them also incorporate the ``Hand Contacts`` formulation in one way or
another. For the moment, the code implementing these formulations (the
``SearchEnergy`` class) is some of the ugliest in the GraspIt! codebase.
We hope to change it soon, at which point we will also provide more
documentation. For now, the most tested and trusted energy formulation
is Hand Contacts described above.

Optimization method and planner types
-------------------------------------

The core optimization methods, that is at the base of all Eigengrasp
planners, is Simulated Annealing. The most basic type of planning is to
run a Simulated Annealing search using the Hand Contacts energy
function, over all Eiegengrasp variables plus hand position
parameterized as axis-angle. This is shown in the practical example
below. From this, we have tried quite a number of variations on this
theme.

One in particular might prove useful, so it is also exemplified below:
the MultiThreaded Planner. One of the shortcomings of the Hand Contacts
energy function is that is does not guarantee form-closure: it will find
states where the hand conforms to the shape of the object, but not
necessarily form-closed. To fix this, the MultiThreaded planner runs an
optimization exactly as before; however, whenever it finds a state that
has a good value for the Hand Contacts energy function, it fires off a
child thread which will search that area in more detail, by applying the
``AutoGrasp`` function and computing the exact Quality Measure that
results. The MultiThreaded planner is your best bet if you need to find
form-closed grasps of an arbitrary object with one of the dexterous
hands in GraspIt!.

From a code standpoint, the ``EGPlanner`` pure abstract class is the
base for all of these planners: it contains the functionality for
looping, computing a given hand energy formulation, saving the best
solutions etc. The one thing that it does not know how to do is run an
optimization algorithm. Its direct offspring, the ``SimAnnPlanner``
combines that framework with a Simulated Annealing optimization. To get
started, check out the ``EGPlanner`` class for most details, and the
``SimAnnPlanner`` class for an example implementation of it. All other
planners inherit from ``EGPlanner``. The setup for this code is OK, but
could still take quite a bit of improvement, which we hope to get to at
some point.

Practical example: the Simulated Annealing planner
--------------------------------------------------

-  load the ``HumanHand20DOF``

-  load the ``flask.iv``

-  fire up the EigenGrasp planner dialog (Grasp->EigenGrasp Planner menu)

-  select ``Axis-angle`` as ``Space Search Type``

-  select ``Hand Contacts`` as ``Energy formulation``

-  check the ``Preset contacts`` box. If prompted to select a contact
   file, select ``all_18_contacts.vgr`` ( should be found in
   ``$GRASPIT/models/robots/humanHand/virtual`` )

-  select ``Sim. Ann.`` as ``Planner Type``

-  set 70000 as ``Max Steps``

-  click ``Init``

-  click ``>``

-  allow the planner to run until finished.

-  use the ``Show Results`` buttons ( ``< Best >`` ) to see the results.

Practical example: the Multithreaded planner
--------------------------------------------

Not yet written... Coming soon...
