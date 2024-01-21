---
order: 96
icon: link
label: Joos and FTC Dashboard
---

# Joos + FTC Dashboard (=:fire:)

The biggest change from just FTC Dashboard to Joos is changing all `@Config` to `@JoosConfig`. What's the difference?

 - First-class Kotlin support. No more `@JvmStatic` and support for top-level properties.
 - Works on individual variables, so not all static variables are editable.
 - Works on immutable variables.
 - Supports immutable Kotlin data classes via `@Immutable`.
 - Supports custom config providers with `@MutableConfigProvider` and `@ImmutableConfigProvider`.

Here's an example of some of the differences:

+++ FTC Dashboard (Kotlin)
```kotlin
@Config
class MyOpMode : OpMode {
    companion object {
        @JvmStatic //You need JvmStatic
        var myVariable = 1.0
        
        //Needs to be mutable
        var myPIDCoefficients = PIDCoefficients()
        
        //id isn't editable here
        var myData = MyDataClass(1)
    }
    
    data class MyDataClass(
        val id: Int
    )
}
```
+++ Joos (Kotlin)
```kotlin
@JoosConfig
class MyOpMode : OpMode {
    companion object {
        //No JvmStatic required!
        var myVariable = 1.0

        //Can be immutable!
        val myPIDCoefficients = PIDCoefficients()

        //id can be edited!
        @Immutable
        var myData = MyDataClass(1)
    }

    data class MyDataClass(
        val id: Int
    )
}
```
+++

### SuperTelemetry

Ever had issues trying to use FTC Dashboard's `MultipleTelemetry` and the field overlay at the same time? `SuperTelemetry` solves all
of them! It has all the functionality of `MultipleTelemetry` while giving you access to its field overlay and not requiring any setup! To use it, just replace `telemetry` with `SuperTelemetry` in your OpModes, or just `telem` if using CommandOpModes.