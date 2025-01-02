# Jazzy

## Background

Docker containers by default run as the 'root' user.  This can create massive problems.

### Small Example

When creating ROS2 packages inside the container, all of the newly created files are owned by 'root'.  This prevented editing and deleting the files without using the 'sudo' command outside the container.

## Description

Creates a Docker container that uses a regular user and not the 'root' user.

## Docker Build

Build the container.

```bash
cd Docker
../build.sh
cd ..
```

## Docker Run

Run the container for the first time.

Running this twice in a row will generate an error.

```bash
./run.sh
```

## Docker Start

Restart the container that has already be run once.

```bash
./start.sh
```

## Docker Remove Image

If necessary:

```bash
docker rmi -f ros2_diff_drive_jazzy
```

## ROS2 Workspace

```bash
# Inside the container...
cd /ros2_diff_drive_ws/src

# Create a new package for the controller
ros2 pkg create --build-type ament_python --dependencies rclpy std_msgs geometry_msgs nav_msgs tf2 tf2_ros control_msgs sensor_msgs test_msgs launch_py --description "Generic diff drive controller" --maintainer-email BruceRayWilson42@gmail.com --license MIT diff_drive_controller_generic


# Create a new package for the PWM control
ros2 pkg create --build-type ament_python --dependencies rclpy std_msgs sensor_msgs control_msgs launch_py --description "Generic PWM controller for robotics applications" --maintainer-email BruceRayWilson42@gmail.com --license MIT pwm_controller_generic

# Build the workspace
## This must be done at the workspace directory level!
cd /ros2_diff_drive_ws
colcon build

```

### Colcon Build Output

```text
Starting >>> diff_drive_controller_generic
Starting >>> pwm_controller_generic
Finished <<< pwm_controller_generic [1.10s]                                                             
Finished <<< diff_drive_controller_generic [1.11s]

Summary: 2 packages finished [1.31s]
```

## Commit Changes

1. Exit container
2. git add .
3. git commit -am "After colcon build."

## Finished

All files are owned by the user and not 'root'.

## Docker Docs

[Dockerfile USER Command](https://docs.docker.com/engine/containers/run/#user)

## Ignore from here to the end

## Create Differential Drive Controller Node

## Create PWM Node

### Create pwm_controller_generic.py

Done.

### Next Steps

Build the package using colcon build.
Run the node using ros2 run pwm_controller_generic pwm_controller_generic.
Publish duty cycle values to the /duty_cycle topic using ros2 topic pub /duty_cycle std_msgs/msg/Float64 "data: 0.5".
Observe the PWM state on the /pwm_state topic using ros2 topic echo /pwm_state.
