---
name: 3D Slam and Point Cloud Colourisation
tools: [C++, ROS, 3D SLAM, Clibration, PCL]
image: https://jiasenzheng.github.io/assets/slam1.gif
description: Performed 3D SLAM using RTAB-Map on a Jackal UGV and align the color pixel to the point cloud; developed a calibration ROS package to compute the extrisic parameters between a LiDar and a RGB-D camera.
---

# 3D Slam and Point Cloud Colourisation
<br>
### Brief overview
<br>
3D Simultaneous localization and mapping (SLAM) is a broadly used technology in moblie robots to map the environment and localize the robot. RTAB-Map is one of the most popular packages for 3D SLAM. The package is capable of constructing 3D map with real time performance and optimizing the model based on close loop detections. In this project, RTAB-map uses both sensor data from a Velodyne LiDar and a Realsense RGB-D camera as input. A Jackal UGV was used as the mobile platform. 

Another task in this project is to align color pixels from a camera to the point cloud sensed by LiDar. Colourized point clouds can not only provide a more realistic visulization, but also add another type of information to point cloud analysis. To colorise the point cloud by referencing image, an approach to obtain the extrisic parameters which are transforms from one sensor device to another is important. Thus, a calibration package was developed to compute the extrisic parameters for devices sensing point cloud messages.
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

**Stage 2 - Calibration Package:**
<br>
A calibration ROS package is developed to find the extrisic parameters between the velodyne LiDar and Realsense camera. The point cloud colourisation algorithm is accomplished by the transformation from point cloud 3D coordinates to image pixel coordinates. Thus, an accurate extrisic parameter is curial to obain a good alignment between point clouds and images. Since the Realsense camera provides point cloud messages created by an active infrared stereo, the calibration task becames easier as we only need to align two clusters of point clouds from two sensors. The package can also used to calibrate the extrisic between two sensors which receive point clouds, such as two LiDars. The target object reqired for this project can be any rectgular prism or rectgular board. Two ROS services are used to find and store the reference points from the target object. The following image shows an example to find one reference point from LiDar point cloud data.
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/calib7.png"/>
<br>


<br>
**Stage 2 - Colourisation algorithm Design:**





### Results


### Future Scope 




<p class="text-center">
{% include elements/button.html link="https://github.com/JiasenZheng/3d_slam_colored" text="GitHub: colorized cloud pkg" %}
{% include elements/button.html link="https://github.com/JiasenZheng/velo2rs_calibration" text="GitHub: calibration pkg" %}
</p>