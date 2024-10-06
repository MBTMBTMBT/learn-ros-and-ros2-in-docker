# Install and Setup
Pull the ros image

`
sudo docker pull palroboticssl/tiago_tutorials:noetic
`

Create an container and mount the local work space, at the same time mount x11 display forward

`
xhost +local:root
`

`
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)    && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -    && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
`

`
sudo apt-get update
sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker
`

`
sudo pip install rocker
`

`
sudo rocker palroboticssl/tiago_tutorials:noetic --nvidia --x11 --privileged --user --name mbt_ros --nocleanup --volume ~/work_space/ros_workspace:/work_space --port 2222:22
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
sudo docker start -ai mbt_ros
`

Remove x11 forward with concern of safety (will not be able to display afterward)

`
xhost -local:root
`

Source the tutorial workspace

`
cd /tiago_public_ws/
source ./devel/setup.bash
`

To launch the simulation of the TIAGo the parallel gripper, execute

`
roslaunch tiago_gazebo tiago_gazebo.launch public_sim:=true end_effector:=pal-gripper
`

SSH setup

`
apt update
apt install -y openssh-server
mkdir /var/run/sshd
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config
`

`
echo "root:password" | chpasswd
`

`
service ssh start
`

`
echo "/usr/sbin/sshd -D" >> ~/.bashrc
`
