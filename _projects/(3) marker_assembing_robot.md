---
name: Marker Assembling Robot
tools: [ROS, Python, OpenCV, Moveit, Smach]
image: https://github.com/JiasenZheng/portfolio_images/blob/main/marker.gif?raw=true
description: Assembled markers and caps through pick, place, press, and sort operations sequences. 
---

# Marker Assembling Robot 
<br>
### Brief overview
<br>
The project aims to assemble markers and caps through pick, place, press, and sort operations sequences. The intent was largely inspired by the application of robots in manufacturing and industry. Our project used a RealSense camera to detect the markers' colors and MoveIt manipulation commands to actuate the robot. Franka-specific actions also were used to grip caps and markers during movement. The framework of the project was controlled using a state machine developed in the ROS package called SMACH. The state machine intelligence implemented sorting of colors by hue based on camera data coming from the RealSense perception subsystem. Intelligence then leveraged the manipulation to pick, place and press caps and markers in the assembly tray. 

### Video demo
{% include elements/video.html id="SXJP4yIiKOU" %}
<br>
### Collaborations
* Jiasen Zheng (Preception & 3D Modeling)
* Kojo Welbeck
* Ian Kennedy
* Bhagyesh Agresar
* Keaton Griffith

<br>
### Manipulation
<br>
The manipulation package relies on several different `nodes` in order to function:
1. **manipulation_cap** provides low-level position and orientation sensing services, along with error recovery, movements, and gripper grasping
2. **manipulation_macro_a** provides position movement services for image captures using the RealSense
3. **manipulation_press** provides a pressing service to cap the markers
4. **manipulation_local** provides manipulation services for moving in between trays
5. **manipulation_pnp** provides pick and place services between the feed and assembly trays
6. **debug_manipulation** logs the external forces experienced by the robot
7. **plan_scene** provides a planning scene for simulation-based motion planning in Moveit
8. **limit_set** provides services to be used with the franka_control file launched prior to Moveit being launched. It allows the user to reconfigure the collision limits on the robot. 


Manipulation also relies on a python **manipulation** package with translational, array position, and verification utilities.
A scene. yaml file is used for specifying parameters in the plan_scene node and the main manipulation movements scene elsewhere in the project.

### Perception
<br>
* All the computer vision algorithms are embedded in the **vision** python package, functions in the package can be called in a node by 
```import <package name>.<script name>``` such as 
```shell
import vision.vision1
```
* **sample_capture.py**:  A helper python script to capture images using RealSense 435i RGB-D camera
    1. Connect the RealSense camera to your laptop
    2. Run the python script in a terminal:
    ```shell
    python3 sample_capture.py
    ```
    3. Press 'a' to capture and save an image and use 'q' to quit the image window
* **hsv_slider.py**: A helper python script to find the appropriate HSV range for color detection
    1. Add the path of the image to  `frame = cv. imread()` to read the image
    2. Run the python script in a terminal:
    ```shell
    python3 hsv_slider.py
    ```
    3. A window of the original image and a window of HSV image with slide bars will show up
    4. Test with HSV slide bars to find an appropriate range
* **vision.py** A python script to detect contours and return a list of hue values
    1. For testing purposes, an image can be loaded by setting the path to `image = cv. imread()`
    2. Run the python script in a terminal:
    ```shell
    python3 vision1.py
    ```
    3. A processed image with contours and a list of hue values will be returned

    The node that uses this library is called vision_bridge.
* **vision_bridge node**: Node that publishes a stream of ROS Images and implements a service **capture** that returns a list of H values of the detected markers and caps from an image.
    1. run `rosservice call /capture` and specify the **tray_location** to run the service.
        * tray_location 1 : Represents Assembly Location
        * tray_location 2 : Represents Markers Location
        * tray_location 3 : Represents Caps Location


<p class="text-center">
{% include elements/button.html link="https://github.com/JiasenZheng/Marker_Assembling_Robot" text="GitHub" %}
</p>
