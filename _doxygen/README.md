WUT Velma Python interfaces                         {#mainpage}
============

# Introduction

Documentation describes high level python interface to control system of Velma robot.

The robot can be controlled using class defined in velma_interface.py.

Motion planning is done through planner python interface defined in planner.py.

Integration tests are presented in module @ref integration_tests and they can be used:
 * to evaluate performance and correctness of the whole system,
 * as tutorials on the interfaces.

Additionally, @ref utilities provide useful scripts and ROS launch files.

Brief descriptions of some catkin packages are in [Related pages](pages.html) tab.
It contains sources for ROS launch files in each package. Please refer to launch files sources to
get familiar with setting parameters and running the system and various useful tools.

The tab [Modules](modules.html) groups python executable scripts into @ref integration_tests and @ref utilities.

# Running the system

    roslaunch velma_sim_gazebo velma_gazebo_re.launch

# Installation
Please follow the steps described in section [Set up: Velma (latest version)](https://github.com/RCPRG-ros-pkg/RCPRG_rosinstall/wiki/Set-up:-Velma-(latest-version)).
The simulator is provided in both versions: `latest` and `latest_hw`, but if You don't want to run real hardware, install the simulation-only version (`latest`).

# Running the simulator
The setup.bash script of the workspace must be sourced in each terminal.
Usually is is *setup.bash* script in devel ot install space of top level workspace.

## Running the simulated system:
1. Initialize ROS:

        roscore

2. Run subsystems of *velma_core* and *velma_task* agents - two ways are possible:
    * each subsystem individually (on a single machine):
        - RE of simulated Velma robot:

                roslaunch velma_sim_gazebo velma_gazebo_re.launch

         Please refer to documentation of @ref velma_sim_gazebo_readme for details about launch file arguments.
        - VE of simulated Velma robot:

                roslaunch velma_core_ve_body velma_core_ve_body.launch

         Please refer to documentation of @ref velma_core_ve_body_readme for details about launch file arguments.
        - CS of core agent:

                roslaunch velma_core_cs velma_core_cs.launch

         Please refer to documentation of @ref velma_core_cs_readme for details about launch file arguments.
        - CS of task agent:

                roslaunch velma_task_cs_ros_interface velma_task_cs_ros_interface.launch

         Please refer to documentation of @ref velma_task_cs_ros_interface_readme for details about launch file arguments.
    * or all subsystems together:

                roslaunch velma_common velma_system.launch

     Please refer to @ref velma_common_readme for details about launch file arguments.

## Running the real HW system:
1. Turn on main power supply. It is located in P109 near the main door.

2. Turn on lwr-controller PC loctated in server room.

3. Turn on both KUKA hardware controllers (in the server room, below lwr-controller PC) and wait till they are fully initialized.

4. On both KCP (KUKA Control Panel):

  1. Click Ackn. All.

  2. From menu Configure choose 6 set tool/base, set tool no. parameter to 2 and click OK.

  3. In the file browser select file fri_start and choose Select.

  4. Switch the Mode Selector key to position Automatic (upper-left).

  5. Click button Enable drives (the button next to the Mode Selector key).

  6. Run the script by pushing several times the Start Key button (the green button with + sign). All three indicators at the bottom of the screen should be green.

5. The following commands should be executed in multiple terminals. In each terminal, workspace configuration file should be sourced:

        source path_to_workspace/top/devel/setup.bash

 and ROS master URI should be set to lwr-controller:

        export ROS_MASTER_URI=http://lwr-controller:11311

6. Execute the following commands on lwr-controller using multiple SSH connections:

  1. Initialize ROS:

          roscore

  2. Run real effector of torso, neck and grippers (devices connected to Ethercat):

          roslaunch velma_ec_driver velma_ec_re.launch

  3. Run real effector of right manipulator:

          roslaunch velma_core_re_lwr velma_re_lwr_r.launch

  4. Run real effector of left manipulator:

          roslaunch velma_core_re_lwr velma_re_lwr_l.launch

  5. Run virtual effector of whole body:

          roslaunch velma_core_ve_body velma_core_ve_body.launch cset:=true

   the cset roslaunch argument indicates that the subsystem should run on single, predefined cpu.

  6. Run control subsystem of the core agent:

          roslaunch velma_core_cs velma_core_cs.launch cset:=true

   the cset roslaunch argument indicates that the subsystem should run on single, predefined cpu.

  7. Run ROS interface of the control subsystem of the task agent:

          roslaunch velma_task_cs_ros_interface velma_task_cs_ros_interface.launch


## Running additional tools:
* robot state visualization tools:
  * rviz to see the state of the robot:

          rosrun rviz rviz

   You have to add RobotModel and some relevant ROS topics to see the state of the robot. The documentation is available [here](http://wiki.ros.org/rviz).

  * visualization of self collision geometric shapes and interactions between them:

          roslaunch velma_common show_collisions.launch

   Remember to add appropriate ROS topic in rviz.

  * visialization of camera frustum for kinect

          roslaunch velma_common show_kinect_frustum.launch

   Remember to add appropriate ROS topic in rviz.

  * rqt utility to see the state of the subsystems:

          rosrun rqt_agent rqt_agent

* Gazebo-specific tools:
  * Gazebo client to see the state of simulator:

          roslaunch rcprg_gazebo_utils gazebo_client.launch

  * ROS node that published pose of a specified object to tf ROS topic:

          roslaunch rcprg_gazebo_utils gazebo_publish_ros_tf_object.launch

  * world editor:

          roslaunch rcprg_gazebo_utils gazebo_world_editor.launch

   Please note that this launch file runs new Gazebo server, so all other servers should be closed.

  * ROS node that adds object to simulated environment during simulation:

          roslaunch rcprg_gazebo_utils spawn_object.launch

 Please refer to @ref rcprg_gazebo_utils_readme for details about launch file arguments and other launch files.
* planner for Velma robot:

        roslaunch planner planner.launch

 Please refer to @ref planner_readme for details about launch file arguments.

* octomap server:
  * online octomap server and point cloud filter:

          roslaunch velma_common octomap_server.launch

  * offline octomap server:

          roslaunch velma_common octomap_offline_server.launch octomap_file:=<filename>

 Please refer to @ref velma_common_readme for details about launch file arguments and other launch files.


Please see [Related pages](pages.html) for more launch files.

