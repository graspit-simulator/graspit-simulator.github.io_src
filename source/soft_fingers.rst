.. role:: math(raw)
   :format: html latex
..

Soft Fingers
============

The GraspIt! engine never computes geometry deformation explicitly,
therefore can not find exact contact areas between soft bodies. However,
the frictional implications of soft fingers in contact are too important
to be completely ignored for grasp quality computations. The most
important effect is that contacts over an area (as opposed to point
contacts) can also apply **torsional friction**. The soft finger model
in GraspIt! attempts to capture at least an approximation of this
effect, without explicitly computing the contact deformation. See the
section for complete details on the theoretical aspects of our soft
finger contact computation.

In order to designate a body as “soft”, specify it’s Young’s modulus in
the Body XML file (see the section of this manual for a description of
the Body XML files). The XML tag that should be added is named
``youngs``, and it’s value should be the value of the Young’s modulus in
Pa. For example, an entry can have the following form:

``<youngs>1500000</youngs>``

Analytical contact area model
-----------------------------

Any body that has such an entry in the properties section, including
robot links, is treated by GraspIt! as a soft body. When a soft body is
found to contact another body (irrespective of whether the second body
is also soft or not), the contact engine does the following:

-  find a set of vertices on the surface of each body in a small area
   around the contact points. These vertices define the “soft
   neighborhood” of the contact

-  if multiple point contacts are found close to each other, only one of
   them is kept, and their soft neighborhoods are merged

-  fit an analytical surface to the soft neighborhoods. This is done by
   fitting a surface of the form :math:`z=ax^2+by^2+cxy` to the vertices
   in the neighborhoods

-  the radii of curvature of the analytical surfaces on each body are
   used to approximate the amount of torsional friction that can be
   applied at the contact

-  the Contact Wrench Space is built in order to also contain the effect
   of torsional friction. This wrench space is 4D (unlike the Coulomb
   friction cone which is 3D). Therefore, it can not be displayed as a
   contact marker. Instead, soft contacts are indicated by displaying a
   small patch of the analytical surface fit to each body around the
   contact.

-  the resulting Contact Wrench Space affect both grasp quality
   computations and the behavior of the contact in dynamic simulations.

All of this functionality is encapsulated in the ``SoftContact`` class;
see the code and documentation of this class for details.

Intuitively, this entire computation has the following effect: if the
bodies are locally “flat”, or if their curvatures match in a small
region around the contact, they will produce a larger area of contact
for a given normal force. This will in turn lead to larger torsional
friction. Conversely, sharp edges in contact, even on soft bodies, will
create small torsional friction. The amount of torsional friction is
also influenced by the value of Young’s modulus specified for each body.

This method captures much of the effects of soft contacts on the kind of
simulations that are of primarily interest in GraspIt!. It is important
to note though that it is only an **approximation** of the real-life
phenomenon. It relies on fitting analytical surfaces to each of the
bodies in a small region around the contact. On bodies with very complex
or degenerate geometry the fitting procedure can fail leading to
incorrect amounts of torsional friction applied. The fitting procedure
also is not very good at handling very sharp features such as corners or
edges.
