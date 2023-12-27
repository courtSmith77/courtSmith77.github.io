---
name: BotROS - The Painting Robot
tools: [ROS, Python, MoveIt, OpenCV, Apriltags, Robot Manipulation, Autonomy]
image: https://courtSmith77.github.io/inserts/dot_circle.gif
description: ROS2 package to autonomously paint using an Emika Franka Panda arm.
---

# BotROS
<br>

## Description
The BotROS package uses an Emika Franka Panda arm to autonomously paint a given image. A realsense camera, apriltags, and computer vision were used to detect the location of the paint brushes and the individual paint colors on the paint palette. The listener node identifies the location of the brushes and palette in the robot frame. The colordetection node uses computer vision to create a mask around the palette and braodcasts the location of each color in the robot frame. The robot receives waypoints from the file created by the take_picture package which uses a Canny edge detector and OpenCV to descritize an image. Finally the iliketomoveitmoveit node utilizes MoveIt to excute the painting of the image inculding picking up a paint brush, getting paint, painting the colored waypoints, replacing the brush, and repeating for each color found in the image.

In this project, my main focus was on computer vision and worksapce setup. I worked on the Apriltag intertegration into the tf tree and created the custom publisher to relay the location information. Additionally, I assisted in the state machine flow of the main node that connects all nodes and controls the robots. I also designed and constructed the brush holders that the robot had to grip as well as the brush holder.

Group Members: Courtney Smith, Nader Ahmed, Shail Dalal, Demiana Barsoum, Fiona Neylon
<br>
<br>

## Full Live Demo

<center>
  <video src="https://github.com/courtSmith77/courtSmith77.github.io/assets/144190404/fb37594d-d09f-4ebd-a2ed-cee12fe6d586"></video>
</center> 

<br>
<br>

<!-- ## Subsystems

### MoveIt

### Computer Vision

<br> -->

Check out the project more at our Github!

<!-- hyperlink to github -->
<a href="https://github.com/ME495-EmbeddedSystems/final-project-Group5">BotROS GitHub Repository</a>
