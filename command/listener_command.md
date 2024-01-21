---
order: 91
icon: dot-fill
---

# ListenerCommand

A `ListenerCommand` runs different actions whenever a command initializes, executes, or ends. It takes it the command to listen for and the actions to run on init, execute, and end.

:::sync
+++ Java
```java
new ListenerCommand(
        someCommand,
        () -> {
            //On init
        },
        () -> {
            //On execute
        },
        (interrupted) -> {
            //On end
        }
);
```
+++ Kotlin
```kotlin
ListenerCommand(
    someCommand,
    onInit = {
        //On init
    },
    onExecute = {
        //On execute
    },
    onEnd = {
        //On end
    }
)
```
+++
:::