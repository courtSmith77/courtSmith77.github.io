---
name: Drone Obstacle Avoidance
tools: [Python, OpenCV, Machine Learning, ROS2]
image: https://courtSmith77.github.io/inserts/drone_longer.gif
description: Tello drone to traverse an obstacle coourse.
---

# Drone Obstacle Avoidance
<br>
<!-- hyperlink to github -->
<a href="https://github.com/courtSmith77/Drone-Obstacle-Avoidance">GitHub Repository</a>


Over a span of 10 weeks, the project focused on achieving autonomous navigation through a series of obstacles. The primary tools employed were YOLO's deep convolutional networks for accurate symbol detection and classification, alongside the Tello SDK for precise drone control. Arrows indicate drone movement directions, with special symbols for turning around or finishing the course.
<br>
<br>

## Full Live Demo
<br>

<center><iframe width="560" height="315" src="https://www.youtube.com/embed/siTZoEZFah8?si=i0K699tnvzIae1cj" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe></center>

<br>

## Project Subsystems
<br>

### <b>Symbol Detection and Classification</b>
The Tello drone’s onboard camera records at 30 frames per second with a video resolution at 1280x720 pixels. A custom dataset was compiled containing approximately 1300 images of the cardinal directions and special symbols. Using <a href="https://www.cvat.ai/">CVAT</a>, each arrow in the images was delineated with a bounding rectangle.
<br>

#### <u>Symbol Detection</u>
Employing an 80/20 train-test-split on the dataset, I trained and validated the Ultralytics YOLOv8 <a href="https://docs.ultralytics.com/tasks/detect/">Detection Model</a> on the custom dataset. The primary outcome of interest from this model was the bounding box, denoting the bottom left and top right corners (x1, y1, x2, y2) of the detected arrow or special symbol.
<br>

During real-time operation, the detection mechanism predominantly located objects rather than reliably determining their specific symbol classification. This outcome was anticipated, given YOLO’s architecture which is designed to detect objects irrespective of their orientation or position. Therefore, the principal takeaway from this trained model was the detection box rather than the classification.
<br>

#### <u>Creating Classification Dataset</u>
To train the YOLO classifier, I utilized the previously trained detection model in conjunction with traditional computer vision techniques. First, the original image was inputted into the detection model. Subsequently, the resulting detection box was used to execute the OpenCV functions `getPerspectiveTransform` and `warpPerspective` to isolate and extract the portion of the original image contained within the detection box. These extracted images were then annotated and saved for training purposes. 
<br>
<br>
Originally, a single classification model was used to classify the symbol, however the special symbols were consistently confused for an arrow direction. To prevent this, two separate models were trained: one for arrow classification and one for special symbol classification. To determine which model to use to classify the symbol, I looked at the ratio of width to length of the boxing box detected. The special symbols have a square-like shape with ratios between 0.8 and 1.2 whereas the arrows have a rectangular shape falling well below and well above the square-like ratio. 
<br>

#### Classification Cascade
<center><img src="{{ site.url }}{{ site.baseurl }}/inserts/new_cascade.png"/></center>
<br>

#### <u>Arrow Classification</u>
An 80/20 train-test-split was applied to the cropped image dataset to train and validate the YOLOv8 <a href="https://docs.ultralytics.com/tasks/classify/">Classification Model</a> for arrow directions. The model predicted class IDs representing different directions: "down" (0), "left" (1), "right" (2), and "up" (3).
<br>

#### <u>Symbol Classification</u>
Similarly, an 80/20 train-test-split was applied to the cropped image dataset to train and validate the YOLOv8 <a href="https://docs.ultralytics.com/tasks/classify/">Classification Model</a> for special symbols. The model predicted class IDs representing "star" (0) and "U Turn" (1).
<br>
<br>

### <b>Drone Control</b>
Since the drone lacks an onboard depth sensor, all control decisions are based on the size and location of the detection box. When commanding the drone, motion in the x (+x forward), y (+y left), and z (+z up) planes is taken into consideration. 
<br>

#### Control System Diagram
<center><img src="{{ site.url }}{{ site.baseurl }}/inserts/control_flow_diagram.png"/></center>
<br>

#### <u>Arrow Control</u>

##### <i>Direction Control</i>
Once a direction has been determined for each drone, the controller determines the location of the “important edge” for that specific direction, indicating how far the drone must fly to avoid the obstacle in the designated direction. Additionally, the controller calculates the area of the detection box to indicate whether it is safe to fly forward. The larger the area the closer the drone is to the obstacle.
<br>

##### <i>Orthogonal Control</i>
In addition to direction control, to ensure correct avoidance of the obstacle, the drone aligns itself to be centered on the obstacle in the plane orthogonal to the detected arrow. This part of the controller looks at the center of the detection box relative to the center of the live image center. If it is outside a tuned threshold, the drone will move to center itself in the orthogonal plane.
<br>

#### <u>Special Symbol Control</u>
When a special symbol is detected, the drone will center itself on the symbol and approaches within a specific distance before executing the desired action. Similar to arrow orthogonal control, this controller looks at the center of the detection box relative to the center of the live image center. If it is outside the tuned threshold in either the vertical and horizontal planes, it will move to center itself. Once the area of the of the image gets within a specific threshold the robot will execute the action linked to the specific symbol.
** Disclaimer: When the “star” symbol is detected, and it is time to flip, the drone will confirm with the user to make sure it is safe to complete the flip.
<br>

![Special Control Maneuvers](https://courtSmith77.github.io/inserts/flip_and_rotate.gif)

<br>






