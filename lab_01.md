# Lab 1: Getting Started with ROS and the FR3

The goal of this lab is to jump-start your course project by getting you started with ROS2 Humble and the Franka Research 3 (FR3) robot.
You will:
- Install and configure a ROS2 environment
- Install basic FR3 packages
- Run a simulation of the FR3 with Ignition Gazebo
- Run motion planning for the FR3 through MoveIt
- Go through MoveIt tutorials to get acclimated to the MoveIt infrastructure

We will be following along with the readme from the `franka_ros2` repository:
https://github.com/frankaemika/franka_ros2

If you are ever confused by ROS, take a look at the tutorials: https://docs.ros.org/en/humble/index.html
Note that you are not required to use ROS for the course project, it is merely an option that is useful for most projects.

## Step 1: Installation of ROS2 and franka_ros2

There are four options for setting up ROS2:
- Native installation with Ubuntu 22.04
- Virtual machine
- Docker container
- Robostack Conda environment

### Native or Virtualized Installation
For native installation or a virtual machine, please follow the official documentation provided by ROS: https://docs.ros.org/en/humble/Installation.html

Then follow the instructions available at: https://github.com/frankaemika/franka_ros2?tab=readme-ov-file#local-machine-installation

### Docker

You should first install Docker for your system: https://www.docker.com/

For the container solution, please follow the documentation provided by the `franka_ros2` repository: https://github.com/frankaemika/franka_ros2?tab=readme-ov-file#docker-container-installation

There are also base container images for ARM architectures available here: https://github.com/sloretz/ros_oci_images
If you have to go this route, you will have to follow the native installation instructions in the base image.

Be careful to pay attention if the operating system image or Docker container is for your CPU architecture!
New Mac computers have an ARM architecture, not x86.

### Robostack Installation

This is a virtual environment solution built on top of `conda`
Install `conda`/`mamba`/`micromamba` first.
I recommend micromamba: https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html

Robostack is a set of prebuilt conda packages for a variety of ROS environments: https://robostack.github.io/

To build the `franka_ros2` repo, you'll have to follow these steps:
```
# Create environment and activate
micromamba create -n ros_humble -c conda-forge -c robostack-staging ros-humble-desktop
micromamba activate ros_humble

# Install dev tools
micromamba install -c conda-forge compilers cmake pkg-config make ninja colcon-common-extensions catkin_tools rosdep

# Install dependencies
micromamba install -c robostack-staging -c conda-forge -y ros-humble-backward-ros ros-humble-control-msgs ros-humble-controller-interface ros-humble-generate-parameter-library ros-humble-realtime-tools ros-humble-controller-manager ros-humble-hardware-interface ros-humble-ros2-control-test-assets ros-humble-joint-state-publisher ros-humble-joint-state-broadcaster ros-humble-xacro ros-humble-ros-gz ros-humble-sdformat-urdf ros-humble-joint-state-publisher-gui ros-humble-ros2controlcli ros-humble-hardware-interface-testing libignition-gazebo6 libignition-plugin1 ros-humble-moveit-ros-move-group ros-humble-moveit-kinematics ros-humble-moveit-planners-ompl ros-humble-moveit-ros-visualization ros-humble-joint-trajectory-controller ros-humble-moveit-simple-controller-manager pinocchio poco=1.11

# Make and clone relevant repositories into ROS workspace
mkdir -p ~/franka_ros2_ws/src
cd ~/franka_ros2_ws  # not into src
git clone https://github.com/frankaemika/franka_ros2.git src
git clone --recursive https://github.com/frankaemika/libfranka src/libfranka
git clone https://github.com/frankaemika/franka_description src/franka_description
git clone https://github.com/ament/ament_lint.git src/ament_lint

# Build packages
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release

# Source installation, good to go!
source install/setup.bash
```

## Step 2: Testing Installation

To launch the Gazebo simulator and see the robot:
```
ros2 launch franka_gazebo_bringup visualize_franka_robot.launch.py load_gripper:=true franka_hand:='franka_hand'
```

After you've tested Gazebo, test if MoveIt works with:

```
ros2 launch franka_fr3_moveit_config moveit.launch.py robot_ip:=dont-care use_fake_hardware:=true
```
Follow the tutorials to test if your MoveIt works and plan for some motions of the robot: https://moveit.picknik.ai/main/doc/tutorials/quickstart_in_rviz/quickstart_in_rviz_tutorial.html


## Step 3: Your first MoveIt Project

Follow the tutorial available: https://moveit.picknik.ai/main/doc/tutorials/your_first_project/your_first_project.html
Specifically, try to complete the tutorial about planning around objects: https://moveit.picknik.ai/main/doc/tutorials/planning_around_objects/planning_around_objects.html

Note that the code for all tutorials is available here: https://github.com/moveit/moveit2_tutorials/tree/main/doc/tutorials
This is an exercise to get you familiar with creating ROS2 packages and using the C++ MoveIt interface.

# Grading

To get full completion credit for this project, post a video of the robot navigating around an obstacle in RViz, using the code from the "planning around objects" tutorial.

