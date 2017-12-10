---
layout: page-sidenav
group: "Reinforcement Learning: An Introduction"
title: 1. Introduction 
---

# Introduction

Reinforcement Learning
----------------------

![]({{site.baseurl}}/images/rl_study/rli-1.1.PNG){:class="center-block"}[^1]

- Reinforcement Learning (RL) is learning **how to map situations to actions** to maximize cumulative reward through interaction between agents and environment
- Two main characteristics are
  - Traial-and-error search
    - Learner is not told which actions to take, but instead must discover which actions yield the most reward by trying them
  - Delayed reward
    - Since actions may affect subsequent rewards

Examples
--------
![]({{site.baseurl}}/images/rl_study/rli-1.2.PNG){:class="center-block"}[^1]

Elements of Reinforcement Learning System
-----------------------------------

1. Policy: defines the learning agent's way of behaving at a given time. (perceived states of the environment to actions to be taken when in those states)
2. Reward: defines the goal in a reinforcement learning problem. The agent's objective is to maximize the total reward at the end. Thus, It defines what are good and bad events for the agent.
3. Value function: specifies what is good state in the long run (in terms of the total amount of reward at the end).
4. model (of environment): mimics the behavior of the environment which allows inferences to be made about hot the environment will behave. For example, given a state and action, the model might predict the next state and next reward.

The Problems in Reinforcement Learning
-----------------------
- How to **model** an RL problem
- How to **solve**
  - <span style="color:red">exactly</span> an RL problem
  - <span style="color:red">incrementally</span> an RL problem
  - <span style="color:red">approximately</span> an RL problem
- How to <span style="color:red">efficiently</span> **explore** in an RL problem

References
----------
[^1]: https://www.slideshare.net/carpedm20/reinforcement-learning-an-introduction-64037079
