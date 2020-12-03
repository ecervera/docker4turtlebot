FROM ros:kinetic-ros-base

RUN apt-get update && apt-get install -y \
      python-catkin-tools \
    && rm -rf /var/lib/apt/lists/*
    
ENV ROS_WS /opt/ros_ws
RUN mkdir -p $ROS_WS/src
WORKDIR $ROS_WS

RUN git -C src clone https://github.com/turtlebot/turtlebot.git
RUN git -C src clone https://github.com/turtlebot/turtlebot_msgs.git

RUN git -C src clone https://github.com/yujinrobot/kobuki_msgs.git
RUN git -C src clone --single-branch --branch kinetic https://github.com/yujinrobot/kobuki.git \
 && cd src \
 && mv kobuki/kobuki_description kobuki/kobuki_node \
       kobuki/kobuki_keyop kobuki/kobuki_safety_controller \
       kobuki/kobuki_bumper2pc kobuki/kobuki_rapps . \
 && rm -rf kobuki

RUN git -C src clone https://github.com/yujinrobot/yujin_ocs.git \
 && cd src \
 && mv yujin_ocs/yocs_cmd_vel_mux yujin_ocs/yocs_controllers . \
 && rm -rf yujin_ocs

RUN apt-get update && apt-get install -y \
      ros-kinetic-urdf ros-kinetic-xacro libyaml-cpp-dev \
      ros-kinetic-ecl-threads ros-kinetic-ecl-streams \
      ros-kinetic-tf ros-kinetic-angles ros-kinetic-joy \
      ros-kinetic-diagnostic-updater ros-kinetic-kobuki-driver \
    && rm -rf /var/lib/apt/lists/*

RUN catkin config \
      --extend /opt/ros/$ROS_DISTRO && \
    catkin build

RUN apt-get update && apt-get install -y \
      ros-kinetic-laptop-battery-monitor \
      ros-kinetic-diagnostic-aggregator \
      ros-kinetic-robot-state-publisher \
    && rm -rf /var/lib/apt/lists/*

ADD turtlebot_entrypoint.sh /turtlebot_entrypoint.sh
ENTRYPOINT ["/turtlebot_entrypoint.sh"]
CMD ["roslaunch", "turtlebot_bringup", "minimal.launch"]
