# Perception-of-RX200-using-AR-Taging-A
This repository discusses in detail the steps to use the April Tag and Kinect camera to get the transformation between the Kinect camera and RX200 robot. 

## Step 1: Install the AprilTag library and ROS April Tag packages using the following links:

https://github.com/AprilRobotics/apriltag_ros

https://github.com/AprilRobotics/apriltag?tab=readme-ov-file#install


# Step 2: Modify the camera topics to match the Kinect camera instead of Realsence.
 In this file "/apriltag_ros/apriltag_ros/launch/continuous_detection.launch", change camera topics as follows:

    <remap from="image_rect" to="kinect2/hd/image_color_rect" />

    <remap from="camera_info" to="kinect2/hd/camera_info" />


