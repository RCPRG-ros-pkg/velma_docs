---
title: Installation
category: Installation
order: 1
brief: Installation
---

# Setup of Velma robot controller

This page describes how to set up workspace for WUT Velma robot simulation. There are two options:
* installation from a .deb package
* build from sources
Both were tested in Ubuntu 18.04 with ROS melodic.
All dependencies should be installed automatically during installation / build.
In case of any errors during set-up procedure please contact the maintainer.

The software with its all dependencies is installed on machines in labs 012 and P109 at WUT FEIT.


## Organisation of workspaces

The software for WUT Velma robot control system uses a concept of stack of catkin workspaces.
The full stack of workspaces is (each workspace extends the previous one):
* workspace for ROS melodic (usually in /opt/ros/melodic) - original ROS workspace
* ws_gazebo - workspace for Gazebo simulator and software it depends on
* ws_orocos - workspace for Orocos
* ws_fabric - workspace for FABRIC
* ws_velma_os - workspace for WUT Velma specific software


## Installation from package

Packages (.deb) are available at [.deb packages](https://cloud.robotyka.ia.pw.edu.pl/index.php/s/8H6CCC2Jac7sBE8).

To install the package, you have to install ros-melodic-desktop first (not ros-melodic-desktop-full). Please refer to [ROS installation instructions](http://wiki.ros.org/melodic/Installation/Ubuntu).

Update you system:
```bash
sudo apt update
sudo apt upgrade
```

Download the latest .deb package, and install it:
```bash
sudo dpkg -i velma-system_<version number>_amd64.deb
```
In the last step, install all dependencies and configure the installed packages:
```bash
sudo apt install -f
```

The procedure will install all woekspaces in /opt directory.
After the installation is complete, source to the workspace with command:

```bash
source /opt/ws_velma_os/setup.bash
```


## Building from sources

Clone branch melodic-devel of RCPRG_rosinstall repo:

```bash
git clone -b melodic-setup-working https://github.com/RCPRG-ros-pkg/RCPRG_rosinstall.git
```

Change current working directory to the downloaded folder:

```bash
cd RCPRG_rosinstall
```

Source ROS melodic:

```bash
source /opt/ros/melodic/setup.bash
```

Run the setup script. The script has several arguments.
The whole setup procedure may take several hours and consume lots of RAM.
Try to avoid consuming all RAM, as it may use swap and make the build very slow or hang the machine. You should limit the number of CPUs used for build:
* 32GB RAM, use up to 12 CPUs
* 16GB RAM, use up to 5 CPUs
* 8GB RAM, use up to 2 CPUs
To limit the number of CPUs to e.g. 4, use the parameter "-- -j 4".

To build all required workspaces, using 4 CPUs, run the following command:

```bash
./setup.sh -x /opt/ros/melodic -d ~/velma -b Release -g -o -f -v -- -j 4
```

The command will install 4 workspaces: for Gazebo, Orocos, FABRIC and Velma in directory velma in your home folder.
After the build is complete, source to the built workspace with command:

```bash
source ~/velma/ws_velma_os/devel/setup.bash
```
