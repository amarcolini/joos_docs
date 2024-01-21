---
order: 100
icon: home
---

# Getting Started

It's recommended to glance through the Java or Kotlin docs to get a feel for what kind of stuff is provided by this module,
but here's a quick rundown:

- Basic geometry classes (`Pose2d`, `Vector2d`, and `Angle`)
- Kinematics for mecanum, tank, swerve, and differential swerve drives, as well as general robot kinematics
- A PID controller and DC motor feedforward
- Odometry localization for all the drives mentioned above and dead wheels (2 or 3)
- Path, motion profile, and trajectory generation
- State-of-the-art trajectory and path following algorithms
- JSON-serializable trajectories

Kinematics, localization, and the following algorithms all require robot-specific parameters which can only be found through tuning. For that, see the quickstart. It provides plenty of tuning OpModes to get you up and running.

[!ref](/quickstart/quickstart_overview.md)