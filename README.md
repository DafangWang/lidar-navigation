# Lidar-navigation

## Description:
This github contains multiple packages which enables a robot to navigate through a mapped environment with a Hokuyo UBG-04LX-F01 lidar. This lidar is tested with a Turtlebot 2. While launching the package it is possible to send 2D nav goals to the Turtlebot via RVIZ. The turtlebot will drive to the given goal using the lidar and the encoders of the turtlebot.

## Prerequisites:
- Ros Kinetic on Ubuntu 16.04 or higher
- Hokuyo ubg-04lx-f01 link:https://www.hokuyo-aut.jp/search/single.php?serial=164

## The packages:
#### Laser_proc:
The laser_proc package is used to convert the representations of sensor_msgs/LaserScan and sensor_msgs/MultiEchoLaserScan. For more information about this package, go to the developers repository: https://github.com/ros-perception/laser_proc
#### Lidar_navigation:
The lidar_navigation package is made for building a map and opening a created map. The created maps can be placed in the directory "/maps". There is a launch directory where the launchfiles for mapping and for driving through the mapped map are placed. The "lidar_gmapping_navigation.launch" in the launch directory is made for mapping. The "lidar_amcl_navigation.launch" is created to open the mapped map and open the other packages that are used for navigating through the map.
#### Turtlebot_lidar_bringup:
This package is used to start up the turtlebot
#### Turtlebot_lidar_description:
This package contains the descriptions of the turtlebot.
#### Turtlebot_lidar_navigation:
In this package all navigation parameters are defined. Like all costmap parameters and path planner parameters, these are defined in the directory "param", these parameters are defined in the .yaml files.
#### Turtlebot_lidar_rviz_launchers:
This package is used for opening RVIZ on your screen.
#### Urg_c:
This urg_c is a libary for the urg_node, more information can be found on this github repository: https://github.com/ros-drivers/urg_c
#### Urg_node:
The urg_node is a driver for Hokuyo lidar systems which makes it possible to use a Hokuyo sensor in ROS and in this driver the settings of the hokuyo lidar are defined. These can be configured in the "/cfg" directory. More information could be found on this github repository:https://github.com/ros-drivers/urg_node
#### Teb_local_planner:
This local planner will replace the DWA_local_planner in order to improve the behavior of navigation while facing dynamic obstacles, more information could be found on this repository: https://github.com/rst-tu-dortmund/teb_local_planner

## Installation:
The first step explains how to install the packages located in this repository and the next part shows how to launch the gmapping and amcl launch files. 

First go to the source of the catkin_ws and git clone this repository.
```
cd ~/catkin_ws/src
git clone https://github.com/Pim1997/lidar-navigation.git
```
Then execute this command:

```
chmod +x lidar-navigation/urg_node/cfg/URG.cfg
```
In order to use the teb_local_planner package, fill in the following command line:
```
rosdep install teb_local_planner
```
Finally build the packages.
```
cd ..
catkin_make
```
## Launching gmapping:
In order to launch gmapping the following lines should be executed in the terminal:
```
sudo chmod a+rw /dev/ttyACM0
```
This command is necessary to prevent this error: Could not open serial Hokuyo: /dev/ttyACM0 @ 115200 could not open serial device.

Next type in the following command:
```
roslaunch lidar_navigation lidar_gmapping_navigation.launch
```

To save the map use:
```
rosrun map_server map_saver -f <mapname>
```
In order to easily retreive the saved map, it is highly recommended to save the map in the maps directory, which can be found in the lidar_navigation package --> /catkin_ws/src/lidar_navigation/maps. Next go to /catkin_ws/src/lidar_navigation/launch, open the "lidar_amcl_navigation.launch" file and follow the next step.
```
<arg name="map_file" default= "$(find lidar_navigation)/maps/hallway.yaml"/>
```
Change hallway.yaml to <file name>.yaml in order to retreive the map
```
<arg name="map_file" default= "$(find lidar_navigation)/maps/<file name>.yaml"/>
```
## Launching amcl:
Now you should be able to launch amcl with the following command lines:

```
sudo chmod a+rw /dev/ttyACM0
roslaunch lidar_navigation lidar_amcl_navigation.launch
```
