Publications and References
===========================

Copyright notice: this material is presented to ensure timely
dissemination of scholarly and technical work. Copyright and all rights
therein are retained by authors or by other copyright holders. All
persons copying this information are expected to adhere to the terms and
constraints invoked by each author’s copyright. In most cases, these
works may not be reposted without the explicit permission of the
copyright holder.

All of the publication below are available at .

For an introduction to GraspIt!, the most complete overview of its core
features is in:

-  Andrew Miller and Peter K. Allen. “Graspit!: A Versatile Simulator
   for Robotic Grasping”. IEEE Robotics and Automation Magazine, V. 11,
   No.4, Dec. 2004, pp. 110-122.

We recommend starting with that paper for the best introduction to the
system. Most of the papers below address individual features of the
simulator, you can read those that are relevant to the particular
project you are working on. The list of publication is presented in
chronological order, from oldest to newest. For each publication, we
also provide a short description of the parts of GraspIt! that it is
most relevant for.

-  Andrew T. Miller and Peter K. Allen. “Examples of 3D Grasp Quality
   Computations”. In Proceedings IEEE International Conference on
   Robotics and Automation, Detroit, MI, pp. 1240-1246, May 1999.

   Introductory theory on the grasp quality metrics used by GraspIt!.
   Discussed topics such as the Grasp Wrench Space, L1 and LInf norms,
   epsilon and volume quality metrics, etc.

-  Andrew T. Miller. “GraspIt!: A Versatile Simulator for Robotic
   Grasping”, Ph.D. Thesis, Department of Computer Science, Columbia
   University, June 2001.

   Extremely detailed presentation of the GraspIt! core. The
   presentation is mostly from a theoretical and research standpoint,
   but also covers a number of practical implementation issues.

-  Danica Kragic, Andrew T. Miller, Peter K. Allen. “Real-time tracking
   meets online grasp planning”. In Proceedings IEEE International
   Conference on Robotics and Automation, Seoul, Republic of Korea, pp.
   2460-2465, May 2001.

   Application of GraspIt! to execute a grasping task with a real robot.
   A real-life object is tracked using a camera, its position is
   replicated in GraspIt! where a grasp is planned using a virtual
   Barrett hand. The grasp is then executed using a real Barrett hand.

-  Andrew T. Miller, Steffen Knoop, Peter K. Allen, Henrik I.
   Christensen. “Automatic Grasp Planning Using Shape Primitives,” In
   Proceedings of the IEEE International Conference on Robotics and
   Automation, pp. 1824-1829, September 2003.

   Detailed discussion of the Primitive-based grasp planner.

-  Andrew T. Miller, Henrik I. Christensen. “Implementation of
   Multi-rigid-body Dynamics within a Robotic Grasping Simulator” In
   Proceedings of the IEEE International Conference on Robotics and
   Automation, pp. 2262 - 2268, September 2003.

   Presents the theoretical framework between the dynamics engine in
   GraspIt!. Covers topics such as time step integration, formulation of
   contact and joint constraints as Linear Complementarity constraints,
   etc. Shows how the full Linear Complementarity problem is assembled
   and solved at each time step of the dynamic engine. A must-read for
   understanding GraspIt! dynamics.

-  Rafael Pelossof, Andrew Miller, Peter Allen and Tony Jebara. “An SVM
   Learning Approach to Robotic Grasping”. In IEEE Int. Conf. on
   Robotics and Automation, New Orleans, April 29, 2004, pp. 3512-3518.

   Proposed the use of GraspIt! to generate large amounts labeled
   grasping data that can be used to apply machine learning algorithms.
   This code is not included in the current GraspIt! distribution.

-  Matei Ciocarlie, Claire Lackner and Peter Allen. “Soft finger model
   with adaptive contact geometry for grasping and manipulation tasks”.
   In IEEE Symposium on Haptic Interfaces for Virtual Environment and
   Teleoperator Systems, Tsukuba, JP, March 19-21, 2007.

   Discusses the Soft Finger contact as implemented in GraspIt!,
   covering the analytical surface approximation, soft finger grasp
   wrench space and formulation as linear complementarity constraints.

-  Corey Goldfeder, Peter K. Allen, Claire Lackner, Raphael Pelossof.
   “Grasp Planning via Decomposition Trees”. In IEEE Int. Conference on
   Robotics and Automation, April 13, 2007, Rome.

   Proposes an automatic method of decomposing an object into primitives
   (superquadrics) to fully automate the task of primitive-based grasp
   planning. This code is not included in the current GraspIt!
   distribution.

-  Matei Ciocarlie, Corey Goldfeder and Peter Allen. “Dimensionality
   reduction for hand-independent dexterous robotic grasping”. In IEEE /
   RSJ Conference on Intelligent Robots and Systems (IROS) 2007, San
   Diego, Oct. 29 - Nov. 2

   Introduces the eigengrasp concept and grasp planning in eigengrasp
   space as an optimization problem solved through Simulated Annealing.
   This is the recommended starting point if you are interested in
   eigengrasps.

-  Matei T. Ciocarlie and Peter K. Allen. “On-Line Interactive Dexterous
   Grasping”. In Eurohaptics 2008, Madrid, June 10-13, 2008.

   Presents an application of eigengrasp planning for on-line
   interaction with a human user. This is the theory behind the
   OnLinePlanner class included in the distribution.

-  Corey Goldfeder, Matei Ciocarlie, Hao Dang and Peter K. Allen. “The
   Columbia Grasp Database”. In IEEE Int. Conf. on Robotics and
   Automation 2009, Kobe.

-  Corey Goldfeder, Matei Ciocarlie, Jaime Peretzman, Hao Dang and Peter
   K. Allen. “Data-Driven Grasping Using Partial Sensor Data”. In
   IEEE/RSJ Conference on Intelligent Robots and Systems (IROS) 2009,
   St. Louis

   These two papers show how GraspIt! can be used to generate a huge
   database of labeled grasp data, and how this database can be used for
   data-driven grasp planning algorithms. The database is . GraspIt!
   also provides an interface for accessing and using this information;
   see the chapter in this manual.

-  Matei Ciocarlie and Peter Allen. “A Design and Analysis Tool for
   Underactuated Compliant Hands”. In IEEE/RSJ Conference on Intelligent
   Robots and Systems (IROS) 2009, St. Louis

   Presents one use of the GraspIt Grasp Force Optimization (GFO) engine
   and introduces the mathematical framework used by this engine.
