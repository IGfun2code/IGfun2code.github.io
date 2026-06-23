---
layout: page
title: Multi-Agent Reinforcement Learning for a Hip Exoskeleton
description: Ongoing project exploring whether a modular multi-agent controller can improve interpretability and assistance quality for a SCONE-based hip exoskeleton walking model.
img: '/assets/img/exo_marl_cover.png'
importance: 1
category: Bio Engineering
related_publications: false
skills:
  - TorchRL
  - PPO
  - Multi-Agent Reinforcement Learning
  - SCONE
  - Biomechanics
  - Temporal Convolutional Networks
---

This project explores whether a **modular multi-agent reinforcement learning controller** can improve the interpretability and quality of assistance in a hip exoskeleton walking task compared with a single monolithic controller. The environment is built in **SCONE / Hyfydy** using a musculoskeletal walking model with bilateral hip exoskeleton actuators. Instead of asking one policy to control everything, I split the problem into a **human policy** that controls muscle and lumbar actions and an **exoskeleton policy** that controls bilateral hip torques.

The main idea is to use learning where coordination is difficult while preserving enough structure to analyze what each part of the controller is doing. The human policy is modeled with an MLP, the exoskeleton policy uses a temporal network over hip kinematic history, and a centralized critic helps estimate shared value during PPO training. The long-term goal is to compare total reward, gait stability, and the physical meaning of learned assistance between the modular MARL system and a monolithic baseline.

## Current Progress

The project has moved from a single-agent imitation RL pipeline toward a working **TorchRL PPO MARL framework**. I first reproduced the original SCONE setup in TorchRL with environment wrapping, observation normalization, replay minibatches, evaluation, and checkpointing. After that baseline was stable, I modified the environment to expose separate observations for the human actor, exo actor, and critic while still preserving one flat actuator vector for SCONE.

The current architecture uses a combined action layout with **18 muscle actions, 2 exoskeleton torque actions, and 1 lumbar action**. The human actor receives a privileged human-state observation, while the exoskeleton actor receives a temporal history of bilateral hip angle and hip angular velocity. The critic receives a centralized observation that combines both views, allowing centralized training with decentralized execution.

A major recent step was integrating a **temporal convolutional exoskeleton actor** and a custom joint action distribution so TorchRL PPO can sample one flat environment action while still recomputing the correct joint log probability during updates. I also added staged training modes—**human_only, exo_only, and joint**—to reduce nonstationarity and make learning easier to debug.

## Technical Highlights

- Built a **SCONE / Hyfydy MARL environment** for bilateral hip exoskeleton assistance
- Split the controller into a **human actor**, **exo actor**, and **centralized critic**
- Implemented **TorchRL PPO** with checkpointing, evaluation, and resume support
- Added **ObservationNorm** and **StepCounter** transforms for stable training
- Designed a **temporal exoskeleton policy** using hip angle and hip angular velocity history
- Created a custom **joint human-exo TanhNormal distribution** for flat SCONE action compatibility
- Added staged training modes to separately train walking recovery, assistance learning, and joint cooperation
- Instrumented logging for reward components, exo actions, human actions, KL, timings, and human-exoskeleton interaction metrics

## Current Research Focus

Right now the project is less about final performance numbers and more about making the training process both **stable and physically meaningful**. One major focus is validating the **human-exoskeleton interaction (HEI) reward**, which penalizes resistive mechanical power so the exoskeleton learns assistance rather than interference. Another focus is stabilizing long-running joint training runs, especially because the native SCONE/Hyfydy simulator can accumulate resources and crash after millions of frames if resets are not handled carefully.

This has made the project a strong systems-level exercise in robotics learning. Beyond reinforcement learning itself, much of the work has involved environment design, distribution interfaces, reward interpretation, simulator debugging, staged training, and logging infrastructure. Those pieces are critical for making the learned behavior trustworthy rather than just optimizing a single scalar reward.

## Current Challenges

The largest remaining engineering challenge is **long-run native stability** in SCONE/Hyfydy. Earlier versions of the training loop repeatedly reloaded the simulator model on reset, which likely contributed to resource leaks and eventual segmentation faults during long experiments. The current direction is to reuse the loaded model during normal training resets, reduce unnecessary storage calls, and rely on frequent checkpoints so progress is not lost during native crashes.

Another active challenge is making sure the exoskeleton behavior is not only reward-maximizing, but also biomechanically sensible. A high total reward does not necessarily mean the exoskeleton is providing meaningful help, so I am tracking torque magnitude, saturation, smoothness, and the sign of exoskeleton mechanical power in addition to imitation reward and episode length.

## Next Steps

The immediate next step is to finalize the simulator reset strategy and run smaller controlled stability tests before returning to long joint training. After that, I plan to expand diagnostics around the HEI reward and compare multiple conditions: no exoskeleton, human-only training, exo-only training, joint training, and HEI reward on versus off.

Once training is stable, the project will shift toward answering the actual research question: whether the modular MARL architecture improves assistance quality, interpretability, or robustness relative to a single monolithic controller. That comparison will use not only total reward, but also gait duration, torque behavior, effort penalties, tracking quality, and human-exoskeleton power interaction.

## Tools and Methods

**Simulation:** SCONE / Hyfydy  
**Learning:** TorchRL PPO  
**Control Architecture:** human actor + exo actor + centralized critic  
**Human Policy:** MLP  
**Exo Policy:** temporal convolution over bilateral hip kinematic history  
**Focus Areas:** multi-agent reinforcement learning, biomechanics, assistive robotics, reward design, simulator stability
