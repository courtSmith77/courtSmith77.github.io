---
name: Demonstration Learning via Diffusion Policy on 7 DOF Robot Arm
tools: [Python, Machine Learning, ROS2]
image: https://courtSmith77.github.io/inserts/diffusion_thumbnail.gif
description: Implement diffusion policy models on specialized task using a Franka Panda Arm.
---

# Under Construction, will be done by 12/12



# Demonstration Learning via Diffusion Policy
<br>
<!-- hyperlink to github -->
<a href="https://github.com/courtSmith77/diffusion_policy">Diffusion Policy Repository</a>
<a href="https://github.com/courtSmith77/FrankaTeleop">Franka Arm Control Repository</a>

Demonstration learning has grown in popularity to illustrate how complex models can teach robots specific complex tasks through successful demonstrations. This project focused on implementing the diffusion policy developed by scientist at Columbia, MIT, and Toyota Research Institue to complete a task using the 7 Degree of Freedom Emika Franka Panda Arm (ADD LINK TO PAPER). A ROS2 frame work was developed to communicate data streams between the diffusion model and the Franka controller. The following sections will outline the various technologies used to complete this project.

<br>


## Table of Contents
<br>
1. Diffusion Policy
2. Data Collection
    - Push T Task
    - Data Streams
    - Collection Frameworks
        - Mouse Control
        - Teleop one Robot via manipulating a Second Robot
3. Franka Controller
4. Results
5. Future Works
6. Appendix: Impendance Control on Emika Franka Panda Arm
7. Citations
<br>

## Diffusion Policy
<br>
** add repository again **

What is Diffusion Policy?
Why is it relevant?
Cite Diffusion Paper here.

How is it utilized in the project.
Inputs (observations)
Outputs (actions)
Breifly mention posiiton control of the franka to explain the end effector position as inputs and outputs
<br>

## Data Collection
<br>

### Push T task
<br>
This task was chosen to see if we could successfully replicate the results from the Diffusion Policy paper cited above. In this task the robot pushes a T block into a designated goal pose. This task utilizes both camera data as well as position data streams which requires a large model specifically to handle the image inputs. Additionally, this task shows the robots ability to adapt to changing environments and perform precise movements.

To condense the task, the demonstrations collected focused on correctly orienting the T block with it placed within 4 centimeters of the goal pose at various orientations up to 180 degrees. The model was trained on 101 demonstrations varying in length from 45 seconds to 2 minutes.

** attach a series of starting images displaying various starting positions **
<br>

### Data Streams
<br>
As mentioned above, the Push T task utilizes both image and position data as input observations. A Realsense d435 was used to capture the scene images and a Realsense d405 was used to capture the end effector images. Both cameras are run at 30 fps but are downsampled to 10 hz to match the frequency used in CITE PAPER. The scene image was cropped to exclude any extraneous objects in the image and then futher resized to decrease the model size and computation. The end effector image was resized for the same reason.

** attach diagram of the camera streams and what it looks like as an input to the model **

Position control is used to command the robot arm, so the end effector of the robot is used as both an observation and action during training and only an observation during inference. The end effector position is collected from the TF tree in the ROS2 framework sampled at 10 hz.
<br>

### Collection Frameworks
<br>
Two data collection frameworks were developed. Mouse control has the user demonstrate the task by teleoping the robot in the x and y axes with a computer mouse.
The second method utilizes a novel impedance controller to remote control one robot arm with another robot arm. See the Appendix for more information on this topic
<br>

## Franka Controller
<br>
** add repository again **

Moveit Plan Cartesian Path and Execute Trajectory

** Diagram of ROS framework: diffusion_policy, action_franka, model_input **
<br>

## Results

<br>
** official model used and trained
** video of model running
** image of actions commanded and franka position
<br>

## Future Works
<br>

<br>

## Appendix: Impedance Controller on Emika Panda Arm
<br>
Compare Modes:
- White Light mode (default)
- friction compensation
- damping coefficent

** include diagram of equations from Modern Robotics (ADD Citations) and explain relavance
** locking the end effector
** include diagram of decreased magnitude of forces in the directions across modes
<br>

## Citations
- diffusion policy
- jihai
- graham
- nick



