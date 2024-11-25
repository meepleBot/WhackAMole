---

layout: default
title: "Introduction to Sensors"
parent: "Sensors"
nav_order: 1

---

# Sensors
Sensors play a pivotal role in the world of FRC robotics. They help in perceiving the environment or measuring physical properties and can be used for a variety of functions, like determining robot positioning, measuring motor speed, detecting objects, and much more.

This lesson will cover some of the most commonly used sensors in FRC, how to interface them with your robot using WPILib, and the necessary code to get them working.

## Encoders
An encoder is a type of sensor that measures the rotational movement of a shaft. Encoders consist of a rotating disk, an LED or similar light source, and a photosensor, which works by detecting the interruptions of light caused by the disk's rotation. Encoders in FRC are often used to measure the distance a robot has traveled or to regulate the speed of a mechanism.

Encoders return the number of "counts" or "ticks" that have occurred, which corresponds to the number of times the disk has interrupted the light to the photosensor. By knowing the distance your robot travels per tick (which depends on your specific encoder, gear ratios, and wheel diameter), you can convert this tick count to a more meaningful distance.

## Gyroscopes
A gyroscope, or gyro for short, is a device that measures rotational velocity around a particular axis. In FRC, gyros are often used to help robots drive straight or rotate to a specific angle. There are several types of gyros, but most FRC teams use a type called a MEMS gyro that measures the rate of rotation using microscopic mechanical systems.

Gyroscopes return the rate of rotation in degrees per second or radians per second, and also keep track of the total angle that the robot has turned since the gyro was last reset. The angle is calculated by integrating (summing over time) the rate of rotation.

## Potentiometers
A potentiometer is a type of sensor that measures the rotational position of a component. It operates on the principle of varying resistance. In FRC, potentiometers are often used to understand the position of an arm or other rotating mechanism.

Potentiometers return a voltage that is proportional to the angular position of the sensor. This voltage can be converted into an angle by knowing the total voltage range of the sensor and the total angle range that the sensor measures.

## Color Sensors
A color sensor is a type of sensor that detects color using a light source and a photodiode to measure the reflection of light off a surface. In FRC, color sensors are used for tasks like sorting objects by color or following a colored line.

Color sensors return the amount of red, green, and blue light that was detected. This can be used to identify the color of an object, either by comparing the raw RGB values to known values for different colors, or by converting the RGB values to a different color space like HSV.

## Accelerometers
An accelerometer is a type of sensor that measures acceleration, which is the rate of change of velocity. In FRC, accelerometers can be used to measure the impact of a collision, detect if the robot is falling, or help estimate the robot's velocity or position.

Accelerometers return the acceleration of the robot in three axes (X, Y, Z), in units of "g's", where 1 g is approximately 9.81 m/s². The X-axis typically corresponds to forward/backward movement, the Y-axis to left/right movement, and the Z-axis to up/down movement.

## Limit Switches
A limit switch is a simple sensor that closes an electrical circuit when physically pressed. In FRC, limit switches are often used to detect the end of travel of a mechanism, such as an arm or elevator.

Limit switches return a boolean value (true/false) that indicates whether the switch is currently being pressed. Some switches are "normally open" (NO), which means they return false when not pressed and true when pressed, while others are "normally closed" (NC), which means they return true when not pressed and false when pressed. 

    import edu.wpi.first.wpilibj.DigitalInput;
    import edu.wpi.first.wpilibj.TimedRobot;

    public class Robot extends TimedRobot {
        // Initialize a limit switch connected to digital port 0
        DigitalInput limitSwitch = new DigitalInput(0);

        @Override
        public void teleopPeriodic() {
            // Print whether the switch is currently pressed
            System.out.println("Limit switch pressed: " + limitSwitch.get());
        }
    }
