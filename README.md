# docker4turtlebot

Copy the rule into /etc/udev/rules.d and run the command:
```
  sudo udevadm trigger --action=change
```

## Networking

Create a docker network:
```
docker network create -d bridge ros_network
```

Launch the TurtleBot server:
```
docker run --rm -it --net=ros_network --name turtlebot \
  --env ROS_HOSTNAME=turtlebot \
  --env ROS_MASTER_URI=http://turtlebot:11311 \
  robinlab/turtlebot:melodic
```

Launch the TurtleBot teleoperation client:
```
docker run --rm -it --net=ros_network --name client \
  --env ROS_HOSTNAME=client \
  --env ROS_MASTER_URI=http://turtlebot:11311 \
  robinlab/turtlebot:melodic roslaunch turtlebot_teleop keyboard_teleop.launch
```
