Getting the XRP to recognize lines
==================================

Introduction
------------

The XRP has two **reflectance** sensors mounted under the front bumber. Each
reflectance sensor shines an infrared light at the ground and measures the intensity of the 
light reflected from the driving surface. The darker an object is, the less light it reflects.
The sensor uses infrared light, just like a TV remote, so the light is not 
visible to the human eye.


.. figure:: line_sensors.png
    :width: 450

    The reflectance sensor as it is mounted on the underside of the robot front bumper.


This sensor is perfect for sensing dark lines on a light background! If the 
sensor is on top of a dark line, less light will be reflected back, and if it is
not on a line, more light will be reflected back. The left and right sensor objects
return values that are between 0.0 and 1.0 depending on the intensity of the reflected light.

**XRPLib** provides functions to read the values of the reflectance sensors:

.. code-block:: python

    from XRPLib.defaults import *

    # Reads the left sensor and stores the value in the variable "left"
    left = reflectance.get_left()

    # Reads the right sensor and stores the value in the variable "right"
    right = reflectance.get_right()

Characterizing the sensors
--------------------------
Before using a new sensor in an application, it's important to understand
the values it will return in different conditions. For the reflectance 
sensor, determine the values the sensor reports when it is:

* completely off the line (seeing a white surface),
* completely on the line (seeing a black surface),
* and straddling the line.

.. tip:: 

    Test a single sensor at a time, either the left or right.
    Make sure that the correct part of the sensor board over
    the line when taking measurements.

.. admonition:: Try it out

    To test the sensor over the surface that will be used, write a program to print the value
    of the chosen reflectance sensor and then, 
    print out the values in an infinite loop as the robot is moved around on the white surface
    with a line, and take record the values the sensor reads.

What do you notice from the values measured. The documentation for the 
``reflectance`` module in **XRPLib** states that the ``get_left()`` and 
``get_right()`` functions return a number between 0 and 1. Did the values ever 
reach exactly 0 or exactly 1? Which range of values corresponds to
seeing white and which values corresponds to seeing black?

Given the values determined in the previous step, what would be a good 
"threshold" value, such that if the sensor reports a 
value greater than the threshold, we can confidently assume the sensor is seeing
a line, and if the sensor reports a value below the threshold, we can assume it 
is not seeing a line.

.. admonition:: Try it out

    Select a threshold value that makes sense.
    A number around halfway between the minimum and the maximum value they 
    measured is a good starting point.

    Given the threshold value, write a function called ``is_over_line()`` which returns ``True``
    if the sensor sees a line
    (value above the threshold) or ``False`` if it does not. Don't delete this
    function, because it will be used for the rest of the module!

    Print out the result of calling the function in an infinite 
    loop. Move their robot around a surface with lines on it to make sure it 
    always returns the correct value based on the sensor position relative to the line. If it's
    returning incorrect values, try adjusting the threshold value.

Drive Until Line 
----------------

Using the function to determine if the sensor is over a line, write a program to to drive the robot forward until both
sensors detect the line.

.. admonition:: Try it out

    Write a program to drive the robot forward until both sensors detect the line. Use the ``is_over_line()`` function written
    earlier to check the sensor values.

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