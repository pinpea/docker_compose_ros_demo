# Notes on Docker-compose for ROS

```
sudo groupadd docker
sudo gpasswd -a $USER docker
newgrp docker   #(or log out and log in again)

docker run hello-world #to test docker runs without sudo
```

```bash
xhost +
```

ENV DEBIAN_FRONTEND=noninteractive

## Utilising Graphics for using Gazebo/RViz

## Open source (e.g., Intel/AMD )

On host
install mesa

### Driver mis-match between host and container

Running a GUI glxgears or gazebo (i.e., utilising graphics) gives an output, but with the bellow error. In the case of Gazebo, the programme crashes when more intensive tasks, such as loading the laser scanner in RVIZ, are required.

```bash
libGL error: pci id for fd 4: 1002:694c, driver (null)
libGL error: No driver found
libGL error: failed to load driver: (null)
libGL error: pci id for fd 4: 1002:694c, driver (null)
pci id for fd 5: 1002:694c, driver (null)
libGL error: failed to create dri screen
libGL error: failed to load driver: radeonsi
```

This error is caused by a version mismatch between libmesa drivers on the host and the container, using the below command on both systems:

```bash
dpkg -s libgl1-mesa-glx | grep Version
```

Currently, I am not sure what to do to solve this issue, since I also use a melodic Docker container, which also require matching libGL versions. I will proceed using only ROS melodic / Ubuntu 18.04 for now...

### NVIDIA

[Looks like a good blog post](http://moore-mike.com/docker-ros.html)

## SSH keys for private repos

ARG ssh_prv_key
ARG ssh_pub_key

# Add the keys and set permissions

RUN echo "$ssh_prv_key" > /root/.ssh/id_rsa && \
    echo "$ssh_pub_key" > /root/.ssh/id_rsa.pub && \
 chmod 600 /root/.ssh/id_rsa && \
 chmod 600 /root/.ssh/id_rsa.pub

#could also use copy
RUN mkdir -p ~/catkin_ws
RUN git clone --recursive git@github.com:git_user/git_repo.git ~/catkin_ws

## Other Useful commands

```bash
#stop all containers:
docker kill $(docker ps -q)

#remove all containers
docker rm $(docker ps -a -q)

#remove all docker images
docker rmi $(docker images -q)
```

## Debugging setup

#To prevent Error in REST request ; libcurl: (51) SSL: no alternative certificate subject name matches target host name 'api.ignitionfuel.org'
COPY ignition_conf.yaml /root/.ignition/fuel/config.yaml

    from https://api.ignitionfuel.org
    to url: https://api.ignitionrobotics.org
