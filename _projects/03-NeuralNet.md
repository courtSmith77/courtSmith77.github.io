---
name: Neural Net from Scratch
tools: [Python, Machine Learning, Mobile Robots]
image: 
description: Coded a fully connected neural net from scratch.
---

## Description

Neural networks and various machine learning models are becoming increasingly popular to solve complex learning aims. In this assignment, I developed and optimized a neural network to learn the mobile robots motion model. The inputs to the model were the robots current state at t (x,y,heading), linear and angular velocities (v,w), and the change in time (dt), and the model output the new state at t+dt x,y,heading. The data used was from this <a href="http://asrl.utias.utoronto.ca/datasets/mrclam/index.html">experiment</a> and is included in the provided Github Repository below.

The model's weights and biases were learned using gradient descent. I also applied mini batching to decrease the model's training time. More of these techniques are broken down in the assignment writeup that can be found on Github.

Check out the Code and Report <a href="https://github.com/courtSmith77/AStar-Algorithm">here!</a>
