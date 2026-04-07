---
layout: page
title: Motion Control for Autonomous Vehicles and Drones
description: Integrated planning, state estimation, optimal control, and adaptive flight control in Webots.
img: assets/img/12.jpg
importance: 3
category: Autonomous Vehicles
related_publications: false
---

This project explores how modern control and autonomy methods can be applied across both ground and aerial systems within a shared simulation workflow in Webots. On the ground side, I developed controllers and planning pipelines for a Tesla vehicle driving on the Schenley Park map, progressing from baseline trajectory tracking to obstacle-aware replanning and localization. In parallel, I extended the same control-focused approach to a quadrotor, designing an adaptive controller that maintained flight performance under severe actuator degradation.

For the autonomous vehicle, I first built PID and pole-placement controllers for lateral and longitudinal regulation, then improved the stack with LQR control and A* replanning to handle dynamic obstacles more effectively. I also implemented an EKF-based SLAM pipeline to support localization and map-consistent navigation.

For the drone, I designed a Model Reference Adaptive Control (MRAC) architecture on top of a baseline LQR controller to improve robustness when a motor experiences thrust loss.

What makes this project especially interesting is the range of control ideas applied to different vehicle classes. Rather than focusing on a single controller, I compared classical feedback control, optimal control, estimation, motion planning, and adaptive control in one end-to-end autonomy workflow. The result was a broader understanding of how controller choice changes system behavior, tracking quality, robustness, and recovery under uncertainty or failure.

## Technical Highlights

- Built lateral and longitudinal control pipelines for a simulated autonomous vehicle in Webots
- Linearized vehicle dynamics and applied PID, pole-placement, and LQR control strategies
- Integrated A* replanning for obstacle-aware route adjustment around moving vehicles
- Implemented EKF-based SLAM for localization and trajectory execution
- Designed an MRAC controller with an LQR baseline for quadrotor fault-tolerant flight
- Evaluated performance through trajectory error, task completion time, and robustness to actuator degradation

## Results

The ground vehicle achieved an average trajectory discrepancy of **0.47 m** using the baseline PID and pole-placement control approach. After upgrading the autonomy stack with **LQR control and A* replanning**, the vehicle completed the route **20% faster** when navigating around a moving obstacle. With the addition of **EKF-based SLAM**, the platform was able to complete the track while staying within an average of **0.7 m** of the intended trajectory.

On the aerial side, the quadrotor control system demonstrated strong fault tolerance. The **MRAC + LQR** architecture was able to keep the drone flying with up to **68% thrust loss in one motor**, showing the value of adaptive control for maintaining performance under large model mismatch and actuator failure.

<div class="row">
    <div class="col-12 mt-3">
        {% include figure.liquid loading="eager" path="assets/img/modern_control_theory/a_star_path.jpg" title="A * generated path to overtake vehicle in lane" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-12 mt-3">
        {% include figure.liquid loading="eager" path="assets/img/modern_control_theory/a_star_vehicle_specs.png" title="Vehicle response with A*" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-12 mt-3">
        {% include figure.liquid loading="eager" path="assets/img/modern_control_theory/LQR_vehicle_specs.png" title="Vehicle response with LQR" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Example visuals for the project 3: LQR and A star method to overtake a vehicle on the path.
</div>

## Tools and Methods

**Simulation:** Webots  
**Methods:** PID, Pole Placement, LQR, A*, EKF-SLAM, MRAC  
**Focus Areas:** trajectory tracking, dynamic obstacle handling, localization, robustness, adaptive control

## Repository

<a href="https://github.com/IGfun2code/controls_tesla" target="_blank" rel="noopener noreferrer">View the project repository on GitHub</a>