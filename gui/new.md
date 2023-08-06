---
order: 99
icon: cloud
---

# Web

The Joos GUI is currently moving to the web! It currently supports nearly all previous GUI features, but lacks any styling / theming. You can try it out [here](/web_gui/).

### Current Features

- Draggable waypoints and tangents
- Editable turn and wait segments
- Trajectory visualization with scrub bar and custom constraints
- Custom field backgrounds

### Planned features

- Better styling and custom themes
- Webserver integration to be able to edit trajectories directly on the Robot Controller

If you have any more suggestions or questions on how any of this works, please let me know!

## Exporting

JSON is the new serialization format for trajectories. Currently, trajectory constraints are not included in serialization, and thus cannot be exported. To export a trajectory from the GUI, copy the outputted JSON, and paste it into `SerializableTrajectory.fromJSON`. From there you can call `toTrajectory` to get a result. An example might look like this:

:::sync
+++ Java
```java
Trajectory trajectory = SerializableTrajectory.fromJSON("Insert JSON here!").toTrajectory(
    new GenericConstraints() // These should be your robot-specific constraints
).getTrajectory();
```
+++ Kotlin
```kotlin
val trajectory = SerializableTrajectory.fromJSON("Insert JSON here!").toTrajectory(
    GenericConstraints() // These should be your robot-specific constraints
).trajectory
```
+++
:::