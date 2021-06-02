---
title: Installation
category: Installation
order: 1
brief: Installation
---

# Setup of Velma robot controller

This page describes how to set up workspace for WUT Velma robot simulation. The build procedure
was tested in Ubuntu 18.04 with ROS melodic. The details on workspace dependencies are provided
during the set-up procedure. All dependencies should be satisfied on machines in labs 012 and P109.
In case of any errors please contact the maintainer. Also, the workspaces are installed on all
machines in P109 in /opt directory.

## Organisation of workspaces

The software for WUT Velma robot control system uses a concept of stack of catkin workspaces.
The full stack of workspaces is (each workspace extends the previous one):
* workspace for ROS melodic (usually in /opt/ros/melodic)
* ws_gazebo - workspace for Gazebo simulator and some stuff it uses
* ws_orocos - workspace for Orocos
* ws_fabric - workspace for FABRIC
* ws_velma_os - workspace for WUT Velma specific software

## Building from sources

Clone branch melodic-devel of this repo:

```bash
git clone -b melodic-setup-working https://github.com/RCPRG-ros-pkg/RCPRG_rosinstall.git
```

Change current working directory to the downloaded folder:

```bash
cd RCPRG_rosinstall
```

For documentation of all arguments type:

```bash
./setup.sh --help
```

Source ROS melodic:

```bash
source /opt/ros/melodic/setup.bash
```

Now you can run the setup script. The script has several arguments. To build all required workspaces run the following command:

```bash
./setup.sh -x /opt/ros/melodic -d ~/velma -b Release -g -o -f -v
```

The command will install 4 workspaces: for Gazebo, Orocos, FABRIC and Velma in directory velma in your home folder. The whole setup procedure may take several hours and consume lots of RAM.

If the build procedure consumes all your RAM, then you should limit the number of CPUs using e.g. parameter: -- -j 2

```bash
./setup.sh -x /opt/ros/melodic -d ~/velma -b Release -g -o -f -v -- -j 2
```

You can now source to the built workspace with command:

```bash
source ~/velma/ws_velma_os/devel/setup.bash
```
