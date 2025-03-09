Understanding the line sensor
=============================

Introduction
------------

The line following sensor consists of two "reflectance" sensors. Simply put, the
reflectance sensor shines a light at the ground and measures how much of the 
light is reflected back. The darker an object is, the less light it reflects.
The sensor uses infrared light, just like a TV remote, so the light is not 
visible to the human eye.

This sensor is perfect for sensing dark lines on a light background! If the 
sensor is on top of a dark line, less light will be reflected back, and if it is
not on a line, more light will be reflected back. Students can use this information 
in their code to let the robot decide what to do in these situations.

The XRP has *two* reflectance sensors, a left sensor and a right sensor. If you
look at the bottom of your XRP on the sensor board, you will see the two
sensors. **S1** is the left sensor and **S2** is the right sensor. Later in the
module, students will learn a way to use both sensors to follow lines very smoothly. 
For this lesson, focus on getting an intuition for what values to expect from the sensors.

**XRPLib** provides functions to read the values of the reflectance sensors:

.. code-block:: python

    from XRPLib.defaults import *

    # Reads the left sensor and stores the value in the variable "left"
    left = reflectance.get_left()

    # Reads the right sensor and stores the value in the variable "right"
    right = reflectance.get_right()

Getting Started
---------------
Before doing anything with a new sensor, students need to have a good understanding 
of the values it will give in different conditions. For the reflectance 
sensor, it would be good to know what the sensor reports when it is completely 
off of the line (seeing a white surface), completely on the line (seeing a 
black surface), and some "middle of the road" values, when the sensor is half 
on the line and half off the line.

.. tip:: 

    Remember that for this exercise students should start by focusing on a single sensor. 
    Make sure that they center the correct part of the sensor board over
    the line when taking measurements.

.. admonition:: Try it out

    Have students write code to read the value of the chosen reflectance sensor. Then, 
    have them print out the values in an infinite loop as they move the robot.
    Move the XRP around on a white surface with a line, and take note of the values the
    sensor reads.

Discuss with students what they notice from the values measured. The documentation for the 
``reflectance`` module in **XRPLib** states that the ``get_left()`` and 
``get_right()`` functions return a number between 0 and 1. Did their values ever 
reach exactly 0 or exactly 1? Can they tell which range of numbers corresponds to
seeing white and which range of numbers corresponds to seeing black?

It's good for students to experiment with the reflectance sensor to see what it does, but they
took this data for a reason. The line following sensor reports back a number, 
but what we'd really like it to tell us is whether it sees a line or not. To do 
this, they'll need to select a "threshold" value, where if the sensor reports a 
value greater than the threshold, we can confidently assume the sensor is seeing
a line, and if the sensor reports a value below the threshold, we can assume it 
is not seeing a line.

.. admonition:: Try it out

    Have students look at their graph and select a threshold value that makes sense to them.
    A number around halfway between the minimum and the maximum value they 
    measured is a good starting point.

    Have students write a function called ``is_over_line()`` which reads the value of the
    right reflectance sensor and returns ``True`` if the sensor sees a line
    (value above the threshold) or ``False`` if it does not. Instruct them not to delete this
    function when they're done, because they'll use it for the rest of the module!

    Have students print out the result of calling their function in an infinite 
    loop. Have them move their robot around a surface with lines on it to make sure it 
    always returns the correct value based on what the sensor is seeing. If they 
    are getting incorrect values, have them adjust their threshold value.

Drive Until Line 
----------------

Now that students have a function to determine if the sensor is over a line, they can use this to drive the robot forward until both sensors detect the line.

.. admonition:: Try it out

    Have students write code to drive the robot forward until both sensors detect the line. They can use the ``is_over_line()`` function they wrote earlier to check the sensor values.

    Here is an example of how they might write this code:

    .. code-block:: python

        from XRPLib.defaults import *

        def is_over_line():
            threshold = 0.5  # Example threshold value
            return reflectance.get_right() > threshold

        # Set the speed of both wheels
        drivetrain.set_speed(5, 5)

        # Drive forward until both sensors detect the line
        while not (reflectance.get_left() > threshold and reflectance.get_right() > threshold):
            pass

        # Stop the drivetrain
        drivetrain.stop()

    Have students test their code by placing the robot on a surface with a line and observing if it stops when both sensors are over the line. If the robot does not stop correctly, have them adjust their threshold value or check their ``is_over_line()`` function.

check_line()
------------

Have students define a function called ``check_line()`` which takes in a threshold value as a parameter. This function should return ``True`` if either of the sensors detects a line (value above the threshold) or ``False`` if neither sensor detects a line.

Here is an example of how they might write this function:

.. code-block:: python

    from XRPLib.defaults import *

    def check_line(threshold):
        return reflectance.get_left() > threshold or reflectance.get_right() > threshold

Next, have students incorporate this function into their program to drive the robot forward until both sensors detect the line.

Here is an example of how they might modify their code:

.. code-block:: python

    from XRPLib.defaults import *

    def check_line(threshold):
        return reflectance.get_left() > threshold or reflectance.get_right() > threshold

    line_threshold = 0.5  # Example threshold value

    # Set the speed of both wheels
    drivetrain.set_speed(5, 5)

    # Drive forward until both sensors detect the line
    while not check_line(line_threshold):
        pass

    # Stop the drivetrain
    drivetrain.stop()

Have students test their code by placing the robot on a surface with a line and observing if it stops when both sensors are over the line. If the robot does not stop correctly, have them adjust their threshold value or check their ``check_line()`` function.

Advanced Maneuvers
------------------

In this section, students will learn how to make the robot perform more complex maneuvers. Specifically, the robot will drive forward until it detects a line, stop, turn 180 degrees, and then drive backward until it detects the line again, and stop.

.. admonition:: Try it out

    Have students write code to perform the advanced maneuver described above. They can use the ``check_line()`` function they wrote earlier to check the sensor values.

    Here is an example of how they might write this code:

    .. code-block:: python

        from XRPLib.defaults import *

        def check_line(threshold):
            return reflectance.get_left() > threshold or reflectance.get_right() > threshold

        line_threshold = 0.5  # Example threshold value

        # Set the speed of both wheels
        drivetrain.set_speed(5, 5)

        # Drive forward until both sensors detect the line
        while not check_line(line_threshold):
            pass

        # Stop the drivetrain
        drivetrain.stop()

        # Turn 180 degrees
        drivetrain.turn_degrees(180)

        # Set the speed of both wheels to drive backward
        drivetrain.set_speed(-5, -5)

        # Drive backward until both sensors detect the line
        while not check_line(line_threshold):
            pass

        # Stop the drivetrain
        drivetrain.stop()

    Have students test their code by placing the robot on a surface with a line and observing if it performs the maneuver correctly. If the robot does not stop correctly, have them adjust their threshold value or check their ``check_line()`` function.