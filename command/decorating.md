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

Here are some of the shortcuts you can use to add functionality to your commands:

==- Delays
:::sync
+++ Java
```java
//Original
new SequentialCommand(
        myCommand,
        new WaitCommand(1.0)
);

//Shortcut
myCommand.wait(1.0);
```
+++ Kotlin
```kotlin
//Original
SequentialCommand(
    myCommand,
    WaitCommand(1.0)
)

//Shortcut
myCommand.wait(1.0)
```
+++
:::
===
==- Command Groups
:::sync
+++ Java
```java
//Original
new SequentialCommand(myCommand, otherCommand);

new RaceCommand(myCommand, otherCommand);

new ParallelCommand(myCommand, otherCommand);

//Shortcut
myCommand.then(otherCommand);

myCommand.race(otherCommand);

myCommand.and(otherCommand);
```
+++ Kotlin
```kotlin
//Original
SequentialCommand(myCommand, otherCommand)

RaceCommand(myCommand, otherCommand)

ParallelCommand(myCommand, otherCommand)


//Shortcut
myCommand.then(otherCommand)

myCommand.race(otherCommand)

myCommand.and(otherCommand)
```
+++
:::
===

For the rest of the ways you can decorate commands, see the [Kotlin docs](/kotlin_docs/command/com.amarcolini.joos.command/-command/index.html) or [Java docs](/java_docs/command/com.amarcolini.joos.command/-command/index.html).