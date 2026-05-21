# Trajectory Tracing
## Overview
Trajectory tracing has been implemented using an open loop approach as the trajectories required for parking are generally relatively short which does not yield the necessity to achieve highly accurate trajectory tracing.

## Implementation
Each loop iteration, a step in parameter space of the cubic bezier is calculated by considering robot speed and the current derivative of the bezier curve in order to compute the updated s-parameter. The step in parameter space is limited to not skip the middle part of the s-curve.
To further increase tracing accuracy, velocities are controlled using the kinematic controller. Furthermore, a trapezoidal velocity profile has been implemented by defining a maximum acceleration. The robot accelerates at the beginning, then drives with a constant velocity along the bezier trajectory, and finally breaks with its maximum acceleration. Finally the robot performs a final angle adjustment with the help of the navigation module.