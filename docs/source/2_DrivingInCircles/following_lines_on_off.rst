Following Lines: On-Off
=======================

Now, let's turn our attention towards one of the core challenges in the final
project - following a line. In the project, the robot will need to drive to
multiple different locations, but doing this just based on distance can result
in the robot not getting to exactly the right place. What if the wheels slip
while driving? What if the robot needs to drive along a complex curve? It's
easier to follow a line than it is to exactly measure out the course the robot
needs to follow and program it.

.. note:: 
    Relying solely on positional estimates, known as odometry, can lead to errors over time due to factors like wheel slippage or uneven surfaces. To navigate more accurately and reliably, we often use sensor-based methods, such as line following. This approach helps our robot stay on track and reach its destination more precisely.

    Let's dive into how we can make our robot follow a line using reflectance sensors!


How do we follow a line?
------------------------

Imagine you're driving a little robot car that wants to stay on a black line on the floor. How would you drive the car to keep it on the line? 
Remember that the line sensors give a reading from 0 (black) to 1 (white). Assuming that the reflectance sensors are
placed on the left and right sides of the robot, the readings from the sensors can inform you when the robot is centered on the line, or, 
when one side is veering off the line. We can write out the logic for this as follows:

* If both sensors read approximately the same value, the robot is centered on the line and should go straight.
* If the `left` sensor reads that it is off the line (the difference between the `left` and `right` sensor readings is greater than a threshold), the robot should turn right.
* If the `right` sensor reads that it is off the line (the difference between the `right` and `left` sensor readings is greater than a threshold), the robot should turn left.

This type of control architecture is known as an *on-off controller*, where the robot switches between two states (on the line or off the line) and takes action accordingly. In
this kind of controller, there's no smooth turning, just quick corrections left or right to stay on the line. It's a bit like tapping the steering wheel back and forth to keep a car centered in its lane!

This seems like the perfect problem for an if-else statement! As a quick reminder, an :code:`if` / :code:`else` statement allows you to run different blocks of
code based on a *condition* (the same kind of *condition* you used in a :code:`while` loop)

Consider the following example code:

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

        def on_off_signal(self, threshold):
            """Generates control signals based on sensor readings and a threshold."""
            left_sensor = self.left_sensor()
            right_sensor = self.right_sensor()
            error = left_sensor - right_sensor
            
            if abs(error) < threshold:
            return 50, 50  # Go straight
            elif error > threshold:
            return 30, 50  # Turn right
            elif error < -threshold:
            return 50, 30  # Turn left

    line_tracker = LineTracker()

    while True:
        left_speed, right_speed = line_tracker.on_off_signal()
        drivetrain.set_speed(left_speed, right_speed)

In this example code, we adjust the motor speeds based on the values of the
left and right reflectance sensors. Note that we denote the difference between the left and right sensor readings as the `error`.
This is because, in a perfect scenario, the robot would be centered on the line, and the difference between the left and right sensor readings would be zero.
Thus, the `error` is a measure of how far off the line the robot is. We then use this `error` to determine the motor speeds for the robot to stay on the line.

.. error:: 
    include a vid 

Next time, we'll see how we can use a more intuitive approach to follow the lines in a much smoother way. Stay tuned!
