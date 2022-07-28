---
order: 95
icon: dot-fill
---

# FunctionalCommand

`FunctionalCommand`s, like `BasicCommand`s, are meant for inlining, but are more customizable. A `FunctionalCommand` takes all the `Command` methods, along with `isInterruptable` and component requirements, as constructor parameters, allowing for the dynamic creation of entire commands.

:::sync
+++ Java
```java
new FunctionalCommand(
        () -> {
            //init
        },
        () -> {
            //execute
        },
        (interrupted) -> {
            //end
        },
        () -> {
            //isFinished
            return true;
        }, true, //isInterruptable
        requirements
);
```
+++ Kotlin
```kotlin
FunctionalCommand(
    {
        //init
    },
    {
        //execute
    },
    {
        //end
    },
    {
        //isFinished
        true
    }, true,  //isInterruptable
    requirements
)
```
+++
:::

`FunctionalCommand`s can also be created through decoration. See this in action [here](/command/decorating.md#something).
