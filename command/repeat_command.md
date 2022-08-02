---
order: 90
icon: dot-fill
---

# RepeatCommand

A `RepeatCommand` runs a command multiple times. Once the command finishes, it is initialized and run again as many times as specified. It takes in the command to repeat and the number of times to run it. If the number is less than zero, it will repeat forever.

:::sync
+++ Java
```java
//This will run 4 times
new RepeatCommand(someCommand, 4);

//This will repeat forever
new RepeatCommand(someCommand, -1);
```
+++ Kotlin
```kotlin
//This will run 4 times
RepeatCommand(someCommand, 4)

//This will repeat forever
RepeatCommand(someCommand, -1)
```
+++
:::