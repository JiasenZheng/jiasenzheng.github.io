---
name: Slam and Point Cloud Colourisation
tools: [C++, ROS, 3D SLAM, Clibration, Direct Linear Transform]
image: https://jiasenzheng.github.io/assets/velo1.jpg
description: Implemented 3D SLAM using RTAB-Map on a Jackal UGV and align the color pixel to the point cloud. Developed a calibration ROS package to compute the extrisic parameters between a LiDar and a RGB-D camera.
---

# Slam and Point Cloud Colourisation
<br>
### Brief overview
<br>
3D Simultaneous localization and mapping (SLAM) is a broadly used technology in moblie robots to sense the environment and localize the robot. RTAB-Map is one of the most popular packages for 3D SLAM. The package is capable of constructing 3D map with real time performance and optimizing the model based on close loop detections. In this project, RTAB-map uses both sensor data from a Velodyne LiDar and a Realsense RGB-D camera as input. A Jackal UGV was used as the mobile platform. 

Another task in this project to map color pixels from a camera to the point cloud sensed by LiDar. Coloured point clouds can not only provide a more realistic visulization, but also add another type of information to point cloud analysis. To colorise the point cloud by referencing image, an approach to obtain the extrisic parameters which are transforms from one sensor device to another is important. Thus, a calibration package was developed to compute the extrisic parameters for devices sensing point cloud messages.

### Hardware
* Jackal mobile platform 
* Intel RealSense camera D435i
* Velodyne Puck LiDar sensor (VLP-16)

The LiDar and camera are position on the top front of the Jackal UGV as shown in the following picture.
![alt text]({{ site.url }}{{ site.baseurl }}/assets/velo1.png)


### Software


### Results


### Future Scope 




<p class="text-center">
{% include elements/button.html link="https://github.com/JiasenZheng/Stereo_Visual_Odometry" text="GitHub" %}
</p>