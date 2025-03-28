Following the Line: Proportional Control
========================================

Introduction to Proportional Control
------------------------------------

In the previous lesson, we learned about an *on-off* controller for line following. This type of controller works by making quick, discrete corrections based on whether the robot is on or off the line. While it can work, you might have noticed that the robot's movement wasn't always very smooth, and it might have wobbled back and forth quite a bit. This back-and-forth movement is called **oscillation**.

In this lesson, we're going to explore a more advanced and smoother way to control the robot called *proportional control*. This technique is widely used in robotics to make robots follow paths more accurately and gracefully.

The main idea behind proportional control is to make the robot's corrections **proportional** to how far it is off the line. Think of it this way:

* **Big Error, Big Correction:** If the robot is far away from the line, the proportional controller will tell the motors to make a large, quick correction to get back on track.
* **Small Error, Small Correction:** If the robot is only slightly off the line, the controller will make a small, gentle correction.
* **No Error, No Correction:** If the robot is perfectly centered on the line, the controller won't make any unnecessary corrections.

This is different from the on-off controller, which always makes the same size of turn whether the robot is just a tiny bit off or way off the line. Proportional control allows for much smoother and more precise line following.

Let's see how we can implement this! Instead of just having on or off actions, we'll create a **proportional signal**. This signal will be calculated based on the difference between the readings of the left and right sensors (our "error"). The bigger the error, the bigger the proportional signal will be. We'll then use this signal to adjust the speeds of the robot's motors.

To do this, we'll add a new function called `line_follow_proportional` to our `LineTracker` class. This function will take a value called **proportional gain** (often abbreviated as KP) and a base speed as inputs.

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

        def line_follow_on_off(self, threshold, speed):
            """Adjusts the motor speeds based on the error between the left and right sensor readings (on-off control)."""
            error = self.sensor.get_error()
            if error > threshold:
                self.drivetrain.set_speed(speed, speed - 0.1)
            elif error < -threshold:
                self.drivetrain.set_speed(speed - 0.1, speed)
            else:
                self.drivetrain.set_speed(speed, speed)

        def line_follow_proportional(self, KP, base_speed):
            """Follows the line using proportional control."""
            error = self.sensor.get_error()
            proportional_signal = KP * error
            left_speed = base_speed - proportional_signal
            right_speed = base_speed + proportional_signal
            self.drivetrain.set_speed(left_speed, right_speed)

Let's break down the `line_follow_proportional` function:

1.  `error = self.sensor.get_error()`: First, we get the current error (the difference between the left and right sensor readings) using the `get_error` function from our `LineSensor` class.
2.  `proportional_signal = KP * error`: This is the core of proportional control! We calculate the `proportional_signal` by multiplying the `error` by the **proportional gain** (KP). KP is a number that determines how strongly the robot reacts to the error. We'll talk more about tuning this value in a moment.
3.  `left_speed = base_speed - proportional_signal`: We calculate the speed for the left motor. If the `error` is positive (meaning the left sensor is seeing more white), the `proportional_signal` will also be positive (assuming KP is positive). Subtracting this from the `base_speed` will make the left motor go slower.
4.  `right_speed = base_speed + proportional_signal`: We calculate the speed for the right motor. If the `error` is positive, adding the `proportional_signal` to the `base_speed` will make the right motor go faster. This difference in speed between the left and right wheels will cause the robot to turn right, back towards the line. The opposite happens if the `error` is negative.
5.  `self.drivetrain.set_speed(left_speed, right_speed)`: Finally, we set the calculated speeds for the left and right motors.

Tuning Proportional Gain
------------------------

The value of KP is super important for getting the proportional control to work well. It's like finding the right volume on a radio – too low, and you can't hear it; too high, and it's too loud and distorted.

.. figure:: images/kp_tuning.png
    :align: center

    The effect of different KP values on the robot's behavior.

* **KP Too High:** If your KP value is too high, the robot will be very sensitive to even small errors. It will try to correct too quickly and too strongly, which can cause it to overshoot the line and then overcorrect in the other direction. This leads to the robot **oscillating** wildly back and forth across the line, like a shaky driver overcorrecting their steering.
* **KP Too Low:** If your KP value is too low, the robot won't react strongly enough to errors. It will make very gentle corrections, and if it starts to drift off the line, it might not correct quickly enough to get back on track. This can result in the robot slowly wandering away from the line, especially on curves.

Think of KP as the **aggressiveness** of the robot's corrections. A high KP means the robot is very aggressive in trying to get back on the line, while a low KP means it's more gentle.

To find the best KP value for your robot and the line you're using, you'll likely need to do some **tuning**. This means starting with a small KP value and gradually increasing it while observing how the robot follows the line. You're looking for a value that allows the robot to follow the line smoothly without too much oscillation. You might need to try a few different KP values to find the sweet spot!

Let's try setting up some code to make the robot follow a line using our proportional control function. Here's an example to get you started:

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
            left = self.left_sensor()
            right = self.reflectance.get_right()
            print(f'left: {left}, right: {right}')

        def get_error(self):
            """Calculates the error as the difference between the left and right sensor readings."""
            left = self.left_sensor()
            right = self.right_sensor()
            return left - right

    class LineTracker:
        def __init__(self, drivetrain):
            """Initializes the line tracker with a LineSensor object and a drivetrain object."""
            self.sensor = LineSensor()
            self.drivetrain = drivetrain

        def line_follow_on_off(self, threshold, speed):
            """Adjusts the motor speeds based on the error between the left and right sensor readings."""
            error = self.sensor.get_error()
            if error > threshold:
                self.drivetrain.set_speed(speed, speed - 0.1)
            elif error < -threshold:
                self.drivetrain.set_speed(speed - 0.1, speed)
            else:
                self.drivetrain.set_speed(speed, speed)

        def proportional_signal(self, KP, base_speed):
            """Generates motor speeds using proportional control based on the error between the sensors."""
            error = self.sensor.get_error()
            proportional_signal = KP * error
            left_motor_effort = base_speed - proportional_signal
            right_motor_effort = base_speed + proportional_signal
            return left_motor_effort, right_motor_effort

    drivetrain = Drivetrain()  # Initialize the drivetrain
    line_tracker = LineTracker(drivetrain)
    KP = 0.1  # Start with a small KP value
    base_speed = 50  # Base speed for the robot

    while True:
        left_speed, right_speed = line_tracker.proportional_signal(KP, base_speed)
        drivetrain.set_speed(left_speed, right_speed)

Here's what a well-tuned proportional controller can achieve:

.. figure:: images/proportional_line_following.gif
    :align: center

    XRP following a line with proportional control. Notice how much smoother the movement is compared to the on-off control! The robot is even able to follow a curved line at a decent speed.

Activity: Racing Around a Circle
--------------------------------

Now that you have a good understanding of proportional control for line following, let's put it to the test with a fun activity! In this activity, you will race your robot around a circular track that has an intersection (a point where the line crosses itself). When the robot's sensors detect this intersection, we'll program it to turn around and race back to where it started. The fastest "full lap" wins the competition!

Here's a step-by-step guide to set up the activity:

1.  **Create the Track:** Use black tape on a white surface to create a circular track. Make sure to include a clear intersection point where the line crosses itself.
2.  **Program Your Robot:** Use the proportional control code provided earlier as a starting point.
3.  **Detect the Intersection:** You can use the `is_over_line` function from your `LineSensor` class to detect when the robot reaches the intersection. Since the intersection will likely be a wider or darker area, the sensor readings might go above your regular line-following threshold.
4.  **Turn Around:** When the intersection is detected, program the robot to turn around 180 degrees. You can use the `drivetrain.turn_degrees(180)` function for this. You might need to add a short `time.sleep()` after the turn to ensure it's completed before the robot starts moving forward again.
5.  **Time the Lap:** Use a stopwatch to measure how long it takes for the robot to complete one full lap – from the starting point, around the circle to the intersection, and then back to the starting point.
6.  **The Race:** Have a friendly competition with your classmates to see whose robot can complete the lap the fastest!

Here's a sample code snippet to get you started with the racing activity:

.. code-block:: python

    from XRPLib.defaults import *
    import time  # We'll need this to add a delay

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

        def get_error(self):
            """Calculates the error as the difference between the left and right sensor readings."""
            left = self.left_sensor()
            right = self.right_sensor()
            return left - right

    class LineTracker:
        def __init__(self, drivetrain):
            """Initializes the line tracker with a LineSensor object and a drivetrain object."""
            self.sensor = LineSensor()
            self.drivetrain = drivetrain

        def line_follow_on_off(self, threshold, speed):
            """Adjusts the motor speeds based on the error between the left and right sensor readings."""
            error = self.sensor.get_error()
            if error > threshold:
                self.drivetrain.set_speed(speed, speed - 0.1)
            elif error < -threshold:
                self.drivetrain.set_speed(speed - 0.1, speed)
            else:
                self.drivetrain.set_speed(speed, speed)

        def proportional_signal(self, KP, base_speed):
            """Generates motor speeds using proportional control based on the error between the sensors."""
            error = self.sensor.get_error()
            proportional_signal = KP * error
            left_motor_effort = base_speed - proportional_signal
            right_motor_effort = base_speed + proportional_signal
            return left_motor_effort, right_motor_effort

    KP = 0.1  # TODO: replace with your tuned KP value
    line_threshold = 0.5  # TODO: you might need to adjust this for intersection detection
    drivetrain = Drivetrain()  # Initialize the drivetrain
    line_tracker = LineTracker(drivetrain)
    base_speed = 50

    while True:
        left_speed, right_speed = line_tracker.proportional_signal(KP, base_speed)
        drivetrain.set_speed(left_speed, right_speed)

        if line_tracker.sensor.is_over_line(line_threshold):
            # Code to turn the robot around
            drivetrain.turn_degrees(180)
            time.sleep(1)  # Give the robot a moment to complete the turn
            drivetrain.set_speed(50, 50) # Start moving forward again

.. error::

    add a vid