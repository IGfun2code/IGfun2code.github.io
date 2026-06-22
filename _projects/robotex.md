---
layout: page
title: Robotex Competition
description: 2nd place in the international Robotics competition
img: assets/img/robotex/robot.png
importance: 4
category: Robotics
skills:
  - SolidWorks
  - 3D Printing
  - DFMA
  - System Integration
  - Sensor Packaging
giscus_comments: true
---

This project was an end-to-end autonomous robotics effort for the **Robotex International Mini Moon Rover Competition** in Estonia. The objective was to design and build a compact rover from scratch that could autonomously traverse an obstacle field, identify and collect specific colored rock samples, and return to base. The system had to fit within a **20 cm³ box**, remain under **2 kg**, and stay within a strict **$150 budget**, which made every design choice a tradeoff between capability, manufacturability, and reliability. Our team ultimately earned **2nd place** in the international competition representing the USA.

My role focused on the **mechanical design and system integration** of the robot. I led the CAD and manufacturing of the rover structure in **SolidWorks**, rapidly prototyped parts using **PLA 3D printing**, and worked across the major subsystems to make the full robot practical within the time and cost limits. I contributed to the **chassis**, **intake**, and **sorting architecture**, and collaborated closely on integrating the sensors and electronics package, including **ToF sensors, touch sensors, color sensors, an IMU, microcontroller hardware, and a solar panel**. The final architecture centered around four primary systems: **drive, intake, sorting, and solar**.

A major design priority was creating a rover that could remain stable on uneven terrain while still fitting all collection and sorting hardware into a very small footprint. I designed a **low-center-of-gravity 4-wheel-drive chassis** to maximize stability and ground contact, while using additive manufacturing to create custom geometry that would have been difficult to achieve with off-the-shelf parts alone. I also performed basic speed, mass, and torque calculations to help determine the proper motor selection for the drivetrain.

For sample collection, I designed an **intake module** that acted like a compact vertical conveyor, pulling rocks up the front face of the rover and transferring them toward the sorting and storage system. I also helped develop the **sorting and dispensing layout**, where rocks needed to be identified by color and routed correctly inside the robot. Because the rover had to operate autonomously, the mechanical packaging had to work cleanly with the sensing and electronics stack without causing obstructions or assembly bottlenecks.

## Technical Highlights

- Designed and manufactured an autonomous mini moon rover for Robotex International
- Led rover CAD development in **SolidWorks** and rapid prototyping with **PLA 3D printing**
- Created a **low-center-of-gravity 4WD chassis** for improved stability on rough terrain
- Designed a **vertical conveyor-style intake** for rock collection and transfer
- Helped develop the **sorting and dispensing system** for colored sample handling
- Integrated mechanical packaging around **ToF sensors, touch sensors, color sensors, IMU, control electronics, and solar hardware**
- Balanced performance, manufacturability, assembly speed, and cost under a **$150 budget**

## Results

The rover was successfully built and made operational under extreme competition constraints, and our team earned **2nd place in the international Robotex competition**. One of the strongest outcomes of the project was not just the final placement, but the fact that the robot remained competitive even after major failures during travel. After air transport, critical electromechanical components were damaged, and the intake system performance dropped to roughly **40% effectiveness** because the conveyor developed too much play and began inverting on itself.

Under severe time pressure in an unfamiliar environment/location, I helped repair and adapt the robot using whatever resources were available on site. In the intake system, I reinforced weak points with improvised supports to make the conveyor more rigid and improve sample capture reliability. In parallel, when one motor fault limited turning behavior, I adapted the autonomy logic around the sensing stack so the rover could still navigate by using **ToF-based scanning** and a **greedy-like obstacle-avoidance strategy** that compensated for the hardware limitation. Even with these setbacks, we were able to make the rover operational in time for competition and finish in second place.

## Key Challenges

The biggest challenge in this project was designing for a competition environment that was only partially known ahead of time. The rover had to operate on rough terrain, collect samples reliably, and survive transportation and rapid setup, all while staying inside strict cost and packaging limits. Because so much of the budget had to go toward motors and electronics, the mechanical system had to be highly efficient in both material use and assembly complexity.

Another challenge was designing under uncertainty. Looking back, I realized the drivetrain was influenced by my prior experience with RC cars and combat robots, so I chose a familiar **4-wheel tank-drive style approach** rather than a more terrain-optimized treaded or 6-wheel system. While the final rover performed well enough to place internationally, that tradeoff taught me the importance of designing more directly for the operating environment rather than relying too heavily on previous design intuition.

## What I Learned

This project taught me a great deal about **rapid prototyping, DFMA, electromechanical integration, and engineering under pressure**. It reinforced how important it is to think about **assembly during the design phase**, especially when using 3D-printed parts that can introduce tolerance and warping issues. It also taught me how valuable adaptability and team trust are when a system fails close to deployment.

More broadly, Robotex was one of my strongest experiences in building a robot as a fully integrated system rather than as separate mechanical and software pieces. The project required packaging, sensing, structure, collection, autonomy, and troubleshooting to all work together. That combination of design ownership, hands-on fabrication, and fast iteration is a big reason why this project remains one of the most meaningful robotics experiences I’ve had.

<div class="row">
    <div class="col-12 mt-3">
        {% include figure.liquid loading="eager" path="assets/img/robotex/robot.png" caption="CAD model of Robotex Rover" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-12 mt-3">
        {% include figure.liquid loading="eager" path="assets/img/robotex/stage.png" caption="Robotex competition field" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-12 mt-3">
        {% include figure.liquid loading="eager" path="assets/img/robotex/team.png" caption="Robotex team Image: Ranit, Ishan, Raghav (left to right)" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="row">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/robotex/subsystems.png" caption="Robot key systems" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid loading="eager"  path="assets/img/robotex/bom.png" caption="Robot BOM" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
The rover architecture centered around four main subsystems: intake, drive, sorting, and solar, all developed within an approximately $150 total budget.

## Tools and Methods

**CAD:** SolidWorks  
**Manufacturing:** PLA 3D printing, rapid prototyping  
**Systems:** drivetrain, intake, sorting, sensor integration, electronics packaging  
**Sensors and Components:** ToF sensors, touch sensors, color sensors, IMU, microcontroller, solar panel  
**Focus Areas:** DFMA, autonomous robotics, compact electromechanical design, troubleshooting under constraints
