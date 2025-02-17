# yolov5_pytorch_ros  
This package accompanies software from the following repositories  
https://github.com/open-rdc/scenario_navigation  

### Execute
```
docker run -it --net host --name roscore ros:melodic

cd ~/catkin_ws/src/yolov5_pytorch_ros/docker
docker-compose up -d
docker exec -it yolo bash
mkdir -p catkin_ws/src
cd catkin_ws/src
catkin_init_workspace
git clone https://github.com/open-rdc/yolov5_pytorch_ros
cd ..
catkin_make
source ~/catkin_ws/devel/setup.bash
roslaunch yolov5_pytorch_ros detector_action.launch
```

---

This package provides a ROS wrapper for YOLOv5 based on [PyTorch-YOLOv5](v). The package has NOT been tested yet.

**Authors**: Vasileios Vasilopoulos (<vvasilo@seas.upenn.edu>), Georgios Pavlakos (<pavlakos@seas.upenn.edu>)
**Adapted by**: Raghava Uppuluri

## Prerequisites
1. Have a conda environmeent
2. Have ROS installed
If you haven't done any of those: see [this tutorial](https://wiki.purduearc.com/wiki/tutorials/setup-ros) to do both.

## Quick Start

1. Download the prerequisites for this package, navigate to the package folder and run:
```
# Ensure your conda environment is activated
conda install -f requirements.txt
```

## Installation
Clone the repo into your `src` folder of your `catkinn_ws`. 
```
git clone https://github.com/raghavauppuluri13/yolov5_pytorch_ros.git
```

Navigate to your catkin workspace and run:
```
catkin build yolov5_pytorch_ros
# adds package to your path
source ~/catkin_ws/devel/setup.bash 
```

## Basic Usage
To maximize portability, create a separate package and launch file. Add your weights into a `weights` folder of that package.
```
catkin_create_pkg my_detector
mkdir weights
mkdir launch
# Add weights
# Don't forget to build and source after
```

Then, add the following to `mydetector.launch` in the launch folder:
```xml
<launch>
  <include file="$(find yolov5_pytorch_ros)/launch/detector.launch">
    <!-- Camera topic and weights, config and classes files -->
    <arg name="image_topic"	                value="/camera/image_raw"/>
    <!-- Absolute path to weights file (change this) -->
    <arg name="weights_name"	            value="$(find my_detector)/weights/weights.pt"/>

    <!-- Published topics -->
    <arg name="publish_image"	            value="true"/>
    <arg name="detected_objects_topic"      value="detected_objects_in_image"/>
    <arg name="detections_image_topic"      value="detections_image_topic"/>

    <!-- Detection confidence -->
    <arg name="confidence"                  value="0.7"/>
  </include>
</launch>
```

Finally, run the detector:
```
roslaunch my_detector mydetector.launch
```
![detector](https://github.com/purdue-arc/wiki/blob/master/wiki/robot-arm/assets/images/obj_det_may_21.png)
> Should get something like this when viewed from rviz

### Node parameters

* **`image_topic`** (string)

    Subscribed camera topic.

* **`weights_name`** (string)

    Weights to be used from the [models](models) folder.

* **`publish_image`** (bool)

    Set to true to get the camera image along with the detected bounding boxes, or false otherwise.

* **`detected_objects_topic`** (string)

    Published topic with the detected bounding boxes.

* **`detections_image_topic`** (string)

    Published topic with the detected bounding boxes on top of the image.

* **`confidence`** (float)

    Confidence threshold for detected objects.

### Subscribed topics

* **`image_topic`** (sensor_msgs::Image)

    Subscribed camera topic.

### Published topics    

* **`detected_objects_topic`** (yolov3_pytorch_ros::BoundingBoxes)

    Published topic with the detected bounding boxes.

* **`detections_image_topic`** (sensor_msgs::Image)

    Published topic with the detected bounding boxes on top of the image (only published if `publish_image` is set to true).

## Citing

The YOLO methods used in this software are described in the paper: [You Only Look Once: Unified, Real-Time Object Detection](https://arxiv.org/abs/1506.02640).

If you are using this package, please add the following citation to your publication:

    @misc{vasilopoulos_pavlakos_yolov3ros_2019,
      author = {Vasileios Vasilopoulos and Georgios Pavlakos},
      title = {{yolov3_pytorch_ros}: Object Detection for {ROS} using {PyTorch}},
      howpublished = {\url{https://github.com/vvasilo/yolov3_pytorch_ros}},
      year = {2019},
    }
# yolov5_pytorch_ros
