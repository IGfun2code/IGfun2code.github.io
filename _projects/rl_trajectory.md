---
layout: page
title: RL Trajectory Generation for Interactive Autonomous Driving
description: Closed-loop trajectory planning in CARLA using PPO for highway merges, left turns, and cut-in analysis.
img: assets/img/12.jpg
importance: 2
category: Autonomous Vehicles
related_publications: false
---

This project explores reinforcement learning for short-horizon trajectory generation in highly interactive driving scenarios using CARLA. The goal was to move beyond a purely reactive rule-based planner and instead train a policy that can decide when to yield, commit, slow down, or merge by generating a local trajectory in closed loop. The framework focuses on situations where autonomous vehicles still struggle most: unprotected left turns, highway merges, and sudden cut-ins.

The overall system keeps planning and control separated. A global A*-based route provides the long-horizon path, while a PPO policy reshapes the near-term trajectory online in response to nearby traffic. That local trajectory is then executed by a fixed CARLA PID controller. This architecture makes it possible to improve the planning layer while keeping low-level control unchanged, so performance differences can be attributed mainly to the learned planner rather than to retuned actuation.

A major focus of the project was building a repeatable evaluation pipeline instead of only training a policy once and reporting a few successful rollouts. I created controlled closed-loop scenarios in CARLA for left turns, highway merges, and abrupt cut-ins, then compared a classical baseline against a PPO-based planner using the same tracker and the same scenario-level metrics for safety, efficiency, and comfort.

## Technical Highlights

- Built a closed-loop autonomous driving pipeline in CARLA for interactive traffic scenarios
- Implemented a hierarchical stack with global A* routing, short-horizon local planning, and fixed PID tracking
- Developed a PPO planner that outputs a local trajectory and target speed instead of direct steering/throttle commands
- Designed observations that combine ego state, route-following status, gap-level merge features, and nearby-vehicle geometry
- Parameterized local trajectories using three lateral anchor offsets, a reconnection heading bias, and a target-speed command
- Converted PPO actions into smooth local paths with Catmull–Rom spline interpolation
- Created repeatable evaluation scenarios for unprotected left turns, highway merges, and TTC-triggered abrupt cut-ins

## Results

The classical baseline established a strong and measurable reference point across all three scenarios. In the left-turn and highway-merge evaluations, success rates ranged from **73% to 86%** depending on ego behavior and surrounding traffic aggressiveness. In the cut-in scenario, the same baseline controller successfully avoided collision at **70 km/h** but failed at **90 km/h**, revealing a clear sensitivity to higher closing speeds and showing the limitations of purely reactive control.

The learned planner showed its strongest results in the highway-merge task. With the tuned PPO policy, the system achieved an **86% success rate** and reduced the crash rate to **10%**. For successful runs, the **90th-percentile completion time was 17.4 s**, and the **90th-percentile maximum jerk was 547.138 m/s^3**. These results demonstrated that the RL trajectory-generation pipeline worked end-to-end in closed loop and could reliably complete interactive merges in a large majority of trials.

One of the most important outcomes from this project was architectural rather than numerical: PPO performed better when treated as a **local trajectory generator** instead of a selector over predefined route waypoints. Keeping the global route stable while allowing the policy to reshape only the short-horizon path made the planner more responsive to surrounding traffic and improved merge reliability. Reward design also mattered significantly, especially progress shaping, collision avoidance, route adherence, and anti-idling penalties.

The current system still has room to improve. Although PPO improved merge success, some successful trajectories remain abrupt, and the planner does not yet generalize as strongly to the left-turn and cut-in scenarios as it does to highway merging. That makes this project a strong foundation for future work on smoother policy behavior, broader scenario generalization, and robustness under noisy or degraded perception.

## Simulation

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
      <img src="{{ '/assets/video/rl_trajectory/left_turn.gif' | relative_url }}" 
         alt="RL trajectory generation demo" 
         class="img-fluid rounded z-depth-1">
    </div>
    <div class="col-sm mt-3 mt-md-0">
      <img src="{{ '/assets/video/rl_trajectory/highway_merge.gif' | relative_url }}" 
         alt="RL trajectory generation demo" 
         class="img-fluid rounded z-depth-1">
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/rl_results_compare.png" caption="Baseline vs PPO performance summary" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
Suggested visuals for the page: the CARLA merge environment, the local-trajectory generation method, and a baseline-versus-PPO results summary.

## Project Paper

<div class="mt-3">
  <iframe
    src="{{ '/assets/pdf/rl_trajectory.pdf' | relative_url }}"
    width="100%"
    height="900"
    style="border: 1px solid var(--global-divider-color); border-radius: 6px;"
  ></iframe>
</div>

<p class="mt-2">
  <a href="{{ '/assets/pdf/rl_trajectory.pdf' | relative_url }}" target="_blank" rel="noopener noreferrer">Open the RL trajectory paper in a new tab</a>
</p>

## Tools and Methods

**Simulation:** CARLA  
**Planning:** A* global routing, PPO short-horizon local planning  
**Control:** CARLA VehiclePIDController  
**Policy Design:** continuous action space, spline-based trajectory generation, interaction-aware reward shaping  
**Scenarios:** unprotected left turn, highway merge, abrupt cut-in

## Repository

<a href="https://github.com/IGfun2code/AD_RL_CARLA_Project" target="_blank" rel="noopener noreferrer">View the project repository on GitHub</a>