---
title: Liquid Metal 3D Printer
permalink: /projects/3d-printing/
author_profile: true
layout: single
classes: wide
---

## Overview
I built the software and control infrastructure for a modified 3D printer that fabricates stretchable, liquid metal strain sensors.

While the mechanical modifications to the printer were handled separately, I was responsible for the entire software stack. This included the ROS2 control architecture, printer–dispenser coordination, GUI development, firmware modifications, and calibration tools for repeatable liquid metal deposition.

<figure class="align-center">
  <img src="/assets/images/system_diagram.png" 
       alt="Physical system overview" 
       style="max-width: 900px; display: block; margin: 0 auto;">
</figure>

## Sensor Fabrication Workflow
The system enables a layered fabrication process for stretchable strain sensors without requiring a dedicated mold for each geometry.

Liquid silicone is poured into a base mold and flattened using a lid press. After curing, gallium is printed directly onto the silicone, solidified in a freezer, and encapsulated with a second silicone layer to complete the sensor.

This approach replaces an earlier mold-based injection method that was slow and difficult to scale to complex designs. Direct printing enables faster iteration, lower cost, and fabrication of complex sensor geometries without custom tooling.

<figure class="align-center">
  <img src="/assets/images/completed_sensor.png"
       alt="Completed liquid metal sensor"
       style="max-width: 500px; display: block; margin: 0 auto;">
</figure>

## System Architecture
I designed the control system around ROS2 to separate printer motion, pneumatic control, and user interaction into independent nodes. This decoupling keeps the printer’s G-code stream fast and stable while allowing real-time adjustment of extrusion parameters.

- **Printer Node:** Streams G-code to the Prusa i3 MK3S+ and handles coordinate transforms for mold alignment. A dedicated reader thread queues all serial input so writes stay non-blocking and the motion buffer never stalls. Position queries use a ROS service for synchronous reads during registration.
- **Dispenser Node:** Implements the Nordson Ultimus V serial protocol, including STX/ETX framing, checksum calculation, and ENQ/ACK handshaking. Commands go through a worker thread with debouncing to prevent duplicate toggles when the GUI sends rapid start/stop calls.
- **GUI Node:** Provides real-time control for jogging, registration, and parameter tuning without interrupting the printer’s serial buffer. An emergency stop button publishes to a shared topic that both nodes subscribe to, triggering M112 on the printer and immediate shutoff on the dispenser.

<figure class="align-center">
  <img src="/assets/images/ros_diagram.png"
       alt="ROS2 control architecture"
       style="max-width: 800px; display: block; margin: 0 auto;">
  <figcaption style="max-width: 800px; margin: 0 auto;">
  ROS2 node architecture showing how motion execution, pneumatic control, and the GUI are decoupled and synchronized during printing.
  </figcaption>
</figure>

## Software Control & GUI
I developed a wxPython-based GUI that serves as the control layer for the ROS2 system. Running the GUI as its own node keeps the interface responsive during long print jobs and allows manual interaction without interrupting the printer’s motion buffer.

The interface provides four core functions:
- **Manual Control:** Jogging axes, homing, and sending raw G-code, with dedicated controls to prime and clear the Nordson dispenser.
- **Coordinate Registration:** Recording reference points on a substrate and computing a transform to align print paths with custom molds. Registration profiles can be saved and loaded as JSON, so switching between mold geometries doesn't require re-registration.
- **Live Parameter Tuning:** Adjusting extrusion pressure, vacuum retract, and temperature settings mid-print to compensate for material behavior.
- **Path Visualization:** A 3D matplotlib plot showing raw G-code paths alongside transformed output, used to verify alignment before printing.

<figure class="align-center">
  <img src="/assets/images/gui.png" 
       alt="User Interface Preview" 
       style="max-width: 1000px; display: block; margin: 0 auto;">
</figure>

## Custom Firmware Modifications (Prusa firmware, C/C++)
I modified Prusa firmware to support syringe-based liquid metal extrusion and an external heater. Gallium operates in a narrow temperature window, so the printer’s default thermoplastic assumptions led to false faults and unstable deposition until the control and safety behavior were adjusted.

- **Thermal Safety Behavior:** Updated temperature limits and fault checks so an external heater could be used without triggering temperature errors, while keeping safety protections intact.
- **Kinematic Tuning:** Reduced acceleration/jerk limits to account for the heavier syringe toolhead and cut down vibration that caused trace breakage during fast moves.
- **Thermistor Calibration:** Updated thermistor lookup tables to match the external heater's response curve, so temperature readings stayed accurate outside the stock operating range.

## Calibration and Path Optimization
To achieve repeatable deposition, I calibrated the interaction between printer motion and pneumatic extrusion. Gallium’s material properties make deposition highly sensitive to mismatches between speed, pressure, and timing.

- **Pressure–Speed Matching:** I used spiral test patterns to tune extrusion pressure against feed rate, preventing pooling at corners and discontinuities along straight paths.
- **Z-Height Calibration:** I used a stepped calibration block to find the nozzle offset that maintains a continuous material bridge without dragging or beading.
- **Start/Stop Timing:** I tuned G-code trigger timing and vacuum retract settings to compensate for pneumatic latency, eliminating tails and blobs at trace endpoints.

For multi-layer prints, the toolpath alternates direction each layer to avoid repositioning jumps and keep deposition continuous.

<figure class="align-center">
  <img src="/assets/images/sensor_samples.png"
       alt="Liquid metal pressure sweep test"
       style="max-width: 600px; display: block; margin: 0 auto;">
  <figcaption style="max-width: 600px; margin: 0 auto;">
    Pressure sweep showing six gallium prints deposited at a fixed feed rate (12000 mm/min) and Z height (15 mm).
    Pressure was increased from 1.2 bar to 1.7 bar in 0.1 bar increments to evaluate trace continuity and pooling behavior.
  </figcaption>
</figure>

## Sensor Characterization

Once fabrication was repeatable, I built the software to characterize sensor performance. Python scripts controlled the Dynamixel-driven test rig, applied repeatable displacement profiles, and logged force and resistance measurements in sync. Post-processing scripts interpolated data across trials to analyze how resistance changed under strain.

<figure class="align-center">
  <img src="/assets/images/experimental_setup.png"
       alt="Experimental setup for liquid metal strain characterization"
       style="max-width: 600px; display: block; margin: 0 auto;">
  <figcaption style="max-width: 600px; margin: 0 auto;">
    Experimental setup for characterizing liquid metal strain sensors using a range of test objects with different geometries and surface features.
  </figcaption>
</figure>
