Objectives - Module 4
=====================

.. image:: ManhattanStreets.png
  :width: 400

The objectives for this module are to:

* Learn about the use of lists and tuples, two important data types in the
  Python language.
* Learn how to keep track of the robot *pose* (position and heading) in the
  grid while driving.
* Have the robot navigate to absolute coordinates (intersections defined by
  the row and column indicies.)
* Use lists of intersections to define multiple goals for the robot to
  implement.
* Learn about the most basic navigation algorithm we call Manhattan driving
  named after the rectangular (and regular) streets and avenues in the
  New York city.
* Implement a path creation algorithm as a class in a way that allows
  many such algorithms to be used interchangeably.
* Implement a navigation class to contain your code to drive to the list
  of intersections in a way that it can be used with the path creation code. 

Final Project
-------------

You will be given intersections defined as a Python list containing
tuples. You will program your robot to drive to all the intersections in
the list in order using Manhattan navigation to figure out the path to get
from the current position to each intersection in turn.
