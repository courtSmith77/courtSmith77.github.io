---
name: Demonstration Learning via Diffusion Policy on 7 DOF Robot Arm
tools: [Python, Machine Learning, ROS2]
image: https://courtSmith77.github.io/inserts/diffusion_thumbnail.gif
description: Implement diffusion policy models on specialized task using a Franka Panda Arm.
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

*** Final Project Video ***
- data collection
- inference


# Demonstration Learning via Diffusion Policy
<br>
<!-- hyperlink to github -->
<a href="https://github.com/courtSmith77/diffusion_policy">Diffusion Policy Repository</a>
<br>
<a href="https://github.com/courtSmith77/FrankaTeleop">Franka Arm Control Repository</a>

Demonstration learning has grown in popularity to illustrate how complex models can teach robots specific complex tasks through successful demonstrations. This project focused on implementing the diffusion policy developed by scientist at Columbia, MIT, and Toyota Research Institue to complete the Push T task using the 7 degree of freedom Emika Franka Panda Arm [1]. Two data collection methods were developed for collecting training data on the Franka Panda Arm. A ROS2 frame work was developed to communicate data streams between the diffusion model and the Franka controller. The following sections will outline the various technologies used to complete this project.

<br>

## <u>Table of Contents</u>
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

## <b>Diffusion Policy</b>
<a href="https://github.com/courtSmith77/diffusion_policy">Diffusion Policy Repository</a>
<br>

Diffusion policy in robotics refers to a technique that uses a probabilistic process to guide decision-making and control in complex environments. The model generates a sequence of actions conditioned on a series of state observations from the robot and its environment sensors. This approach is particularly useful in tasks like the Push T task, where robots  manipulate objects by pushing them in a controlled manner based on the current state of both the robot and environment. Diffusion policies enable robots to manage uncertainty and adapt in dynamic conditions.

In this project, diffusion policy was used to predict a sequence of end-effector positions in the x and y planes. The observations included the current end-effector position (x,y) and two camera images: one mounted on the end-effector and the other positioned above the table at an angle to capture the scene. End-effector position is included in both the observation and action sequences because position control was used to manipulate the robot.

<br>

## <b>Data Collection</b>
<br>

### <u>Push T task</u>

<center>
<img src="https://courtSmith77.github.io/inserts/pusht_taskexample.gif" alt="Push T Task" />
<figcaption style="font-size: 16px;">Demonstration of the Push T Task using Bilateral Teleop Control.</figcaption>
</center>

<br>
This task was chosen to see if we could successfully replicate the results from the Diffusion Policy paper cited above. In this task the robot pushes a T block into a designated goal pose. This task utilizes both camera data as well as position data streams which requires a large model specifically to handle the image inputs. Additionally, this task shows the robots ability to adapt to changing environments and perform precise movements.

To condense the task, the demonstrations collected focused on correctly orienting the T block with it placed within 10 centimeters of the goal pose at various orientations up to 180 degrees. The model was trained on 101 demonstrations varying in length from 20 seconds to 2 minutes.

<br>
<center>
<img src="{{ site.url }}{{ site.baseurl }}/inserts/dp_starting_poses.png"/>
<figcaption style="font-size: 16px;">Training data subset of various starting poses.</figcaption>
</center>
<br>


### <u>Data Streams</u>
<br>
As mentioned above, the Push T task utilizes both image and position data as input observations. A Realsense d435 was used to capture the scene images and a Realsense d405 was used to capture the end effector images. Both cameras are run at 30 fps but are downsampled to 10 hz to match the frequency used in CITE PAPER. The scene image was cropped to exclude any extraneous objects in the image and then futher resized to decrease the model size and computation. The end effector image was resized for the same reason.

##### Image Resizing
<center><img src="{{ site.url }}{{ site.baseurl }}/inserts/obs_data_image_resize.jpg"/></center>
<br>

Position control is used to command the robot arm, so the end effector of the robot is used as both an observation and action during training and only an observation during inference. The end effector position is collected from the TF tree in the ROS2 framework sampled at 10 hz.
<br>

### <u>Collection Frameworks</u>
<br>
Two data collection frameworks were developed. Mouse control has the user demonstrate the task by teleoping the robot in the x and y axes with a computer mouse.

The second method utilizes a novel impedance controller to remote control one robot arm with another robot arm. See the Appendix for more information on this topic
<br>

## <b>Franka Controller</b>
<a href="https://github.com/courtSmith77/FrankaTeleop">Franka Arm Control Repository</a>
<br>

Moveit Plan Cartesian Path and Execute Trajectory

** Diagram of ROS framework: diffusion_policy, action_franka, model_input, moveit **
** Pull from rqt_graph

<br>

## <b>Results</b>

<br>
** official model used and trained
** video of model running
** compare models from the different collection methods
** graphs of actions commanded and franka position
<br>

## <b>Future Works</b>
<br>

<br>

## <b>Appendix: Impedance Controller on Emika Panda Arm</b>
<br>
Compare Modes:
- White Light mode (default)
- friction compensation
- damping coefficent

** include diagram of equations from Modern Robotics (ADD Citations) and explain relavance (page 444 in book)
** locking the end effector
** include diagram of decreased magnitude of forces in the directions across modes

<br>

##### <u>Spring-Damper System<u>
<center><img src="{{ site.url }}{{ site.baseurl }}/inserts/spring_damper_system_image.png"/></center>
<br>

##### <u>Impendance Control Algorithm<u>
<center><img src="{{ site.url }}{{ site.baseurl }}/inserts/impedance_algorithm_equation.png"/></center>
<br>


<br>

## <b>Citations</b>
1. <a href="https://diffusion-policy.cs.columbia.edu/">Diffusion Policy</a>
2. <a href="https://jihaizhao.github.io/">Jihai Zhao</a>
- graham
- nick
- modern robotics



