---
order: 98
icon: dot-fill
---

# Command

## Creating a Command

A command can be created in one of two ways: a separate class and inlining.

### A Command as a Class

Making a command a separate class is very useful when dealing with very complicated commands and commands that extend other commands.

:::sync
+++ Java
```java
public class MyCommand extends Command {
    //It's better to have requirements as a single set
    private final HashSet<Component> requirements = new HashSet<>();
    private final Component myComponent;

    //Constructors are useful for taking components
    public MyCommand(Component myComponent) {
        this.myComponent = myComponent;
        //Make sure do add ALL your components to requirements
        requirements.add(myComponent);
    }

    @Override
    public boolean isInterruptable() {
        //Whether this command can be interrupted by the scheduling of other commands
        return true; 
    }

    @NotNull
    @Override
    public Set<Component> getRequirements() {
        return requirements;
    }

    @Override
    public void init() {
        //Initialize here
    }

    @Override
    public void execute() {
        //Execute here
    }

    @Override
    public void end(boolean interrupted) {
        if (interrupted) {
            //Shutdown command
        } else {
            //Finish cleanly
        }
    }

    @Override
    public boolean isFinished() {
        return false; //Some end condition
    }
}
```
+++ Kotlin
```kotlin
//Constructors are useful for taking components
class MyCommand(private val myComponent: Component) : Command() {
    //Make sure do add ALL your components to requirements
    override val requirements: Set<Component> = setOf(myComponent)

    //Whether this command can be interrupted by the scheduling of other commands
    override val isInterruptable = true

    override fun init() {
        //Initialize here
    }

    override fun execute() {
        //Execute here
    }

    override fun end(interrupted: Boolean) {
        if (interrupted) {
            //Shutdown command
        } else {
            //Finish cleanly
        }
    }

    override fun isFinished() = false //Some end condition
}
```
+++
:::

Here you have fine control over everything that your command does. If you're looking for something simpler, try inlining your command.

## Inlining Commands

What if you wanted a command that ran two commands in parallel and after both finished waited 5 seconds before finally running another command? With inlining that would look something like this:

:::sync
+++ Java
```java
new SequentialCommand(
        new ParallelCommand(
                oneCommand,
                otherCommand
        ),
        new WaitCommand(5),
        finalCommand
);
```
+++ Kotlin
```kotlin
SequentialCommand(
    ParallelCommand(
        oneCommand,
        otherCommand
    ),
    WaitCommand(5.0),
    finalCommand
)
```
+++
:::

!!!warning :construction: Warning :construction:
This page is under construction.
!!!
