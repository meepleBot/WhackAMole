---

layout: default
title: Controlling Motors
parent: Introduction to the WPILib Framework
nav_order: 2

---

# Controlling Motors

Controlling your motors is an essential part of FRC Robotics. It's not just about setting a power and leaving it; you'll often need to dynamically control motors based on sensors, user input, or pre-defined conditions. In this lesson, we will explore some of the advanced controls available in WPILib.

## Understanding Motor Controllers
In FRC Robotics, motor controllers are the interfaces that allow us to electronically control the motors. Various types of motor controllers are used in FRC, each with their own unique properties and methods of control. Some common examples include the SparkMax, Talon, and Victor motor controllers.

WPILib has classes to represent these motor controllers, such as SparkMax, Talon, Victor, etc. To use a motor controller in your code, you instantiate the corresponding class and use its methods to control the motor.

## Example
Here is a basic example of how to use a `SparkMax` motor controller to control a motor:

    import com.revrobotics.CANSparkMax;
    import com.revrobotics.CANSparkMaxLowLevel.MotorType;
    import edu.wpi.first.wpilibj.TimedRobot;
    import edu.wpi.first.wpilibj.motorcontrol.Spark;
    import edu.wpi.first.wpilibj2.command.CommandScheduler;

    public class Robot extends TimedRobot {
    private CANSparkMax testMotor;

    @Override
    public void robotInit() {
        testMotor = new CANSparkMax(1, MotorType.kBrushless);
    }

    @Override
    public void robotPeriodic() {
        CommandScheduler.getInstance().run();
    }

    @Override
    public void teleopInit() {

    }

    @Override
    public void teleopPeriodic() {
        testMotor.set(0.5); // Set the motor to half speed
    
    }

In the `robotInit` method, we initialize a new SparkMax motor controller on CAN id 1. Then, in `teleopPeriodic`, we set the power of the motor to 0.5, which is half power.

## Control a Motor with a Joystick
In many robots, you'll want to use a joystick to control the motor. In the `teleopPeriodic()` method, the joystick's Y-axis value is continually read and directly set as the output for the motor controller. As `controller.getLeftY()` returns a value between -1 and 1 (representing the full range of the joystick), the motor speed varies from full reverse (-1), to stop (0), to full forward (+1) corresponding to the movement of the joystick.

Here's how to do this:

    import com.revrobotics.CANSparkMax;
    import com.revrobotics.CANSparkMaxLowLevel.MotorType;
    import edu.wpi.first.wpilibj.TimedRobot;
    import edu.wpi.first.wpilibj.XboxController;
    import edu.wpi.first.wpilibj.motorcontrol.Spark;
    import edu.wpi.first.wpilibj2.command.Command;
    import edu.wpi.first.wpilibj2.command.CommandScheduler;

    public class Robot extends TimedRobot {
        private XboxController driveController = new XboxController(0);
        private CANSparkMax testMotor;

        @Override
        public void robotInit() {
            testMotor =  = new CANSparkMax(1, MotorType.kBrushless);
        }

        public void teleopPeriodic() {
            testMotor.set(driveController.getLeftY()); // Set the motor to half speed
        }

This results in a straightforward one-to-one control system where the position of the joystick directly influences the speed and direction of the motor.

## Inverting Motor Direction
In many robotic systems, especially those with multiple motor assemblies like a drivetrain, one motor may need to spin in the forward direction while another needs to spin in a backward direction for the motors to work togehter. Therefore one motor needs to be "inverted" in the direction it runs.

    motor.setInverted(true); // Inverts the direction of the motor

## Create a Follower Motor
In many robotic systems, especially those with multiple motor assemblies like a drivetrain, there might be a requirement for one motor to mirror or "follow" the actions of another. This is typically done to ensure synchronized operation of multiple motors for executing coordinated tasks.

In the example given below, a second motor, `followerMotor`, is declared and set to follow the motor. The follower motor is also a SPARK MAX motor controller, connected through the CAN bus on port 2.

Once `followerMotor.follow(testMotor)` is called, the followerMotor will now mirror the actions of motor. So, if testMotor's speed is set to 0.5, the followerMotor will also run at a speed of 0.5. If testMotor's direction changes, the followerMotor will change its direction as well. The follower motor's operation is now entirely dependent on the leading motor unless otherwise specified.

    testMotor = new CANSparkMax(1, MotorType.kBrushless);
    followerMotor = new CANSparkMax(2, MotorType.kBrushless);
    followerMotor.follow(testMotor);

## Stop Motor with Limit Switch
A limit switch halts a motor when it reaches a specified limit. This can be crucial in preventing mechanical damage or undesired behavior. Upon the switch's trigger, the software stops the motor, ensuring controlled movement.

    private DigitalInput limitSwitch = new DigitalInput(0); //limit switch plugged into DIO port 0
    
    @Override
    public void teleopPeriodic() {
      if (limitSwitch.get()){ 
        testMotor.set(0.0);  // Set the motor speed to 0.0, which is off
      } else {
        testMotor.set(driveController.getLeftY()); // Set the motor to half speed
      }
    }

The `limitSwitch.get()` function returns a boolean value that indicates the state of the limit switch. If the switch is pressed (i.e., the limit has been reached), it returns true. If the switch is not pressed (i.e., the limit has not been reached), it returns false. The return value of this function can be used to conditionally control the motor, stopping it when the limit is reached.

# Tasks
1. **Explore SparkMax motor controller:** Using the project from the previous section, modify it to perform the following. Instantiate a SparkMax motor controller and write a simple program that sets its speed to half power in the teleoperated mode. Test this program in the simulator to ensure the motor is working correctly.
2. **Control a motor with a joystick:** Expand your program to control the SparkMax motor with a joystick. Map the Y-axis of the joystick to the power of the motor. Observe how the motor's speed and direction change with the joystick's movement in the simulator.
3. **Invert motor direction:** Experiment with the `setInverted` method. Write a program where the motor direction is inverted after a certain time or based on a button press on the joystick. Observe the behavior in the simulator.
4. **Create a follower motor:** Extend your robot setup to include a second motor. Use the follow method to make the second motor follow the first one. Observe how the follower motor mirrors the actions of the leading motor in the simulator.
5. **Implement a limit switch:** Use a digital input to simulate a limit switch in your code. Program the motor to stop when the limit switch is triggered and run otherwise. Test your setup in the simulator.


