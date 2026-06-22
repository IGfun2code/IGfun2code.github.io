```markdown
---
layout: page
title: Learning Defensive Blocking for F1TENTH Racing
description: Developed a hybrid autonomy stack for two-agent F1TENTH racing that used PPO to learn defensive blocking decisions while keeping low-level path planning and control classical.
img: '/assets/img/f1tenth_blocking_cover.png'
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

This project explored whether reinforcement learning could improve **defensive behavior selection** in autonomous racing without replacing the full planning and control stack. Instead of training an end-to-end racing agent, I focused on a narrower and more practical problem: learning when and how an ego vehicle should block an opponent attempting an overtake. The goal was to determine whether learning adds value at the behavior layer while keeping path generation and tracking classical and interpretable.

The system used a **hybrid autonomy architecture**. A PPO policy selected high-level blocking parameters, while the downstream path was executed through a blocking-path generator, local RRT* replanning, and dynamic pure pursuit control. This structure made the learned component easier to train, compare, and debug, since the policy only had to reason about defensive intent rather than raw steering and speed control.

To make the learned behavior responsive to race context, I designed an observation space that combined opponent-relative motion history, distance, closing speed, track geometry, ego lateral offset, signed progress gap, and opponent probing behavior. The policy then produced continuous blocking decisions that adjusted how aggressively the ego vehicle moved to defend its line and how quickly it returned to the raceline afterward.

## Technical Highlights

- Built a **two-agent F1TENTH racing stack** for interactive defensive behavior
- Designed a **PPO policy** for blocking decisions instead of end-to-end racing control
- Used a **38-dimensional observation space** capturing opponent intent and track context
- Defined a **3-dimensional continuous action space** for blocking offset, hold time, and return-to-raceline rate
- Integrated RL with **blocking path generation, local RRT\***, and **dynamic pure pursuit**
- Created a randomized training environment with varied spawn gap, lateral offset, and yaw for robustness
- Compared the learned policy against a **history-based rule baseline** using the same downstream action interface
- Built a sim-to-real perception path using a **rear-facing RealSense + YOLO pipeline** for opponent-relative state estimation

## Results

The main result of the project was that both the PPO policy and the rule-based baseline achieved **100% task success** in the reported simulation evaluation. The baseline was slightly faster and more consistent, while the learned policy showed more adaptive and context-dependent blocking behavior.

The **rule-based baseline** completed the task in an average of **13.37 s**, with **6.92 s** of threat time, **8.41 s** of block time, and **0.83 s** response time. The **PPO policy** completed the task in an average of **13.73 s**, with **7.97 s** of threat time, **5.50 s** of block time, and **1.13 s** response time. Gap-over-time analysis showed that the rule baseline produced smoother, more monotonic defense, while PPO sometimes allowed the opponent to get closer before recovering and rebuilding the lead.

The key takeaway was that reinforcement learning did not outperform the baseline in raw speed or consistency, but it matched task success while producing more flexible blocking behavior that depended on opponent motion and race context. That result supports the idea that learning is most useful at the **behavior-selection layer**, where hard-coded thresholds can become brittle.

## Simulation

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
      <img src="{{ '/assets/video/f1tenth_blocking/ppo_blocking.gif' | relative_url }}" 
         alt="PPO defensive blocking in F1TENTH simulation" 
         class="img-fluid rounded z-depth-1">
    </div>
    <div class="col-sm mt-3 mt-md-0">
      <img src="{{ '/assets/video/f1tenth_blocking/rule_baseline.gif' | relative_url }}" 
         alt="Rule-based defensive blocking baseline" 
         class="img-fluid rounded z-depth-1">
    </div>
    <div class="col-sm mt-3 mt-md-0">
      <img src="{{ '/assets/img/f1tenth_blocking_metrics.png' | relative_url }}" 
         alt="Blocking evaluation metrics and gap analysis" 
         class="img-fluid rounded z-depth-1">
    </div>
</div>

The visuals compare the PPO blocker, the rule-based baseline, and the resulting evaluation metrics. Together they show how the learned policy produced more flexible context-dependent defense, while the baseline remained smoother and slightly faster.

## Key Takeaways

This project reinforced the value of **hybrid autonomy systems** that combine learning with classical robotics methods. Instead of forcing RL to solve the entire racing problem, I used it where it was most likely to help: deciding how to respond to uncertain opponent behavior. The project also showed that careful observation design, reward shaping, and evaluation tooling are just as important as the choice of learning algorithm.

More broadly, this work helped me better understand how to structure learning problems in robotics so they remain measurable, debuggable, and relevant to real autonomous systems. That is especially important in multi-agent settings, where the hardest part is often not the low-level control itself, but deciding how the robot should behave in response to another agent.

## Tools and Methods

**Platform:** F1TENTH simulation  
**Learning:** PPO behavior policy  
**Planning:** blocking path generation, local RRT* replanning  
**Control:** dynamic pure pursuit  
**Perception for deployment:** rear-view RealSense + YOLO  
**Focus Areas:** multi-agent autonomy, defensive behavior planning, hybrid learning and control

## Repository

<a href="https://github.com/IGfun2code/f1tenth_blocking_rl" target="_blank" rel="noopener noreferrer">View the project repository on GitHub</a>
```
