# LeKiwi_GazeboSim

A ROS 2 stack for the LeKiwi mobile manipulator: URDF description, Gazebo simulation, ros2_control, and MoveIt 2 motion planning.
---
## Overview

LeKiwi is a mobile manipulator that combines:

- A **3-wheel omni-directional base** driven by `OmniWheelDriveController`
- A **5-DOF SO-ARM100 arm** with a parallel gripper
- **Gazebo Sim** integration via `ros_gz_sim` and `gz_ros2_control`
- **MoveIt 2** configuration for arm and gripper planning
---
## Requirements

- [ROS 2](https://docs.ros.org/) jazzy
- Ubuntu 24.04 
- Python 3.10+
- CMake 3.8+
- [Colcon](https://colcon.readthedocs.io/)

---

## Installation

Clone the repository into your ROS 2 workspace, then build and source it:

```bash
mkdir -p lekiwi_ws/src
cd ~/lekiwi_ws/src
git clone https://github.com/mohmedatwa/LeKiwi_GazeboSim.git
cd ~/lekiwi_ws
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install
source install/setup.bash
```

---

## Usage

### Full simulation (Gazebo + controllers + RViz)

Starts Gazebo Sim, spawns the robot, loads ros2_control controllers, and opens RViz:

```bash
ros2 launch lekiwi_bringup lekiwi.launch.py
```

Drive the base by publishing to `/lekiwi_controller/cmd_vel`.

### Visualize the robot model (no simulation)

Opens RViz with a joint-state GUI for manual joint control:

```bash
ros2 launch lekiwi_description display.launch.py
```

---

### Gazebo simulation only

```bash
ros2 launch lekiwi_description lekiwi_gz.launch.py
```

---

### Controllers only

Useful when simulation is already running:

```bash
ros2 launch lekiwi_controller lekiwi_control.launch.py
```

This spawns:


| Controller                | Type                    | Purpose                            |
| ------------------------- | ----------------------- | ---------------------------------- |
| `joint_state_broadcaster` | Joint state broadcaster | Publishes joint states             |
| `lekiwi_controller`       | Omni wheel drive        | Base velocity control and odometry |
| `so100_controller`        | Joint trajectory        | Arm motion                         |
| `gripper_controller`      | Gripper action          | Gripper control                    |



---
### MoveIt 2

Planning groups:

- `so100` — 5-DOF arm
- `gripper` — gripper joint

Predefined arm states: `home`, `pose1`.
---
## Project Structure

```
LeKiwi_ROS/
├── lekiwi_description/   # URDF/xacro, meshes, Gazebo and RViz launch files
├── lekiwi_controller/    # ros2_control controller configuration and spawners
├── lekiwi_bringup/       # Top-level launch files for simulation and MoveIt
├── lekiwi_moveit/        # MoveIt 2 configuration (SRDF, kinematics, planners)
├── LICENSE
└── README.md
```


---
## License

This project is licensed under the Apache License 2.0. See [LICENSE](LICENSE) for details.
---
## References

- [ROS 2 Documentation](https://docs.ros.org/)
- [MoveIt 2](https://moveit.picknik.ai/)
- [Gazebo Sim](https://gazebosim.org/docs)
- [ros2_control](https://control.ros.org/)
- [Colcon](https://colcon.readthedocs.io/)

