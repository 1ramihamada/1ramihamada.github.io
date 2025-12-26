---
title: Colonoscopy Robot Development
permalink: /projects/colonoscopy/
author_profile: true
layout: single
classes: wide
---

## Overview
This project involved developing and integrating software components for a teleoperated colonoscopy research platform. The system was used to study navigation, sensing, and operator interaction during controlled experimental trials.

My contributions focused on control software, sensing integration, and operator interfaces. This included implementing multi-degree-of-freedom control using a game controller, integrating magnetic tracking and vision data, and supporting real-time teleoperation and visualization.

<figure class="align-center">
  <img src="/assets/images/colon_setup.png"
       alt="Experimental colonoscopy robot setup"
       style="max-width: 800px; display: block; margin: 0 auto;">
  <figcaption style="max-width: 800px; margin: 0 auto;">
    <strong>Fig. 10.</strong> Experimental setup used for performing the case study, including:
    ① CH gripping mechanism on a rotary stand,
    ② passive rotary stand,
    ③ PENTAX EC-3840LK colonoscope,
    ④ internal tube monitoring for marker detection,
    ⑤ NDI Aurora magnetic tracker,
    ⑥ feeder mechanism,
    ⑦ Xbox 360 joystick controller,
    ⑧ U-shaped tube with internal markers,
    and ⑨ end-effector camera (MISUMI XD-VB164A03LH-120).
  </figcaption>
</figure>

## Control & Teleoperation
I wrote a 4-DOF control code in python running on a jetson nano, allowing an operator to teleoperate the system using an Xbox controller. The input mapping was designed to provide smooth, continuous control over articulation and motion during navigation tasks.

This control interface was used during experimental trials to evaluate usability and responsiveness under realistic operating conditions.

## Sensing & Localization
The system integrates **NDI Aurora magnetic tracking** to estimate the position of the colonoscope tip during navigation. I worked on integrating the tracking data into the software stack and making it available for visualization and analysis during experiments.

This provided spatial context beyond what was available from the onboard camera alone, especially in regions where visual feedback was limited.

## Vision Integration
Live video from the end-effector camera was incorporated into the operator interface to support navigation and situational awareness. In addition, a trained vision model was integrated into the software pipeline to perform **real-time tumor detection**, with results displayed alongside the live video feed.

This integration allowed sensing and perception outputs to be evaluated in the context of teleoperated navigation.

## Remote Operation
To support remote experimentation, the system was configured for **secure, low-latency teleoperation** using Tailscale. This enabled the platform to be controlled across networks without exposing open ports, while maintaining responsiveness suitable for real-time use.

The setup was validated during long-distance operation to confirm stability and acceptable control latency.

## Impact
This project contributed to the development of a research platform for studying teleoperated colonoscopy with integrated sensing and perception. My work supported experimental evaluation of control interfaces, localization methods, and real-time vision assistance in a medical robotics context.
