# Perception-of-RX200-using-AR-Tag
This repository discusses in detail the steps to use the April Tag and Kinect camera to get the transformation between the Kinect camera and RX200 robot. 

This work is done with the help of my student: Chenghao Lin

## Step 1: Install the AprilTag library and ROS April Tag packages using the following links:

https://github.com/AprilRobotics/apriltag_ros

https://github.com/AprilRobotics/apriltag?tab=readme-ov-file#install


## Step 2: Modify the camera topics to match the Kinect camera instead of Realsence.
 In this file "/apriltag_ros/apriltag_ros/launch/continuous_detection.launch", change camera topics as follows:

    <remap from="image_rect" to="kinect2/hd/image_color_rect" />

    <remap from="camera_info" to="kinect2/hd/camera_info" />

## Step 3: Specify the AR Tag family 

Set the tag_family in this file "/apriltag_ros/apriltag_ros/config/settings.yaml"

tag_family:   'tagStandard41h12' 

Other options for tag_family: tagStandard52h13, tagStandard41h12, tag36h11, tag25h9, tag16h5, tagCustom48h12, tagCircle21h7, tagCircle49h12

In our case, it is "tagStandard41h12" based on the outer square shape and the 5 cell length of the inner shape of the Air tag as defined in the following link:

https://github.com/AprilRobotics/apriltag_ros?tab=readme-ov-file

<div align="center">
   <img src="https://github.com/AmnaMazen/Perception-of-RX200-using-AR-Taging-A/blob/main/AR%20TAg.jpeg" width="300" height="300">
</div>



## Step 4: Copy tag Id from interbotics_ws and place it inside the AR Tag package

Copy "interbotix_ws/src/interbotix_ros_toolboxes/interbotix_perception_toolbox/interbotix_perception_modules/config/tags.yaml":

standalone_tags:
  [
    {id:    5, size: 0.02},
    
    {id:  413, size: 0.02},
    
    {id:  820, size: 0.02},
    
    {id:  875, size: 0.02},
    
    {id: 1050, size: 0.02}
  ]

  And add it to the following file "/apriltag_ros/apriltag_ros/config/tags.yaml"

  To generate more April Tags, you can use the following link: https://github.com/AprilRobotics/apriltag-generation

  ## Step 5: Detect the orientation of the April tag and make the x-axis match the x-axis of the arm 

  roslaunch '/home/chenghao/interbotix_ws/src/apriltag_ros/apriltag_ros/launch/continuous_detection.launch'

  The x-axis is in red 

<div align="center">
   <img src="https://github.com/AmnaMazen/Perception-of-RX200-using-AR-Taging-A/blob/main/orientation_AR.png" width="300" height="300">
</div>

  ## Step 6: Get the transformation between the rx200 and Kinect camera


roslaunch interbotix_xsarm_perception xsarm_perception.launch robot_model:=rx200 use_armtag_tuner_gui:=true

We need to change the camera topic inside "/home/chenghao/interbotix_ws/src/interbotix_ros_manipulators/interbotix_ros_xsarms/interbotix_xsarm_perception/launch/xsarm_perception.launch" to match kinect camera not realsense camera


  <arg name="camera_frame"                      default="kinect2_rgb_optical_frame"/>

  <arg name="camera_color_topic"                default="kinect2/hd/image_color_rect"/>

  <arg name="camera_info_topic"                 default="kinect2/hd/camera_info"/>


We set the num Samples to 10 and press snap pose. This automatically adds the transformation to the transformation tree. 

<div align="center">
   <img src="https://github.com/AmnaMazen/Perception-of-RX200-using-AR-Taging-A/blob/main/ARM_transformation.png" width="300" height="300">
</div>
  
