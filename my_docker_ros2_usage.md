# Install and Setup
Pull the ros2 humble desktop image

`
sudo docker pull osrf/ros:humble-desktop
`

Create an container and mount the local work space

`
sudo docker run -it --name mbt_ros2 -v ~/work_space/ros2_workspace:/work_space  osrf/ros:humble-desktop bash
`

Check the existance

`
sudo docker ps -a
`

Start it again

`
sudo docker start -ai mbt_ros2
`
