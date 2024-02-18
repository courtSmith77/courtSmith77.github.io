---
name: Drone Obstacle Avoidance
tools: [Python, OpenCV, Machine Learning]
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
Over a span of 10 weeks, the project focused on achieving autonomous navigation through a series of obstacles. The primary tools employed were YOLO's deep convolutional networks for accurate object detection and classification, alongside the Tello SDK for precise drone control. Each obstacle has an arrow label indicating which direction the drone should move to a avoid it. The drone will identify the arrow direction and distance to the obstacle before flying in the necessary directions.
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

#### <b>Arrow Detection and Classification</b>
The Tello drone onboard camera records at 30 frames per second with a video resolution at 1280x720 pixels. A custom dataset was compiled containing 915 images of the cardinal directions. Using <a href="https://www.cvat.ai/">CVAT</a>, each arrow in the images was delineated with a bounding rectangle.
<br>

##### <u>Arrow Detection</u>
Employing an 80/20 train-test-split on the dataset, I trained and validated the Ultralytics YOLOv8 <a href="https://docs.ultralytics.com/tasks/detect/">Detection Model</a> on the custom dataset. The primary outcome of interest from this model was the bounding box, denoting the bottom left and top right corners (x1, y1, x2, y2) of the detected arrow.
<br>

During real-time operation, the arrow detection mechanism predominantly pinpointed the location of arrows rather than reliably determining their directional orientation. This outcome was anticipated, given YOLO’s architecture which is designed to detect objects irrespective of their orientation or position. Therefore, the principal takeaway from this trained model was the detection box rather than the classification.
<br>

##### <u>Creating Classification Dataset</u>
To train the YOLO classifier, I utilized the previously trained arrow detection model in conjunction with traditional computer vision techniques. First the original image was inputted into the detection model. Subsequently, the resulting detection box was used to execute the OpenCV functions `getPerspectiveTransform` and `warpPerspective` to isolate and extract the portion of the original image contained within the detection box. These extracted images were then annotated and saved for training purposes.
<br>

##### <u>Arrow Classification</u>
An 80/20 train-test-split was applied to the cropped image dataset to train and validate the Ultralytics YOLOv8 <a href="https://www.cvat.ai/">Classification Model</a> using a custom dataset. The focal point of interest from this model's predictions was the class ID, represented by an integer ranging from 0 to 3, where 0 denotes "down," 1 denotes "left," 2 denotes "right," and 3 denotes "up." 
<br>
<br>

#### Drone Control


<br>
<br>






