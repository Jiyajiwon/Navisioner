![header](https://capsule-render.vercel.app/api?type=transparent&color=FFA73F&height=100&section=header&text=NAVISIONER&fontSize=90&fontColor=FFA73F)
<div align="center">

_**Navisioner**_ is a robot
</br> guides visually impaired individuals from the starting point to their destination,
</br> avoids and responds to surrounding hazards, and safely assists them along the way.

----

<img width="929" alt="스크린샷 2025-01-14 오후 3 16 24" src="https://github.com/user-attachments/assets/fdb7c241-6f73-49df-82d4-da9c28eb2b2d" />

## Table of Contents
</div>

- [Project Overview](#project-overview)
- [Structure](#structure)
- [Reference](#reference)
- [Technologies](#technologies)
  </br>&nbsp;&nbsp;&nbsp;&nbsp;[(A) Voice Commands and Route Voice Guidance]((a)-voice-commands-and-route-voice-guidance)
  </br>&nbsp;&nbsp;&nbsp;&nbsp;[(B) Robot Localization and Pose Estimation]((b)-robot-localization-and-pose-estimation)
  </br>&nbsp;&nbsp;&nbsp;&nbsp;[(C) Sidewalk Detection]((c)-sidewalk-detection)
  </br>&nbsp;&nbsp;&nbsp;&nbsp;[(D) Traffic Light Detection]((d)-traffic-light-detection)
  </br>&nbsp;&nbsp;&nbsp;&nbsp;[(E) Obstacle Detection]((e)-obstacle-detection)


<div align="center">





## Project Overview


<img width="800" alt="스크린샷 2025-01-14 오후 4 37 25" src="https://github.com/user-attachments/assets/2184c88e-d2e2-42b2-917a-2691f427053f" />

## Structure
<img width="800" alt="스크린샷 2025-01-14 오후 5 05 07" src="https://github.com/user-attachments/assets/a7800bb1-aad4-4831-b00b-db2732ffd110" />

## Technologies

### (A) Voice Commands and Route Voice Guidance


### (B) Robot Localization and Pose Estimation 


### (C) Sidewalk Detection
<img width="206" alt="Image" src="https://github.com/user-attachments/assets/dc6d0bf6-cdd3-4932-852d-a64f59b802d2" />

We performed transfer learning on [Ultralytics](https://github.com/ultralytics)’s [YOLOv8](https://github.com/ultralytics/ultralytics) Segmentation model using 45,000 images from [AI Hub](https://www.aihub.or.kr/), an open dataset platform. [The dataset](https://www.aihub.or.kr/aihubdata/data/view.do?currMenu=&topMenu=&aihubDataSe=data&dataSetSn=189) includes annotations in box and polygon formats for 29 types of objects obstructing Indian pedestrian pathways, as well as polygon annotations for sidewalk surface conditions.
<br/>

<img width="756" alt="Image" src="https://github.com/user-attachments/assets/cbfa27b5-b3a2-4e6d-aca7-c8b9bd72ded2" />

Due to the performance limitations of edge devices, there was an issue with slow real-time processing speed.
<br/>
To address this, we converted the PyTorch model to TensorRT and applied INT8 quantization to accelerate the inference speed. As a result, we were able to improve latency by 65% while reducing mAP loss.
<br/>

<img width="660" alt="Image" src="https://github.com/user-attachments/assets/505040da-928d-41d0-8e14-05ba19f63e81" />

The detection process is as follows: 
<br/>The RGB video is captured from the camera, and the pedestrian road area is segmented.
<br/>
An algorithm compares the detected sidewalk's two edges with experimentally obtained threshold points to localize the robot, ensuring that it moves only within the sidewalk.
<br/>

<img width="1283" alt="Image" src="https://github.com/user-attachments/assets/bf09993b-f353-4299-8354-363a2f5b690e" />

This diagram simplifies the algorithm.
<br/>
In the first image, when comparing the threshold with the sidewalk edges, the robot is at the center of the sidewalk, so it moves straight ahead.
<br/>
Depending on the condition, if the robot is shifted to the left, it moves right; if shifted to the right, it moves left.
<br/>
In this way, the robot's angular velocity and speed are adjusted and controlled via ROS according to each situation.
<br/>

### (D) Traffic Light Detection

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/411e0f85-9686-4081-9a63-be5ef50376fe/05668a10-1aef-4412-9a73-b3d00bbce9a8/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/411e0f85-9686-4081-9a63-be5ef50376fe/a0535548-4010-45c5-9ba9-5da161510ac7/image.png)

Using the crosswalk location information received from the Tmap API, the robot verifies that it has reached the crosswalk. If it is determined that the crosswalk has been reached, the YOLOv8 model will crop only the traffic light in the image, and use this cropped image to determine if it is a green light and to cross.

The cropped image is determined whether it is a green light through three steps.

1. Check the color with the HSV filter.
2. A moving average filter is applied to prevent noise-induced errors, and it is considered green only when the average value is above 0.5.
3. Using the ‘red flag’, it does not cross when the signal's remaining time is short, but moves only when it changes from red to green. This allows the robot to safely cross the crosswalk.

### (E) Obstacle Detection







## Reference
| Reference | 
|----|
| [yolo_ros](https://github.com/mgonzs13/yolo_ros) | 
| [scout_ros2](https://github.com/agilexrobotics/scout_ros2) | 
| [ugv_sdk](https://github.com/westonrobot/ugv_sdk) |
| [ROS2 Foxy](https://docs.ros.org/en/foxy/Installation.html) | 
| [zed-ros2-wrapper](https://github.com/stereolabs/zed-ros2-wrapper) |

</div>
