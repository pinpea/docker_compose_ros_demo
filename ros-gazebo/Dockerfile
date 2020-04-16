FROM ubuntu
MAINTAINER github/pinpea

ARG ssh_prv_key
ARG ssh_pub_key

ENV DEBIAN_FRONTEND=noninteractive 
RUN apt-get update
RUN apt-get install -y mesa-utils
RUN apt-get install -y xserver-xorg-video-all
RUN apt-get install -y gnupg2

#For ROS Melodic
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu bionic main" > /etc/apt/sources.list.d/ros-latest.list'
RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
RUN apt-get update
RUN apt install -y ros-melodic-desktop-full
RUN apt install python-rosdep
RUN /bin/bash -c "source /opt/ros/melodic/setup.bash; rosdep init; rosdep update"

#Set up non-ROS dependencies


#Set up the custom_jackal packages 


# Downloading the Gazebo models
RUN mkdir -p ~/.gazebo/
RUN wget https://bitbucket.org/osrf/gazebo_models/get/e6d645674e8a.zip -P ~/.gazebo
RUN unzip ~/.gazebo/e6d645674e8a.zip -d ~/.gazebo/
RUN mv ~/.gazebo/osrf-gazebo_models-e6d645674e8a ~/.gazebo/models/

#Environment setup

#To prevent  Error in REST request ;  libcurl: (51) SSL: no alternative certificate subject name matches target host name 'api.ignitionfuel.org'
COPY ignition_conf.yaml /root/.ignition/fuel/config.yaml
# COPY control.yaml /opt/ros/melodic/share/jackal_control/config/control.yaml

RUN /bin/bash -c "echo source /opt/ros/melodic/setup.bash  >> ~/.bashrc"
RUN /bin/bash -c "source /opt/ros/melodic/setup.bash"
CMD ["roslaunch", "jackal_gazebo", "jackal_world.launch"]


