---
name: BotROS - The Painting Robot
tools: [ROS, Python, MoveIt, OpenCV, Apriltags, Robot Manipulation, Autonomy]
image: https://courtSmith77.github.io/inserts/dot_circle.gif
description: ROS2 package to autonomously paint using an Emika Franka Panda arm.
---

# BotROS
<br>
<!-- hyperlink to github -->
<a href="https://github.com/nahder/BotROS-Franka">GitHub Repository</a>

For a final project in Northwestern University's ME495 Embedded Systems in Robotics class, my team programmed a 7-DoF Emika Franka Panda arm to autonomously paint a given image. 
<br>

<div style="display: flex;">
    <div style="flex: 1; text-align: left;">
        <img src="{{ site.url }}{{ site.baseurl }}/inserts/BotROS_Picture.png"/>
    </div>
    <div style="flex: 1;">
        <br>
        <br>
        BotROS (the robot's name), must pick up a paint brush, dip it into paint with the designated color, and begin painting the image. It will get more paint on the brush every 8 dots, then continue the painting. Once the first color has completed, BotROS will return the paint brush to the holder, pick up the next paint brush, and paint the dots of the second color before replacing the brush and completing the painting.
    </div>
</div>


<br>
<br>

## Full Live Demo

<!-- <center>
<video src="https://github.com/courtSmith77/courtSmith77.github.io/assets/144190404/fb37594d-d09f-4ebd-a2ed-cee12fe6d586"></video>
</center> -->

<center>
    <div style="position: relative; padding-bottom: 28.125%; height:0; overflow: hidden;">
        <video src="https://github.com/courtSmith77/courtSmith77.github.io/assets/144190404/fb37594d-d09f-4ebd-a2ed-cee12fe6d586" controls style="position: absolute; top:0; left:0; width: 100%; height: 100%;"></video>
    </div>
</center>

<br>
<br>

## Project Subsystems

### Computer Vision
A realsense camera, apriltags, and computer vision were used to detect the location of the paint brushes and the individual paint colors on the paint palette. The `listener` node identifies the location of the brushes and palette in the robot frame. The `colordetection` node uses computer vision to create a mask around the palette and braodcasts the location of each color in the robot frame. The robot receives waypoints from the file created by the `take_picture` package which uses a Canny edge detector and OpenCV to descritize an image.

### Motion
Finally the `iliketomoveitmoveit` node utilizes a custom MoveIt API to excute the painting of the image. Its functionalities inculde picking up a paint brush, getting paint, painting the colored waypoints, replacing the brush, and repeating for each color found in the image.

### My Focus
In this project, my main focus was on computer vision and worksapce setup. I developed the Apriltag intertegration into the tf tree and created the custom publisher to relay the location information. Additionally, I assisted in the state machine flow of the main node that connects all nodes and controls the robots. I also designed and constructed the brush holders that the robot had to grip as well as the brush holder.

Group Members: Courtney Smith, Nader Ahmed, Shail Dalal, Demiana Barsoum, Fiona Neylon
<br>
<br>






