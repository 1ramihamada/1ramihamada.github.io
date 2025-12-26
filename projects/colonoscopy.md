---
title: Colonoscopy Robot Development
permalink: /projects/colonoscopy/
author_profile: true
layout: single
classes: wide
---

## Overview
This project developed a software-controlled framework for robotic colonoscopy aimed at reducing operator fatigue during manual endoscope procedures. The system enables teleoperated control of a colonoscope through a robotic platform with four motorized degrees of freedom.

My contributions focused on the software side: teleoperation control, sensing integration, and the operator interface. I implemented game-controller-based control, integrated live video from a miniature end-effector camera, added magnetic localization using an NDI Aurora tracker, and integrated a pre-trained vision model to assist with tumor detection during navigation.

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
I implemented a 4-DOF control system in Python on a Jetson Nano, using a wired Xbox 360 controller for teleoperation. Controller inputs were mapped to the robot’s degrees of freedom to support smooth, continuous navigation control.

Motor commands were issued through the Dynamixel SDK, with ROS used as middleware to connect control, sensing, and visualization. For remote testing, the system was connected using Tailscale and validated through long-distance operation with minimal perceived delay.

## Sensing & Localization
I integrated NDI Aurora magnetic tracking to estimate the position and orientation of the colonoscope tip during navigation. Tracking data was acquired in real time and recorded during maneuvering, then used alongside the video feed to provide spatial context during experiments.

## Vision Integration
I streamed live video from the end-effector camera into the operator interface and integrated a pre-trained vision model for real-time tumor detection. Detection results were overlaid on the video stream so the operator could see flagged regions without switching views.

## Impact
This project contributed to the development of a research platform for studying teleoperated colonoscopy with integrated sensing and perception. My work supported experimental evaluation of control interfaces, localization methods, and real-time vision assistance in a medical robotics context.
