---
order: 100
icon: book
---

# Intro to Commands

One key aspect of efficient robots is being able to coordinate multiple actions simultaneously. Whether it be spinning and scoring ducks at the same time, or picking up and shooting rings at the same time, there are parts of a game that lend themselves to multitasking. Accomplishing this behavior, however, can sometimes prove to be difficult. 

The most common method of doing multiple things at once is through state machines, where each part of the robot can be updated at different times while they all keep track of their states:

```java
public class MyOpMode extends OpMode {
    // List of possible arm states
    enum ArmState {
        RETRACTED,
        MIDDLE,
        OUT
    }
    // Keeps track of current state
    ArmState currentArmState = ArmState.RETRACTED;
    Arm arm = new Arm();
    
    @Override
    public void init() {
        
    }

    @Override
    public void loop() {
        switch (currentArmState) {
            case RETRACTED: {
                // Do stuff here
                if (arm.shouldChangeState()) {
                    currentArmState = ArmState.MIDDLE;
                }
                break;
            }
            case MIDDLE: {
                //Do other stuff here
                if (arm.shouldChangeState()) {
                    currentArmState = ArmState.OUT;
                }
                break;
            }
            case OUT: {
                //Do more stuff here
                if (arm.shouldChangeState()) {
                    currentArmState = ArmState.RETRACTED;
                }
                break;
            }
        }
        
        
        //While all that is going on, we can have more logic here
        if (gamepad1.a) {
            //Do other things *simultaneously*
        }
    }
}
```

When dealing with many robot parts, however, this code can increase in size and complexity very quickly. You can remedy this problem slightly by putting the logic of each part in its own class and calling some update method, but each individual class can still be quite complicated.

Joos's command system aims to make asynchronous code more powerful and easy to write, making your code more effective.

## The Command

A `Command` is just a simple state machine with 2 states: running and not running. The state is returned by a command's `isFinished` method. If `isFinished` returns false, `execute` is called and the command keeps on running. Otherwise, it stops.

Besides these two basic methods, commands have 2 more: `init` and `end`. `init` is called right when the command is scheduled for execution, and `end`, naturally, is called when the command ends.

## The Component

Robot mechanisms have to do other things besides following commands, like updating a pose estimate or a PID controller. These things do not fit nicely into a `Command`, which is where the `Component` comes in. The `Component` interface has only one method: `update`, but is still very useful for separating code for different parts of your robot.

In addition, components are able to be required by a command. This feature becomes useful when cancelling commands, or scheduling conflicting commands. For example, when 2 commands that require the same component are scheduled, one of them is automatically cancelled.

## The Robot

The `Robot` class serves only to organize your command-based code. In it you can construct and register all your components as well as set up basic commands for every OpMode. It provides `init` and `start` methods that are called before init and start in `RobotOpMode`s, which just automatically handle the creation and updating of robots.

## Putting It All Together

Creating a component is as easy as implementing the `Component` interface:

:::sync
+++ Java
```java
import com.amarcolini.joos.command.AbstractComponent;
import com.amarcolini.joos.command.Command;
import com.amarcolini.joos.hardware.Motor;

public class Arm extends AbstractComponent {
    private final Motor motor;

    public Arm(Motor motor) {
        this.motor = motor;
        /** 
        Only AbstractComponent has this. If you're using Component, you would just have to call 
        motor.update() in this component's update method.
        **/
        subcomponents.add(motor);
    }
    
    public Command extend() {
        return motor.goToDistance(5.0);
    }
    
    public Command retract() {
        return motor.goToDistance(0.0);
    }
}
```
+++ Kotlin
```kotlin
import com.amarcolini.joos.command.AbstractComponent
import com.amarcolini.joos.hardware.Motor

class Arm(private val motor: Motor) : AbstractComponent() {
    init {
        /** 
        Only AbstractComponent has this. If you're using Component, you would just have to call 
        motor.update() in this component's update method.
        **/
        subcomponents.add(motor)
    }
    
    fun extend() = motor.goToDistance(5.0)

    fun retract() = motor.goToDistance(0.0)
}
```
+++
:::

You may have noticed that this example uses `AbstractComponent` instead of `Component`. The only difference is that `AbstractComponent` makes it easier to deal with subcomponents, like motors.

All of your components, once written, can be put into a `Robot` like so:

:::sync
+++ Java
```java
import com.amarcolini.joos.command.Robot;
import com.amarcolini.joos.hardware.Motor;

public class MyRobot extends Robot {
    public final Arm arm;

    //You can initialize your components in a no-args constructor like this, or in init().
    public MyRobot() {
        arm = new Arm(new Motor(hMap, "myMotorID", 312));

        register(arm);
    }

    @Override
    public void init() {
        //Also doable: register(arm);
        
        if (isInTeleOp) {
            //Maybe do stuff here
        } else if (isInAutonomous) {
            //Or do stuff here
        }
        //init() and start() are for things common across OpModes, like resetting an arm.
        schedule(arm.retract());
    }
}
```
+++ Kotlin
```kotlin
import com.amarcolini.joos.command.Robot
import com.amarcolini.joos.hardware.Motor

class MyRobot : Robot() {
    val arm: Arm = Arm(Motor(hMap, "myMotorID", 312.0))

    //You can initialize your components in an init block like this, or in init().
    init {
        register(arm)
    }

    override fun init() {
        // Also doable: register(arm)

        if (isInTeleOp) {
            //Maybe do stuff here
        } else if (isInAutonomous) {
            //Or do stuff here
        }
        //init() and start() are for things common across OpModes, like resetting an arm.
        schedule(arm.retract())
    }
}
```
+++
:::

Once you have a `Robot` complete with components, rather than use a normal `OpMode` or `LinearOpMode`, you can use a `CommandOpMode` for all your TeleOp and Autonomous needs. It provides you with direct access to the `CommandScheduler`, a `MultipleGamepad`, and automatic handling of `Robot`s. To create one, simply extend it as you would a normal OpMode, and override the `preInit` method. Any telemetry sent in `preInit` will be updated immediately, and any commands scheduled or components registered will update once the OpMode starts. To register a `Robot`, just create a non-final field annotated with `@Register` if using Java, or the `robot` delegate
if using Kotlin.

:::sync
+++ Java
```java
import com.amarcolini.joos.command.CommandOpMode;
import com.qualcomm.robotcore.eventloop.opmode.TeleOp;

@TeleOp
public class MyOpMode extends CommandOpMode {
    @Register
    private MyRobot robot; //This will get automatically instantiated.

    @Override
    public void preInit() {
        //This will update immediately after
        telem.addLine("Ready!");
        
        //These will all update once the OpMode starts
        schedule(robot.arm.extend());
        map(gamepad.p1.a::justActivated, robot.arm.retract());
        register(someComponent);
    }
}
```
+++ Kotlin
```kotlin
import com.amarcolini.joos.command.CommandOpMode
import com.qualcomm.robotcore.eventloop.opmode.TeleOp

@TeleOp
class MyOpMode : CommandOpMode() {
    private val robot by robot<MyRobot>()

    override fun preInit() {
        //This will update immediately after
        telem.addLine("Ready!")

        //These will all update once the OpMode starts
        schedule(robot.arm.extend())
        map(gamepad.p1.a::justActivated, robot.arm.retract())
        register(someComponent)
    }
}
```
+++
:::

That's the basics! For more detailed explanations of every part of Joos' command system, keep reading.