Following the Line: Proportional Control 
========================================

Introduction to Proportional Control
------------------------------------

In the previous lesson, we designed a *on-off* controller which had discrete actions depending on the error between the two sensors. However, this controller is not very smooth and can lead to oscillations. In this lesson, we will introduce a more advanced control technique called *proportional control* to make the robot follow the line more smoothly. 

Line following with proportional control is a common technique used in robotics to keep a robot on a desired path. The idea behind proportional control is to make corrections relative to the amount of error. That is, if there is a large amount of error, the robot will try to correct quickly, and if there is a small amount of error, the robot will make small corrections to get to the desired position.

Here is our previous code snippet using *on-off* control:

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

Instead of having discrete actions depending on the error, let's define a proportional signal that is proportional to the error between the two sensors. The proportional signal will be used to adjust the motor speeds based on the error. To do this, let's define a `proportional_signal` function in the `LineTracker` class which takes in a 'proportional gain' (KP) and returns the signal proportional to the error. Try to design your function such that you can quickly replace the `on_off_signal` function with the `proportional_signal` function.

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

        def proportional_signal(self, KP):
            """Generates a proportional signal based on the error between the sensors."""
            error = self.left_sensor() - self.right_sensor()
            proportional_signal = KP * error
            left_motor_effort = 50 - proportional_signal
            right_motor_effort = 50 + proportional_signal
            return left_motor_effort, right_motor_effort

Tuning Proportional Gain
------------------------
The value of KP is crucial for the stability of the line following behavior. If KP is too high, the robot will overcorrect, causing it to oscillate back and forth across the line. If KP is too low, the robot will not correct quickly enough, and it may drift off the line.

.. figure:: images/kp_tuning.png
    :align: center

    The effect of different KP values on the robot's behavior.

Intuitively, you can think of KP as how aggressively the robot tries to correct its error. A higher KP means more aggressive corrections, which can lead to overshooting and oscillations. A lower KP means more gentle corrections, which can lead to slow response times and drifting.

To tune the KP value, start with a small value and gradually increase it until the robot follows the line smoothly without oscillating. You may need to experiment with different KP values to find the optimal one for your robot and track.

Try to set up some code to start line following using the proportional control signal. Here's an example code snippet to get you started:

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

        def proportional_signal(self, KP):
            """Generates a proportional signal based on the error between the sensors."""
            error = self.left_sensor() - self.right_sensor()
            proportional_signal = KP * error
            left_motor_effort = 50 - proportional_signal
            right_motor_effort = 50 + proportional_signal
            return left_motor_effort, right_motor_effort

    line_tracker = LineTracker()
    KP = 0.1 # Start with a small KP value 

    while True:
        left_speed, right_speed = line_tracker.proportional_signal(KP)
        drivetrain.set_speed(left_speed, right_speed)

Here's what that a well-tuned controller looks like:

.. figure:: images/proportional_line_following.gif
    :align: center

    XRP following a line with proportional control. The robot would not be able 
    to follow a curved line this quickly using on-off control!

Activity: Racing Around a Circle
--------------------------------
Now that you have a good understanding of proportional control for line following, let's put it to the test with a fun activity! In this activity, you will race your robot around a circular track that has an intersection. When the robot hits the intersection, the line tracker's `is_over_line` function should trigger the robot to turn around and race back to where it started. The fastest "full lap" wins the competition!

Here's a step-by-step guide to set up the activity:

1. Set up a circular track with an intersection. You can use black tape on a white surface to create the track.
2. Program your robot to follow the line using the proportional control code provided earlier.
3. Use the `is_over_line` function to detect when the robot hits the intersection.
4. When the intersection is detected, have the robot turn around and race back to the starting point.
5. Time how long it takes for the robot to complete the full lap (from start to intersection and back to start).
6. The robot with the fastest time wins the competition!

Here's a sample code snippet to get you started:

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

        def proportional_signal(self, KP):
            """Generates a proportional signal based on the error between the sensors."""
            error = self.left_sensor() - self.right_sensor()
            proportional_signal = KP * error
            left_motor_effort = 50 - proportional_signal
            right_motor_effort = 50 + proportional_signal
            return left_motor_effort, right_motor_effort

    KP = 0.1  # TODO: replace with your value
    line_threshold = 0.5  # TODO: replace with your value
    line_tracker = LineTracker()

    while True:
        left_speed, right_speed = line_tracker.proportional_signal(KP)
        drivetrain.set_speed(left_speed, right_speed)
        
        if line_tracker.is_over_line(line_threshold):
            # Code to turn the robot around
            drivetrain.turn_degrees(180)
            time.sleep(1)  # Adjust the sleep time to complete the turn
            drivetrain.set_speed(50, 50)

.. error:: 
    
    TODO add video