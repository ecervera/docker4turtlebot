# docker4turtlebot

Copy the rule into `/etc/udev/rules.d` and run the command:
```
  sudo udevadm trigger --action=change
```

## Networking

Create a docker network:
```
docker network create rosnet
```

Launch the TurtleBot server:
```
docker run --rm -it --net=rosnet --name turtlebot \
  --env ROS_HOSTNAME=turtlebot \
  --env ROS_MASTER_URI=http://turtlebot:11311 \
  robinlab/turtlebot:melodic roslaunch turtlebot_bringup minimal.launch
```

Launch the TurtleBot teleoperation client:
```
docker run --rm -it --net=rosnet --name client \
  --env ROS_HOSTNAME=client \
  --env ROS_MASTER_URI=http://turtlebot:11311 \
  robinlab/turtlebot:melodic roslaunch turtlebot_teleop keyboard_teleop.launch
```
