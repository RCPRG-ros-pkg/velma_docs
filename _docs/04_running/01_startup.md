---
title: Running the system
category: Running
order: 1
brief: 
---

This section contains information about various tools for runing the control system of Velma.





# Running the simulated system

**Command**

```bash
roslaunch velma_common velma_system.launch
```

**Description**

The launch file runs the whole system with robot and environment simulated in Gazebo.

**Important notice:** Before every start-up of the simulated system, shared memory channels must be reset
using command described [below](#cleanup-of-shared-memory-communication-channels).
Omission of such cleanup can occasionally cause a deadlock between pair of subsystems.







# Cleanup of shared memory communication channels

**Command**

```bash
rosrun velma_common reset_shm_comm
```

**Description**

This script removes all shared memory files, in `/dev/shm/` directory, related to control system of Velma.
This cleanup is needed before every start-up of the system.




# Running initialization and homing procedure

**Command**

```bash
rosrun velma_task_cs_ros_interface initialize_robot.py
```

**Description**

After every startup, the robot have to be initialized with `initialize_robot.py` script.
The script changes state of hardware controllers of motors to *enabled*. Also, all motors that
require homing are homed.








# Geometric planner for Velma

**Command**

```bash
roslaunch velma_ros_plugin velma_planner.launch
```

**Description**

This launch file runs ROS node with geometric planner for Velma.
The planner can be accessed using ROS services. There is a convenient Python interface
for those services in *rcprg_planner* package.

Please refer to the [test scripts](#test-scripts) and to Python API page for examples of use.








# Test scripts

**Command**

```bash
rosrun velma_task_cs_ros_interface <script_name>
```

**Description**

The scripts test some basic performance of the whole system:
 * `test_head.py` - movement of head only,
 * `test_head_complex.py` - movement of head combined with movement of arms and torso; tests of behavior of head controller in situations of fault and behavior switch,
 * `test_safe_col.py` - tests for behavior for self collision avoidance,
 * `test_jimp.py` - simple movement of arms and torso,
 * `test_grippers.py` - simple movement of gripper fingers,
 * `test_jimp_planning.py` - movement of arms and torso using trajectory generated by planner,
 * `test_jimp_planning_attached.py` - planning movement of arms and torso with virtual object attached to gripper,
 * `test_cimp_pose.py` - simple movements of end effectors using control in 6-DOF task space,
 * `test_cimp_tool.py` - changing the tool frame of end effectors,
 * `test_cimp_imp.py` - changing the impedance of end effectors,
 * `test_all.py` - runs all tests in sequence.

Please refer to the sources of the scripts as examples of high level control of Velma robot.

Some scripts require the [planner node](#planner-for-velma).





# Creating a snapshot of simulation state

**Example command**

```bash
rosrun rcprg_gazebo_utils create_state_snapshot
```

**Description**

This script creates snapshot of the current state of all joints in simulation and saves it to file `state_snapshot.txt` in the current working directory.
The snapshot can be restored, together with saved world using argument `state_snapshot` in launch
file `velma_system.launch` in package `velma_common` or in launch file `velma_gazebo_re.launch` in package `velma_sim_gazebo`.

Below, there is an example usage of state snapshot saving and loading.
We assume, that the system is running, and the world state is stable (e.g. no fast moving objects, no robot motion),
and current working directory is `/home/user/`.

**Saving the current state**

First, we create a snapshot:
```bash
rosrun rcprg_gazebo_utils create_state_snapshot
```
The above command creates a file `state_snapshot.txt` in the current working directory.
Then, in Gazebo Client, save the world using *File -> Save world as*.

**Restoring the saved state**

To run the simulation starting from the saved state, execute command:
```bash
roslaunch velma_common velma_system.launch world_name:=/home/user/saved.world state_snapshot:=/home/user/state_snapshot.txt spawn_velma:=false
```
Please note that full paths are required. In the above example, we assume, that world was saved in file `/home/user/saved.world`.
The third argument `spawn_velma:=false` tells the launch file that model of Velma robot is already in the saved world file, and it should not be
spawned from urdf file.

