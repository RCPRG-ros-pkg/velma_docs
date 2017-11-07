---
layout: task
title: Example system
category: lab
lab: 1
task: 3
brief: An example system.
---

## About the example

The presented system consists of multiple agents and it can realize complex tasks,
such as environment exploration, exerting forces on environment, grasping and manipulation.
The presented structure is common for both simulated and real HW setup.

## Structure of the system

The structure of the control system is dependent on tasks realized by robot.
However, some parts are common for different tasks.
Agents used in this example system are:

* *velma_core* - embodied agent, the robot hardware and its low-level controller.
* *velma_task* - computational agent; ROS interface between *velma_core* and other agents.
* *octomap* - computational agent that processes data acquired from kinect (*velma_core_vr_kinect*) and virtual effector (*velma_core_ve_body*)
and builds occupancy map of environment represented as octomap; the map contains information about both explored and occupied space, and unexplored space.
* *visual_recognition* - computational agent that processes data acquired from kinect and virtual effector to recognize and localize objects in environment.
* *planner* - computational agent used for geometric planning of robot motion in joint space.
* *task_executor* - computational agent that acquires high level information about the state of robot and environment, and commands the robot to
realize a specified task.

![]({{site.baseurl}}/public/img/agents_example.dot.png)
*Example of the control system of Velma with common agents (velma_core, velma_task) and task specific agents*

Communication between *velma_task*, *octomap*, *visual_recognition*, *planner* and *task_executor* agents is realized using ROS communication methods
(i.e. topics, actions and services).
Agents *velma_task*, *octomap* and *planner* are implemented as ROS nodes and can be easily accessed from *task_executor*:

* through Python API, if *task_executor* is written in Python,
* through ROS topics and services, if *task_executor* is written in another language.

Agent *visual_recognition* is strictly dependent on task, so it can be implemented in many ways.
In simulation, the agent can use exact pose data obtained from simulator to emulate visual localization algorithms.

## Documentation for API, tools and tutorials

Detailed documentation of *velma_task*, *octomap*, *visual_recognition* and *planner* agents is presented in section
[Python interfaces]({{ site.baseurl }}{% post_url 2017-10-13-02_api_tools_01_python_interface %}).

