---
layout: page
title: F1 Tenth Racing
description: Built ROS2 autonomy modules for an F1TENTH racing platform, combining LiDAR processing, SLAM/localization, and closed-loop control for indoor navigation. I also trained a YOLO detector on a custom dataset to support car-behind-ego perception for race-relevant interactions.
img: 'assets/video/f1tenth/race_example.gif'
importance: 1
category: Autonomous Vehicles
related_publications: false
skills:
  - ROS 2
  - LiDAR Perception
  - PID Control
  - Particle Filter Localization
  - RRT/RRT*
  - Pure Pursuit
  - Model Predictive Control
  - YOLO
  - Autonomous Racing
---

This project page summarizes the autonomy modules I developed throughout the F1TENTH autonomous racing course. The class followed the full stack of a small-scale autonomous race car: low-level safety and reactive navigation, map-based localization, sampling-based motion planning, trajectory tracking, model predictive control, visual perception, and a final project focused on competitive multi-agent racing behavior.

The goal was to move from simple sensor-to-control policies toward a complete racing architecture that could perceive the environment, estimate vehicle state, plan around obstacles, track a raceline, and reason about other vehicles on the track. Each assignment added one layer to the stack, and the later projects reused earlier components such as LiDAR preprocessing, occupancy-grid reasoning, waypoint tracking, and speed/lookahead tuning.

## Reactive control and obstacle avoidance

The first autonomy modules focused on driving directly from LiDAR observations. In **wall following**, I implemented a PID-based controller that estimated the car's distance and orientation relative to a track wall and converted the cross-track error into steering commands. This introduced the core closed-loop control problem in F1TENTH: noisy range measurements, limited actuation, speed-dependent steering response, and the need to tune a controller that remains stable at race speeds.

In **gap following**, I extended the LiDAR pipeline from tracking a wall to selecting safe free space. The algorithm filtered the scan, created a safety bubble around the closest obstacle, identified the largest navigable gap, and steered toward a goal point inside that gap. This produced a more flexible reactive obstacle-avoidance behavior, useful for cluttered tracks and as a baseline for later planning methods.

Together, these labs formed the first racing baseline: sense the nearest geometry, choose a local steering target, and command speed based on how confidently the car could continue through the environment.

## Localization and map-based racing

After the reactive racing stack, I worked on **particle filter localization** to estimate the vehicle's pose on a known map. The particle filter represented uncertainty with many sampled vehicle poses, propagated those particles using a motion model, scored them with LiDAR measurements, and resampled particles based on likelihood. This made the autonomy stack less dependent on purely local LiDAR geometry and enabled map-based racing behavior.

This localization layer became important for the later pure-pursuit and planning modules because the car needed a reliable pose estimate before it could follow global waypoints or reason about its location on the racetrack.

## Planning with RRT/RRT* and occupancy grids

For motion planning, I implemented sampling-based planning using **RRT/RRT\*** with an occupancy grid representation. The planner sampled candidate states, expanded a search tree through free space, checked candidate edges against obstacles, and generated a collision-free local path toward a target. The RRT* version added the idea of cost-aware improvement through nearby nodes and rewiring, making the planner more suitable for generating smoother or lower-cost paths.

This project connected perception to planning: LiDAR and map information were converted into an occupancy grid, the planner searched over free cells, and the resulting path could be passed to a local controller. It also exposed many practical issues that matter in robotics software, including coordinate-frame consistency, grid indexing, collision checking, and debugging ROS 2 package builds and launch behavior.

## Trajectory tracking with pure pursuit

For waypoint tracking, I built a **pure pursuit** controller that selected a lookahead point on a raceline or planned path and computed the curvature required to drive toward it. I also worked on a global pure-pursuit node that loaded waypoints, visualized target points, and adjusted behavior based on track geometry.

A major part of this work was tuning speed and lookahead together. Larger lookahead distances produced smoother tracking but could cut corners, while smaller lookahead distances improved responsiveness but could introduce oscillation. I used turn-dependent scaling so the car could slow down and reduce lookahead in sharper sections while driving faster on straights.

## Model predictive control

The **MPC** assignment introduced a more model-based approach to racing control. Instead of choosing steering from a single lookahead target, MPC optimizes a sequence of future controls over a prediction horizon while considering vehicle dynamics and constraints. This made it possible to reason about speed, steering limits, and upcoming curvature more explicitly than a purely reactive controller.

In the context of the full stack, MPC served as a comparison point for pure pursuit: pure pursuit is simple, fast, and reliable for waypoint tracking, while MPC offers a more principled framework for handling constraints and optimizing future motion.

## Vision perception with YOLO

The perception module focused on **YOLO-based object detection** for identifying other F1TENTH vehicles. This added a camera-based perception layer to the autonomy stack, complementing LiDAR and map-based reasoning. For racing, detecting another car is not only a perception task; it directly affects behavior planning because the ego car must decide whether to follow, block, overtake, or return to the optimal line.

In my final project workflow, the vision component was especially relevant for rear-facing opponent awareness. The broader goal was to use visual detections to estimate whether another vehicle was attempting to pass and then feed that information into a higher-level racing strategy.

## Final project: vision and reinforcement learning for racing strategy

For the final project, my team explored a vision and reinforcement learning strategy for autonomous racing. The project was motivated by the difficulty of manually tuning waypoint following and speed control across the full map, especially when another vehicle is trying to overtake, block, or force a defensive maneuver.

The high-level architecture separated the stack into perception, behavior planning, local planning, and control. The RL policy was designed to reason about opponent behavior and select among racing behaviors such as following the optimal line, blocking, overtaking left or right, disengaging, and adjusting target speed or lookahead. The local planners and low-level controller would then turn those behavior decisions into executable motion.

In the final scoped version, the project emphasized defensive racing behavior: using opponent-relative state and track context to decide when to hold the raceline and when to block an attempted overtake. This connected the full course progression into one system: YOLO-style perception for opponent awareness, map and LiDAR information for state estimation, RRT/RRT* and raceline planning for feasible paths, and pure pursuit or MPC for control.

## What I learned

Across these projects, I built and tested the major components of an autonomous racing pipeline:

- LiDAR preprocessing and reactive control for wall following and gap following.
- PID tuning for closed-loop vehicle control.
- Particle-filter localization for map-based pose estimation.
- Occupancy-grid construction and sampling-based planning with RRT/RRT*.
- Waypoint tracking with pure pursuit and dynamic speed/lookahead scaling.
- Model predictive control for constrained trajectory tracking.
- YOLO-based visual perception for detecting other vehicles.
- Behavior-level racing strategy using reinforcement learning concepts.

The main takeaway was that autonomous racing is a systems problem. A fast controller alone is not enough; the car must localize reliably, choose feasible paths, track them smoothly, adapt speed to curvature, and reason about other vehicles. The course projects gave me experience integrating these pieces into a ROS 2 racing stack and debugging the practical issues that appear when algorithms move from clean theory to a real-time autonomous vehicle platform.
