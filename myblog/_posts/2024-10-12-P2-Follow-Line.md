---
layout: post
title:  "P2 - Follow Line"
date:   2024-10-12
categories: jekyll update
---
## Practice 2: Follow Line: PID Controller for a Race Car

### Objective
The goal of this practice is to program a controller for a Formula 1 car using images from a forward-facing color camera to follow a red line on the ground, which represents the center of the track. The challenge is for the car to complete a lap around the circuit as fast as possible, while staying centered on the lane using a PID control algorithm.

---

### Algorithm Overview

#### Image Processing

To detect the red line on the circuit, we used the OpenCV library to process the images captured by the car's camera. The camera takes an image of the surroundings and converts it to the HSV format (Hue, Saturation, Value), which makes it easier to detect specific colors.

We defined thresholds for the red color, which allow us to create a binary mask where pixels matching the red color are marked as 1 (white), and the rest as 0 (black). We then analyze this mask to find the center of the red line in the image.
---

### PID Control

The car's control is managed by a PID controller (Proportional, Integral, Derivative), which adjusts the car's angular velocity based on the deviation from the center of the lane. The PID allows precise adjustments to the car's steering, keeping the path stable and fast.

 - **Proportional part (P)**: The current deviation (error) between the center of the image and the center of the red line.
 - **Derivative part (D)**: The rate of change of the error over time, which helps smooth out oscillations.
 - **Integral part (I)**: Accumulates errors over time to correct small persistent deviations.
 
The car stays centered by adjusting the angular velocity according to the error calculated by the PID. When the car detects that it's deviating from the center, the controller adjusts the steering in real-time to correct the trajectory.
### PID Parameter Tuning

The initial PID parameters were manually tuned through various tests on the track:

- **Kp (Proportional)**: Controls the direct reaction to the error. A value too high causes the car to oscillate, while a low value makes the car respond slowly.
- **Kd (Derivative)**: Helps reduce oscillations when the car attempts to correct its path. A suitable value was found to smooth the car's movement.
- **Ki (Integral)**: Since the deviations are small and fast, a very low value was set to avoid unnecessary accumulation of error.
---

### Key Challenges and Solutions

1. **Noise and inconsistencies in image detection**: One of the challenges was dealing with noise or inconsistent detection of the red line in the images. Variations in lighting, shadows, or even slight variations in the camera angle sometimes caused false positives or gaps in the detection of the red pixels. This led to occasional abrupt movements of the car as it tried to correct its trajectory based on noisy or incomplete information. To mitigate this, we considered smoothing techniques, such as averaging the detected positions over a few frames to reduce jitter.

2. **Handling sharp turns**: Another challenge was maintaining control during sharp turns. Initially, when the car approached a curve, the PID controller struggled to react fast enough to the large error values, especially at high speeds. The car would often overshoot the turn, go off track, or oscillate as it tried to regain the line. This required careful tuning of the proportional (Kp) and derivative (Kd) terms to ensure the car could handle both gentle and sharp turns smoothly, adjusting quickly but without overcorrecting.

3. **Balancing speed and control**: Speed is a critical factor in racing, but increasing the car's speed also made it harder to maintain stable control, especially when the camera’s refresh rate was insufficient to keep up with the car's movement. The faster the car moved, the less time it had to react to the line’s position, which caused more frequent overshooting or loss of line detection. To address this, we had to adjust the maximum speed dynamically, allowing the car to slow down slightly when approaching curves and accelerating on straight sections, but without sacrificing overall lap time.

---

### Results and Performance

After implementing and tuning the PID controller, the car was able to complete a lap around the circuit efficiently, staying in the center of the lane in most of the tests. Lap times improved significantly once the parameters were optimized to allow the car to reach its maximum speed without losing stability.

---

### Video Demonstration

Below is a video showing the car completing a lap around the circuit using the developed PID controller: 




---
