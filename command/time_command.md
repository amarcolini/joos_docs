---
order: 93
icon: dot-fill
---

# TimeCommand

A `TimeCommand` is an easy way to create time-based commands without having to create a separate class and use a timer. Its only argument is a function that takes in two doubles, the total amount of time the command has been running and the time elapsed since the last call to the function (both in seconds), and returns whether the command is finished. This type of command is useful for things that require the current time, like motion profiles.

:::sync
+++ Java
```java
new TimeCommand((t, dt) -> {
    //Do something with t or dt here
    return t > 5; // Whether the command is finished
});
```
+++ Kotlin
```kotlin
TimeCommand { t: Double, dt: Double ->
    //Do something with t or dt here
    t > 5 // Whether the command is finished
}
```
+++
:::