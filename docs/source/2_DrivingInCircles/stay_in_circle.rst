Staying Inside a Circle
=======================

Introduction
------------

In this lesson, you will program the XRP to stay inside a circle by using you developed in the previous lesson. This time however, we'll learn how to package code in `functions` to make important blocks of code easier to use. 

We'll then use our function to ensure that the robot stays inside a circular boundary. The robot will drive forward until it reaches the boundary, back up, turn a random angle, and then continue driving.

Functions in Python
-------------------

Functions are a fundamental concept in programming. They allow you to encapsulate code into reusable blocks, making your programs more modular and easier to understand. Functions provide several benefits:

- **Code Reuse**: Write a function once and use it multiple times.
- **Separation of Responsibilities**: Break down complex tasks into simpler, manageable functions.
- **Improved Readability**: Functions make your code easier to read and understand.

A function typically has the following parts:

- **Function Name**: A unique name that identifies the function.
- **Parameters**: Values that you can pass to the function to customize its behavior.
- **Return Value**: The result that the function produces and sends back to the caller.

Here is an example of a simple function in Python:

.. code-block:: python

    def greet(name):
        return f"Hello, {name}!"

    print(greet("Alice"))  # Output: Hello, Alice!

Writing the `is_over_line` Function
-----------------------------------

Define a function called ``is_over_line()`` which takes in a threshold value as a parameter. This function should return ``True`` if both of the sensors detects a line (value above the threshold) or ``False`` otherwise.

.. note:: 

    Recall how you checked if both sensors detected the line in the previous lesson. You can use the same logic to write the ``is_over_line()`` function.

Here is an example of how you might write this function:

.. code-block:: python

    from XRPLib.defaults import *

    def is_over_line(threshold):
        # Check the left sensor
        if reflectance.get_left() > threshold:
            left_over_line = True
        else:
            left_over_line = False

        # Check the right sensor
        if reflectance.get_right() > threshold:
            right_over_line = True
        else:
            right_over_line = False

        # Return True if both sensors are over the line
        return left_over_line and right_over_line

.. note::

    Like before, note that you can write the ``is_over_line()`` function in a more concise way: 

    .. code-block:: python

        from XRPLib.defaults import *

        def is_over_line(threshold):
            return reflectance.get_left() > threshold and reflectance.get_right() > threshold

    To increase readability, we will write the ``is_over_line()`` function in the previous way to make it easier to understand.

Next, incorporate this function into your program to drive the robot forward until both sensors detect the line.

Here is an example of how you might modify your code:

.. code-block:: python

    from XRPLib.defaults import *

    def is_over_line(threshold):
        # Check the left sensor
        if reflectance.get_left() > threshold:
            left_over_line = True
        else:
            left_over_line = False

        # Check the right sensor
        if reflectance.get_right() > threshold:
            right_over_line = True
        else:
            right_over_line = False

        # Return True if both sensors are over the line
        return left_over_line and right_over_line

    line_threshold = 0.5  # Example threshold value

    # Set the speed of both wheels
    drivetrain.set_speed(5, 5)

    # Drive forward until both sensors detect the line
    while not is_over_line(line_threshold):
        pass # This function allows the loop to continue running since all
             # of the "line checking" code is in the is_over_line function

    # Stop the drivetrain
    drivetrain.stop()

Test your code by placing the robot on a surface with a line and observing if it stops when both sensors are over the line. If the robot does not stop correctly, adjust your threshold value or check your ``is_over_line()`` function. Your XRP should do something similar to this:

.. figure:: images/stop_at_line.webp

Staying Inside a Circle
-----------------------

Now that you have written and verified the `is_over_line` function, let's move on to the main activity: keeping the robot inside a circular boundary.

To keep the robot inside the circle, we will use the reflectance sensors to detect when the robot reaches the line. Once the robot sees the line, it will:

1. Stop moving forward.
2. Back up to move away from the boundary.
3. Turn a random angle between 135 and 225 degrees.
4. Continue moving forward.

Here is how you can write this program:

.. code-block:: python

    from XRPLib.defaults import *  
    import random  # Import a python library to generate random numbers

    def is_over_line(threshold):
        # Check the left sensor
        if reflectance.get_left() > threshold:
            left_over_line = True
        else:
            left_over_line = False

        # Check the right sensor
        if reflectance.get_right() > threshold:
            right_over_line = True
        else:
            right_over_line = False

        # Return True if both sensors are over the line
        return left_over_line and right_over_line

    line_threshold = 0.5  # Example threshold value  

    while True:  
        # Move forward  
        drivetrain.set_speed(5, 5)  

        # Drive until the robot detects the line  
        while not is_over_line(line_threshold):  
            pass  

        # Stop the drivetrain  
        drivetrain.stop()  

        # Back up  
        drivetrain.set_speed(-5, -5)  
        sleep(0.5)  # Move back for 0.5 seconds  
        drivetrain.stop()  

        # Turn a random angle between 135 and 225 degrees  
        random_angle = random.randint(135, 225)  
        drivetrain.turn_degrees(random_angle)  

        # Continue the loop, driving forward again  

.. admonition:: Try it out

    Run the code on your XRP and place it inside a circle drawn with a dark boundary line.  
    Observe how the robot moves—does it successfully stay inside the circle?  
    If the robot sometimes crosses the boundary, try adjusting the threshold value.  
    By implementing this logic, the robot continuously detects the boundary, reacts, and keeps moving inside the circle without escaping!


.. error:: 

    add a video 