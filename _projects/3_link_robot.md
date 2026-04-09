---
layout: page
title: Simulated 3-linked Robot
description: Used an Actor Critic model to find the optimal gait of a 3-link robot.
img: 'assets/video/snake_bot/rl_singularity.gif'
importance: 1
category: Robotics
related_publications: false
---

This personal project explored how reinforcement learning can be used to generate locomotion strategies for a **3-link planar snake robot** without prescribing a reference gait. Conducted with support from **Dr. Howie**, the project focused on building a physics-based simulation environment and training a policy that could discover coordinated joint motion directly through interaction with the dynamics. Rather than hand-designing sinusoidal motion patterns, the goal was to let the robot learn how to move forward on its own.

The robot was modeled in **MuJoCo** as an articulated planar system with **nonholonomic contact constraints**, restricting the body to **SE(2)** motion while preserving realistic wheel-ground interactions. This made the problem more representative of real locomotion than a purely kinematic model, since propulsion depended on contact forces, friction, and coordinated body motion across multiple joints. The result was a nonlinear control problem in which forward movement emerged from the interaction between joint torques and the environment rather than from a predefined path.

To learn locomotion, I formulated the task as a reinforcement learning problem and trained an **actor-critic policy** using **PyTorch**, **Gymnasium**, and **generalized advantage estimation (GAE)**. The policy took in joint states and body motion information and output joint torques, learning how to coordinate the links to maximize forward displacement. This setup allowed the robot to learn gaits autonomously, which is especially useful for systems where classical control is difficult to apply because of strong joint coupling and contact-driven dynamics.

## Technical Highlights

- Developed a **3-link planar snake robot** simulation in **MuJoCo**
- Modeled **nonholonomic wheel-ground contact constraints** while restricting motion to **SE(2)**
- Designed and trained an **actor-critic reinforcement learning controller**
- Used **GAE** to improve policy gradient stability during training
- Implemented the learning pipeline in **Python, PyTorch, and Gymnasium**
- Studied how reward shaping affects emergent locomotion behavior in articulated robots

## Reward Design

A large part of the project was designing a reward that encouraged locomotion without allowing the policy to exploit unrealistic behavior. The reward combined several competing objectives. A forward-progress term encouraged net motion, while torque penalties discouraged wasteful actuation. A lateral-slip term promoted more physically meaningful motion by reducing inefficient sideways behavior.

I also introduced a **singularity-aware shaping term**, which turned out to be especially important. Without it, the policy could settle into static or inefficient configurations that looked locally favorable to the optimizer but did not produce useful locomotion. By explicitly penalizing bad postures, the training process became more stable and the policy was more likely to discover coordinated motion patterns that produced consistent forward travel.

## Results

The trained policy learned **coordinated oscillatory joint behavior** that produced forward propulsion without demonstration data or predefined trajectories. The resulting motion resembled a traveling-wave style gait, where phase-shifted joint motion generated net displacement through repeated interaction with the ground. This was one of the most interesting outcomes of the project, since it showed that the learning system could discover an effective locomotion strategy rather than simply copy one.

The project also highlighted how strongly **reward design affects RL behavior**. When singularity-aware shaping was not included, the policy was more likely to converge to static poses, inefficient oscillations, or unstable configurations. With the shaping term included, the learned behavior became more stable, torque usage became smoother, and forward locomotion improved. The project showed that for articulated robots, performance is driven not just by the learning algorithm itself, but by how carefully the task is structured.

## Key Takeaways

This project reinforced several important ideas for robotics and machine learning. First, reinforcement learning can be a powerful tool for discovering locomotion strategies in systems where hand-designed control is difficult. Second, reward shaping can completely determine whether the robot learns meaningful behavior or gets stuck in poor local optima. Although reinforcement learning is great, it has a major drawback. This method requires you to have a repeatible simulated environment that must be close to the actual environment, else the results of the model are useless.

More broadly, the project helped me deepen my understanding of **robot learning, articulated dynamics, physics-based simulation, and control through interaction**. It was a strong example of how machine learning can be applied to robotics not just for perception or classification, but for generating behavior in complex dynamical systems.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/rl_snake/reward_function.png" caption="Reward function for actor-critic model" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
      <img src="{{ '/assets/video/snake_bot/with_out_singularity_2000eps.gif' | relative_url }}" 
         alt="RL trajectory generation demo" 
         class="img-fluid rounded z-depth-1">
      <div class="caption mt-2">
         3-link robot without singularity term in the reward function
      </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
      <img src="{{ '/assets/video/snake_bot/with_singularity_2000eps.gif' | relative_url }}" 
         alt="RL trajectory generation demo" 
         class="img-fluid rounded z-depth-1">
      <div class="caption mt-2">
         3-link robot with singularity term in reward function
      </div>
    </div>
</div>

## Tools and Methods

**Simulation:** MuJoCo  
**Learning Framework:** PyTorch, Gymnasium  
**Method:** Actor-critic reinforcement learning with GAE  
**Focus Areas:** locomotion learning, articulated robot dynamics, reward shaping, nonlinear contact-driven behavior

## Repository

<a href="https://github.com/IGfun2code/controls_tesla" target="_blank" rel="noopener noreferrer">View the project repository on GitHub</a>