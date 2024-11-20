---

layout: default
title: Introduction to Command Based Programming
parent: Command Based Programming

---
# Introduction to Command Based Programming

Command-based programming offers a structured approach for controlling robot operations in the FIRST Robotics Competition (FRC). By defining individual commands and subsystems, teams can organize their code, make it more readable, and facilitate troubleshooting.

## Why Command Based Programming?

Command-based programming divides robot operations into a series of commands and subsystems. This approach provides:

1. **Structured Robot Code:** Each command represents a single robot operation, and each subsystem represents a collection of related hardware components. This modular approach promotes clarity and simplifies debugging.
2. **Benefits:** With commands acting as "tasks" and subsystems as "resources", the command-based framework provides an intuitive way to schedule and handle multiple operations concurrently.

## Subsystems

Subsystems group related hardware components. For example, a drivetrain could be a subsystem composed of multiple motors.

    import edu.wpi.first.wpilibj2.command.SubsystemBase;
    
    public class Drivetrain extends SubsystemBase {
        // Example motor declaration
        private final Motor leftMotor = new Motor();
        private final Motor rightMotor = new Motor();
    
        public void drive(double speed) {
            leftMotor.set(speed);
            rightMotor.set(speed);
        }
    }

1. **Definition and Purpose:** Subsystems manage related robot components as cohesive units. They also provide a structured way to organize code around these components.
2. **Declaring Subsystems:** Typically defined as classes, subsystems incorporate methods to control their associated components.
3. **Controlling Hardware Components:** Subsystems contain methods to read from and write to the physical hardware, like setting motor speeds or reading sensor data.

## Commands
Commands represent individual robot operations and use subsystems to achieve their desired effects.

    import edu.wpi.first.wpilibj2.command.CommandBase;

    public class DriveForward extends CommandBase {
        private final Drivetrain drivetrain;

        public DriveForward(Drivetrain dt) {
            drivetrain = dt;
            addRequirements(drivetrain); // This command requires the drivetrain subsystem.
        }

        @Override
        public void initialize() {}

        @Override
        public void execute() {
            drivetrain.drive(0.5);  // Drive forward at half speed
        }

        @Override
        public boolean isFinished() {
            return false;  // This command never ends on its own.
        }

        @Override
        public void end(boolean interrupted) {
            drivetrain.drive(0);  // Stop the drivetrain when the command ends
        }   
    }


1. **Definition and Purpose:** Commands are actions or operations the robot can perform. For instance, a command might make the robot drive forward or rotate.
2. **Defining Commands:** Commands are usually defined as classes with methods to handle their lifecycle:
+ initialize(): Called when the command starts.
+ execute(): Repeatedly called until the command finishes.
+ end(): Called once when the command completes.
+ isFinished(): Indicates when the command should stop.
3. **Interaction with Subsystems:** Commands utilize subsystems to interact with hardware components. A command will "require" a subsystem, ensuring exclusive access to it for the command's duration. Default

## Default Commands
Subsystems can have associated default commands that run when no other command requires the subsystem.

    // In robot container
    drivetrain.setDefaultCommand(new DriveForward(drivetrain));

1. **Importance:** Default commands allow for continuous operation of a subsystem when not interrupted by other commands.
2. **Setting Default Commands:** The subsystem's setDefaultCommand() method assigns its default command.

# Tasks:
1. **Setting Up a Project:** Initialize a robot project with a command-based structure using WPILib in your preferred programming environment.
2. **Defining a Subsystem:** Create a Drivetrain subsystem class, integrating motors as components.
3. **Creating a Command:** Develop a DriveForward command. Within the command's execute() method, instruct the Drivetrain subsystem to move forward.

## Additional Resources

1. [WPILib Command Based Programming Guide](https://docs.wpilib.org/en/stable/docs/software/commandbased/index.html)
2. [Creating a Subsystem in WPILib](https://docs.wpilib.org/en/stable/docs/software/commandbased/subsystems.html)
3. [Creating Commands with WPILib](https://docs.wpilib.org/en/stable/docs/software/commandbased/commands.html)

