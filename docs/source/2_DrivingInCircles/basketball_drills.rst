Basketball Drills
=================

Introduction to Classes
-----------------------

In Python, a class is a way to group related data and functions together. A class allows us to create objects that store information and perform actions. Instead of writing separate functions for different tasks, we can bundle them into a class, making our program more organized and reusable.

For example, rather than having separate variables for sensor values and multiple functions spread out in our code, we can create a LineTracker class that handles all line-tracking functionality in one place.

Parts of a Class
----------------

A class consists of:

- Fields (Attributes) - Variables that store information related to the object.
- Methods (Functions) - Functions that operate on the object's data and define its behavior.
- Constructor (__init__ method) - A special method that initializes an object when it is created.

Defining the LineTracker Class
------------------------------

We will create a LineTracker class that:

- Initializes the left and right reflectance sensors.
- Provides a function to check if the robot is over a line based on a given threshold.
- Can be expanded later with more line-tracking functionality.

Class Implementation
--------------------

.. code-block:: python

    from XRPLib.defaults import *

    class LineTracker:
        def __init__(self):
            """Initializes the line tracker by setting up the reflectance sensors."""
            self.left_sensor = reflectance.get_left
            self.right_sensor = reflectance.get_right

        def is_over_line(self, threshold):
            """Checks if both sensors detect the line based on the given threshold."""
            return self.left_sensor() > threshold and self.right_sensor() > threshold

Testing the Class
-----------------

Now, let's create an instance of the `LineTracker` class and use it to check if the robot is over a line.

Try It Out
----------

Write a program that creates a `LineTracker` object and prints whether the robot is over the line:

.. code-block:: python

    tracker = LineTracker()  # Create a LineTracker object

    if tracker.is_over_line(0.5):
        print("Robot is over the line")
    else:
        print("Robot is not over the line")

Driving Until the Line
----------------------

Now that we have our `LineTracker` class, let's use it to drive the robot forward until it detects a line.

.. code-block:: python

    from XRPLib.defaults import *

    tracker = LineTracker()
    drivetrain.set_speed(5, 5)

    while not tracker.is_over_line(0.5):
        pass  # Keep driving until the line is detected

    drivetrain.stop()

Ensure that the XRP behaves as it should:

.. figure:: images/stop_at_line.webp
    :width: 450

    The XRP driving forward until both sensors detect the line.


Basketball Drills Activity
==========================

Introduction to Lists
---------------------

A list in Python is a way to store multiple values in a single variable. We can use lists to store different distances that the robot will travel.

Example of a list:

.. code-block:: python

    distances = [10, 20, 15, 25]  # Distances in some unit

We can use a for loop to iterate through each distance in the list.

Example:

.. code-block:: python

    for distance in distances:
        print("Traveling", distance, "units")

Basketball Drill: Pacer
-----------------------

In basketball, a pacer drill involves running to a series of increasing distances, turning around, and returning to the starting line. We will program the robot to:

- Travel a distance from the list.
- Turn around.
- Drive back to the starting line using the `LineTracker`.
- Repeat for all distances in the list.

Code Implementation
-------------------

.. code-block:: python

    from XRPLib.defaults import *

    class LineTracker:
        def __init__(self):
            """Initializes the line tracker by setting up the reflectance sensors."""
            self.left_sensor = reflectance.get_left
            self.right_sensor = reflectance.get_right

        def is_over_line(self, threshold):
            """Checks if both sensors detect the line based on the given threshold."""
            return self.left_sensor() > threshold and self.right_sensor() > threshold

    tracker = LineTracker()
    distances = [10, 20, 15, 25]  # Define the list of distances

    for distance in distances:
        drivetrain.set_speed(5, 5)  # Drive forward
        drivetrain.drive_distance(distance)  # Travel the given distance
        drivetrain.turn_degrees(180)  # Turn around

        drivetrain.set_speed(5, 5)  # Drive back to start
        while not tracker.is_over_line(0.5):
            pass  # Keep driving until the line is detected

        drivetrain.stop()  # Stop at the line
        print(f"Completed drill for {distance} units.")

Try It Out
----------

Run the program on the robot.
Observe how it travels to different distances, turns around, and stops at the line.
Modify the list of distances and see how it changes the robot's movement.
