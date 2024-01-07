---
name: Mobile Manipulator Pick and Place
tools: [Python, Robot Manipulation, Motion Planning, Inverse Kinematics, PID Control, Screw Theory, CoppeliaSim]
image: https://courtSmith77.github.io/inserts/kuka_gif.gif
description: Python simulation of a pick and place task using a Kuka youBot.
---

# Mobile Manipulator Pick and Place
<br>
<!-- hyperlink to github -->
<a href="https://github.com/courtSmith77/Kuka-youBot-Manipulation">GitHub Repository</a>

<br>
For a capstone project in Northwestern University's Robotics Manipulation class, students simulated controlling a Kuka youBot mobile manipulator by combining several elements of the course.

The goal of this project was to use modern screw theory to generate the desired reference trajectory for the end-effector to pick up and place a block in a target location. A feedforward + PI controller was used to minimize error between the actual trajectory and the reference trajectory. An ODE was used to simulate the physics of the youBot base, manipulator, and gripper. The simulation was performed in CoppeliaSim.

### Solution

<center>
    <div style="position: relative; padding-bottom: 28.125%; height:0; overflow: hidden;">
        <video src="![kuka_gif](https://github.com/courtSmith77/courtSmith77.github.io/assets/144190404/c0cbd415-e795-4416-bb4b-37ec833cbc8b)
" controls style="position: absolute; top:0; left:0; width: 100%; height: 100%;"></video>
    </div>
</center>
