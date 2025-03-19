Basketball Drills
=================

Introduction
------------

Today, we're going to make the XRP perform basketball drills! We'll use the reflectance sensors to detect lines on the floor and drive the robot to different distances. We'll also introduce the concept of classes to organize our code better.

In Python, a class is a way to group related data and functions together. A class allows us to create objects that store information and perform actions. Instead of writing separate functions for different tasks, we can bundle them into a class, making our program more organized and reusable.

For example, rather than having separate variables for sensor values and multiple functions spread out in our code, we can create a set of classes that handle all line-tracking functionality.

Parts of a Class
----------------

A class consists of:

- Fields (Attributes) - Variables that store information related to the object.
- Methods (Functions) - Functions that operate on the object's data and define its behavior.
- Constructor (__init__ method) - A special method that initializes an object when it is created.

Using functionality from our previous lessons, let's create two classes: `LineSensor` and `LineTracker`.

- `LineSensor` will directly interact with the reflectance sensors to determine if the robot is over a line.
- `LineTracker` will use a `LineSensor` object to manage line-tracking functionality and provide higher-level operations.

Class Implementation
--------------------

Let's first implement thee `LineSensor` class. As a reminder, the `LineSensor` class will solely return information directly from the reflectance sensors.

.. code-block:: python

    from XRPLib.defaults import *

    class LineSensor:
        def __init__(self):
            """Initializes the line sensor by setting up the reflectance sensors."""
            self.left_sensor = reflectance.get_left
            self.right_sensor = reflectance.get_right

        def is_over_line(self, threshold):
            """Checks if either sensor is over the line."""
            left_over_line = self.left_sensor() > threshold
            right_over_line = self.right_sensor() > threshold
            return left_over_line and right_over_line

        def report_values(self):
            left = self.reflectance.get_left()
            right = self.reflectance.get_right()
            print(f'left: {left}, right: {right}')

Defining the `LineSensor` class allows us to encapsulate the sensor functionality and reuse it in other parts of our code. Furthermore, it provides a modular foundation which can be easily modified or replicated in the future in case we would want to use a different type of line sensor. Now, let's create the `LineTracker` class that uses the `LineSensor` class to track lines.

.. code-block:: python

    from XRPLib.defaults import *

    class LineTracker:
        def __init__(self, drivetrain):
            """Initializes the line tracker with a LineSensor object and a drivetrain object."""
            self.sensor = LineSensor()
            self.drivetrain = drivetrain

        def drive_until_line(self, threshold, speed):
            """Drives forward until the line sensors detect the robot is over the line."""
            self.drivetrain.set_speed(speed, speed)
            while not self.is_over_line(threshold):
                pass  # Keep driving until the line is detected
            self.drivetrain.stop()

Understanding `self` and Class Variables
----------------------------------------

In these classes, you might have noticed the use of `self`. Let's break down what it means and why it's important.

- `self` is a reference to the current instance of the class. It allows us to access the attributes and methods of the class in Python.
- When we define a method in a class, we always include `self` as the first parameter. This is how Python knows that the method belongs to the class.

In this program, we have two classes: `LineSensor` and `LineTracker`. The `LineTracker` class uses an instance of the `LineSensor` class to perform its operations. For example, in the `__init__` method of `LineTracker`, we create a `LineSensor` object and store it as an attribute of the `LineTracker` object using `self.sensor`. This allows the `LineTracker` to access the functionality of the `LineSensor` class.

Here's why this approach is useful:
- **Encapsulation**: The `LineSensor` class encapsulates all the functionality related to the reflectance sensors, while the `LineTracker` class focuses on higher-level line-tracking operations. This separation of concerns makes the code more modular.
- **Reusability**: By using `self.sensor`, the `LineTracker` class can easily interact with its own `LineSensor` object. This means we can create multiple `LineTracker` objects, each with its own `LineSensor` instance, without conflicts.
- **Maintainability**: If we need to update how the sensors work, we only need to modify the `LineSensor` class. The `LineTracker` class will automatically use the updated functionality.

For example, in the `is_over_line` method of `LineTracker`, we call `self.sensor.is_over_line(threshold)` to check if the robot is over the line. This demonstrates how the `LineTracker` relies on its own `LineSensor` instance to perform sensor-related tasks.

By using classes and `self`, we make our code cleaner, more modular, and easier to manage. This design also allows us to build more complex functionality by combining simpler, well-defined components.

Driving Until the Line
----------------------

Now that we have our `LineTracker` class, let's use it to drive the robot forward until it detects a line.

.. code-block:: python

    from XRPLib.defaults import *

    tracker = LineTracker(drivetrain)
    tracker.drive_until_line(0.5, 5)

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

    tracker = LineTracker(drivetrain)  # Create a LineTracker object
    distances = [10, 20, 15, 25]  # Define the list of distances

    for distance in distances:  # Iterate through each distance
        drivetrain.set_speed(5, 5)  # Drive forward
        drivetrain.drive_distance(distance)  # Travel the given distance
        drivetrain.turn_degrees(180)  # Turn around

        tracker.drive_until_line(0.5, 5)  # Drive back to start using LineTracker

        print(f"Completed drill for {distance} units.")

Try It Out
----------

Run the program on the robot.
Observe how it travels to different distances, turns around, and stops at the line.
Modify the list of distances and see how it changes the robot's movement.

.. error:: 

    add a video 