---
order: 97
icon: dot-fill
---

# BasicCommand

The `BasicCommand` was designed with inlining in mind and, as such, is easily [decorated](/command/decorating.md). To create a `BasicCommand`, pass in a runnable to its primary constructor, or alternatively, use `Command.of`.

:::sync
+++ Java
```java
new BasicCommand(() -> {
    //Action goes here 
});

Command.of(() -> {
    //Action goes here
});
```
+++ Kotlin
```kotlin
BasicCommand {
    //Action goes here
}

Command.of {
    //Action goes here
}
```
+++
:::
