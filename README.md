# TurtleBot3 Project 

The gazebo launch file, I launch the gazebo with the world and also launch the extrinsic camera and intrinsic camera with this file so i dont have to do it in seperate terminals.

```
<launch>
  <env name="GAZEBO_RESOURCE_PATH" value="$(find turtlebot3_gazebo)/models/autorace/ground_picture" />

  <arg name="x_pos" default="1.589"/>
  <arg name="y_pos" default="-0.4832"/>
  <arg name="z_pos" default="0.175280"/>
  <arg name="yaw_angle" default="1.562509" />  

  <include file="$(find gazebo_ros)/launch/empty_world.launch">
    <arg name="world_name" value="$(find turtlebot3_gazebo)/worlds/Phase_c_lights.world" />
    <arg name="paused" value="false"/>
    <arg name="use_sim_time" value="true"/>
    <arg name="gui" value="true"/>
    <arg name="headless" value="false"/>
    <arg name="debug" value="false"/>
  </include>  

  <param name="robot_description" command="$(find xacro)/xacro --inorder $(find turtlebot3_description)/urdf/turtlebot3_burger_for_autorace_2020.urdf.xacro" />
  <node pkg="gazebo_ros" type="spawn_model" name="spawn_urdf" args="-urdf -model autorace -x $(arg x_pos) -y $(arg y_pos) -z $(arg z_pos) -Y $(arg yaw_angle) -param robot_description" />
   
  <include file="$(find turtlebot3_autorace_camera)/launch/extrinsic_camera_calibration.launch">
  </include>  
  <include file="$(find turtlebot3_autorace_camera)/launch/intrinsic_camera_calibration.launch">
  </include>
</launch>
```

## Note:
- Right now I have it configured so initially the robot has to be in the outmost lane then it goes to the innermost lane when it sees a cone, you could change this easily whenever you want, by just changint them in the cone_decider node.

# How to run the project:
- The main launch file is the traffic_cone.launch inside the turtlebot3_autorace_detect launch files.
_ This launch file will run 2 things, one is the detect_cone node, the other is the cone_decider node which handles the mode stuff. The detect_cone will detect the cone and publish on a topic control/mode which is subbed to by the cone_decider node.
- The cone decider node has the logic for the lane change and all the stuff.

# Parameters that you will need to change:
- Lane detection params, lane.yaml inside param/lane/ in turtlebot3_autorace_detect.
- Cone detection params, cone.yaml inside param/cone/ in the same package, if this is not working i did a temporary change in the cbGetDetectConeParams function you can put your parameters here for now after calibrating the cone detection. The image is called /image/cone/compressed in rqt. Also if you detect cones sometimes when there are none you can change the filters in the fnFindCircleOfCone function.

Let me know if you have any questions.
- 


