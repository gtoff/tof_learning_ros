# Lidar ROS node
https://github.com/robopeak/rplidar_ros

# Steps for installation

## On ubuntu:

### Install ros (Jade) 
according to this guide:
http://wiki.ros.org/jade/Installation/Ubuntu

sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net:80 --recv-key 0xB01FA116
sudo apt-get update
sudo apt-get install ros-jade-desktop-full

sudo rosdep init
rosdep update

echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
source ~/.bashrc

sudo apt-get install python-rosinstall

### Create a ROS Workspace

mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace

### Clone the ROS node for the Lidar in the catkin workspace src dir
git clone https://github.com/robopeak/rplidar_ros.git

### Build with catkin
cd ~/catkin_ws/
catkin_make

### Set environment when build is complete
source devel/setup.bash

### Launch demo with rviz
roslaunch rplidar_ros view_rplidar.launch

# Troubleshooting
If you get permission errors on accessing the USB device with ROS take a look here:
http://question2722.rssing.com/browser.php?indx=42655234&last=1&item=4

Run this:
sudo gpasswd --add ${USER} dialout

Then logout of your system and log in again.


# Other notes

### Show topics
rostopic list -v

### Record selectively
rosbag record -O subset /turtle1/cmd_vel /turtle1/pose

### Replay
rosbag play subset.bag

### Setup remote master address to connect to another node
Guide: http://wiki.ros.org/ROS/Tutorials/MultipleMachines

export ROS_MASTER_URI=http://<master_ip>:11311

VERY IMPORTANT: /etc/hosts on both machines have to allow resolution by name (e.g., over the VPN)
If not the master node will log: Couldn't find an AF_INET address for [client_hostname]

Once you fix that rostopic echo should work.

### Check topics
rostopic list

### Listen
rostopic echo /topic_name

### 

# Setup openvpn

### Server
sudo apt-get install openvpn easy-rsa
openvpn --genkey --secret static.key
sudo openvpn /etc/openvpn/static-office.conf

### server/client conf example
https://openvpn.net/index.php/open-source/documentation/miscellaneous/78-static-key-mini-howto.html

Remeber to open ports on FW: port 1194 on UDP or TCP depending on choice.
sudo cp /usr/share/doc/openvpn/examples/sample-config-files/static-office.conf /etc/openvpn/

### Simulation with gazebo + navigation

#### Gazebo
export TURTLEBOT_GAZEBO_MAP_FILE=/opt/ros/indigo/share/turtlebot_gazebo/maps/playground.yaml
export TURTLEBOT_GAZEBO_WORLD_FILE=/opt/ros/indigo/share/turtlebot_gazebo/worlds/playground.world
roslaunch turtlebot_gazebo turtlebot_world.launch
#### Mapping
roslaunch turtlebot_gazebo gmapping_demo.launch
#### View with rviz
roslaunch turtlebot_rviz_launchers view_navigation_tof.launch
#### Move robot with keyboard
roslaunch turtlebot_teleop keyboard_teleop.launch

# show graph of ROS nodes
rqt_graph


