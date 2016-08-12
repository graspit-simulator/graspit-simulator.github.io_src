.. role:: math(raw)
   :format: html latex
..

The Columbia Grasp Database - Part I
====================================

Overview
--------

The Columbia Grasp Database (CGDB) is a repository of more than 250,000
grasps over a set of approximately 8,000 3D models. The interface
discussed here allows you to load these models into GraspIt, load and
inspect the grasps for these objects stored in the database and use them
for data-drive, machine-learning based algorithms inside GraspIt.

This chapter is dedicated to the interface between the CGDB and GraspIt.
It will show you how to install and use this interface. The next
chapter, , presents the database itself in much more detail, discusses
some of its design choices, shows you how to extend it, etc. Finally,
see the section for a complete discussion of theoretical aspects and
research questions.

Formally, the database contains:

-  the set of objects used is the Princeton Shape Benchmark (PSB). This
   set consists of approx. 1800 3D models, and is separate from GraspIt.
   However, it is freely available for download (see below for
   installation instructions).

-  since grasping is inherently scale-dependant, in the CGDB, each of
   these 1,800 models is used at 4 different scales. Therefore, the CGDB
   can be considered to use approx. 1,800 \* 4 = 7,200 individual 3D
   models.

-  for each of these models, the CGDB contains a number of grasps. A
   grasp is defined as:

   -  a hand position relative to the object

   -  a set of hand joint angles

-  quality metrics for all the grasps. Note that all of the

-  hand-object contact points for all the grasps, for use in machine
   learning grasps from the CGDB have form-closure.

-  grasps have been stored in the database for 3 hand models:

   -  the Barrett hand assuming plastic finger covers (low friction
      coefficient)

   -  the Barrett hand assuming rubber finger covers (high friction
      coefficient)

   -  the Human Hand model

-  in total, the CGDB contains more than 250,000 grasps.

The interface between GraspIt and the CGDB will enable you to:

-  load any model from the PSB into GraspIt!, automatically rescaled to
   a size appropriate for grasping.

-  see the other models in the PSB that are “geometrically similar” to
   the loaded model.

-  load the grasps for that model that have been saved in the CGDB,
   using any of the three hand models above.

-  use this information for data-driven grasp planning.

Caveats
-------

An aspect of particular importance concerns the quality of the
information available in the CGDB. Many of the objects in the PSB are
not intuitively graspable. You will find extensive sets of ants and
spiders, battleships, furniture etc. For the purposes of the CGDB, all
objects have been rescaled to what we empirically considered to be
“graspable” size. Informally, this is approximately the size of a toy
model of the object.

Since the majority of the grasps in the database were found using an
**automated planner**, not all of the grasps are truly humanlike or
reliable. There can be cases where a grasp satisfies our quality
metrics, but would require a degree of precision that cannot be obtained
in real-life execution. Aside from the intrinsic limitations of grasp
quality metrics, for which there is as of yet no firm consensus on which
to use, our approach to grasp planning is purely geometric. This
presents problems for objects that do not match our assumptions. For
example, our assumption that all objects are rigid plastic results in
geometrically correct but unrealistic grasps on objects such as flowers
or leaves. Furthermore, the lack of domain-specific knowledge means that
some of our grasps are semantically incorrect, such as a mug grasped by
placing the fingers inside.

Finally, our automatically computed grasps were obtained from pre-grasps
that sample a low-dimensional subspace of the hand DOF space. This is
for the moment a necessary simplification, without which the planning
problem for dexterous hands is intractable at this scale. While our
choice of subspace is theoretically justified and shown to be effective
[3], we cannot reasonably claim that the database covers the entire
space of possible grasps.

Database installation
---------------------

The CGDB comes as a separate download from GraspIt!, at the . It is
available for download as a PostgreSQL database backup file (70M) and
requires a PostgreSQL installation to use. In order to use the interface
presented here, you will need to install a PostgreSQL server on your
machine and load the provided backup file. PostgreSQL is open source,
and easy to install. We recommend getting the binary package.

GraspIt Interface Installation
------------------------------

Windows
~~~~~~~

-  Download PostgreSQL from the and install. We recommend installing the
   binary package.

   -  add ``$PSQLDIR/bin`` to your ``PATH``, where ``$PSQLDIR`` is the
      directory where you installed PostgreSQL (*e.g.*
      ``C:/postgresql-8.3.4``).

   -  WARNING: if Qt can not find the PostgreSQL dll’s it will NOT
      complain about it, but just fail to open any databases. If that
      happens, make sure the path to the dll is in your ``PATH``.

   -  WARNING: some of the dll’s from ``$PSQLDIR/bin`` are sometimes
      also present in other places on your system, such as
      ``Windows/system32`` etc. For some reason, the system will try to
      use the wrong version at run-time and ``libpq`` won’t operate
      well. If this happens, you will get a “driver not loaded” error
      from the GraspIt CGDB interface. The solution is to copy some of
      the dll’s from ``$PSQLDIR/bin`` to the directory that you’re
      running GraspIt from (``$GRASPIT`` if running from Visual Studio
      or ``$GRASPIT/bin`` if running the executable directly). For
      example, I have had to do this with ``libeay32.dll`` and
      ``ssleay32.dll``.

-  Build the ``qpsql`` according to “How to Build the QPSQL Plugin on
   Windows” in the Qt Assistant:

   -  ``cd $QTDIR/src/plugins/sqldrivers/psql``

   -  ``qmake INCLUDEPATH+=$PSQLDIR/include LIBS+=$PSQLDIR/lib/libpq.lib psql.pro``

   -  ``nmake``

   -  after this, check that the qsqlpsql lib and dll files have been
      built in ``$QTDIR/plugins/sqldrivers``

-  Create your root for the CGDB, such as ``C:/cgdb``. Set the
   environment variable ``CGDB_MODEL_ROOT`` to point to it.

-  download the and unpack it in to ``$CGDB_MODEL_ROOT/psb``.

   -  for example, the model ``m0`` should be in
      ``$CGDB_MODEL_ROOT/psb/benchmark/db/0/m0/m0.off``

-  Enable the CGDB by uncommenting the appropriate line in the
   ``graspit.pro`` file and rebuild GraspIt.

Linux
~~~~~

-  install the SQL module for Qt. Using the Package Manager, install the
   package ``libqt4-sql``.

-  Create your root for the CGDB, such as ``/data/cgdb``. Set the
   environment variable ``CGDB_MODEL_ROOT`` to point to it.

-  download the and unpack it in to ``$CGDB_MODEL_ROOT/psb``.

   -  for example, the model ``m0`` should be in
      ``$CGDB_MODEL_ROOT/psb/benchmark/db/0/m0/m0.off``

-  Enable the CGDB by uncommenting the appropriate line in the
   ``graspit.pro`` file and rebuild GraspIt.

Connecting to and browsing the database
---------------------------------------

Connecting to the CGDB:

-  start GraspIt and load one of the hand models that are used in the
   CGDB (the Human hand or the Barrett hand). You don’t need to
   explicitly load any objects.

-  The Database menu gives you access to all CGDB functionality from
   GraspIt. The first step is to establish a connection to the CGDB,
   using the Database-> Connect and Browse menu. After
   a connection is established, you can use the other functions in the
   Database menu as well. Click Database->Connect and
   Browse to bring up the CGDB-Browser dialog.

-  set the connection parameters based on PostgreSQL server that you are
   connecting to. You must have access to a machine running a PostgreSQL
   server that serves the CGDB, as downloaded above. Most often, the
   machine running the server will be the same that GraspIt is running
   on; in this case, set the Host to ``localhost``. The Port number is
   usually 5432. Set the User Name and Password based on the settings
   that you used when setting up your PostgreSQL server.

-  note that it is also possible to connect to a remote machine running
   the PostgreSQL server. In the future, we might set up a machine in
   the Columbia Robotics lab that will offer a read-only version of the
   database for everybody. For now though, you have to set up your own
   PostgreSQL server.

-  click ``Connect``. If the connection is successful, the ``Browser``
   group will become enabled, the ``Models`` drop-down list will be
   populated with a list of models and the currently selected model
   thumbnail will be shown in the dedicated space.

-  if the connection fails, you will get an error message in the
   console. Usually, this error message will say that the Qt SQL driver
   is not properly loaded. Go back to the section for details.

Browsing the CGDB:

-  select a model from the ``Models`` drop-down list. The model’s
   thumbnail will be shown. Note that the names of the models from the
   PSB are set as follows: psb\_scale\_modelnumber. Each model can be
   selected at one of four different scales (``0.75, 1.0, 1.25 or 1.5``,
   each compared to a reference size which has been determined
   empirically by us).

-  click ``Load Model``. The model will be loaded into GraspIt and set
   as the reference object for grasp quality computations.

-  select the type of grasps that you want to load. There are three
   grasp types stored in the database:

   -  ``EIGENGRASPS`` - this is the actual database, the grasps computed
      by our automatic planner. You will find this type of grasps for
      all the objects in the database. We recommend using only this type
      of grasps when using the CGDB.

   -  ``HUMAN`` - grasps created by a human operator. This are being
      used for a current unfinished project in the Columbia Robotics
      Lab. For the moment, there are very few grasps of this type saved
      in the CGDB, approx 100 grasps over a set of 15 objects. In the
      future, the CGDB might include more grasps of this type, for
      comparison against the automatically generated grasps above.

   -  ``HUMAN_REFINED`` - the same operator-created grasps, but further
      refined inside GraspIt by an operator. Again, very few of these
      are available.

-  click ``Load Grasps`` and use the ``Grasps`` button group to browse
   the loaded grasps. Note that, for each grasp, you can look at either
   the final grasp posture, or the pre-grasp. See the CGDB Publications
   for the difference between these two.

Geometric similarity
--------------------

A very important aspect of the CGDB concerns geometric similarity
between objects. Our group has been implementing existing tools, and
also developing new methods for this area. These tools are not included
with GraspIt! - this means that, if you have a **new 3D model**, this
interface will not be able to find its geometric “neighbors” in the
CGDB. However, we have **precomputed** this information for all the
models that are already part of the CGDB, and **included** this
information in the CGDB. This means that, for any model in the CGDB, you
can see which other models are “geometrically similar” based on our set
of tools.

Geometric similarity is a vibrant research area, far exceeding the scope
of this user manual. For more details, please see the section.

Database-backed grasp planning
------------------------------

Database-backed grasp planning works by finding geometrically similar
“neighbor” objects to the target object in the CGDB. Due to the reasons
explained above, the version of the planner included with this
distribution only works for models that are **already in the CGDB** and
as such have pre-computed neighbor information. However, we are
providing this code in the hope that it will serve as a blueprint for
developing your own CGDB-backed algorithms.

Please note that this is a fairly complex machinery, and all the details
of its execution exceed the scope of this chapter. More information is
provided in the next chapter, which discusses advanced concepts
pertaining to the CGDB. You might need to peruse the code itself, and
its documentation for more details.

To start, use the following steps:

-  establish a connection to the CGDB as discussed above.

-  load a model from the CGDB. This will serve as the target object, the
   one that we will plan grasps on.

-  start the planner using the Database->Database
   Planner menu.

The CGDB Planner goes through a few steps, each with its own dedicated
button group in the Planner dialog.

-  **Neighbor Generator**

   -  select the distance function used for finding geometric neighbors.
      Use ``SIFT_12_view_0`` for our most up-to-date results on shape
      search. For more details, please see the CGDB publications.

   -  choose the number of neighbors you want to use. Usually, a number
      between 3 and 5 does the job, but you can choose to use more of
      fewer neighbors based on your computational resources. Note that
      the more neighbors you use, the worse the “geometric matching”
      will get.

   -  click ``Get Neighbors``. The drop-down list of neighbors will be
      populated and you can browse through it and see the associated
      thumbnails.

-  **Alignment** - Choose a method for aligning neighbors to the
   original model. ``SIFT_PI_ICP_FULL`` gives the best results.

-  **Grasp retrieval and ranking**. Here we retrieve the grasps from the
   neighbors and rank them. Click ``Retrieve Grasps``. Currently, the
   ranking methods we are using are still under development, so choose
   ``No ranking``, then click ``Rank Grasps``. After you have done this,
   the ``Selected Grasps`` group should become enabled, and the counter
   will show you the total number of grasps retrieved from neighbors.

-  **Grasp Browsing** - you can browse through the list of retrieved
   grasps in the ``Selected Grasps`` group. Make sure that
   ``aligned original grasp`` is selected.

-  **Grasp execution** - here we execute the retrieved grasps on the
   target object, and compute their quality. Make sure ``Static`` is
   selected in the grasp execution type drop down box (the other option,
   ``Dynamic`` execution, is still under development). You can test the
   grasps one at a time, by selecting ``Test current``, or all at once,
   by selecting ``Test All``. Note that this option will usually take up
   to a minute of computation, depending on the number of neighbors and
   the number of retrieved grasps.

-  **Solution inspection** - after using the ``Test All`` option, you
   can browse the results of the planner, sorted by quality. Use the
   ``Selected Grasps`` group, but this time select ``tested grasps`` in
   the radio box.

This is just a very high-level overview and walk-through for the
CGDB-backed planner. For more details, please see the next chapter of
this manual, the source code documentation, the Publications chapter, or
contact us.
