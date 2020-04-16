FROM osrf/ros:melodic-desktop-full
MAINTAINER github/pinpea

ARG ssh_prv_key
ARG ssh_pub_key
ARG git_repo


# Dependencies for graphics 
ENV DEBIAN_FRONTEND=noninteractive 
RUN apt-get update -qq && apt-get install -y \
    mesa-utils \
    xserver-xorg-video-all \
    gnupg2 

#Set up non-ROS dependencies - gdb is for move_base_flex, wget is to get Gazebo models

RUN apt-get update -qq && apt-get install -y \
    git\
    ssh \
    openssh-client \
    vim \
    gdb \
    wget \
    python-catkin-tools  

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# RUN mkdir -p -m 0600 ~/.ssh && ssh-keyscan github.com >> ~/.ssh/known_hosts
# Downloading the Gazebo models
RUN mkdir -p ~/.gazebo/
RUN /bin/bash -c "echo 'Get models for gazebo, wget set to quiet, THIS MAY TAKE SOME TIME'; wget https://bitbucket.org/osrf/gazebo_models/get/e6d645674e8a.zip --quiet --directory-prefix ~/.gazebo" 
RUN /bin/bash -c "echo 'Unzipping Gazebo models'; unzip -qq ~/.gazebo/e6d645674e8a.zip -d ~/.gazebo/"
RUN mv ~/.gazebo/osrf-gazebo_models-e6d645674e8a ~/.gazebo/models/

#Set up the custom_jackal packages 
# Authorize SSH Host
RUN mkdir -p /root/.ssh && \
    chmod 0700 /root/.ssh && \
    ssh-keyscan github.com > /root/.ssh/known_hosts

# Add the keys and set permissions
RUN echo "$ssh_prv_key" > /root/.ssh/id_rsa && \
    echo "$ssh_pub_key" > /root/.ssh/id_rsa.pub && \
    chmod 600 /root/.ssh/id_rsa && \
    chmod 600 /root/.ssh/id_rsa.pub

# Set up catkin repo 
RUN mkdir -p ~/catkin_ws
RUN echo "RUN FROM HERE PLEASE"
#https://smartystreets.com/blog/2018/09/private-dependencies-in-docker-and-go/
RUN git config --global url."git@github.com:".insteadOf https://github.com/
RUN git clone -b simulation --recursive "$git_repo" ~/catkin_ws/src/
# RUN /bin/bash -c "cd ~/catkin_ws/src/ ; git checkout simulation"

#Install package dependencies
RUN apt-get update -qq
RUN /bin/bash -c "cd ~/catkin_ws; rosdep install --from-paths src --ignore-src -r -y"
# Add missing dependency
RUN apt-get install -y ros-melodic-eband-local-planner

RUN apt-get clean && rm -rf /var/lib/apt/lists/*



#Environment setup

#To prevent  Error in REST request ;  libcurl: (51) SSL: no alternative certificate subject name matches target host name 'api.ignitionfuel.org'
COPY ignition_conf.yaml /root/.ignition/fuel/config.yaml

RUN /bin/bash -c "cd ~/catkin_ws/ ; source /opt/ros/melodic/setup.bash;  catkin init ; catkin build"

# CMD ["roslaunch", "jackal_gazebo", "jackal_world.launch"]


