Syllabus: Introduction to Programming
-------------------------------------

.. image:: Bender_Rodriguez.png
    :width: 400

"My story is a lot like yours, only more interesting ‘cause it involves robots."


The XRP programming class is an introduction to computer science for high school and early
college age students. Using robots in a computer science makes learning exciting and much more
engaging. In this class you will write a program that solves an interesting problem, planning
a path to get from one place to another. Your project will work the same way that cars use
to find the shortest path to navigate from one place to another.

Course flow
===========
Throughout the course you will program your robot to navigate on a grid from one intersection to another taking the shortest path possible while avoiding obstacles along the way. This algorithm, called Dijkstra's Algorithm, is similar to those used by GPS mapping software such as Google Maps to find the shortest driving route between two points.
Each week of the course there will be project that you are responsible for completing. Each of those projects will hopefully be built upon to create the final project. 
The idea is that in solving this problem, you will learn basic to intermediate Python programming to serve as a base for more advanced CS and RBE classes later.

You will:

* Implement 7 projects, about one per week

* You will learn to program your robot with **Blockly**, a graphical programming system designed to get
  you familiar with programming and robot concepts very quickly. Then you will learn the more
  professional **Python** programming language that is used throughout industry. 

* We will try to do something with the robots most days, so bring them every day to the classroom.

* You should have fresh batteries, a laptop, and a USB cable. The robot works better with fully charged batteries (this isn't meant to be a snarky comment, some of the sensors don't work very well on lower voltage). 

* Projects will build on each other, so each weeks work should be similar to the previous week - no getting crushed on a two week marathon final project. But it is important to not get behind or you'll have a hard time catching up.

What you'll learn:

* Programming paradigms using the Python language running on your own personal computers and on the XRP robot.

* Some very basic Object Oriented program design concepts that will apply to other languages such as Java and C++.

* Some robotics concepts such as robot control and the use of sensors for  following lines and measuring distances.

* Top down design for breaking down complex problems into more manageable simpler tasks.

* Testing and debugging techniques to make it easier for program development.

The Projects
============
Here is a list of the projects that you will do throughout the class. Each of these projects will be
built on the code you wrote from the previous project. By building to the large final project,
you will get an idea of how
large programs are built up based standard design principles. The code you write each week will be
reused in the next week, so try to follow the design suggestions that are designed to maximize the
reusability of all the code.

Project 1: (Need to figure this out)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In this project you will program the robot to drive through a virtual *parking lot* navigating from
one spot to another without crossing any of the lines drawn on the course. It will require getting
the robot to drive precise distances and make accurate turns in sequence to get through the space.
You will do this in both Blockly then Python to see how to write programs in both languages.

Project 2: Following a line around a circular course
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Your robot will drive around a circle using the line following sensor. At one point on the circle, there
is a perpendicular line across the circle where your robot should turn around and continue driving in
the opposite direction.

Project 3: Navigate on a grid using relative coordinates
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Program your robot to drive on the grid in a square following lines and turning at intersections. The
robot will have to know how to recognize intersections (hint: same as the previous project), and make
turns to the next square edge.

Project 4: Navigating on the grid between stated intersections
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In this project, you will program your robot to first, compute the path between intersections, and then
follow that path. You will be given a series of intersections that the robot should navigate to.

Project 5: Implementing Dijkstra's Algorithm to compute shortest paths
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
This is similar to the previous project but it uses a technique called Dijkstra's Algorithm to compute
the shortest path between the intersections. In this more advanced version of the previous program,
the robot will be able to compute a path so as to avoid blocked intersections on the grid.

Project 6: Detecting blocked intersections while driving
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In the previous project, you were given the list of blocked intersections to avoid. In this project,
the robot will detect blocked intersections as it is driving using the rangefinder on the front of the
bot, then take these into account when planning a path.

