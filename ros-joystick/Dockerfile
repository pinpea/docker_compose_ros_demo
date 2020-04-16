FROM ros:melodic

RUN apt-get update -qq && apt-get install -y \
    ros-melodic-jackal-control \
    ros-melodic-joy \
    ros-melodic-teleop-twist-joy \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# COPY ps4_teleop.launch /opt/ros/melodic/share/jackal_control/launch/
#COPY control.yaml /opt/ros/melodic/share/jackal_control/control_config/control.yaml

RUN /root/

