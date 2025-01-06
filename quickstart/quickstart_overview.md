---
order: 100
icon: home
---

# Quickstart Overview

The tuning process to derive the constants to control a robot can be divided into 4 basic parts:

1. Basic hardware configuration.
2. Localization tuning.
3. Feedforward tuning.
4. Feedback tuning.

As of right now, the quickstart only has out-of-the-box support for mecanum drives and dead wheel localization for three wheels. Most of the actual tuning logic has been abstracted enough to be easily adaptable to any drive and localization setup, however, so most of these steps will apply regardless.

### Hardware Configuration

All of the hardware ID's in the quickstart are stored in the `SampleRobot` class. Make sure they are all set to match your robot configuration. Note that the IMU is only required for `AngularRampLogger` and your localization if it needs an external heading sensor.

Once you have your hardware correctly configured, you have to make sure all motors and encoders are set to the correct directions. To aid in this, you have three options: `MotorDirectionDebugger`, `LocalizationTest`, and `EncoderDirectionDebugger`. For your drive motors, you can either use `MotorDirectionDebugger` and run each motor individually to see which need to be reversed (they should all drive forwards), or use `LocalizationTest` to drive forward and diagnose all the motors at once. For `LocalizationTest` it may be beneficial to have the robot on its side or another angle where it is easier to see the wheels.

If you have encoders separate from your drive motors, you can use `EncoderDirectionDebugger` and spin each encoder by hand forwards to see which are reversed. Note that for perpendicular encoders that point left or right, forwards is to the left of the robot.


### Localization Tuning

The first step to tuning your localization is `ForwardPushTest` (and `LateralPushTest` if you're using mecanum drive). Push the robot forwards a specific distance, and measure it. The telemetry will print out the estimated distance in ticks. Multiply your `distancePerTick` (in `SampleMecanumDrive` or your sample drive class) by the actual distance divided by the estimated distance. To get a more accurate result, you can take the average of multiple repetitions of this process.

For mecanum drives, since they move slower when strafing, a separate number has to account for distance travelled when strafing. Repeat the same pushing process as for `ForwardTest`, except this time to the left. Multiply `lateralMultiplier` (in `SampleMecanumDrive`) by the actual distance divided by the estimated distance. This number should be less than one.

Once you've completed the push tests, you can determine the position of your encoders through either `AngularRampLogger` or `EncoderPositionTuner`, the former being automatic and the latter manual. Both accomplish the same thing, but may yield different results. For `AngularRampLogger`, the robot will spin counter-clockwise for 5 seconds (by default) to determine the position of your encoders. For `EncoderPositionTuner`, you will have to spin the robot *exactly* 10 times counter-clockwise (by default) with the right joystick and then press A/X (Xbox/PS) to get your positions.

That's it! You can run `LocalizationTest` to test your localization accuracy. If you find it lacking, you can either repeat the previous tuning steps or fine-tune your constants manually.

### Feedforward Tuning

Run `ManualFeedforwardTuner`. The robot will eventually move back and forth roughly 4 feet, once all values are tuned. It's recommended for this step and for the feedback tuning to use [FTC Dashboard](https://github.com/acmerobotics/ftc-dashboard). It comes pre-installed with the quickstart, so there's no setup required.

Begin by setting all your feedforward values (found in `feedforward` in `SampleMecanumDrive` or your sample drive class) to 0. Then follow these steps:

1. Increase kStatic until the robot just barely starts to move.
2. Increase kV until the robot reaches the maximum target speed. It's easier to see this with FTC Dashboard's graph view.
3. Increase kA to make the acceleration and decceleration portions of the graph more accurate.

If at any point the robot drifts away from where you want it, you can press Y/△ (Xbox/PS) to enter drive control, and then B/〇 (Xbox/PS) to continue tuning once it's reset. Note also that you should start at really small values when tuning (like 0.00001) and then increase until it begins to have an effect. It's better to have your robot be unresponsive at first then speed off uncontrollably.

### Feedback tuning

Run `ManualFeedbackTuner`. The robot will move forward and back while turning. The variables to modify here are all those that end in "coeffs" in `SampleMecanumDrive` (or whatever drive class you're using). These are the PID coefficients for a basic trajectory-following controller. To tune them, first set them all to 0, then:

1. Increase kP to reduce error
2. If you feel it is necessary, increase kD slightly to dampen oscillations.
3. Do not use kI. It is not beneficial in a trajectory-following context and may reduce performance.

That's it! You're all tuned.