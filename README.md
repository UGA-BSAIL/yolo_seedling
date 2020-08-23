# ```YOLO_Seedling``` :seedling:
- Package tested on - NVIDIA Jetson TX2 running Jetpack 4.3 [L4T 32.3.1] CUDA 10 and OpenCV4 with ROS melodic
@TODO
- Package tested on - NVIDIA GTX1060 
- Package tested on - NVIDIA Jetson Nano
- Purpose of this project: Real-time plant tracking using Yolov3/Yolov4 on edge devices

## Setup
clone this ```darknet_ros``` into your ```~/catkin_ws/src```
clone https://github.com/Spain2394/darknet_bckup into ```~/catkin_ws/src/darknet_ros```

## Build 
1. Ensure that you have compiled ```darknet_bckup``` seperately, you can do so by runnning ```cd ~/catkin_ws/src/darknet_ros/darknet_bckup``` and using ```make```

To maximize performance, make sure to build in *Release* mode. You can specify the build type by setting

    catkin_make -DCMAKE_BUILD_TYPE=Release

or using the [Catkin Command Line Tools](http://catkin-tools.readthedocs.io/en/latest/index.html#)

    catkin build darknet_ros -DCMAKE_BUILD_TYPE=Release

## Models
download original models (trained for MS COCO dataset):

<details><summary><b>HERE</b> - Yolo v4-tiny files</summary>
    
    * [yolov4-tiny.cfg](https://raw.githubusercontent.com/AlexeyAB/darknet/master/cfg/yolov4-tiny.cfg) - **40.2% mAP@0.5 - 371(1080Ti) FPS / 330(RTX2070) FPS** - 6.9            BFlops - 23.1 MB: [yolov4-tiny.weights](https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v4_pre/yolov4-tiny.weights)
    
</details>

<details><summary><b>HERE</b> - Yolo v3-tiny files</summary>
    
    * [yolov3-tiny.cfg](https://raw.githubusercontent.com/AlexeyAB/darknet/master/cfg/yolov3-tiny.cfg) - **33.1% mAP@0.5 - 345(R) FPS** - 5.6 BFlops - 33.7 MB: [yolov3-tiny.weights](https://pjreddie.com/media/files/yolov3-tiny.weights)

</details>


My seedling models
|model| maP| BFLOPS|FPS|weights|config|
|:---:|:----:|:---:|:----:|:----:|:----:|
|Yolov3-tiny-big||---||[weights](weights)|[cfg](cfg)|
|Yolov3-tiny-med||---||[weights](weights)|[cfg](cfg)|
|Yolov3-tiny-small||---||[weights](weights)|[cfg](cfg)|

|model| maP| BFLOPS|FPS|weights|config|
|:---:|:----:|:---:|:----:|:----:|:----:|
|Yolov4-tiny-big||---||[weights](weights)|[cfg](cfg)|
|Yolov4-tiny-med||---||[weights](weights)|[cfg](cfg)|
|Yolov4-tiny-small||---||[weights](weights)|[cfg](cfg)|

### Computer: Jetson Nano
|model| maP| BFLOPS|FPS|weights|config|
|:---:|:----:|:---:|:----:|:----:|:----:|
|Yolov3-tiny-big||---||[weights](weights)|[cfg](cfg)|
|Yolov3-tiny-med||---||[weights](weights)|[cfg](cfg)|
|Yolov3-tiny-small||---||----|----||


### System 76 w/ GTX1060
|model| maP| BFLOPS|FPS|weights|config|
|:---:|:----:|:---:|:----:|:----:|:----:|
|Yolov3-tiny-big||---||[weights](weights)|[cfg](cfg)|
|Yolov3-tiny-med||---||[weights](weights)|[cfg](cfg)|
|Yolov3-tiny-small||---||----|----||

## Configure
### Camera
run with camera connected to ```dev/video<n>```  ```roslaunch video_stream_opencv camera.launch```
run with video feed run: ```roslaunch video_stream_opencv video_file.launch```

Note: you can view the camera topic by running: ```rqt_image_view```

Configuration using [video_stream_opencv](https://wiki.ros.org/video_stream_opencv) or any camera reading that publishes ```[sensor_msgs/Image]```
In ```~/catkin_ws/darknet_ros/config/ros.yaml``` make sure your ```camera_read``` topic is set to ```videofile/image_raw``` for video feed and ```camera/image_raw``` for camera feed.

## Run 
```roslaunch darknet_ros plant_weed_yolo_v3_tiny.launch```

## Nodes

### Node: ```darknet_ros```

This is the main YOLO ROS: Real-Time Object Detection for ROS node. It uses the camera measurements to detect pre-learned objects in the frames.

### ROS related parameters

You can change the names and other parameters of the publishers, subscribers and actions inside `darkned_ros/config/ros.yaml`.

#### Subscribed Topics

* **`/camera_reading`** ([sensor_msgs/Image])

    The camera measurements.

#### Published Topics

* **`object_detector`** ([std_msgs::Int8])

    Publishes the number of detected objects.

* **`bounding_boxes`** ([darknet_ros_msgs::BoundingBoxes])

    Publishes an array of bounding boxes that gives information of the position and size of the bounding box in pixel coordinates.

* **`detection_image`** ([sensor_msgs::Image])

    Publishes an image of the detection image including the bounding boxes.

#### Actions

* **`camera_reading`** ([sensor_msgs::Image])

    Sends an action with an image and the result is an array of bounding boxes.

### Detection related parameters

You can change the parameters that are related to the detection by adding a new config file that looks similar to `darknet_ros/config/yolo.yaml`.

* **`image_view/enable_opencv`** (bool)

    Enable or disable the open cv view of the detection image including the bounding boxes.

* **`image_view/wait_key_delay`** (int)

    Wait key delay in ms of the open cv window.

* **`yolo_model/config_file/name`** (string)

    Name of the cfg file of the network that is used for detection. The code searches for this name inside `darkned_ros/yolo_network_config/cfg/`.

* **`yolo_model/weight_file/name`** (string)

    Name of the weights file of the network that is used for detection. The code searches for this name inside `darkned_ros/yolo_network_config/weights/`.

* **`yolo_model/threshold/value`** (float)

    Threshold of the detection algorithm. It is defined between 0 and 1.

* **`yolo_model/detection_classes/names`** (array of strings)

    Detection names of the network used by the cfg and weights file inside `darkned_ros/yolo_network_config/`.


## Credit

[pjreddie](https://github.com/pjreddie/darknet), [AlexeyAB](https://github.com/AlexeyAB/darknet) darknet

[leggedrobotics (Marko Bjelonic)](https://github.com/leggedrobotics/darknet_ros) darknet_ros

[abewley](https://github.com/abewley/sort) sort
