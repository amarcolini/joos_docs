---
order: 94
icon: dot-fill
---

# WaitCommand

A `WaitCommand` does simply that: waits a specified duration before ending. This command is useful for adding delay between actions.

:::sync
+++ Java
```java
//Duration in seconds
new WaitCommand(5);
```
+++ Kotlin
```kotlin
//Duration in seconds
WaitCommand(5)
```
+++
:::