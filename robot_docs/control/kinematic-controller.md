# Kinematic Controller
## Overview
The kinematic controller uses two pi controllers for the motor speeds in order to realize a specific angular and forward speed for the robot.

## Key Features

1. Takes desired kinematic state of the robot as input and calculates motor speeds.
- Uses two pi controllers
- Controls wheel speed in rad/s, then converts to motor drive value using an experimentally determined function.
- Safety factor regarding maximum motor speeds

## Recording The Motor Speed To Motor Drive Value Conversion Function
In order to record a function to determine the motor control value based on the measured motor speed, a simple algorithm was implemented which cycles through all relevant motor drive values and records the motor speed just before stepping to the next highest drive value. Using regression with a cubic polynomial to approximate the functional relation yields the following.

```python
# yM (for the right motor) as a function of right wheel speed
def yMRight(self, wheel_speed_right):
    if wheel_speed_right < 0:
        return -0.004496663911564471 * wheel_speed_right * wheel_speed_right * wheel_speed_right - 0.3495643763999523 * wheel_speed_right * wheel_speed_right + 53.6919009995278 * wheel_speed_right - 165.18648593231
    elif wheel_speed_right > 0:
        return wheel_speed_right * 1700 / (29.38887) - 300 * (29.59929) / 1700
    return 0

# yM (for the left motor) as a function of left wheel speed
def yMLeft(self, wheel_speed_left):
    if wheel_speed_left < -0:
        return -0.00199006068026202 * wheel_speed_left * wheel_speed_left * wheel_speed_left - 0.135572309354336 * wheel_speed_left * wheel_speed_left + 56.2486786207565 * wheel_speed_left - 188.22243084817
    elif wheel_speed_left > 0:
        return wheel_speed_left * 1700 / (29.59929 - 1.052107) - 300 * (29.59929 - 1.052107) / 1700
    return 0
```
A cubic polynomial was used in order to have the function be smooth around the origin.

## Tuning
Tuning the pi controllers was achieved by hand with the help of a function which allowed realtime parameter modification via the console.
```python
def alter_parameters(uart, clazz, parameter):
    uart.write(f"Enter value for {parameter}: \n")
    buf = uart.readline().decode("utf-8", "strict")
    val = float(buf.strip())
    try:
        uart.write(f"Value: '{val}'\n")
        setattr(clazz, parameter, val)
    except AttributeError:
        uart.write(f"Unknown attribute '{parameter}'. Could not assign value '{val}'.")
```
After creating this function, tuning became significantly more time efficient &mdash; not just for the kinematic controller, but even more so for the line follower. The pi values that yielded the best step response as well as sufficiently good performance while driving were the following.

```python
self.kp = 0.5
self.ki = 2
```