---
name: 3D Object Detection and Pose Estimation
tools: [C++, ROS, Pytorch, PCL, OpenCV]
image: https://jiasenzheng.github.io/assets/detection.gif
description: Designed a computer vision setup and implemented software to detect objects and estimate their poses in 3D space.
---

# Point Cloud Object Detection and Pose Estimation
<br>
### Objective
<br>
**Shape-Based Remote Manipulation (NSF)**: We are building a system, consisting of an operating interface coupled with a manipulator, promises a communication across the distance barrier. 
<br>
**My Focus**: As the first stage of this project, I am responsible to develop a perception system to sense the manipulator’s environment.
<br>
**Goal**: This project aims to design a computer vision setup and implement software to detect objects and estimate their poses in 3D space.
<br>
### Video demo
{% include elements/video.html id="uvhC8gluA9o" %}
<br>

### Hardware
* Intel Realsense Lidar L515

Intel realsense L515 uses a solid-state Lidar to sense depth. The Lidar can provide organized point clouds. It also has an RGB camera which is calibrated with the Lidar.

### Pipeline
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/det_pipeline.png"/>
<br>

### Software
The software of this project can be explained in two stages based on the two ROS nodes I have in the project:
* Inference server
* Perception core

**Inference server**<br>
The inference server is a ROS node that runs a deep learning model (CNN) to detect objects in the image space. The inference server is implemented in Python using [Detectron2](https://github.com/facebookresearch/detectron2) and Pytorch as the deep learning framework. For the use case of this project, a custom dataset is created to train the model. Mask-RCNN is used as the model architecture since we need to implement instance segmentation for our purposes. The model was then trained with 100 self-labeled images with an ontology of a "cheez-it" box. The labelled images are shown below:
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/annotation_results.gif"/>
<br>
The images are annotated on an open-sourced plattform, [CVAT](https://github.com/opencv/cvat). CVAT provides many tools to accelerate the labeling processes and improve the qualities, such as machine learning models. 
<br>
The model was then trained using Detectron2 with 100 labeled images. The inference results are shown below:
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/instance_segmentation.gif"/>
<br>
The inference server subscribes to the image topic and compute the target object masks. The inference server then publishes the masks to the perception core node.

**Perception core**<br>
The perception core is a ROS node that performs the following tasks:
* Receives the point cloud data from the LiDAR
* Receives the target object masks from the inference server
* Performs the following tasks:
    * Colorize the point cloud data
    * Perform 3D object detection
    * Perform 3D object pose estimation
    * Publish the results to the ROS topic

The perception core is implemented in C++ using ROS and PCL as the main libraries. The perception core subscribes to the point cloud data and the target object masks. The point cloud data is then colorized using the target object masks. The colorized point cloud data is then used to perform 3D object detection and pose estimation. The results are then published to the ROS topic.

### Future Scope 
