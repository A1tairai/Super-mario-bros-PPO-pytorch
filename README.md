# [PYTORCH] Proximal Policy Optimization (PPO) for playing Super Mario Bros

## Introduction

Here is my python source code for training an agent to play super mario bros. By using Proximal Policy Optimization (PPO) algorithm introduced in the paper **Proximal Policy Optimization Algorithms** [paper](https://arxiv.org/abs/1707.06347).

Talking about performance, my PPO-trained agent could complete 29/32 levels, which is much better than what I expected at the beginning. 

For your information, PPO is the algorithm proposed by OpenAI and used for training OpenAI Five, which is the first AI to beat the world champions in an esports game. Specifically, The OpenAI Five dispatched a team of casters and ex-pros with MMR rankings in the 99.95th percentile of Dota 2 players in August 2018.

<p align="center">
  <img src="demo/video_1_1.gif">
  <img src="demo/video_1_2.gif">
  <img src="demo/video_1_3.gif">
  <img src="demo/video_1_4.gif"><br/>
  <img src="demo/video_2_1.gif">
  <img src="demo/video_2_2.gif">
  <img src="demo/video_2_3.gif">
  <img src="demo/video_2_4.gif"><br/>
  <img src="demo/video_3_1.gif">
  <img src="demo/video_3_2.gif">
  <img src="demo/video_3_3.gif">
  <img src="demo/video_3_4.gif"><br/>
  <img src="demo/video_4_1.gif">
  <img src="demo/video_4_2.gif">
  <img src="demo/video_4_3.gif">
  <img src="demo/video_4_4.gif"><br/>
  <img src="demo/video_5_1.gif">
  <img src="demo/video_5_2.gif">
  <img src="demo/video_5_3.gif">
  <img src="demo/video_5_4.gif"><br/>
  <img src="demo/video_6_1.gif">
  <img src="demo/video_6_2.gif">
  <img src="demo/video_6_3.gif">
  <img src="demo/video_6_4.gif"><br/>
  <img src="demo/video_7_1.gif">
  <img src="demo/video_7_2.gif">
  <img src="demo/video_7_3.gif">
  <img src="demo/video_7_4.gif"><br/>
  <img src="demo/video_8_1.gif">
  <img src="demo/video_8_2.gif">
  <img src="demo/video_8_3.gif">
  <img src="demo/video_8_4.gif"><br/>
  <i>Sample results</i>
</p>

## Motivation

It has been a while since I have released my A3C implementation ([A3C code](https://github.com/uvipen/Super-mario-bros-A3C-pytorch)) for training an agent to play super mario bros. Although the trained agent could complete levels quite fast and quite well (at least faster and better than I played :sweat_smile:), it still did not totally satisfy me. The main reason is, agent trained with A3C could only complete 9/32 levels, no matter how much I fine-tuned and tested. It motivated me to look for a new approach.

Before I decided to choose PPO as my next complete implementation, I had partially implemented a couple of other algorithms, including A2C and Rainbow. While the former did not show a big jump in performance, the latter is more suitable for more randomized environments/games, like ping-pong or space invaders.


## How to use my code

With my code, you can:

* **Train your model** by running `python train.py`. For example: `python train.py --world 5 --stage 2 --lr 1e-4`
* **Test your trained model** by running `python test.py`. For example: `python test.py --world 5 --stage 2`

**Note**: If you got stuck at any level, try training again with different **learning rates**. You could conquer 29/32 levels like what I did, by changing only **learning rate**. Normally I set **learning rate** as **1e-3**, **1e-4** or **1e-5**. However, there are some difficult levels, including level **1-3**, in which I finally trained successfully with **learning rate** of **7e-5** after failed for 70 times.

## Docker

For being convenient, I provide Dockerfile which could be used for running training as well as test phases

Assume that docker image's name is ppo. You only want to use the first gpu. You already clone this repository and cd into it.

Build:

`sudo docker build --network=host -t ppo .`

Run:

`docker run --runtime=nvidia -it --rm --volume="$PWD"/../Super-mario-bros-PPO-pytorch:/Super-mario-bros-PPO-pytorch --gpus device=0 ppo`

Then inside docker container, you could simply run **train.py** or **test.py** scripts as mentioned above.

**Note**: There is a bug for rendering when using docker. Therefore, when you train or test by using docker, please comment line `env.render()` on script **src/process.py** for training or **test.py** for test. Then, you will not be able to see the window pop up for visualization anymore. But it is not a big problem, since the training process will still run, and the test process will end up with an output mp4 file for visualization

## Why there are still 3 levels missing?

In world 4-4, 7-4 and 8-4, map consists of puzzles where the player must choose the correct the path in order to move forward. If you choose a wrong path, you have to go through path you visited again. That's why my agent at the moment can not complete these 3 levels