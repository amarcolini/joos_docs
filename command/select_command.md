---
order: 92
icon: dot-fill
---

# SelectCommand

A `SelectCommand` runs a different command every time it is scheduled, taking in a function that provides a command which it then runs. `SelectCommand`s are useful when you need to create a command before it is scheduled, but still want it to have dynamic behavior. `Command.select` is also available to create `SelectCommand`s.

:::sync
+++ Java
```java
new SelectCommand(() -> {
    return someCommand; //Return command to use here
});

Command.select(() -> {
    return someCommand; //Return command to use here
});
```
+++ Kotlin
```kotlin
SelectCommand {
    //Return command to use here
    someCommand
}

Command.select {
    //Return command to use here
    someCommand
}
```
+++
:::

One use case of `SelectCommand` is button mapping, where depending on some other current state, a different action is performed:

:::sync
+++ Java
```java
/*
Here we map A to toggling the bucket state. 
Whenever A is pressed, the command is scheduled, and the code inside the block runs.
*/
map(gamepad.p1.a::justActivated, Command.select(() -> {
    if (bucket.isOpen()) return bucket.close();
    else return bucket.open();
}));
```
+++ Kotlin
```kotlin
/*
Here we map A to toggling the bucket state. 
Whenever A is pressed, the command is scheduled, and the code inside the block runs.
*/
map(gamepad.p1.a::justActivated, Command.select {
    if (bucket.isOpen()) bucket.close()
    else bucket.open()
})
```
+++
:::