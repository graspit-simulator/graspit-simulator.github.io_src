The Columbia Grasp Database: Part II
====================================

Overview
--------

This section contains material describing the CGDB’s internal structure.
It is intended for those who wish to modify the CGDB or use it in more
advanced ways than the basic database browser described in . For the
remainder of this chapter, when we say the “CGDB” we are referring to
the PostgreSQL database, and not to the accompanying geometry files that
the database references.

The CGDB is not only of data, but also a database in the strong sense; a
structured system for encapsulating relationships between various types
of geometric, semantic, and robotic data. The preloaded data consists of
grasps computed by our Eigengrasps Planner on the models from the
Princeton Shape Benchmark with a small number of hands, but the
structure can be expanded with new data in any dimension. Using the CGDB
to the fullest extent requires a substantial knowledge of SQL, both to
add new information to the system and to calculate statistics and
properties of the grasps.

The CGDB follows the following naming conventions:

-  every column name is prefixed with the name of its table

-  except for fields referencing the primary key of another table

Some tables contain two unique columns. Columns that end with the suffix
“\_id” are numeric automatically generated identifiers and are used as
primary keys. Columns that end with the suffix “\_name” are
human-readable identifiers intended to simplify interaction with the
database from code and from the *GraspIt!* browser.

3D Models
---------

We first describe how 3D model information is stored in the database.

-  contains one entry for each geometry file.

   -  *original\_model\_id* is a primary key

   -  *original\_model\_name* is a convenient human-readable name for
      the model. For the models included with the CGDB this is just the
      model number from the PSB.

   -  *original\_model\_geometry\_path* and
      *original\_model\_thumbnail\_path* contain the paths - relative to
      the ``CGDB_MODEL_ROOT`` environment variable - to the geometry
      file and an associated thumbnail file.

   -  *original\_model\_tags* contains an array of text tags that refer
      to the model. For the models that come with the CGDB, this field
      is populated with the names of the classes and subclasses in the
      Princeton Shape Benchmark’s fine level classification that apply
      to this model.

   -  *original\_model\_grasping\_rescale* gives a conversion from the
      units in the geometry file to millimeters, where the model is at
      its “graspable size” as discussed in the . For most PSB models we
      took the units in the original file to be 10 inches.

   -  *original\_approximate\_radius* contains a statistical measure of
      the scale of the original model. We assume that all points on the
      surface of a mesh are normally distributed around a sphere of
      fixed radius centered on the center of mass. This value is that
      radius, plus 2 standard deviations of that distribution. It is
      used by the CGDB planner to find models at “similar” sizes to
      other models.

-  contains one entry for each scaled copy of an original model. Since
   grasping is scale-dependent, we duplicate original models at multiple
   scales (in this release, 0.75, 1, 1.25 and 1.5 times the original
   scale).

   -  *scaled\_model\_id* is a primary key

   -  *scaled\_model\_name* contains a human-readable name for the
      scaled model. Following the current convention
      (“collectionname\_scale\_modelname”) is a good idea.

   -  *scaled\_model\_scale* contains the rescale factor relative to the
      original model.

   -  *original\_model\_id* is a foreign key on the
      *original\_model*\ table, linking this scaled model to an unscaled
      original model.

Adding new models
~~~~~~~~~~~~~~~~~

To add new models to the CGDB, first place the geometry files (and
preferably, thumbnails) under the ``CGDB_MODEL_ROOT`` directory. Then,
create a row in *original\_model* referencing the new files. There is no
need to follow the PSB naming convention for files as long as the path
are stored correctly in the database. Make sure to store the
units-to-millimeters conversion in *original\_model\_grasping\_rescale*
and the approximate radius in *original\_model\_approximate\_radius*.
Unfortunately we are not currently providing code for computing the
approximate radius, but the description of the method above should help
those who wish to recreate it.

The next step is to add at least one row, to *scaled\_model*, or as many
rows as you like. (You can also add new *scaled\_model* rows for
existing models in the database at new scales.) After that, the model is
in the database and will be visible to the browser and to all CGDB code,
although it will not yet have any associated grasps, alignments, or
neighbors.

Robotic Hands
-------------

In this section we describe how hands are stored in the database.

-  contains one entry for each robotic hand used in the database by any
   grasp. We store a unique hand if any parameter has changed, so for
   example the Barrett Hand with different friction coefficients appears
   more than once.

   -  *hand\_id* is a primary key

   -  *hand\_name* is a convenient human-readable name for the hand.

Adding new hands
~~~~~~~~~~~~~~~~

At the moment, the hand information is tightly coupled to *GraspIt!*.
While it is possible to add new hands to the database without doing
anything in GraspIt!, the only way to link a database hand to a
*GraspIt!* hand model is to edit the code. This is not particularly
difficult, but we should fix it in the future.

Grasps
------

Now that we have models and hands, we can talk about storing grasps.

-  lists the possible sources of grasps. The primary source of grasps in
   the CGDB is from the Eigengrasp Planner, but there are some
   human-created grasp sources as well. In theory a database-backed
   planner can prioritize different sources, say for example preferring
   human created grasps to automatically created grasps when both are
   available. Our current planner only uses the automated grasps and so
   does not do this.

   -  *grasp\_source\_id* is a primary key

   -  *grasp\_source\_name* is a convenient human-readable name for the
      grasp source.

   -  *grasp\_source\_description* is a free-form field for more in
      depth comments.

-  contains one entry for each grasp done with a hand on a model in the
   database.

   -  *grasp\_id* is a primary key

   -  *scaled\_model\_id* references the scaled model the grasp was
      performed on.

   -  *hand\_id* references the hand used to perform the grasp.

   -  *grasp\_source\_id* references the grasp\_source where this grasp
      was created from.

   -  *grasp\_pregrasp\_joints* and *grasp\_pregrasp\_position* contain
      the information necessary to recreate the pregrasp.

   -  *grasp\_grasp\_joints* and *grasp\_grasp\_position* contain the
      information necessary to recreate the grasp.

   -  *grasp\_contacts* is an array of contact points between the hand
      and object. Each three numbers forms an (x,y,z) triple.

   -  *grasp\_epsilon\_quality* and *grasp\_volume\_quality* contain
      Ferrari-Canny grasp quality measures.

Adding new grasp sources
~~~~~~~~~~~~~~~~~~~~~~~~

If you wish to add data from your own planner to the CGDB, we suggest
you create a new entry in *grasp\_source*.

Adding new grasps
~~~~~~~~~~~~~~~~~

If you wish to add new grasps to the CGDB, you must fill in the various
fields of this table with the grasp information. The coordinate system
of the grasp positions is the model’s coordinate system.

Neighbors
---------

Aside from model and grasp information, the CGDB also stores geometric
relationships between 3D models to assist in grasp planning. If you wish
to use *GraspIt!*\ ’s built-in CGDB planner on new models that were not
distributed with the PSB, you will need to add information to this part
of the database.

In this section we describe how geometric neighbors are stored in the
database. We store neighbors according to a number of “distance
functions” that measure the similarities between 3D models. At this
time, we do not provide code for calculating such similarities yourself,
but only precomputed neighbor relationships for the existing models. We
are distributing 9 sets of neighbors for each model, as given by the
following distance functions.

-  ZERNIKE - Novotni and Klein’s Zernike descriptors, as described .

-  RANDOM - randomly selected neighbors for benchmarking purposes.

-  PSB - neighbors within the same subclass in the PSB as the current
   model. The ordering within the classes is arbitrary. Used for
   benchmarking.

-  SIFT\_12\_view\_x - A SIFT-PI descriptor, as described in the CGDB
   publications, which describes similarity using only what is visible
   from one side of the object. There are 6 such distance function in
   the database with different view numbers, and they correspond to
   descriptors taken from the centers of the 6 sides of the enclosing
   box.

For each distance function we store the 25 nearest neighbors of each
object, along with the distance to the neighbor (higher distance is less
similar). Note that the neighbor relationships are between *original*
models and not scaled models, since most measures of 3D similarity are
scale independent.

-  contains an entry for each similarity metric used in comparing 3D
   models and finding neighbors. Currently we provide 9 such distance
   functions (the 10th, “Eigengrasps”, is not actually a distance
   function and is used as a marker in some experiments that pit the
   Eigengrasp Planner against the Database Planner.)

   -  *distance\_function\_id* is a primary key

   -  *distance\_function\_name* is a convenient human-readable name for
      the distance function.

   -  *distance\_function\_description* is a more detailed comment about
      the distance function.

-  contains an entry for each neighbor relationship between two models
   in the database.

   -  *neighbor\_id* is a primary key

   -  *original\_model\_id* references the model we are finding
      neighbors for

   -  *neighbor\_original\_model\_id* references the model that has been
      found as a neighbor

   -  *distance\_function\_id* references the distance function used in
      finding this neighbor

   -  *neighbor\_distance* gives the distance to the neighbor (higher is
      less similar)

Adding new distance functions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To add a new distance function, simply add a new entry to the
*distance\_function* table. This is useful if you want to try your own
shape matching functions.

Adding new neighbors
~~~~~~~~~~~~~~~~~~~~

To add a new neighbor to the database, fill in a new row in the
*neighbor* table. Note that neighbor relationships are not automatically
symmetric, and so if you want the reverse relationship to be in the
database as well you must add it yourself. As we do not provide the
original descriptors or code used in creating the neighbors we have
distributed, we strongly recommend not adding any new neighbors to the
original distance functions. If you wish to add new models, we encourage
you to create a new distance function for all the models, including
those we have distributed.

Alignments
----------

To transfer grasps from one model to another we need a transformation
between their coordinate systems. The CGDB currently provides
precomputed rotational alignments for all neighbor relationships that
are in the database; translational alignments are done by colocating the
models’ centers of mass. At the moment, we do not provide the alignment
code itself. We currently provide data from 2 alignment methods:

-  TRIMESH\_LIB PCA - Models are aligned by aligning their principle
   axes. The was used to calculate PCAs.

-  SIFT\_FULL\_ICP - A SIFT - based alignment, as described in the CGDB
   publications, refined by ICP.

The database structure is as follows:

-  contains an entry for each method of computing alignments.

   -  *alignment\_method\_id* is a primary key

   -  *alignment\_method\_name* is a convenient human-readable name for
      the alignment method.

   -  *distance\_function\_description* is a more detailed comment about
      the alignment method.

-  contains an entry for each stored alignment between two models in the
   database.

   -  *alignment\_id* is a primary key

   -  *original\_model\_id* references the model we are trying to align
      (the one whose coordinates change)

   -  *alignment\_original\_model\_id* references the model that we are
      aligning to (the one whose coordinates remain fixed)

   -  *alignment\_method\_id* references the alignment method used in
      creating this alignment.

   -  *aignment* gives the transformation as a matrix

Adding new alignment methods
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To add a new distance function, simply add a new entry to the
*alignment\_method* table.

Adding new alignments
~~~~~~~~~~~~~~~~~~~~~

To add a new alignment to the database, fill in a new row in the
*alignment* table. Note that alignments are not automatically symmetric,
and so if you want the reverse relationship to be in the database as
well you must add it yourself. As we do not provide the code used in
creating the alignments we have distributed, we strongly recommend not
adding any new alignments to the original alignment methods.

Stored Procedures
-----------------

Along with the data and table structures, several stored procedures are
built into the database to ease use. We briefly describe them here:

-  turns a scaled\_model\_name into a scaled\_model\_id

-  turns a hand\_name into a scaled\_model\_id

-  turns a grasp\_source\_name into a grasp\_source\_id

-  retrieves a list of alignment method names

-  retrieves a list of distance function names

-  retrieves a list of grasp type names

-  retrieves basic information about all scaled models.

-  retrieves the largest scaled version of an original model with
   approximate radius smaller than a given radius, and the smallest
   version larger than that radius. One of these may not exist (if the
   radius is larger than the largest scaled model, or smaller than the
   smallest).

-  retrieves a list of saved neighbors for a given model

-  retrieves a list of saved grasps for a given model

-  retrieves a saved alignment between two models

More functions exist, but serve as helpers for those listed above.
