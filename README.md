# docker4turtlebot

Copy the rule into `/etc/udev/rules.d` and run the command:
```
  sudo udevadm trigger --action=change
```

## Networking containers in a single host

Create a docker network:
```
docker network create rosnet
```

Launch the TurtleBot server:
```
docker run --rm -it --net=rosnet --name turtlebot \
  --env ROS_HOSTNAME=turtlebot \
  --env ROS_MASTER_URI=http://turtlebot:11311 \
  --env TURTLEBOT_BATTERY=None \
  --device=/dev/kobuki:/dev/kobuki \
  robinlab/turtlebot:kinetic roslaunch turtlebot_bringup minimal.launch
```

Launch the TurtleBot teleoperation client:
```
docker run --rm -it --net=rosnet --name client \
  --env ROS_HOSTNAME=client \
  --env ROS_MASTER_URI=http://turtlebot:11311 \
  robinlab/turtlebot:kinetic roslaunch turtlebot_teleop keyboard_teleop.launch
```

## Networking containers across multiple hosts (in the same network)

Based on [the Docker docs](https://docs.docker.com/network/network-tutorial-overlay/#use-an-overlay-network-for-standalone-containers).

1. In external PC
```
docker swarm init
```
(keep the token step 2)

2. In TurtleBot
```
docker swarm join --token <TOKEN> <IP-ADDRESS-OF-MANAGER>:2377
```

3. In external PC
```
docker network create -d overlay --attachable rosnet
```

4. In TurtleBot
```
docker run --rm -it --net=rosnet --name turtlebot \
  --env ROS_HOSTNAME=turtlebot \
  --env ROS_MASTER_URI=http://turtlebot:11311 \
  --env TURTLEBOT_BATTERY=None \
  --device=/dev/kobuki:/dev/kobuki \
  robinlab/turtlebot:kinetic roslaunch turtlebot_bringup minimal.launch
```

5. In external PC
```
docker run --rm -it --net=rosnet --name client \
  --env ROS_HOSTNAME=client \
  --env ROS_MASTER_URI=http://turtlebot:11311 \
  robinlab/turtlebot:kinetic roslaunch turtlebot_teleop keyboard_teleop.launch
```
