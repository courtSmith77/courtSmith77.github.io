---
name: Demonstration Learning via Diffusion Policy on 7 DOF Robot Arm
tools: [Python, Machine Learning, ROS2]
image: https://courtSmith77.github.io/inserts/diffusion_thumbnail.gif
description: Implement diffusion policy models on specialized task using a Franka Panda Arm.
---

<center><iframe width="567" height="504" src="https://www.youtube.com/embed/Eey6MCQP1fo?si=vkhXDw02OkKrk4kz" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe></center>

# Demonstration Learning via Diffusion Policy
<br>
<!-- hyperlink to github -->
<a href="https://github.com/courtSmith77/diffusion_policy">Diffusion Policy Repository</a>
<br>
<a href="https://github.com/courtSmith77/FrankaDiffusionPolicy">Franka Arm Control Repository</a>

Demonstration learning has grown in popularity to illustrate how complex models can teach robots specific complex tasks through successful demonstrations. This project focused on implementing the <a href="https://diffusion-policy.cs.columbia.edu/">diffusion policy</a> developed by scientist at Columbia, MIT, and Toyota Research Institue to complete the Push T task using the 7 degree of freedom Emika Franka Panda Arm. Two data collection methods were developed for collecting training data on the Franka Panda Arm. A ROS2 frame work was developed to communicate data streams between the diffusion model and the Franka controller. The following sections will outline the various technologies used to complete this project.

<br>

## <u>Table of Contents</u>
<br>

1. Diffusion Policy
2. Data Collection
3. Franka Controller
4. Results
5. Future Works

<br>

## <b>Diffusion Policy</b>
<a href="https://github.com/courtSmith77/diffusion_policy">Diffusion Policy Repository</a>
<br>

Diffusion policy in robotics refers to a technique that uses a probabilistic process to guide decision-making and control in complex environments. The model generates a sequence of actions conditioned on a series of state observations from the robot and its environment sensors. This approach is particularly useful in tasks like the Push T task, where robots  manipulate objects by pushing them in a controlled manner based on the current state of both the robot and environment. Diffusion policies enable robots to manage uncertainty and adapt in dynamic conditions.

In this project, diffusion policy was used to predict a sequence of end-effector positions in the x and y planes. The observations included the current end-effector position (x,y) and two camera images: one mounted on the end-effector and the other positioned above the table at an angle to capture the scene. End-effector position is included in both the observation and action sequences because position control was used to manipulate the robot.


## <b>Data Collection</b>
<br>

### <u>Push T task</u>
<br>

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
As mentioned above, the Push T task utilizes both image and position data as input observations. A Realsense d435 was used to capture the scene images and a Realsense d405 was used to capture the end effector images. Both cameras are run at 30 fps but are downsampled to 10 hz to match the frequency used in <a href="https://diffusion-policy.cs.columbia.edu/">diffusion policy</a>. The scene image was cropped to exclude any extraneous objects in the image and then futher resized to decrease the model size and computation. The end effector image was resized for the same reason.

<center>
<h5>Image Resizing</h5>
<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/inserts/obs_data_image_resize.png"/>
    <figcaption style="font-size: 16px;">Cropping and resizing image data (dimensions in pixels).</figcaption>
</figure>
</center>
<br>

Position control is used to command the robot arm, so the end effector of the robot is used as both an observation and action during training and only an observation during inference. The end effector position is collected from the TF tree in the ROS2 framework sampled at 10 hz.
<br>

### <u>Collection Frameworks</u>
<br>

When training demonstration learning models, it has been shown that few excellent demonstrations performs better than many okay demonstrations. In this project, two data collection methods were developed to determine which data collection method was most optimal for the user and in the training outcome.

The first method utilized mouse control where the user demonstrates the task by teleoping the robot in the x and y axes with a computer mouse. This approach utilizes Moveit Servo with a custom PID controller developed by  <a href="https://graham-clifford.com/Robot-Arm-Teleoperation-Through-Computer-Vision-Hand-Tracking/">Graham Clifford</a>.

The second method utilizes a novel impedance controller to preform bilateral control of the franka robot. While a user manipulates the first robot with the impedance controller, the second franka robot immitates the movements of the first. The impedance controller was developed with <a href="https://jihaizhao.github.io/">Jihai Zhao</a>, and he also developed the pipeline for bilateral control and all protocal associated with it. Check out his implementation <a href="https://jihaizhao.github.io/linked_posts/Ergodic.html">here</a>. For more information on the impedance controller, check out <a href="https://courtsmith77.github.io/projects/08-impedancecontrol">this</a> project page.

<center>
<img src="https://courtSmith77.github.io/inserts/teleop_demo.gif" alt="Teleop Demonstration" />
<figcaption style="font-size: 16px;">Bilateral Teleop Control using a novel impedance controller.</figcaption>
</center>
<br>

From a users perspective, the bilateral teleop control was much easier to control and manipulate. Additionally, the franka movements were smoother and maintained a set z height better than the mouse control. However, in the mouse control, the end effector was perfectly locked to the optimal orientation whereas in the teleop control, the user had to consciously maintain this locked orientation. 


## <b>Franka Controller</b>
<a href="hhttps://github.com/courtSmith77/FrankaDiffusionPolicy">Franka Arm Control Repository</a>
<br>

<center>
<img src="https://courtSmith77.github.io/inserts/diffusion_ros_framework.jpg" alt="=Diffusion Ros Framework" />
<figcaption style="font-size: 16px;">ROS2 framework for Executing Diffusion Policy on the Franka Arm.</figcaption>
</center>
<br>

This system operates as illustrated in the diagram above. Data streams are fed into the `model_input_publisher` which configures the data into the appropriate size and format for the model to receive the observations. The `action_predictor` performs the model inference and outputs an action sequence of end effector positions. Finally, the `action_franka_bridge` utilizes a MoveIt API to convert the action sequence into robot movements and executes them on the robot. The `command_mode` allows the user to enable or disable diffusion inference via keyboard input.

The MoveIt API utilizes the `GetCartesianPath` service and `ExecuteTrajectory` action to plan and execute the action sequences on the robot. The `GetCartesianPath` service accepts in waypoints as a list of poses and generates a `RobotTrajectory`. The` ExecuteTrajectory` action takes the `RobotTrajectory ` as input and executes it on the robot.

Once the robot successfully completes the `ExecuteTrajectory` action, the `action_franka_bridge` triggers the `action_predictor` with a service to perform another diffusion inference. This process repeats until the user stops the diffusion loop using `command_mode`.

<br>

## <b>Results</b>

Various models were trained using data from both data collection methods. When executed on the robot, the end effector followed the desired action sequence with errors averaging ~ 3.31 mm. The following graph shows the predicted action horizon, the select action sequence to be executed, and the end effector position throughout the trial.


<center>
<img src="https://courtSmith77.github.io/inserts/ah_video.gif" alt="Action Horizon Video" />
</center>

<center>
<img src="https://courtSmith77.github.io/inserts/action_horizon.png" alt="=Action Horizon Graph" />
<figcaption style="font-size: 12px;">The left shows the full action horizon from the diffusion model whereas the right shows the actions executed on the robot. The black dots represent the end effector position.</figcaption>
</center>
<br>


In the example above, the model hangs at one stop at the end of the video. This behavior can be seen across many of the trained models and various dataset. After futher investigation we are able to see that our models are extremely overfitting when run at many epochs unlike what is seen in the Diffusion Policy paper. Below are some training loss and value loss graphs from our trained models.

<center>
<img src="https://courtSmith77.github.io/inserts/epoch_loss.png" alt="=Epoch Loss Graph" />
<figcaption style="font-size: 12px;">The loss after each epoch. These models begin to overfit after just 20 epochs.</figcaption>
</center>
<br>


In the end, three different datasets were used to train models:
1. Mouse Control (MC) Dataset - 101 demonstrations
2. Bilateral Impedance Control (BIC) Dataset- 100 demonstrations
3. Combined Dataset - 201 demonstrations

Based on the data collection process, I hypothesized that the model trained on the BIC dataset would perform the best because of the ease and smoothness during collection in contrast to the MC's jerkiness. I also hypothesized that the combined dataset would perform better than the MC but worse than the BIC because it would include more demonstrations decreasing the risk of overfitting.

The results were surprising. The MC model performed the best with the least overfitting and the most adaptive model. The model successfully completed the task once complete time as sceen in the video below. Other times, the robot would complete one push then hang when moving to complete the next. The BIC and Combined models on the otherhand performed very similarly. Both models were extremely overfit even when trained on only 20 epochs in contrast to the 2000 epochs the Diffusion Paper did. Each model would proceed to the same end position no matter the initial robot or T configuration.


<center>
<img src="https://courtSmith77.github.io/inserts/diffusion_thumbnail.gif" alt="Push T Success" />
<figcaption style="font-size: 16px;">Successful Push T task using an MC trained model.</figcaption>
</center>


## <b>Future Works</b>

The future of demoonstration learning in robotics will heavily involve the use of diffusion models. To continue the work that has been done, it would be great to continue to explore the training proceedure to understand why 200 demonstrations are being overfit after only 20 epochs. Once the training and all demonstration policy hyperparameters are properly tuned, it would be interesting to revisit the topic of data collection methods and how each one impacts the quality of data and the subsequently trained models. Diving futher into the data collection modes, utilizing haptic feedback with the BIC method could provide users a better experience and improve data collection even further.


<br>


Thank you to my peer Jihai Zhao for being a great partner in developing the Impedance Controller and a great mind to bounce ideas off of. I'd also like to thank Matthew Elwin for all the guidance and insight throughout the project.


