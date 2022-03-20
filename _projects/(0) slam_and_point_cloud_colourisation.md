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
3D Simultaneous localization and mapping (SLAM) is a broadly used technology in moblie robots to sense the environment and localize the robot. RTAB-Map is one of the most popular packages for 3D SLAM. The package is capable of constructing 3D map with real time performance and optimizing the model based on close loop detections. In this project, RTAB-map uses both sensor data from a Velodyne LiDar and a Realsense RGB-D camera as input. A Jackal UGV was used as the mobile platform. 

Another task in this project to map color pixels from a camera to the point cloud sensed by LiDar. Coloured point clouds can not only provide a more realistic visulization, but also add another type of information to point cloud analysis. To colorise the point cloud by referencing image, an approach to obtain the extrisic parameters which are transforms from one sensor device to another is important. Thus, a calibration package was developed to compute the extrisic parameters for devices sensing point cloud messages.
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
<img src="{{ site.url }}{{ site.baseurl }}/assets/velo1.jpg" style="height: 300px; width:425px;"/>


### Software
The project pipeline includes three stages.

**Stage 1 - 3D SLAM Implementation:**
<br>
The Jackal robot is equipped with a LiDar and a RGB-D camera. The lidar provides 3D point cloud messages to the mapping algorithm, while the camera provides both rgb images and depth-registerd images.  


**Stage 2 - Calibration Package:**



**Stage 2 - Colourisation algorithm Design:**





### Results


### Future Scope 




<p class="text-center">
{% include elements/button.html link="https://github.com/JiasenZheng/3d_slam_colored" text="Github: Colorized Cloud pkg" %}
{% include elements/button.html link="https://github.com/JiasenZheng/velo2rs_calibration" text="GitHub: Calibration pkg" %}
</p>