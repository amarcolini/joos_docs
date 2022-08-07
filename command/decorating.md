---
order: 0
icon: ":tulip:"
---

# Decorating Commands

Although the different commands Joos provides are very useful, they can sometimes be cumbersome to put together to form new commands. Take this example:

:::sync
+++ Java
```java
new SequentialCommand(
        myCommand,
        new WaitCommand(4)
);
```
+++ Kotlin
```kotlin
SequentialCommand(
    myCommand,
    WaitCommand(4.0)
)
```
+++
:::

It adds a delay of 4 seconds after `myCommand` finishes. This is not difficult to write, but the syntax can be condensed greatly:

:::sync
+++ Java
```java
myCommand.wait(4.0);
```
+++ Kotlin
```kotlin
myCommand wait 4.0
```
+++
:::

Here are all the shortcuts you can use to add functionality to your commands:

!!!warning :construction: Warning :construction:
This page is under construction. In the meantime, check the [kotlin](../kotlin_docs/) or [java](../java_docs/) docs.
!!!
