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

In addition, all of the `FunctionalCommand`'s properties are mutable, and so can be configured after creation:

:::sync
+++ Java
```java
FunctionalCommand myCommand = new FunctionalCommand();

myCommand.init = () -> {
    //init
};

myCommand.setInterruptable(true);

myCommand.getRequirements().add(myComponent);
```
+++ Kotlin
```kotlin
val myCommand = FunctionalCommand()

myCommand.init = Runnable {
    //init
}

myCommand.isInterruptable = true

myCommand.requirements += myComponent
```
+++
:::
