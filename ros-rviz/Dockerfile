FROM ros:melodic

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -qq \
    &&  apt-get install -y xserver-xorg-video-all \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN apt-get update -qq \
    && apt-get install -y libusb-1.0-0 dkms libssl-dev libusb-1.0-0-dev linux-headers-generic \
    && apt-get -y install mesa-utils libgl1-mesa-glx libgl1-mesa-dri \
    && apt-get install -yf \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN apt-get update -qq && apt-get install -y \
    ros-melodic-jackal-simulator \
    ros-melodic-jackal-control \
    ros-melodic-jackal-viz \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

COPY ps4_teleop.launch /opt/ros/melodic/share/jackal_control/launch/

VOLUME /root/



CMD ["roslaunch", "turtlebot_gazebo", "turtlebot_world.launch"]
