---
order: 99
icon: dot-fill
---

# Component

A `Component` has one main method, `update`, which is repeatedly called by the `CommandScheduler`. In addition, `Component`s have an optional `getDefaultCommand` method which returns a command that is scheduled whenever they are not in use. `Component`s can only be used by one `Command` at a time to avoid conflict between commands.

Instead of creating a separate class, components can also be inlined with `Component.of`:
:::sync
+++ Java
```java
import com.amarcolini.joos.command.Component;

Component.of(() -> {
    //This is the component's update method
}, someCommand //You can optionally provide a default command
);
```
+++ Kotlin
```kotlin
import com.amarcolini.joos.command.Component

Component.of(
    someCommand //You can optionally provide a default command
) {
    //This is the component's update method
}
```
:::

## AbstractComponent

`AbstractComponent` is an abstract version of `Component` with several utility methods that make your life easier.

Currently, the only added functionality `AbstractComponent` provides is support for subcomponents, like `Motor`s and `Servo`s, that need their own update methods called, but aren't a separate component of the robot. It also gives you `telem` to access [`SuperTelemetry`](/dashboard.md#supertelemetry). To add a subcomponent, use `subcomponents.add()`, and if you override `update()`, make sure to insert a call to `super.update()` as well.

:::sync
+++ Java
```java
import com.amarcolini.joos.command.AbstractComponent;
import com.amarcolini.joos.hardware.Motor;

public class MyComponent extends AbstractComponent {
    public MyComponent(Motor subcomponent) {
        subcomponents.add(subcomponent);
    }

    @Override
    public void update() {
        super.update(); //Make sure to include this

        //Do other stuff here
    }
}
```
+++ Kotlin
```kotlin
import com.amarcolini.joos.command.AbstractComponent
import com.amarcolini.joos.hardware.Motor

class MyComponent(subcomponent: Motor) : AbstractComponent() {
    init {
        subcomponents.add(subcomponent)
    }

    override fun update() {
        super.update() //Make sure to include this

        //Do other stuff here
    }
}
```
+++
:::