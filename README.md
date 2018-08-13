# Carmageddon team's project

## Introduction

The purpose of this (final) project was the implementation of an integrated system
capable of controlling the Carla, an autonomous Lincoln MKZ.

(TODO: write more)

## Team members

* Maciej Dziubinski (ponadto@gmail.com)
* Ankit Shrivastava (shrivastavaanki700@gmail.com)
* Mohamed Ameen (mohamed.a.ameen93@gmail.com)

## Project structure

The following picture presents the ROS nodes and ROS topics comprising the system:

![alt text](imgs/final-project-ros-graph-v2.png)

As students, we were responsible for three nodes in particular:

* the `'tl_detector'` node (implemented in the `tl_detector` package) that publishes
  to the `/traffic_waypoint` topic the location to stop before a red light;
* the `'waypoint_updater'` node (implement in the `waypoint_updater` package)
  that publishes a list of waypoints (which determine the velocity) ahead of the
  car to the `/final_waypoints` topic;
* the `'dbw_node'` (in the `twist_controller` package) that publishes to three
  topics: `vehicle/throttle_cmd`, `/vehicle/brake_cmd`, and `/vehicle/steering_cmd`
  which control the car.

A good summary of those three nodes is presented below:

![alt text](imgs/three-nodes.png)

## Code structure

The following excerpt from the `tree` command summarizes the structure of the
`ros/src` directory (actually, the nodes that were presented in the
**Project structure** section above):

```
./ros/src
...
│
├── tl_detector
│   ├── light_classification
│   │   └── tl_classifier.py
│   ├── light_publisher.py
│   └── tl_detector.py
│
├── twist_controller
│   ├── dbw_node.py
│   ├── dbw_test.py
│   ├── lowpass.py
│   ├── pid.py
│   ├── twist_controller.py
│   └── yaw_controller.py
│
└── waypoint_updater
    └── waypoint_updater.py
```

### Traffic Light Classification

In our first attempt for the traffic light detection, we trained a basic neural network. The network has been implemented in `tl_classifier.py` using tensorflow 1.3. The neural network is shown below:

![alt text](imgs/arch.png)

The neural network has been taken from: https://github.com/kcg2015/traffic_light_detection_classification

The training image dataset is a mix of simulator images, udacity rosbag images and some more random images.

The model is saved as `frozen_inference_graph.pb` file in `src/tl_detector/ligh
t_classification/retrained_SSD/`.

The output of the classifier is the color of the light – red, green or yellow.