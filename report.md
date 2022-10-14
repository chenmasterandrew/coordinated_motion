COSC.69.13
22F
Andrew Chen

# PA3
## Method
This program performs multi robot task allocation (MRTA) for three robots. Two robots have an associated follower node and one robot has an associated leader node. Each follower node registers itself with the leader node using the NewFollower service. As a result, the leader holds references to all three robots.

There are three pose_setter nodes, one associated with each robot. Each pose_setter node sends a message to the one broadcaster node containing that robot's initial pose. The broadcaster broadcasts this initial pose as a tf transform associated with the robot.

Next, the user publishes a custom Waypoints message to the /waypoints topic. The leader node is subscribed to this topic. When it receives a Waypoints message, the leader begins task allocation. It greedily assigns each robot to its nearest waypoint. Then, for follower robots, it uses the Allocate action to send the goal nearest waypoint to the follower node. Once the follower node receives this goal, it uses tf to convert this waypoint in world frame to the robot's frame. Then, it rotates in place and moves towards the converted waypoint. For the leader robot, the same process is done without the use of the Allocate action.

## Evaluation
As shown in the video, this program works in the gazebo simulator. Robot followers are correctly registered with the leader node. Robots will be allocated to the nearest waypoint and will move to those waypoints one robot at a time. However, depending on the movement speed/distance and turning speed/distance, robots may drift away from the waypoint coordinates. Sensible weights have been set in leader and follower nodes, and the program performs well with these parameters.