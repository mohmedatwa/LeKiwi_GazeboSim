# LeKiwi_ROS

A ROS 2 stack for the LeKiwi mobile manipulator: URDF description, Gazebo simulation, ros2_control, and MoveIt 2 motion planning.

## Overview

LeKiwi is a mobile manipulator that combines:

- A **3-wheel omni-directional base** driven by `OmniWheelDriveController`
- A **5-DOF SO-ARM100 arm** with a parallel gripper
- **Gazebo Sim** integration via `ros_gz_sim` and `gz_ros2_control`
- **MoveIt 2** configuration for arm and gripper planning

## Requirements

- [ROS 2](https://docs.ros.org/) (Humble or later recommended)
- Ubuntu 22.04 or 24.04
- Python 3.10+
- CMake 3.8+
- [Colcon](https://colcon.readthedocs.io/)

### ROS 2 dependencies

Install the core ROS 2 packages used by this workspace:

```bash
sudo apt update
sudo apt install -y \
  ros-${ROS_DISTRO}-xacro \
  ros-${ROS_DISTRO}-robot-state-publisher \
  ros-${ROS_DISTRO}-joint-state-publisher-gui \
  ros-${ROS_DISTRO}-rviz2 \
  ros-${ROS_DISTRO}-ros2-control \
  ros-${ROS_DISTRO}-ros2-controllers \
  ros-${ROS_DISTRO}-controller-manager \
  ros-${ROS_DISTRO}-ros-gz-sim \
  ros-${ROS_DISTRO}-ros-gz-bridge \
  ros-${ROS_DISTRO}-gz-ros2-control \
  ros-${ROS_DISTRO}-moveit \
  ros-${ROS_DISTRO}-moveit-ros-move-group \
  ros-${ROS_DISTRO}-moveit-configs-utils \
  ros-${ROS_DISTRO}-warehouse-ros-mongo
```

You may also need the `omni_wheel_drive_controller` plugin if it is not already in your workspace.

## Installation

Clone the repository into your ROS 2 workspace, then build and source it:

```bash
cd ~/lekiwi_ws/src
git clone https://github.com/mohmedatwa/LeKiwi_ROS.git
cd ~/lekiwi_ws
colcon build --symlink-install
source install/setup.bash
```

## Usage

### Full simulation (Gazebo + controllers + RViz)

Starts Gazebo Sim, spawns the robot, loads ros2_control controllers, and opens RViz:

```bash
ros2 launch lekiwi_bringup lekiwi.launch.py
```

Drive the base by publishing to `/lekiwi_controller/cmd_vel` (`geometry_msgs/msg/Twist`).

### Visualize the robot model (no simulation)

Opens RViz with a joint-state GUI for manual joint control:

```bash
ros2 launch lekiwi_description display.launch.py
```

### Gazebo simulation only

```bash
ros2 launch lekiwi_description lekiwi_gz.launch.py
```

### Controllers only

Useful when simulation is already running:

```bash
ros2 launch lekiwi_controller lekiwi_control.launch.py
```

This spawns:

| Controller | Type | Purpose |
|---|---|---|
| `joint_state_broadcaster` | Joint state broadcaster | Publishes joint states |
| `lekiwi_controller` | Omni wheel drive | Base velocity control and odometry |
| `so100_controller` | Joint trajectory | Arm motion |
| `gripper_controller` | Gripper action | Gripper control |

### MoveIt 2

Run the MoveIt demo (Move Group + RViz, no Gazebo):

```bash
ros2 launch lekiwi_moveit demo.launch.py
```

Other useful launch files:

```bash
ros2 launch lekiwi_moveit move_group.launch.py
ros2 launch lekiwi_moveit moveit_rviz.launch.py
ros2 launch lekiwi_moveit setup_assistant.launch.py
```

Planning groups:

- `so100` — 5-DOF arm
- `gripper` — gripper joint

Predefined arm states: `home`, `pose1`.

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

## License

This project is licensed under the Apache License 2.0. See [LICENSE](LICENSE) for details.

## References

- [ROS 2 Documentation](https://docs.ros.org/)
- [MoveIt 2](https://moveit.picknik.ai/)
- [Gazebo Sim](https://gazebosim.org/docs)
- [ros2_control](https://control.ros.org/)
- [Colcon](https://colcon.readthedocs.io/)
