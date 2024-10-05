# Install docker
Download docker and docker compose (only needed for managing multiple iamges)

`
sudo apt install docker.io docker-compose
`

Start docker service

`
sudo service docker start
`

List the running containers (-a: all (not just running ones); -q: id only; --filter: filter)

`
sudo docker ps
`

List existing (recognized) images

`
sudo docker images
`

Load image files from local

`
sudo docker load -i /path/to/image.tar
`

Import a snapshot

`
sudo docker import /path/to/filesystem.tar new_image_name:tag
`

Pull from Docker Hub or other online repositories

`
sudo docker pull repository_name:tag
`

restart docker

`
sudo systemctl restart docker
`

Install Nvidia container support

`
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
&& curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
&& curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
`

`
sudo apt update
sudo apt install -y nvidia-docker2
`

# Managing Images and Containers
Download images from Docker Hub (or other repositories)

`
sudo docker pull <image_name>
`

Start a container

`
sudo docker run -it --name <container_name> <image_name> bash
`

No interaction:

`
sudo docker run -d --name <container_name> <image_name>
`

Check all the containers (including stopped ones)

`
sudo docker ps -a
`

To stop a container

`
sudo docker stop <container_name_or_id>
`

To start a (stopped) container

`
sudo docker start <container_name_or_id>
`

To remove a container

`
sudo docker rm <container_name_or_id>
`

To remove all that are currently stopped

`
sudo docker container prune
`

To save a container as an image

`
sudo docker commit <container_name_or_id> <new_image_name>
`

To build a new image with regularized Dockerfile, 
create a Dockerfile with all the process to do, then

`
sudo docker build -t <new_image_name> .
`

Check the log of a container

`
sudo docker logs <container_name_or_id>
`

Check resource status

`
sudo docker stats
`

Delete all images, containers and networks being unused (doesn't feel safe to me)

`
sudo docker system prune
`

Force to remove a running container
`
sudo docker rm -f <container_name_or_id>
`

# Mounting Volumes to Images
Bind mounts: local directory to an image (say /home/user/myproject is the local directory, and /app is the one appears in the container)

`
sudo docker run -v /home/user/myproject:/app my_docker_image
`

Read only

`
sudo docker run -v /home/user/myproject:/app:ro my_docker_image
`

Named volumne (managed by docker itself, with a name, no need to be asscessed within the host)

`
sudo docker run -v my_data:/app/data my_docker_image
`

Check the volumes

`
sudo docker volume ls
`

Delete the volumes

`
sudo docker volume rm my_data
`

# Mounting Volumes to Containers
Need to stop the running containers first then create a new image from it, mount the volume at the same time

`
sudo docker stop <container_name_or_id>
`

`
docker commit <container_name_or_id> my_new_image
`

`
docker run -v /path/on/host:/path/in/container my_new_image
`

If just want to copy files between the host and container

`
docker cp /host/path <container_name_or_id>:/container/path
`

or the inverse

`
docker cp <container_name_or_id>:/container/path /host/path
`

Check details

`
docker inspect <container_name_or_id>
`

