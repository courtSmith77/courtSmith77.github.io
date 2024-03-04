---
name: Drone Obstacle Avoidance
tools: [Python, OpenCV, Machine Learning, ROS2]
image: https://courtSmith77.github.io/inserts/drone.gif
description: Tello drone to traverse an obstacle coourse.
---

# Drone Obstacle Avoidance
<br>
<!-- hyperlink to github -->
<a href="https://github.com/courtSmith77/Drone-Obstacle-Avoidance">GitHub Repository</a>

<!-- <br>
<center><img src="{{ site.url }}{{ site.baseurl }}/inserts/BotROS_Picture.png"/></center>
<br> -->

## Description
Over a span of 10 weeks, the project focused on achieving autonomous navigation through a series of obstacles. The primary tools employed were YOLO's deep convolutional networks for accurate object detection and classification, alongside the Tello SDK for precise drone control. Each obstacle has an arrow label indicating which direction the drone should move to avoid it. Additionally, there are two special symbols: one instructs the drone to turn around and fly back through the course , while the other signals the completion of the course and prompts the drone to flip backwards.
<br>
<br>

## Full Live Demo
** Complete video to come. **

<!-- <center>
    <div style="position: relative; padding-bottom: 28.125%; height:0; overflow: hidden;">
        <video src="https://github.com/courtSmith77/courtSmith77.github.io/assets/144190404/93db4aef-e4eb-40c1-b796-5ee2be089a02" controls style="position: absolute; top:0; left:0; width: 100%; height: 100%;"></video>
    </div>
</center> -->

<br>
<br>

## Project Subsystems
<br>

#### <b>Symbol Detection and Classification</b>
The Tello drone’s onboard camera records at 30 frames per second with a video resolution at 1280x720 pixels. A custom dataset was compiled containing approximately 1300 images of the cardinal directions and special symbols. Using <a href="https://www.cvat.ai/">CVAT</a>, each arrow in the images was delineated with a bounding rectangle.
<br>

##### <u>Symbol Detection</u>
Employing an 80/20 train-test-split on the dataset, I trained and validated the Ultralytics YOLOv8 <a href="https://docs.ultralytics.com/tasks/detect/">Detection Model</a> on the custom dataset. The primary outcome of interest from this model was the bounding box, denoting the bottom left and top right corners (x1, y1, x2, y2) of the detected arrow or special symbol.
<br>

During real-time operation, the detection mechanism predominantly located objects rather than reliably determining their specific symbol classification. This outcome was anticipated, given YOLO’s architecture which is designed to detect objects irrespective of their orientation or position. Therefore, the principal takeaway from this trained model was the detection box rather than the classification.
<br>

##### <u>Creating Classification Dataset</u>
To train the YOLO classifier, I utilized the previously trained detection model in conjunction with traditional computer vision techniques. First, the original image was inputted into the detection model. Subsequently, the resulting detection box was used to execute the OpenCV functions `getPerspectiveTransform` and `warpPerspective` to isolate and extract the portion of the original image contained within the detection box. These extracted images were then annotated and saved for training purposes. 
<br>
Originally, a single classification model was used to classify the symbol, however the special symbols were consistently confused for an arrow direction. To prevent this, two separate models were trained: one for arrow classification and one for special symbol classification. To determine which model to use to classify the symbol, I looked at the ratio of width to length of the boxing box detected. The special symbols have a square-like shape with ratios between 0.8 and 1.2 whereas the arrows have a rectangular shape falling well below and well above the square-like ratio. 
<br>
<br>
<center><img src="{{ site.url }}{{ site.baseurl }}/inserts/cascade.png"/></center>
<br>

##### <u>Arrow Classification</u>
An 80/20 train-test-split was applied to the cropped image dataset to train and validate the YOLOv8 <a href="https://docs.ultralytics.com/tasks/classify/">Classification Model</a> for arrow directions. The model predicted class IDs representing different directions: "down" (0), "left" (1), "right" (2), and "up" (3).
<br>

##### <u>Symbol Classification</u>
Similarly, an 80/20 train-test-split was applied to the cropped image dataset to train and validate the YOLOv8 <a href="https://docs.ultralytics.com/tasks/classify/">Classification Model</a> for special symbols. The model predicted class IDs representing "star" (0) and "U Turn" (1).
<br>
<br>

#### <b>Drone Control</b>
Since the drone lacks an onboard depth sensor, all control decisions are based on the size and location of the detection box. When commanding the drone, motion in the x (+x forward), y (+y left), and z (+z up) planes is taken into consideration. 
<br>

##### <u>Arrow Control</u>

###### Direction Control
Once a direction has been determined for each drone, the controller determines the location of the “important edge” for that specific direction, indicating how far the drone must fly to avoid the obstacle in the designated direction. Additionally, the controller calculates the area of the detection box to indicate whether it is safe to fly forward. The larger the area the closer the drone is to the obstacle.
<br>

###### Orthogonal Control
In addition to direction control, to ensure correct avoidance of the obstacle, the drone aligns itself to be centered on the obstacle in the plane orthogonal to the detected arrow. This part of the controller looks at the center of the detection box relative to the center of the live image center. If it is outside a tuned threshold, the drone will move to center itself in the orthogonal plane.
<br>

##### <u>Special Symbol Control</u>
When a special symbol is detected, the drone will center itself on the symbol and approaches within a specific distance before executing the desired action. Similar to arrow orthogonal control, this controller looks at the center of the detection box relative to the center of the live image center. If it is outside the tuned threshold in either the vertical and horizontal planes, it will move to center itself. Once the area of the of the image gets within a specific threshold the robot will execute the action linked to the specific symbol.
** Disclaimer: When the “star” symbol is detected, and it is time to flip, the drone will confirm with the user to make sure it is safe to complete the flip.
<br>
<br>






