---
name: Impedance Controller on 7 DOF Robot Arm
tools: [C++, ROS2, Libfranka]
image: https://courtSmith77.github.io/inserts/impedance_thumbnail.gif
description: Developed and Implemented novel impedance controller on Franka Panda Arm.
---

# Under Construction, will be done by 12/12/2024

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>



# <b>Impedance Controller on Emika Panda Arm</b>
<br>

## Overview
In modern robotics, developing seamless human-robot interactions is key to making robot more intuitive and easy for humans to adapt to using. This project focuses on implementing impedance control for an Franka Emika Panda Robot Arm to make it more compliant and easier to guide by hand. By designing an impedance model, the system can adjust how the robot responds to user input, enabling smooth and effortless movements

The Franka Emika Panda Robot Arm is known for its precision and advanced control features. The goal of this project is develop a impedance control system that minimizes user effort while keeping the arm stable and responsive. This involves fine-tuning the virtual mass, damping, and stiffness coefficients to balance flexibility and accuracy.

Through impedance control, this project aims to enhance the way people and robots work together, making it ideal for hands-on tasks like collaborative work. Check out this controller being used for data collection in <a href="https://courtsmith77.github.io/projects/01-diffusionpolicy">this</a> project.

### <u>Impendance Control Algorithm<u>
<center>
<img src="{{ site.url }}{{ site.baseurl }}/inserts/impedance_algorithm_equation.png"/>
<figcaption style="font-size: 16px;">Impedance control algorithm from Modern Robotics Textbook.</figcaption>
</center>
<br>

This project focuses on tuning the parameters in the second half of the equation which equate to fext. That equation is derived from a spring damper system, shown below.

<center>
<img src="{{ site.url }}{{ site.baseurl }}/inserts/spring_damper_system_image.png"/>
<figcaption style="font-size: 16px;">Mass-Spring-Damper System from Modern Robotics Textbook.</figcaption>
</center>
<br>

The coefficients from the diagram and equation are defined as the following:
- <b>x</b>: task-space configuration of the robot
- <b>M</b>: virtual mass matrix
- <b>B</b>: virtual damping matrix
- <b>K</b>: virtual stiffness matrix
- <b>fext</b>: external force applied to the robot

In this project, parameters M, B, and K were tuned to achieve the deisred dynamic behavior of the robotic system. Some observations from the tuning:
1. High stiffness (K) - reduces motion for a given applied force, resulting in rigid movements
2. High damping (B) - smooths the motion to decrease any oscillations
3. Low virtual mass (M) - increase the responsiveness to external forces


<!-- Compare Modes:
- White Light mode (default)
- friction compensation
- damping coefficent

** include diagram of equations from Modern Robotics (ADD Citations) and explain relavance (page 444 in book)
** locking the end effector
** include diagram of decreased magnitude of forces in the directions across modes

<br> -->