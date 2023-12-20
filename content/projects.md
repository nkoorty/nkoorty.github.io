+++
title = 'Projects'
draft = false
+++
This is a list of some of the things I've done in my free time. I've excluded online courses on Machine Learning and Deep Learning to increase SNR. Some repos are available, some are private for unspecified reasons. Some projects are semi-school related, some aren't.

### Self Parking with Reinforcement Learning
This was my final year project at King's College London. I used reinforcement learning to train a car to park itself in a parking spot. I used Proximal Policy Optimization (PPO) using PyTorch and Stable Baselines-3 to train the car. I used a combination of OpenAI Gym and Pytorch to simulate the environment and Python for the whole implementation.

I specifically analysed the effect of the environment on the Markov Decision Process (MDP) and how particular environments that involved a particular set of actions affected the 1. likelihood and 2. quality of parking. Obviously, the actual nitty-gritty details are in the thesis, which I'll link here at one point.

**Repository:** [Find repo here](https://github.com/nkoorty/rl_parking)
![RL2](/images/rl_2.gif) 


### Generating X-rays of Hands using GANs
Using ~8000 training images, I trained a GAN model using an NVIDIA V100 to generate X-rays of hands using Pytorch. The images look pretty good, with some detail being lost in the hand shapes, however, for most images it's quite clear that the images are clearly x-rays, since the pixel opacity between the bones and the skin is quite clear.

**Repository:** TBA
![RL2](/images/gan_1.jpg) 

### Computationally Predicting Asteroid Impacts
This project was more so just really solid practice of 1. mathematical 2. physics and 3. programming knowledge. Given entry data of an asteroid, the tool I built calculates the trajectory of the asteroid by modelling its change in mass, velocity, size, etc. while considering air resistance, gravity, ablation, etc.

Additionally, using a large dataset of UK postcodes and their respective population, the tool also calculates the probability of a particular area getting hit by an asteroid, and what pressure levels should be expected. This is also visualised.

**Repository:** TBA
![RL2](/images/ryugu.png) 

### Neural Network in C
This one's a little more simple. I built a neural network in C that predicted XOR logic gate outputs based on 2 binary inputs. I got it to be quite accuracte at ~1000 epochs at a 98% accuracy, which is pretty good. This was my first time dabbling in Machine Learning and Neural Networks, so it was a great learning experience.

### Building ALU using structural VHDL
This was a university coursework, but I was so proud of my work that I had to include it. I built an Arithmetic Logic Unit (ALU) using structural VHDL. I used a massive combination of half-adders, full-adders, multiplexers, etc. to build a fully functioning 32-bit ALU using only logic gates. Additionally, the ALU included a zero flag, overflow flag, and carry flag to indicate the status of the ALU.

![RL2](/images/alu.jpg) 