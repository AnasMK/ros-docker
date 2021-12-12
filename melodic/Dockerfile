From osrf/ros:melodic-desktop-full
LABEL mantainer="Anas Knhouch <anas.m.kan@gmail.com>"

ARG DEBIAN_FRONTEND=noninteractive

# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES \
    ${NVIDIA_VISIBLE_DEVICES:-all}
ENV NVIDIA_DRIVER_CAPABILITIES \
    ${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics

ENV USER_NAME anas
ENV USER_HOME /home/$USER_NAME
ENV DEBIAN_FRONTEND noninteractive

RUN adduser --disabled-password --gecos '' $USER_NAME
RUN adduser $USER_NAME sudo
# Ensure sudo group users are not 
# asked for a password when using 
# sudo command by ammending sudoers file
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> \
    /etc/sudoers
RUN sed -i '/force_color_prompt/s/^#//g' ~/.bashrc
RUN sed -i '/force_color_prompt/s/^#//g' /home/$USER_NAME/.bashrc

USER $USER_NAME


RUN sudo apt-get update
RUN sudo apt-get upgrade -y
RUN sudo -S apt-get update && sudo apt-get -y --quiet --no-install-recommends install software-properties-common apt-utils
RUN sudo apt-get -y install build-essential cmake libgeographic-dev
RUN sudo apt-get -y install libgl1-mesa-glx libgl1-mesa-dri && \
    sudo rm -rf /var/lib/apt/lists/*

RUN curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
RUN sudo apt-get update
# Install needed ros packages
RUN sudo apt-get install -y ros-melodic-geographic-* ros-melodic-twist-mux ros-melodic-joy ros-melodic-joy-teleop ros-melodic-teleop-twist-joy ros-melodic-rviz-imu-plugin ros-melodic-turtlebot3-msgs ros-melodic-teleop-twist-keyboard ros-melodic-octomap ros-melodic-map-server ros-melodic-amcl ros-melodic-move-base ros-melodic-dwa-local-planner ros-melodic-teb-local-planner

RUN echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc

# Prepare catkin workspace
RUN mkdir -p $USER_HOME/catkin_ws/src
RUN /bin/bash -c '. /opt/ros/melodic/setup.bash; cd ~/catkin_ws; catkin_make' 
RUN echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc