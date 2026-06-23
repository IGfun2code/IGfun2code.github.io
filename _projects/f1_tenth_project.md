---
layout: page
title: Learning Defensive Blocking for F1TENTH Racing
description: Developed a hybrid autonomy stack for two-agent F1TENTH racing that used PPO to learn defensive blocking decisions while keeping low-level path planning and control classical.
img: 'assets/video/f1tenth/project/training_ppo_model.gif'
importance: 2
category: Autonomous Vehicles
related_publications: false
skills:
  - F1TENTH
  - PPO
  - Multi-Agent Autonomy
  - Behavior Planning
  - RRT*
  - Pure Pursuit
---

This project studied **defensive blocking for two-agent autonomous racing** in the F1TENTH setting. Rather than training an end-to-end racing agent, I focused on the higher-level question of whether reinforcement learning could improve **behavior selection** in interactive racing while keeping the lower-level planning and control stack classical. The ego vehicle learned how aggressively to block, how long to hold the block, and how quickly to return to the raceline, while nominal path following, local RRT* replanning, and pure pursuit tracking remained fixed and interpretable.

The motivation behind this structure was to isolate where learning actually adds value. A rule-based controller can react to relative distance and speed, but blocking is not purely geometric: the ego vehicle must reason about whether the opponent is truly attempting a pass, which side is threatening, how much lateral defense is appropriate, and when it is safe to return to the optimal line. By restricting RL to the behavioral decision layer, I could compare a learned blocking policy against a rule-based defense algorithm without changing the downstream motion stack.

The final system used a **hybrid autonomy architecture**. The ego vehicle used PPO to output continuous blocking parameters, while the opponent followed a nominal raceline and relied on local RRT* replanning when needed. Both cars tracked their selected paths with dynamic pure pursuit. This produced a modular pipeline in which learning handled interactive decision-making and classical methods handled path generation and tracking.

## Technical Highlights

- Built a **two-agent F1TENTH racing stack** for interactive defensive behavior
- Restricted RL to the **behavioral decision layer** while keeping path generation and tracking classical
- Designed a **38-dimensional observation space** combining opponent-relative motion history and track context
- Used a **3-dimensional continuous action space** for blocking offset magnitude, hold time, and return-to-raceline rate
- Integrated PPO with **blocking path generation, local RRT\***, and **dynamic pure pursuit**
- Trained the policy in simulation with randomized spawn conditions and a **bimodal curriculum**
- Designed a **rear-facing RealSense + YOLO** perception pipeline for sim-to-real transfer of the same opponent-relative state structure

## Policy Design

The policy observed a compact state vector built from both opponent history and instantaneous track geometry. A short history of relative position and velocity helped the model infer whether the opponent was genuinely attempting to pass or merely probing, while the remaining features encoded relative distance, closing speed, curvature ahead, track width, ego lateral offset, signed progress gap, and other context relevant to defensive behavior.

The PPO policy output three continuous values: **blocking offset**, **hold time**, and **return-to-raceline rate**. Instead of asking the model to choose both direction and magnitude, the blocking side was inferred from the opponent’s position and the policy only controlled how strongly and how long to defend. This kept the action design compact while still allowing the model to produce small corrections in turns and more aggressive offsets on straights.

The reward function was shaped around the central tradeoff of the task: protect track position without blocking unnecessarily. The policy was rewarded for forward progress and remaining ahead, and it received pressure-conditioned reward for using blocking only when the opponent posed a real threat. Penalties discouraged excessive lateral deviation, abrupt action changes, being passed, contact, track collisions, and wasted time. This prevented a degenerate solution where the ego vehicle blocked constantly regardless of context.

## Training Setup

Training was performed in a two-agent F1TENTH simulation with the ego controlled by the RL blocking policy and the opponent controlled by a non-learning stack. To improve robustness, the initial conditions were randomized in longitudinal gap, lateral offset, and yaw so that the policy encountered both threatening and non-threatening interactions.

I trained the model with **PPO (Stable-Baselines3)** for **100,000 timesteps**. The observation space was 38-dimensional, the action space was a 3-D continuous vector, and the training curriculum was intentionally bimodal: **70% threatening spawns** within 2 meters and **30% easier spawns** from 3–6 meters. This exposed the policy to both urgent and non-urgent defensive situations and encouraged selective blocking instead of constant defensive motion.

## Results

The learned PPO policy and the rule-based baseline both achieved **100% task success** in the reported simulation evaluation. This meant the task was reliably solved by both approaches, so the main differences appeared in efficiency and behavior rather than binary completion.

The **baseline** was slightly faster and more stable, finishing in **13.37 s** on average with **6.92 s** of threat time, **8.41 s** of block time, and **0.83 s** response time. The **PPO policy** finished in **13.73 s** on average with **7.97 s** of threat time, **5.50 s** of block time, and **1.13 s** response time. The baseline therefore showed a small advantage in pace and responsiveness, while PPO spent less time blocking continuously and reacted in a more selective, context-dependent way.

Gap-over-time analysis showed that the baseline produced smoother and more monotonic defensive behavior, while the PPO policy sometimes allowed the opponent to get closer before recovering and rebuilding the lead. In several high-pressure scenarios, the learned policy restored a larger positive track-progress gap after the opponent nearly reached the ego vehicle. This suggests that its value came from **adaptability and recovery behavior**, not from raw speed or consistency.

The main takeaway from the project was that reinforcement learning was useful when applied to the **behavior-selection layer**, where fixed thresholds become brittle, but it did not automatically outperform a well-structured baseline in all metrics. PPO matched the baseline in task completion while demonstrating more flexible, context-aware defensive behavior.

## Simulation

<div class="row">
    <div class="col-12 mt-3">
        {% include figure.liquid loading="eager" path="assets/video/f1tenth/project/project_agressive_ppo_2.gif" caption="Simulation where blue vehicle uses PPO model to block approaching orange vehicle." %}
    </div>
</div>

The visuals compare the PPO blocker, the rule-based baseline, and the resulting evaluation metrics. Together they show how the learned policy produced more flexible context-dependent defense, while the baseline remained smoother and slightly faster.

## Key Takeaways

This project reinforced the value of **hybrid autonomy systems** that combine learning with classical robotics methods. Instead of forcing RL to solve the entire racing problem, I used it where it was most likely to help: deciding how to respond to uncertain opponent behavior. The project also showed that careful observation design, reward shaping, and evaluation tooling are just as important as the choice of learning algorithm.

More broadly, this work helped me better understand how to structure learning problems in robotics so they remain measurable, debuggable, and relevant to real autonomous systems. That is especially important in multi-agent settings, where the hardest part is often not the low-level control itself, but deciding how the robot should behave in response to another agent.

## Project Paper

<div class="mt-3">
  <iframe
    src="{{ '/assets/pdf/f1tenth_project.pdf' | relative_url }}"
    width="100%"
    height="900"
    style="border: 1px solid var(--global-divider-color); border-radius: 6px;"
  ></iframe>
</div>

<p class="mt-2">
  <a href="{{ '/assets/pdf/f1tenth_project.pdf' | relative_url }}" target="_blank" rel="noopener noreferrer">Open the F1TENTH blocking paper in a new tab</a>
</p>

## Tools and Methods

**Platform:** F1TENTH simulation  
**Learning:** PPO (Stable-Baselines3)  
**Planning:** nominal raceline, local RRT* replanning, blocking path generation  
**Control:** dynamic pure pursuit  
**Perception for deployment:** rear-facing RealSense + YOLO  
**Focus Areas:** defensive behavior planning, multi-agent autonomy, hybrid learning and control, sim-to-real transfer

## Repository

<a href="https://github.com/IGfun2code/f1tenth_blocking_rl" target="_blank" rel="noopener noreferrer">View the project repository on GitHub</a>



