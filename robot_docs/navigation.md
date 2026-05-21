# Navigation Module
## Overview
The navigation module serves the purpose of allowing the robot to accurately determine its position on a given map by using a custom odometry algorithm.

## Key Features

1. Odometry algorithm
    - Uses the incremental encoders of the motors to determine the robot's position.
    - Implements corner detection to increase the robot's positional accuracy.
- Parking spot detection
    - Uses distance data from the perception module to accurately map parking spots.