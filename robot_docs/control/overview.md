# Control Module
## Overview

The control module serves the purpose of controlling the robot's movement and position using data from the navigation and the perception module.

## Key Features

1. Line Follower **(team member's work)**
    - Uses a PD-Controller to follow a line by utilizing sensor data from the perception module.
- Position Controller **(team member's work)**
    - Controls the position of the robot based on data from the navigation module.
- Kinematic Controller **(my work)**
    - Controls motor speeds according to a desired angular and forward speed for the robot.
- Trajectory Generation **(my work)**
    - Generates a bezier trajectory based on a desired start and end pose for the robot.
- Open-Loop Trajectory Controller **(my work)**
    - Utilizes the kinematic controller to follow a given bezier trajectory with reasonable accuracy.
    - Open loop to reduce complexity as tajectories are only for parking and thus generally rather short. Accuracy can therefore be lower without losing functionality.