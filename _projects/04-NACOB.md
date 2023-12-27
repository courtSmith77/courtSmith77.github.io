---
name: Deep Learning for WBAM Estimation
tools: [Python, MATLAB, Machine Learning, Wearable Robotics, Wearable Sensors, PyTorch]
image: 
description: Optimized a Temporal Convolutional Network (TCN) to estimate Whole Body Angular Momentum (WBAM) from wearable sensors
---

## Deep Learning for Whole Body Angular Momentum Estimation

<br>
<center><img src="{{ site.url }}{{ site.baseurl }}/inserts/nacob_poster.pdf"/></center>
<br>
In August 2022, I presented this research at the North American Congress on Biomechanics (NACOB) in Ottawa, Canada. 

### Importance
<br>
Whole body angular momentum (WBAM) is a common metric that measures a person’s total body stability and balance during locomotion. It is typically calculated from full-body motion capture data collected in a laboratory and calculated offline following a time-intensive process including data cleaning and biomechanical analyses. This need for post-hoc processing precludes the use of WBAM in real time, which could be used to control assistive devices for individuals with balance impairments. Additionally, the dependence on in-lab tools prevents the use of WBAM outside of a lab setting, which is important when trying to understand balance in realistic real-world contexts, such as outside or in a patient's home. 
<br>
Machine learning (ML) techniques could be used to overcome these shortcomings by estimating WBAM from wearable sensors rather than marker data, enabling mobile collection and real-time use. Since steady state WBAM is cyclic in nature and highly regulated, simple ML models have shown positive results for estimation during steady state walking. However, estimating WBAM during a perturbation, when balance is suddenly disrupted and there is a transient change in the WBAM signal, is likely more challenging for an ML model and has not yet been expored. In this study, we developed ML models and determined their accuracy in estimating WBAM during perturbations using wearable sensors.
<br>
<br>

### TCN Optimization
<br>
We developed a Temporal Convolutional Network (TCN) to estimate WBAM and perform Sensor Location Optimization. First, we optimized the TCN model to get the best possible estimate for WBAM by performing a grid sweep of five hyperparameters: kernel size, hidden size, dropout, learning rate, and levels. We did this process twice, once using the entire sensor location data set, then a second time using data from the sensor locations on the lower limbs and the pelvis. From there, we selected the hyperparameters that incorporated and performed most optimally across both the full and selective sensor sets. With this optimized model, sensor location selection was performed using a technique similar to forward feature selection. From those results, we determined the most optimal combination of sensor locations. We only used the sensor location data found to be optimal for the rest of the experiment. Again, we optimized the five hyperparameters by performing a grid sweep. Then, using the optimal grid sweep combination as a base, we conducted individual hyperparameter optimization to get the final optimized TCN model with selective sensor locations. Throughout this process, we used leave-one-subject-out validation where with a total of eight subjects, we train the model on seven subjects and test on one. Root Mean Squared Error (RMSE) was used to determine the accuracy of the models’ estimations. 
<br>
<br>

### Results

#### Sensor Location Optimization
<br>
The TCN model found that the sensor locations at the Torso, Femur, Hand, and Pelvis are the most important locations for improving the accuracy of the model. The combination of those four sensors produces an error of about 7.8% of the peak steady-state range for WBAM.
<br>

#### WBAM Estimation
<br>
The most optimal TCN model that was found during sensor location selection estimated WBAM with an R2 value of 0.955 as seen in Figure 2. In the graph, even when WBAM is at the extremities of -0.1 and 0.1, the model is still able to estimate them at high accuracy. This is very promising for the future of using machine learning to estimate WBAM using a smaller-than-necessary sensor set.