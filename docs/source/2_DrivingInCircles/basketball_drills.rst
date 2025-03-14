Basketball Drills
=================

Introduction
------------

Today, we're going to make the XRP perform basketball drills! We'll use the reflectance sensors to detect lines on the floor and drive the robot to different distances. We'll also introduce the concept of classes to organize our code better.

In Python, a class is a way to group related data and functions together. A class allows us to create objects that store information and perform actions. Instead of writing separate functions for different tasks, we can bundle them into a class, making our program more organized and reusable.

For example, rather than having separate variables for sensor values and multiple functions spread out in our code, we can create a LineTracker class that handles all line-tracking functionality in one place.

Parts of a Class
----------------

A class consists of:

- Fields (Attributes) - Variables that store information related to the object.
- Methods (Functions) - Functions that operate on the object's data and define its behavior.
- Constructor (__init__ method) - A special method that initializes an object when it is created.

Using functionality from our previous lessons, let's create a LineTracker class that:

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
            # Check the left sensor
            if self.left_sensor() > threshold:
                left_over_line = True
            else:
                left_over_line = False

            # Check the right sensor
            if self.right_sensor() > threshold:
                right_over_line = True
            else:
                right_over_line = False

            # Return True if both sensors are over the line
            return left_over_line and right_over_line

Understanding `self` and Class Variables
----------------------------------------

In the `LineTracker` class, you might have noticed the use of `self`. Let's break down what it means and why it's important.

- `self` is a reference to the current instance of the class. It allows us to access the attributes and methods of the class in Python.
- When we define a method in a class, we always include `self` as the first parameter. This is how Python knows that the method belongs to the class.

For example, in the `__init__` method, `self.left_sensor` and `self.right_sensor` are used to store the reflectance sensor functions as attributes of the `LineTracker` object. This way, we can easily access these sensors later in other methods of the class.

Here's why defining class variables (like the individual sensors) is useful:
- **Organization**: It keeps related data and functions together, making the code more organized.
- **Reusability**: We can create multiple instances of the `LineTracker` class, each with its own sensor values.
- **Maintainability**: If we need to change how the sensors are initialized or used, we only need to update the class definition, not every part of the code where sensors are used.

By using classes and `self`, we make our code cleaner, more modular, and easier to manage.

Testing the Class
-----------------

Now, let's create an instance of the `LineTracker` class and use it to check if the robot is over a line.

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

.. figure:: images/stop_at_line.webp
    :width: 450

.. note::

    When refactoring code, it's always beneficial to ensure that previous functionality is preserved. This ensures that we haven't lost any functionality in our code, and now, it's just written in a better, more maintainable way. Refactoring should improve the structure and readability of the code without altering its external behavior.

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

To access a specific index in a list, we use square brackets `[]` with the index number. Note that Python uses zero-based indexing, so the first element is at index 0.

Example:

.. code-block:: python

    first_distance = distances[0]  # Access the first element
    print("First distance:", first_distance)

    last_distance = distances[-1]  # Access the last element
    print("Last distance:", last_distance)

Basketball Drill: Pacers
------------------------

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
            # Check the left sensor
            if self.left_sensor() > threshold:
                left_over_line = True
            else:
                left_over_line = False

            # Check the right sensor
            if self.right_sensor() > threshold:
                right_over_line = True
            else:
                right_over_line = False

            # Return True if both sensors are over the line
            return left_over_line and right_over_line

    tracker = LineTracker()
    distances = [10, 20, 15, 25]  # Define the list of distances

    for distance in distances: # Iterate through each distance
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

.. error:: 

    add a video 