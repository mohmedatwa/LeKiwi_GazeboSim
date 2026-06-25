# LeKiwi_ROS

A ROS-based control system for the LeKiwi mobile manipulator robot.

## Overview

LeKiwi is a mobile manipulator platform combining a wheeled mobile base with a multi-DOF robotic arm for autonomous manipulation tasks.

## Requirements

- ROS (Robot Operating System)
- Python 3.6+
- CMake 3.0+
- Colcon build tool

## Installation

Clone the repository into your ROS workspace:

```bash
cd ~/lekiwi_ws/src
git clone https://github.com/mohmedatwa/LeKiwi_ROS.git
cd ~/lekiwi_ws
colcon build
source install/setup.bash
```

## Project Structure

```
LeKiwi_ROS/
├── lekiwi_controller/     
├── lekiwi_description/    
├── LICENSE
└── README.md
```

## License

This project is licensed under the Apache License 2.0. See [LICENSE](LICENSE) for details.

## References

- [ROS Documentation](http://wiki.ros.org/)
- [Colcon Build Tool](https://colcon.readthedocs.io/)
