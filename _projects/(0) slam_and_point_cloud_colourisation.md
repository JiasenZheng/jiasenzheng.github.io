---
name: 3D Slam and Point Cloud Colourisation
tools: [C++, ROS, 3D SLAM, Clibration, PCL]
image: https://jiasenzheng.github.io/assets/slam1.gif
description: Performed 3D SLAM using RTAB-Map on a Jackal UGV and align the color pixel to the point cloud; developed a calibration ROS package to compute the extrinsic parameters between a LiDar and a RGB-D camera.
---

# 3D Slam and Point Cloud Colourisation
<br>
### Brief overview
<br>
3D Simultaneous localization and mapping (SLAM) is a broadly used technology in moblie robots to map the environment and localize the robot. RTAB-Map is one of the most popular packages for 3D SLAM. The package is capable of constructing 3D map with real time performance and optimizing the model based on close loop detections. In this project, RTAB-map uses both sensor data from a Velodyne LiDar and a Realsense RGB-D camera as input. A Jackal UGV was used as the mobile platform. 

Another task in this project is to align color pixels from a camera to the point cloud sensed by LiDar. Colourized point clouds can not only provide a more realistic visulization, but also add another type of information to point cloud analysis. To colorise the point cloud by referencing image, an approach to obtain the extrinsic parameters which are transforms from one sensor device to another is important. Thus, a calibration package was developed to compute the extrinsic parameters for devices sensing point cloud messages.
<br>
### Video demo
{% include elements/video.html id="YdkDaCQD1MY" %}
<br>

### Hardware
* Jackal UGV mobile platform 
* Intel RealSense camera D435i
* Velodyne Puck LiDar sensor (VLP-16)
* PS4 Controller

The LiDar and camera are positioned on the top front of the Jackal UGV as shown in the following picture.
<img src="{{ site.url }}{{ site.baseurl }}/assets/jackal1.png" style="height: 300px; width:400px;"/>


### Software
The project pipeline includes three stages.

**Stage 1 - 3D SLAM Implementation:**
<br>
The Jackal robot is equipped with a LiDar and a RGB-D camera. The lidar provides 3D point cloud messages to the mapping algorithm, while the camera provides both rgb images and depth-registerd images. The flow chart below shows the important messages used in rtabmap with our configuration.
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/rtab1.png"/>

Rtab-map uses loop closure detection to optimize the map. The loop closure detector uses a bag-of-words approach to determinate how likely a new image comes from a previous location or a new location. The easiest way to accomplish a loop closure is to control the robot rotate by itself. As shown in the following gif, when the robot spinning around, both the point cloud map and grid map grows and become more certain about the environment.
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/closure1.gif"/>
The navigation stack is accomplished by move base ROS package which links together a global and local planner and publish a twist control message to move the robot to the desired goal position. The move base works well when the robot has a certain grid map. As shown in the following gif, the robot is implementing navigation stack. Also, the move base package has many configuration parameters, a good navigation requires a lot tunning of those various parameters.
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/nav1.gif"/>
**Stage 2 - Calibration Package:**
<br>
A calibration ROS package is developed to find the extrinsic parameters between the velodyne LiDar and Realsense camera. The point cloud colourisation algorithm is accomplished by the transformation from point cloud 3D coordinates to image pixel coordinates. Thus, an accurate extrinsic parameter is curial to obain a good alignment between point clouds and images. Since the Realsense camera provides point cloud messages created by an active infrared stereo, the calibration task becames easier as we only need to align two clusters of point clouds from two sensors. The package can also used to calibrate the extrinsic between two sensors which receive point clouds, such as two LiDars. The target object reqired for this project can be any rectgular prism or rectgular board. The calibration algorithm requires three corner coordinates from one face of the target object. Two ROS services are used to find and store the reference points. The following image shows an example to find one reference point from LiDar point cloud data.
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/calib7.png"/>
<br>
The green cloud indicates the timestamped lidar data, and the red square region is given by a ROS service with specific coordinates and the length of the square. Another ROS service is used to find the corner coordinates of the target region. A more detailed instruction can be found in the [README](https://github.com/JiasenZheng/velo2rs_calibration) file of the calibration package repository. 

<br>
**Stage 3 - Colourisation algorithm:**
<br>
The colorisation algorithm is designed based on rigid body transforms and direct linear transforms. After performing the calibration package, we can obtain the transformation from LiDar to camera. The point clouds collected by Lidar is originally in the LiDar frame ("/velodyne"). By applying rigid body transform, the LiDar point clouds can be transformed to the camera frame ("/camera_depth_optical_frame"). 

The direct linear transform (DLT) is a technique localize and calibrate a camera jointly. Normally, the camera model has 11 degrees of freedom to be calibrated, which are 6 extrinsic parameters and 5 intrinsic parameters. Since each Realsense camera comes with pre-calibrated intrinsic parameters, and those can be accessed by "/camera_info" topic of the camera, we can focus on computing the extrinsic parameters. The equation of DLT can be expressed as follow.
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/dlt1.png"/>
<br>
The left side of the equation is the homogenous representation of pixel coordinates in the camera plane, while the right side consists of λ (the value of Z after 3D coordinates have been transformed), k (the intrinsic matrix),R (the rotation matrix), t(the translation matrix), and the homogenous representation of point cloud coordinates in camera frame.

After the point cloud coordinates been mapped to the pixel coordinates in camera plane, a threshold is set to obtain only the point that can be received by the field of view of the camera. The RGB values is then added to those point cloud and publish as XYZRGB point clouds in the camera frame. 


### Results and Analysis
<br>
**Calibration:**<br>
The calibration package was tested in a simulation environment in Gazebo, where the extrinsic parameters are specified in the URDF files of our model. The ground truth transformation from the LiDar to thr camera in [XYZ(meters)RPY(radians)]  is [0.195,0.000,-0.128,-1.571,0.000,-1.571], while the calibrated extrinsic parameters gives a transform of [0.219, 0.023, -0.114, -1.566, 0.000, -1.581]. The errors are calculated to be  [0.024,0.023,0.014,0.005,0.000,-0.01].

The tranlation errors are significantly larger than the rotation error. This is because the timestamped lidar point cloud can not provide a high resolution point cloud data, especialy the gap between each laser scans. As a result, the capture reference point may not be the cornor of the plane of our target object. Moreover, the Gazebo simulation has little noise applied to those sensor data, while in reality, both sensor has higher noises. Typically the depth messages obtained by the Realsense camera have larger noises than the velodyne LiDar. 


<br>
**Colourisation:**<br>
The colorisation algorithm can be run and displayed in real time. The following gif shows the demostration of the colorised clouds with real time performance.
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/color1.gif"/>
<br>
The following two pictures show more detailed results each frame with a cheese board and a trash bin.
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/color1.png"/>
<br><br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/color1.png"/>
<br>



### Future Scope 




<p class="text-center">
{% include elements/button.html link="https://github.com/JiasenZheng/3d_slam_colored" text="GitHub: colorized cloud pkg" %}
{% include elements/button.html link="https://github.com/JiasenZheng/velo2rs_calibration" text="GitHub: calibration pkg" %}
</p>