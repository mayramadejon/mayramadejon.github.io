---
layout: post
title:  "P1 - Vacuum Cleaner"
date:   2024-09-29 07:49:08 +0200
categories: jekyll update
---
## Practice 1: Developing a Vacuum Cleaner Exploration Algorithm

### Objective
The aim of this practice is to design and implement an exploration algorithm for a low-end vacuum cleaner robot. The robot has limited sensors, using only laser data and bumper feedback to navigate its environment. Notably, it does not have access to its location or an existing map of the surroundings. Despite these constraints, our goal is to enable the robot to explore as much space as possible efficiently.

---

### Algorithm Overview

#### Finite State Machine (FSM) Structure

The robot’s behavior is controlled by an FSM that includes five distinct states:

1. **Exploring** - The robot moves forward, scanning its environment.
2. **Backing** - Upon collision detection, the robot reverses.
3. **Turning** - After backing, the robot turns to find a new direction.
4. **Spiraling** - The robot moves in an expanding spiral to explore new areas.
5. **Scaping** - A backup mechanism to help the robot escape from tricky areas if it gets stuck.

The state transitions are triggered based on sensor inputs, primarily laser and bumper data, and timers designed to ensure the robot doesn’t get stuck in one area.

---

### Exploring the States

Here’s a breakdown of each state and its role in the exploration algorithm:

- **Exploring State:**
   - The robot moves straight ahead while checking for obstacles. If it detects a collision via its bumper sensors, it switches to the **Backing State**. It can also enter a **Spiraling State** after 3 seconds of forward movement to further explore open areas.
   If a significant amount of time has passed since the last time it was in the SPIRALING state, consider that it is in a closed space, as it has not spent more than 3 seconds without colliding, and switch to the SCAPING state to avoid staying in the same place for too long.

- **Backing State:**
   - If the robot collides with an object, it immediately reverses to create space. After a short backward movement of 0.3 m or the passage of 3 seconds, considering that it cannot reverse the required distance, the robot transitions to the **Turning State** to avoid further obstacles.

- **Turning State:**
   - The robot rotates in place, choosing a random direction, and re-enters the **Exploring State** after half a second. This allows the robot to find new areas to explore after each collision.

- **Spiraling State:**
   - The robot performs an outward spiral motion, allowing it to cover large areas efficiently. If a collision occurs, the robot moves back to the **Backing State**. The spiral state ensures wide area coverage while preventing it from sticking to just one zone. If it has been a considerable amount of time in Spiraling, it returns to the **Exploring State**

- **Scaping State:**
   - This is a failsafe mode where the robot performs random escape maneuvers after spending too long in one spot. It reverses and turns to avoid obstacles and find a new path.


### State Diagram
![Diagrama de Estados](/assets/images/Diagrama.png)
---

### Key Challenges and Solutions

1. **Limited Sensor Data:**
   - The robot only has access to laser and bumper data, making it difficult to map the environment or localize itself. To overcome this, we focused on reactive behavior, ensuring the robot responds instantly to obstacles and intuitive timers.

2. **Avoiding Deadlocks:**
   - One of the key issues encountered was the robot getting stuck in corners or tight spaces. The **Scaping State** was added as a reactive measure, helping the robot escape these tricky spots by making unpredictable movements.

3. **Tuning Spiral Exploration:**
   - A challenge we faced was tuning the speed and width of the spiral in the **Spiraling State**. Too fast, and the robot might suffer damages; too slow, and it wastes time. Through experimentation, we found a balance that maximizes exploration while maintaining robot safe.

---

### Code Efficiency and Design

The algorithm was designed with efficiency in mind. We avoided any calls to blocking functions like `sleep()`, ensuring that the robot’s behavior remains responsive at all times. Additionally, each function is clearly separated to maintain readability and simplicity. The code is modular, with dedicated functions for each state, making it easy to adjust or expand upon as necessary.

---

### Random Turns

I wanted to add a random element to the practice to prevent the robot from always following the same path. To achieve this, the direction of the turns varies throughout the practice; the direction in which the spiral is drawn is chosen randomly each time, as well as the direction it turns in the Turning State. The direction the robot turns is also chosen randomly each time it searches for a path in the Scaping State.

### Results and Performance

In testing, the robot successfully navigated and explored various environments, avoiding obstacles and escaping tight corners. It covered a significant amount of space, demonstrating the effectiveness of the FSM approach. The transition between states was smooth, and the robot’s behavior was both reactive and efficient.
Example that show its evolution in the house simulator:

**After 5 minutes**
---
![5 minutes](/assets/images/5minutes.png)

---

### Video Demonstration

Here’s a video of our vacuum cleaner robot in action, navigating through a test environment using the FSM-controlled exploration algorithm during 15 min.

<video controls>
    <source src="{{ '/assets/videos/vacuumcleaner.mp4' | relative_url }}" type="video/mp4">
    Your browser does not support the video tag.
</video>







---
