Following Lines: On-Off Control
===============================

Introduction
------------

In the final project, your XRP robot will need to navigate to various locations. In previous lessons, we've learned how to make the robot move forward by a certain distance. However, relying only on distance can sometimes lead to the robot not ending up in the exact spot you intended. Think about it: what if the robot's wheels slip a little bit? What if the floor isn't perfectly flat? These small errors can add up over longer distances.

.. note::

    When we try to estimate the robot's position based only on how much its wheels have turned (this is called **odometry**), it can be a bit like guessing. Over time, small slips or bumps can make our guess less accurate. To navigate more precisely, especially when following a specific path, it's often better to use sensors to "see" where we need to go. Line following is one such sensor-based method that helps our robot stay on the right track and reach its destination more reliably.

    Let's learn how we can program our XRP robot to follow a line on the floor using its reflectance sensors!

How do we follow a line?
------------------------

Imagine you are trying to drive a toy car along a black line drawn on a white floor. You wouldn't just close your eyes and hope for the best! You would constantly look at the line and make small adjustments to the steering wheel to keep the car centered.

Our XRP robot can do something similar using its reflectance sensors. Remember from the "Sensing Lines" lesson that these sensors give us a reading between 0 (very dark, like a black line) and 1 (very light, like a white floor). Since the sensors are placed on the left and right sides of the robot's front, we can use their readings to figure out if the robot is right over the line, or if it's starting to drift off to one side.

Here's the basic logic we can use:

* **Going Straight:** If both the left and right sensors are reading approximately the same value, and that value indicates they are over the black line (close to 0), then the robot is likely centered on the line and should continue moving straight ahead.
* **Turning Right:** If the **left** sensor starts reading a value that indicates it's moving off the black line onto the white floor (the value gets closer to 1), while the **right** sensor is still reading the black line, it means the robot is veering to the left. To correct this, the robot should turn slightly to the **right** to get back over the line.
* **Turning Left:** Conversely, if the **right** sensor starts reading a value indicating it's moving off the black line onto the white floor, while the **left** sensor is still reading the black line, it means the robot is veering to the right. To correct this, the robot should turn slightly to the **left**.

This simple way of controlling the robot – by switching between actions (turning left, turning right, or going straight) based on whether the sensors are "on" or "off" the line – is called an **on-off controller**. It's like making quick little taps on the steering wheel to stay in your lane while driving a car. You're either correcting to the left, correcting to the right, or going straight.

This kind of decision-making, where we do different things based on a condition (like a sensor reading), is perfect for using ``if`` and ``else`` statements in our Python code! As you might remember, an ``if`` / ``else`` statement allows you to run different blocks of code depending on whether a certain condition is true or false.

Implementation
--------------

Let's start by looking at our ``LineSensor`` class again. Remember, this class is responsible for getting information directly from the robot's reflectance sensors. For line following, it will be helpful to know the difference between the readings of the left and right sensors. If the robot is perfectly centered on the line, these readings should be very close. If it's off to one side, the readings will be different.

Let's add a new function called ``get_error`` to our ``LineSensor`` class that calculates this difference:

.. code-block:: python

    from XRPLib.defaults import *

    class LineSensor:
        def __init__(self):
            """Initializes the line sensor by setting up the reflectance sensors."""
            self.left_sensor = reflectance.get_left
            self.right_sensor = reflectance.get_right

        def is_over_line(self, threshold):
            """Checks if both sensors are over the line (value above the threshold)."""
            left_over_line = self.left_sensor() > threshold
            right_over_line = self.right_sensor() > threshold
            return left_over_line and right_over_line

        def report_values(self):
            left = self.left_sensor()
            right = self.right_sensor()
            print(f'left: {left}, right: {right}')

        def get_error(self):
            """Calculates the error as the difference between the left and right sensor readings."""
            left = self.left_sensor()
            right = self.right_sensor()
            return left - right

In this ``LineTracker`` class, the ``line_follow_on_off`` function takes a ``threshold`` (a small value to decide when the robot is significantly off-center) and a ``speed`` as input. Here's how it works:

* It first gets the ``error`` value by calling ``self.sensor.get_error()``.
* It then uses ``if`` and ``elif`` (else if) statements to check the value of the ``error``:
    * If ``error`` is greater than the ``threshold`` (meaning the left sensor is seeing more of the white surface), it slightly reduces the speed of the right wheel, causing the robot to turn right.
    * If ``error`` is less than the negative of the ``threshold`` (meaning the right sensor is seeing more of the white surface), it slightly reduces the speed of the left wheel, causing the robot to turn left.
    * If the ``error`` is within the range of ``-threshold`` to ``threshold`` (meaning the robot is likely close to the center of the line), it sets both wheel speeds to the same value, making the robot go straight.

Now, let's modify our ``LineTracker`` class to use this ``get_error`` function to make the robot follow a line using our on-off control logic. We'll add a new function called ``line_follow_on_off``:

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
            while not self.sensor.is_over_line(threshold):
                pass  # Keep driving until the line is detected
            self.drivetrain.stop()

        def line_follow_on_off(self, threshold, speed):
            """Adjusts the motor speeds based on the error between the left and right sensor readings."""
            error = self.sensor.get_error()
            if error > threshold:
                # If the error is positive (left sensor sees more white), turn right by slightly reducing the right wheel speed
                self.drivetrain.set_speed(speed, speed - 0.1)
            elif error < -threshold:
                # If the error is negative (right sensor sees more white), turn left by slightly reducing the left wheel speed
                self.drivetrain.set_speed(speed - 0.1, speed)
            else:
                # If the error is close to zero (robot is likely centered), go straight
                self.drivetrain.set_speed(speed, speed)

In this ``LineTracker`` class, the ``line_follow_on_off`` function takes a ``threshold`` (a small value to decide when the robot is significantly off-center) and a ``speed`` as input. Here's how it works:

1.  It first gets the ``error`` value by calling ``self.sensor.get_error()``.
2.  It then uses ``if`` and ``elif`` (else if) statements to check the value of the ``error``:

    * If ``error`` is greater than the ``threshold`` (meaning the left sensor is seeing more of the white surface), it slightly reduces the speed of the right wheel, causing the robot to turn right.
    * If ``error`` is less than the negative of the ``threshold`` (meaning the right sensor is seeing more of the white surface), it slightly reduces the speed of the left wheel, causing the robot to turn left.
    * If the ``error`` is within the range of ``-threshold`` to ``threshold`` (meaning the robot is likely close to the center of the line), it sets both wheel speeds to the same value, making the robot go straight.

Now, let's see how we can use these modified classes in our main program to make the XRP robot follow a line:

.. code-block:: python

    from XRPLib.defaults import *

    # Initialize the line tracker by creating an instance of the LineTracker class
    line_tracker = LineTracker(drivetrain)

    # Tell the line tracker to follow the line using the on-off control method
    # We'll use a threshold of 0.1 and a base speed of 0.5 (you might need to adjust these values)
    line_tracker.line_follow_on_off(0.1, 0.5)

In this code, we first create an instance of our ``LineTracker`` class, giving it the robot's ``drivetrain``. Then, we call the ``line_follow_on_off`` function on our ``line_tracker`` object, providing a ``threshold`` value of ``0.1`` and a base ``speed`` of ``0.5``. You might need to experiment with these values to find what works best for your robot and the line you are trying to follow.

Remember that the ``error`` value we calculate is a measure of how far off the line the robot is. A perfect error of zero means the robot is perfectly centered. Our ``line_follow_on_off`` function uses this error to make decisions about how to adjust the motor speeds to keep the robot on the line.

.. error::

    Unfortunately, as a large language model, I cannot directly add video files to this document. However, a video here would really help show how the robot makes those quick left and right adjustments to stay on the line. You might consider adding a link to a video or embedding it in your learning materials.

Next time, we'll explore a more advanced and smoother way to make the robot follow lines using a technique called Proportional control. Stay tuned!