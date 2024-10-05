# Install and Setup
Pull the ros2 humble desktop image

`
sudo docker pull osrf/ros:humble-desktop
`

Create an container and mount the local work space, at the same time mount x11 display forward

`
xhost +local:root
`

`
sudo docker run -it --name mbt_ros2 --gpus all -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v ~/work_space/ros2_workspace:/work_space osrf/ros:humble-desktop bash
`

Check the existance

`
sudo docker ps -a
`

Start it again

`
sudo service docker start
`

`
sudo docker start -ai mbt_ros2
`

Remove x11 forward with concern of safety (will not be able to display afterward)

`
xhost -local:root
`

Install terminator

`
apt update
apt install -y terminator
mkdir -p ~/.config/terminator/
touch ~/.config/terminator/config
sudo apt install at-spi2-core
apt install -y libcanberra-gtk-module
`

Install ros2 dependencies

`
apt update && apt install -y \
ros-$ROS_DISTRO-gazebo-ros-pkgs \
ros-$ROS_DISTRO-rviz2 \
ros-$ROS_DISTRO-navigation2 \
ros-$ROS_DISTRO-rqt \
ros-$ROS_DISTRO-rqt-common-plugins \
ros-$ROS_DISTRO-ros2-control \
ros-$ROS_DISTRO-ros2-controllers \
&& apt clean
`

Ros2 dependency management

`
sudo apt install python3-rosdep
rosdep init
rosdep update
sudo apt dist-upgrade
rosdep install --from-paths src --ignore-src --rosdistro $ROS_DISTRO -y
sudo apt install python3-colcon-common-extensions
sudo apt install python3-colcon-mixin
colcon mixin add default https://raw.githubusercontent.com/colcon/colcon-mixin-repository/master/index.yaml
colcon mixin update default
sudo apt install python3-vcstool
`

PAL dependencies

`
mkdir -p /opt/tiago_public_ws/src
cd /opt/tiago_public_ws
vcs import --input https://raw.githubusercontent.com/pal-robotics/tiago_tutorials/humble-devel/tiago_public.repos src
rosdep install --from-paths src -y --ignore-src --rosdistro humble
colcon build --symlink-install

echo "source /opt/tiago_public_ws/install/setup.bash" >> /opt/env.sh

echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
echo "source /opt/tiago_public_ws/install/setup.bash" >> ~/.bashrc
`
