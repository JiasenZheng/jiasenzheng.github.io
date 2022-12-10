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
* Receives both the point cloud from the LiDAR and the image from the RGB camera
* Detects the table in point cloud data and remove it
* Denoises, downsamples, and clusters the point cloud data to create a foreground mask
* Receives the target object masks from the inference server and get a infrence mask
* merge the foreground mask and the inference mask to get a finak mask
* Performs oriented bounding box detection and pose estimation on the final mask with a state machine

The perception core is implemented in C++ using ROS. The node uses PCL for point cloud process and OpenCV for image process. The organized point cloud acquired from the realsense Lidar helped to significantly reduce the runtime of the perception core. The algorithms in the node were implemented to keep the organized point cloud using a customized data structure, **OrderedCloud**. Moreover, some algorithm were implemented in image space to reduce the runtime such as **denoise** and **cluster**. As a result, the node can be run effeciently with real-time performance. A state machine is used to control the detection and pose estimation process based on the motion in the image space.

The below image shows the table removal process:
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/background_remover.gif"/>
<br>

The below image shows the colorized point cloud clusters after denoising, downsampling, and clustering:
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/cluster.gif"/>
<br>

The below image shows the CAD model replacement of the target objects:
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/3d_object_detection.gif"/>
<br>


### Future Scope 
* Implement a faster CNN model for instance segmentation such as [YOLACT-EDGE](https://github.com/haotian-liu/yolact_edge) to reduce the inference time
* Annotate more images to train a more robust and accurate model
* Annotate the model based on the cropped images to reduce the inference time
* Call the inference server with cropped images to reduce the inference time
* Implement a more robust and accurate pose estimation algorithm such as point cloud registration
* Multi-thread CPU implementation on the perception core node to reduce the runtime
* Implement some OpenCV and PCL algorithms in GPU to reduce the runtime
