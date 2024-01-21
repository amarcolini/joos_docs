---
order: 99
icon: light-bulb
---

To get a robot to move how you want it to, you need to figure out three things:

1. Where the robot is.
2. What exact movements the robot needs to make to reach its target.
3. How to get the robot to follow those movements and respond to errors.

In FTC, #1 is typically accomplished using a process called [dead reckoning](https://en.wikipedia.org/wiki/Dead_reckoning) with encoders on either drive wheels, or sprung unpowered wheels known as "dead wheels" or "odometry wheels". To accomplish this, both the exact positions of all encoders and the conversion from encoder ticks to distance must be known.

For #2, paths are pre-defined, typically using [splines](https://en.wikipedia.org/wiki/Spline_(mathematics)). Joos uses parametric representations of quintic bezier curves for this purpose. From there, usually a motion profile is constructed to ensure that any commanded velocities respect the robot's constraints.

Lastly, for #3, there are a variety of different controllers designed to ensure the robot stays on the desired trajectory. Techniques can range from as simple as a basic PID controller to a more complicated guiding vector field. Joos includes a collection of multiple controllers for you to experiment with.

Each of these steps requires some robot-specific knowledge, so the quickstart is designed to help you figure that out, well, quickly.

[!ref](/quickstart/quickstart_overview.md)
