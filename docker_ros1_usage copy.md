# Install and Setup
Pull the ros Noetic desktop image

`
docker pull ros:noetic-desktop-full
`

Create an container and mount the local work space, at the same time mount x11 display forward

`
xhost +local:root
`

`
sudo docker run -it --name mbt_ros --gpus all -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v ~/work_space/ros2_workspace:/work_space ros:noetic-desktop-full bash
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

Install ros and PAL dependencies

`
apt update
apt upgrade -y
apt install ros-noetic-audio-common python3-numpy python3-opencv python3-testresources python3-empy python3-pykdl libasound-dev libportaudio2 libportaudiocpp0 portaudio19-dev ffmpeg ca-certificates curl gnupg bc -y
source /opt/ros/noetic/setup.bash
if [ -f "/opt/pal/gallium/setup.bash" ]; then
    source /opt/pal/gallium/setup.bash
else
    source /opt/tiago_public_ws/devel/setup.bash
fi
mkdir -p /overlay_ws/src
git clone https://github.com/locusrobotics/catkin_virtualenv /overlay_ws/src/catkin_virtualenv
cd /overlay_ws/src/catkin_virtualenv
git reset --hard 4af99703e6c7faf12c5d328dbab346250fd085f7
git clean -df
git fetch --depth=1
git clone https://github.com/ros-drivers/video_stream_opencv /overlay_ws/src/video_stream_opencv
cd /overlay_ws/src/video_stream_opencv
git reset --hard 65949bdc5c9468d18c51aed9073d020bec892532
git clean -df
git fetch --depth=1
cd /overlay_ws
catkin build
if [ -f "/opt/pal/gallium/setup.bash" ]; then
    sed -i '/source \/opt\/pal\/gallium\/setup.bash/a source \/overlay_ws\/devel\/setup.bash' /opt/env.sh
else
    sed -i '/source \/opt\/tiago_public_ws\/devel\/setup.bash/a source \/overlay_ws\/devel\/setup.bash' /opt/env.sh
fi
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
if [ -f "/opt/pal/gallium/setup.bash" ]; then
    echo "source /opt/pal/gallium/setup.bash" >> ~/.bashrc
    echo "source /overlay_ws/devel/setup.bash" >> ~/.bashrc
else
    echo "source /opt/tiago_public_ws/devel/setup.bash" >> ~/.bashrc
    echo "source /overlay_ws/devel/setup.bash" >> ~/.bashrc
fi
source ~/.bashrc
`

