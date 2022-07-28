---
order: 96
icon: dot-fill
---

# InstantCommand

An `InstantCommand` is similar to a `BasicCommand` in that it simply runs an action once, but unlike a `BasicCommand`, and `InstantCommand` runs its action in `init` rather than `execute`, making it effectively instant. `InstantCommand`s are useful only for the most basic actions.

:::sync
+++ Java
```java
new InstantCommand(() -> {
    //Action goes here 
});
```
+++ Kotlin
```kotlin
InstantCommand {
    //Action goes here
}
```
+++
:::