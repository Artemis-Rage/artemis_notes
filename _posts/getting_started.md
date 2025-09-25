---
layout: home
---
These instructions are tailored specifically for using a LEGO Spike Prime with a ChromeBook.

### 1.1 The PyBricks Firmware Installation: A Detailed Walkthrough

> NOTE: I haven't tried this process with a ChromeBook, so let me know if you have any issues!

To run PyBricks programs, the default LEGO firmware on the Spike Prime Hub must be replaced with the PyBricks firmware. This process is safe and fully reversible.

#### Step 1: Preparing the ChromeBook

The Chrome browser has security features that prevent websites from directly accessing USB devices. For the firmware installation, we need to change a setting in the browser:

1. Open the Google Chrome browser.
2. In the address bar, type `chrome://flags` and press Enter.
3. A page with "Experiments" will appear. In the search bar at the top of this page, type `experimental-web-platform-features`.
4. Find the flag titled **Experimental Web Platform features**.
5. Click the dropdown menu next to it and change its value from "Default" or "Disabled" to **Enabled**.
6. A prompt will appear at the bottom of the screen to restart the browser. Click **Relaunch**.

This step grants the official PyBricks website the necessary permissions to communicate with the hub over USB for the firmware update.

#### Step 2: Flashing the PyBricks Firmware

With the ChromeBook and hub prepared, you can now proceed with the installation.
1. Navigate to the PyBricks Code website: [code.pybricks.com](http://code.pybricks.com).
2. In the PyBricks Code interface, click the **Tools** icon (a gear or wrench) in the sidebar, and then select **Install Pybricks Firmware**.
3. The installation wizard will guide you through the installation.

### 1.3 The PyBricks Code Interface and First Connection

With the firmware installed, you can now disconnect the USB cable and begin programming wirelessly.

The PyBricks Code interface (`code.pybricks.com`) is a self-contained Integrated Development Environment (IDE). Key areas include:

- **Code Editor:** The central pane where you will write your Python scripts.
- **File Management:** A sidebar for creating, opening, saving, and managing your program files (`.py` files).
- **Documentation Pane:** A helpful pane that provides quick access to the official PyBricks documentation for various functions and classes.
- **Action Buttons:** Buttons to **Run** (download and execute the current program) and **Stop** the program on the hub.
- **Bluetooth Connection Button:** An icon used to initiate the wireless connection to your hub.
- **Output Pane:** A terminal at the bottom that displays any text from `print()` statements in your code, as well as error messages.

**Connecting to the Hub via Bluetooth:**

1. **Important:** Do not attempt to pair the hub using the ChromeBook's main system settings. The connection must be initiated from within the PyBricks web app.8 If you have previously paired it in the system settings, you must "forget" or "remove" that device before proceeding.
2. Turn on your PyBricks-enabled hub.
3. In the PyBricks Code interface, click the Bluetooth icon.
4. A Chrome pop-up will appear, scanning for nearby devices. Select your uniquely named hub from the list and click **Pair**.    
5. The hub will play a confirmation sound, and its center light will turn a solid color (e.g., blue) to indicate a successful connection.

## Part 2: PyBricks and Python Fundamentals

With the environment set up, the focus now shifts to programming. These initial lessons use only the hub's built-in features—its lights and speaker. This approach isolates the core concepts of coding from the complexities of mechanical design, allowing learners to see immediate, tangible results from their code.

### Lesson 2.1: Anatomy of a PyBricks Program

Let's examine a "Hello, World!" program, the traditional first program for any new language.

```python
 # This first line is called a "shebang." It's a standard header
 # that tells the hub's operating system that this file is a PyBricks
 # MicroPython script and how to execute it. Every program should 
 # start with this line.
 #!/usr/bin/env pybricks-micropython

# Part 1: IMPORTS
# Python code is organized into modules, which are like toolboxes 
# containing specific tools. The `import` statement allows us to bring
# in the tools we need. Here, we import the `PrimeHub` class from the 
# `pybricks.hubs` module and the `wait` function from the 
# `pybricks.tools` module.
from pybricks.hubs import PrimeHub
from pybricks.tools import wait

# Part 2: INITIALIZATION
# This line is an example of object instantiation. We are creating a
# digital representation, or "object," of our physical hardware. We 
# create an instance of the `PrimeHub` class and assign it to a 
# variable named `hub`. Now, whenever we want to control the hub, we 
# will use this `hub` variable.
hub = PrimeHub()

# Part 3: MAIN PROGRAM
# The `print()` function sends text or data from the hub back to the 
# Output Pane in the PyBricks Code interface.
print("Hello, World!")

# Wait for 2 seconds (2000 milliseconds) before the program ends.
wait(2000) 
```

## Part 3: Bringing Your Robot to Life: The Driving Base

In this section we will take control of a simple robot. We will learn how to define a driving base and how to control it using PyBricks.

### Lesson 3.1: Assembling the Driving Base

This lessons below assume that we are using the PyBricks _StarterBot_, a simple robot that will allow us to get familiar with the basics. The directions for assembling the robot can be found [here](https://pybricks.com/learn/building-a-robot/spike-prime/).

### Lesson 3.2: Direct Motor Control

Before making the robot drive as a single unit, we will learn how to drive each motor individually.

#### Initializing Motors in Code:

First we need to tell the program about the motors we have connected to the hub. In particular, we need to specify what port the motor is connected to, and which way we want it to be oriented (i.e. "which way is forward").

```python
from pybricks.pupdevices import Motor
from pybricks.parameters import Port, Direction
from pybricks.tools import wait

# Initialize the left motor, connected to Port A.
# If this motor turns backward when we want it to go forward,
# we add Direction.COUNTERCLOCKWISE to reverse its positive direction.
left_motor = Motor(Port.A, positive_direction=Direction.COUNTERCLOCKWISE)

# Initialize the right motor, connected to Port B.
right_motor = Motor(Port.B, positive_direction=Direction.CLOCKWISE)
```

#### Fundamental Motor Commands:

The Motor object has several methods for controlling its movement 22:

- `motor.run(speed)`: Runs the motor continuously at a given speed, measured in degrees per second. A negative speed value reverses the direction. The motor will run forever until another command is given.
- `motor.stop()`: Stops the motor and lets it "coast" to a halt. The motor shaft can be turned freely.
- `motor.hold()`: Stops the motor and actively holds it in its current position, resisting external force.
- `motor.run_time(speed, time)`: Runs the motor at a given speed for a specific duration in milliseconds.
- `motor.run_angle(speed, angle)`: Rotates the motor by a specific angle in degrees relative to its current position.

#### Motor Connection Test

Now, we can write a short piece of code that tests that we have properly connected to our two motors:
```python
from pybricks.pupdevices import Motor
from pybricks.parameters import Port, Direction
from pybricks.tools import wait

left_motor = Motor(Port.A, positive_direction=Direction.COUNTERCLOCKWISE)
right_motor = Motor(Port.B, positive_direction=Direction.CLOCKWISE)

print("Testing left motor...")
left_motor.run_angle(360, 360) # Run at 360 deg/s for one full rotation
wait(2000)

print("Testing right motor...")
right_motor.run_angle(360, 360)
wait(2000)

print("Test complete.")
```

When this program runs, both wheels should turn one full rotation in the forward direction. If one turns backward, this means we set `positive_direction` incorrectly.

### Lesson 3.3: Using the `DriveBase` Class

> Key Concept: a `Class` is like a plan that tells us how to describe a type of object. For example, the `DriveBase` class tells us how to describe a simple robot. For example, we need to specify where the motors are connected, how big the wheels are and how far apart they are. When we write down a specific description of a robot, it is called an _instance_ of the `DriveBase` class. Lets try an example to make this clear.

#### Creating a  `DriveBase`:

The DriveBase object requires the two motor objects and two key physical measurements of your robot: the diameter of its wheels and the distance between them.

```python
from pybricks.pupdevices import Motor
from pybricks.parameters import Port, Direction
from pybricks.robotics import DriveBase
from pybricks.tools import wait

left_motor = Motor(Port.A, positive_direction=Direction.COUNTERCLOCKWISE)
right_motor = Motor(Port.B, positive_direction=Direction.CLOCKWISE)


# Create the DriveBase object.
# You MUST measure your robot's wheel diameter and axle track in millimeters.
robot = DriveBase(left_motor, right_motor, wheel_diameter=56, axle_track=114)
```

Great! Now we have an _instance_ of the `DriveBase` class called `robot`.

#### `DriveBase` Methods
Now that we have constructed our robot, we want to send it commands. The instructions we can give to an instance of a class are called _methods_. For example, the `DriveBase` class has the following methods:
- `robot.straight(distance)`: Drives the robot in a straight line for a given `distance` in millimeters. A negative value drives backward.
- `robot.turn(angle)`: Turns the robot in place by a given `angle` in degrees. Positive angles turn right (clockwise), and negative angles turn left (counter-clockwise).
- `robot.curve(radius, angle)`: Drives the robot along a circular path with a specified `radius` (in mm) for a certain `angle` (in degrees).

### Lesson 3.4: Navigational Challenges

> Key Concept: when we want to repeat a piece of code over and over, we use a _loop_. A common example of a loop in Python is a `for` loop. We will use a `for` loop below to drive in specific patterns.

#### Challenge 1: Drive a Square

Use a for loop to repeat the actions of driving straight and turning 90 degrees.

```python
from pybricks.pupdevices import Motor
from pybricks.parameters import Port, Direction
from pybricks.robotics import DriveBase
from pybricks.tools import wait

left_motor = Motor(Port.A, positive_direction=Direction.COUNTERCLOCKWISE)
right_motor = Motor(Port.B, positive_direction=Direction.CLOCKWISE)

# Create the DriveBase object.
robot = DriveBase(left_motor, right_motor, wheel_diameter=56, axle_track=114)

# Drive in a 300mm x 300mm square
for i in range(4):
	robot.straight(300)
	robot.turn(90)
```

#### Challenge 2: Drive an Equilateral Triangle

This requires calculating the correct exterior angle for the turn. 

```python
from pybricks.pupdevices import Motor
from pybricks.parameters import Port, Direction
from pybricks.robotics import DriveBase
from pybricks.tools import wait

left_motor = Motor(Port.A, positive_direction=Direction.COUNTERCLOCKWISE)
right_motor = Motor(Port.B, positive_direction=Direction.CLOCKWISE)

# Create the DriveBase object.
robot = DriveBase(left_motor, right_motor, wheel_diameter=56, axle_track=114)

# Drive in an equilateral triangle with 300mm sides
for i in range(3):
	robot.straight(300)
	# Figure out how many degrees we need to turn each time!
	# robot.turn(???)
```

## Part 4: Adding Sensors

Let's add some sensors to our robot to give it eyes and ears.
### Lesson 4.1: Add an Ultrasonic Distance Sensor


```python
from pybricks.pupdevices import UltrasonicSensor
from pybricks.parameters import Port

# Initialize the Ultrasonic Sensor, assuming it is connected to Port D.
# Check your physical robot to confirm the port.
distance_sensor = UltrasonicSensor(Port.D) 
```

#### Reading Data from the Sensor:

The primary _method_ for this sensor is .distance(), which returns the measured distance to the nearest object in millimeters. Let's write a program that continuously prints its readings to the console.

```python
from pybricks.pupdevices import UltrasonicSensor
from pybricks.parameters import Port

# Initialize the Ultrasonic Sensor
distance_sensor = UltrasonicSensor(Port.D) 

while True:
    # Read the distance and print it to the output pane
    dist_mm = distance_sensor.distance()
    print("Distance:", dist_mm)
    
    # Wait a short time to avoid flooding the console
    wait(100)
```

### Lesson 4.2: Responding to Sensor Input

In robotics, we need to use sensor readings to decide what the robot does next. Let's write a program that uses sensor readings to control its behavior.

> Key Concept: an `if...else` statement allows us to take different actions based on inputs such as sensor readings. This is essential for writing responsive programs.

#### The Obstacle Avoider Program:

In this simple program, our robot will move forward until it detects an obstacle. It will then back up and turn. Insert appropriate commands to make the robot perform obstacle avoidance. Methods you may  find useful are
- `robot.straight(d)`: drives straight for `d` centimeters. Note that `d` can be negative to go in reverse.
- `robot.turn(a)`:  turn clockwise `a` degrees. A negative value will turn counterclockwise.
- `robot.stop()`: duh.
- `robot.drive(s, t)`: start driving with a speed of `s` and _turn rate_ `t`. Setting `t=0` will drive straight.

> Question: what is the difference between `robot.straight(d)` and `robot.drive(s, 0)`?


```python
#!/usr/bin/env pybricks-micropython
from pybricks.hubs import PrimeHub
from pybricks.pupdevices import Motor, UltrasonicSensor
from pybricks.parameters import Port, Direction
from pybricks.robotics import DriveBase
from pybricks.tools import wait

# Initialize the Hub, Motors, and DriveBase
hub = PrimeHub()
left_motor = Motor(Port.A, Direction.COUNTERCLOCKWISE)
right_motor = Motor(Port.B)
robot = DriveBase(left_motor, right_motor, wheel_diameter=56, axle_track=114)

# Initialize the Ultrasonic Sensor
distance_sensor = UltrasonicSensor(Port.D)

# Define a threshold distance in millimeters.
# The robot will react when an object is closer than this.
OBSTACLE_DISTANCE = 150

# The main loop for autonomous behavior
while True:
    # Check if an obstacle is detected within the threshold
    if distance_sensor.distance() < OBSTACLE_DISTANCE:
        # Obstacle detected! Stop, back up, and turn right.
        #
        # INSERT COMMAND(S) HERE TO MAKE THE ROBOT STOP, BACK UP AND TURN.
        #
    else:
        # No obstacle nearby, so drive forward slowly.
        #
        # INSERT COMMAND(S) HERE TO MAKE THE ROBOT DRIVE SLOWLY FORWARD.
        #

    # A small delay in the loop is good practice to allow
    # the system to process other tasks.
    wait(20)
```

#### Final Challenge: The Arena

Place the robot in an "arena" made from cardboard boxes or books. Run the obstacle avoidance program and observe its behavior. Does it successfully navigate the space? Does it ever get stuck in a corner? Try improving your program by changing the movement commands, or adding additional logic based on sensor feedback.

## Resources

- **Official PyBricks Documentation:** [docs.pybricks.com](https://docs.pybricks.com/) — The definitive reference for all PyBricks classes, functions, and modules. This should be the first place you look for technical details.2
- **PyBricks Learn Pages:** [pybricks.com/learn/](https://pybricks.com/learn/) — A collection of official tutorials and guides that expand on the topics covered here.4
- **PyBricks GitHub Repository:** [github.com/pybricks/pybricks-micropython](https://github.com/pybricks/pybricks-micropython) — The place to report bugs, ask for help, and see the source code of the project.1
- **PyBricks YouTube Channel:** A source for video tutorials, project demonstrations, and feature announcements.

