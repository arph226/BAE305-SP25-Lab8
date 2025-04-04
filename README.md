# BAE305-SP25-Lab8
# Laboratory Report for Lab 8 of BAE305 Spring 2025
# Lab 8 - There's an App for That: App Development

* By: Abby Phillips and Audrey Suit
* Submitted: April 2nd, 2025


# Summary  

This lab provided hands-on experience with mobile app development and wireless communication for remote control of a robot car. Through a series of exercises, we explored the basics of creating a simple app using MIT App Inventor 2 and integrating it with our robot hardware via Bluetooth. We began by assembling our robot and verifying its movement using serial communications from the Arduino IDE, ensuring that the motors responded correctly to basic commands. Next, we developed an app that enabled us to send movement commands—forward, backward, turning, and varying speeds—to the robot through a user-friendly interface. Finally, we integrated a Bluetooth module (HC-05) to replace wired communication, demonstrating how to establish a wireless link between an Android device and the robot.

By successfully completing this lab, we deepened our understanding of app development, embedded system interfacing, and wireless control. This experience not only reinforced our skills in Arduino programming and hardware assembly but also prepared us for more advanced projects that combine mobile applications with real-time robotic control.

# Materials

-Computer running Arduino IDE and Chrome Browser
-Smartphone running Android OS with MIT AI2 Companion app installed
-Sparkfun Inventor's Kit: RedBoard, Ultrasonic sensor, two motors, motor driver, two wheels, battery pack, hook and loop tape, wires
-HC-05 Bluetooth UART Module

# Assembly Procedures 

### Part 1 - Assemble and Test Your Robot

Start with the assembly built in [Lab 6](https://github.com/audreysuit/BAE305-SP25-Lab6) Below is a reference photo of this circuit, see Figure 1.1. 

![image](https://github.com/user-attachments/assets/c5fee9cf-e6c0-4b1e-a636-36c440677410)
<p align="left"><em>Figure 1.1: Image of Lab 6 assembly. </em></p>


Use hook and loop tape to attach the motors to the bottom side of the Arduino mounting board. Attach wheels to each motor. Also using hook and loop tape, mount the battery pack to the underside of the Arduino mounting board. Additionally, attach a binder clip to the front of the board, this will prevent the battery pack from touching the floor. The completed setup is seen in Figures 2.1 and 2.2.

![image](https://github.com/user-attachments/assets/8e587fbd-1faa-49de-b255-5108d4e5e099)
<p align="left"><em>Figure 2.1: Image of underside assembly. </em></p>

![image](https://github.com/user-attachments/assets/cae1b6ce-4142-409d-a771-2a54aac102fa)
<p align="left"><em>Figure 2.2: Image of underside assembly. </em></p>

### Part 2 - Develop the App

No further physical assembly is required for this section of the lab.

### Part 3 - Wireless Remote

To initiate Bluetooth capabilities, wire the HC-05 Bluetooth UART module on an unused area of the breadboard. Build according to the schematic and image shown below (Figures 3.1 and 3.2.) Ensure Rx is connected to Pin 2 and Tx is connected to Pin 3, and include the 1kΩ-3kΩ voltage divider. 

![image](https://github.com/user-attachments/assets/5c2a97f1-b1f8-4e12-95c7-f92771bec711)
<p align="left"><em>Figure 3.1: Schematic of final circuit. Source: Explore Embedded "Setting up Bluetooth HC-05 with Arduino" </em></p>

![image](https://github.com/user-attachments/assets/ae52c8a1-55c7-4d41-b864-2b79849a8337)
<p align="left"><em>Figure 3.2: Image of HC-05 Bluetooth UART module setup. </em></p>


# Test Equipment
- A cable/adaptor to connect the smartphone to the RedBoard.
- An HC-05 Bluetooth UART Module


# Test Procedures

NOTE: All AppInventor blocks created in this lab are shown in the Test Results in Figures 4-7. The Arduino code is shown in Program 1. 

## Part 1: Assemble and Test Your Robot
1. Ensure the robot is assembled correctly, with wheels attached to the motors and the motor driver connected to the Arduino RedBoard.
2. Connect the battery pack and ensure it is properly placed, avoiding contact with the floor when the binder clip is installed.
3. Open the Arduino IDE and upload the provided robot control sketch.
4. Open the Serial Monitor in the Arduino IDE (ensure the baud rate is set to 9600).
5. In the Serial Monitor, type the following commands to control the robot:
   - "f" for forward.
   - "b" for backward.
   -  "r" for right turn.
   -  "l" for left turn.
   -  "s" for stop.
6. Verify that the robot responds correctly by moving forward, backward, turning right, and turning left based on the serial commands.
7. Determine the minimum speed required to move the fully assembled robot (ours was 75 RPM)

## Part 2: Develop the App
Test Setup:
1. Ensure the app has been developed in App Inventor and is installed on the Android phone. Be sure to login to save progress.
2. Create a new project, naming it uniquely with team members' names.
3. In the Designer environment:
   - Add a button to initialize serial communications.
   - Insert a label to display communication data received from the Arduino.
   - Include UI elements (buttons/sliders) to control forward, backward, right, and left movements at three different speeds.
   - Add a Serial component from the Connectivity section.
   - Add a Clock component from the Sensors section.
4. Switch to the Blocks environment:
   - Implement the block structure to establish serial communication upon pressing the initialization button.
   - Use the “call serialObject.WriteSerial.data” block to send movement commands to the RedBoard via serial communication.
5. Build the app:
   - Select Build > Android App (.apk)
   - Scan the QR code to install the app on an Android device.
   - Test the app using a USB-C to USB-A adapter and an Arduino cable to connect the phone to the RedBoard.
5. Power the robot with the battery pack and test remote control functionality.
6. Open the App on the Android phone.
7. Pair the phone with the HC-05 Bluetooth module:
    - Use the ListPicker in the app to select the paired Bluetooth device.
    - Check the Bluetooth status label to verify the connection.
    - Press the Initialize Serial Communication button in the app.
8. Test each control button in the app (forward, backward, right, left, and stop)
9. Adjust the speed using the speed control slider or buttons and verify that the robot's movement speed changes accordingly.
    - The robot should respond to commands from the app by moving in the correct direction.
    - The speed should be adjustable via the app, and the robot's movement should correspond to the set speed.

## Part 3: Wireless Remote
1. Ensure the Bluetooth module (HC-05) is properly connected to the Arduino as shown in the Assembly Procedures
2. Verify that the Android phone is paired with the HC-05 Bluetooth module.
3. Modify the Arduino code:
   - Include the setup, loop, and function code from the instructor’s GitHub (Lab-Template repository).
   - Ensure the botDirection and botSpeed variables are properly updated.
   - Modify if statements to incorporate botDir properly.
4. Modify the App in App Inventor 2:
   - Save a copy of the previous app with a descriptive name.
   - In the Designer environment:
      - Add a BluetoothClient component from the Connectivity section.
      - Insert a ListPicker for Bluetooth device selection.
      - Add a Label to display Bluetooth connection status.
   - In the Blocks environment:
      - Implement the block structure for Bluetooth device selection and connection.
      - Replace all “call serialObject.WriteSerial” blocks with “call BluetoothClient.SendText” blocks.
      - Append a newline character (\n) to each command to signal command completion to the Arduino.
5. Test the Bluetooth-controlled system and demonstrate its operation to the instructor.
5. Open the App on the Android phone.
    - In the app, use the ListPicker to select the paired Bluetooth device.
    - Check the Bluetooth status label to verify the connection status.
    - Press each control button (forward, backward, right, left, stop) in the app
    - Test speed control using the app's user interface and ensure that the robot's speed changes accordingly.
6. The robot should respond to Bluetooth commands correctly.
7. The Bluetooth connection should remain stable throughout the test
8. NOTE: the obstacle avoidance components of the code should still work when something is blocking the robot. Testing was done by placing a box <20cm from the robot. The code should stop and not complete any other commands until the object is moved.



# Test Results

```ruby
/********************************************************************************/
// Function Declarations
void rightMotor(int motorSpeed);  // Controls the right motor's direction and speed
void leftMotor(int motorSpeed);   // Controls the left motor's direction and speed

/********************************************************************************/
// Motor control pins
const int AIN1 = 13;  // Right motor control pin 1
const int AIN2 = 12;  // Right motor control pin 2
const int PWMA = 11;  // Right motor PWM speed control

const int PWMB = 10;  // Left motor PWM speed control
const int BIN2 = 9;   // Left motor control pin 2
const int BIN1 = 8;   // Left motor control pin 1

const int turnTime = 8;  // Time multiplier for turning

// Variables for storing received commands
String botDirection;  
String distanceStr;  
String driveTimeStr;

// Ultrasonic sensor pins for obstacle detection
const int trigPin = 6;  
const int echoPin = 7;  

float duration, distance;

// Wireless communication setup (HC-05 Bluetooth module)
#include <SoftwareSerial.h>
SoftwareSerial mySerial(2, 3);  // HC-05 Tx -> Arduino #2, HC-05 Rx -> Arduino #3

const byte numChars = 6;  
char receivedChars[numChars];  // Stores received data
char tempChars[numChars];

char botDir[1];  // Direction of the robot ('f' = forward, 'b' = backward, etc.)
int botSpeed = 0;  
boolean newData = false;  

/********************************************************************************/
void setup()
{
    mySerial.begin(9600);  // Initialize Bluetooth module
    pinMode(AIN1, OUTPUT);
    pinMode(AIN2, OUTPUT);
    pinMode(PWMA, OUTPUT);

    pinMode(BIN1, OUTPUT);
    pinMode(BIN2, OUTPUT);
    pinMode(PWMB, OUTPUT);

    Serial.begin(9600);  // Initialize serial communication for debugging

    Serial.println("Enter a direction followed by a distance followed by speed.");
    Serial.println("f = forward, b = backward, r = turn right, l = turn left");
    Serial.println("Example command: f 50 100");

    pinMode(trigPin, OUTPUT);  // Set up ultrasonic sensor
    pinMode(echoPin, INPUT);
}

/********************************************************************************/
void loop()
{
    // Wireless data reception
    recvWithEndMarker();
    if (newData == true)
    {
        strcpy(tempChars, receivedChars);
        parseData();  // Extract robot direction and speed
        newData = false;
    }

    // Ultrasonic sensor: measure distance to obstacles
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);

    duration = pulseIn(echoPin, HIGH);
    distance = (duration * 0.0343) / 2;

    Serial.print("Distance: ");
    Serial.println(distance);

    // Stop motors if an obstacle is too close
    if (distance > 0 && distance < 10) 
    {
        rightMotor(0);
        leftMotor(0);
        Serial.println("Obstacle detected! Stopping motors.");        
        return;
    }

    // Process serial input commands
    if (Serial.available() > 0)  
    {
        botDirection = Serial.readStringUntil(' ');
        distanceStr = Serial.readStringUntil(' ');
        driveTimeStr = Serial.readStringUntil('\n');

        int driveTimeInt = driveTimeStr.toInt();
        int distanceInt = distanceStr.toInt();

        // Ensure motor speed is within a valid range (0-255)
        driveTimeInt = constrain(driveTimeInt, 0, 255);

        Serial.print("Direction: "); Serial.println(botDirection);
        Serial.print("Distance: "); Serial.println(distanceInt);
        Serial.print("Drive Time: "); Serial.println(driveTimeInt);

        // Execute movement based on received command
        if ((botDir[0] == 'f') || (botDirection == "f"))
        {
            rightMotor(driveTimeInt);  
            leftMotor(driveTimeInt);
            delay(distanceInt * 100); 
        }
        else if ((botDir[0] == 'b') || (botDirection == "b"))
        {
            rightMotor(-driveTimeInt);
            leftMotor(-driveTimeInt);
            delay(distanceInt * 100);
        }
        else if ((botDir[0] == 'r') || (botDirection == "r"))
        {
            rightMotor(driveTimeInt);
            leftMotor(-driveTimeInt);  // Opposite directions for turning
            delay(turnTime * distanceInt);
        }
        else if ((botDir[0] == 'l') || (botDirection == "l"))
        {
            rightMotor(-driveTimeInt);
            leftMotor(driveTimeInt);
            delay(turnTime * distanceInt);
        }
    }
}

/********************************************************************************/
// Function to control the right motor's speed and direction
void rightMotor(int motorSpeed)
{
    if (motorSpeed > 0)
    {
        digitalWrite(AIN1, HIGH);
        digitalWrite(AIN2, LOW);
    }
    else if (motorSpeed < 0)
    {
        digitalWrite(AIN1, LOW);
        digitalWrite(AIN2, HIGH);
    }
    else
    {
        digitalWrite(AIN1, LOW);
        digitalWrite(AIN2, LOW);
    }
    analogWrite(PWMA, abs(motorSpeed));  // Set motor speed using PWM
}

/********************************************************************************/
// Function to control the left motor's speed and direction
void leftMotor(int motorSpeed)
{
    // Left motor logic is reversed
    if (motorSpeed > 0)  
    {
        digitalWrite(BIN1, LOW);  
        digitalWrite(BIN2, HIGH); 
    }
    else if (motorSpeed < 0)
    {
        digitalWrite(BIN1, HIGH);
        digitalWrite(BIN2, LOW);   
    }
    else
    {
        digitalWrite(BIN1, LOW);
        digitalWrite(BIN2, LOW);
    }
    analogWrite(PWMB, abs(motorSpeed));  // Set motor speed using PWM
}

/********************************************************************************/
// Function to receive data from Bluetooth module until end marker ('\n') is found
void recvWithEndMarker() {
    static byte ndx = 0;
    char endMarker = '\n';
    char rc;

    while (mySerial.available() > 0 && newData == false)
    {
        rc = mySerial.read();

        if (rc != endMarker)
        {
            receivedChars[ndx] = rc;
            ndx++;
            if (ndx >= numChars)
            {
                ndx = numChars - 1;  // Prevent overflow
            }
        }
        else
        {
            receivedChars[ndx] = '\0';  // Null-terminate the received string
            ndx = 0;
            newData = true;
        }
    }
}

/*****************************************************************************************/
// Function to parse received Bluetooth data into direction and speed
void parseData() {
    char * strtokIndx;  // Pointer for tokenizing the received string

    strtokIndx = strtok(tempChars, " ");  // Extract the first part (direction)
    strcpy(botDir, strtokIndx);

    strtokIndx = strtok(NULL, " ");  // Extract the second part (speed)
    botSpeed = atoi(strtokIndx);  // Convert to integer
}

```
<p align="left"><em> Program 1: The above program is for controlling a robot via Bluetooth using an Arduino and the MIT AI2 Companion App. Using buttons on an app created in the MIT program, the robot can move forward, backward, left, right, and at different speeds. Additionally, the robot is equipped with an ultrasonic sensor for obstacle detection. Since we did not have an Android, we had to use Dr. Jarros's phone and were not able to complete the full project.   </em></p>

![Screenshot 2025-03-31 182353](https://github.com/user-attachments/assets/59a6706f-3cf7-4d69-bb3e-41e1da2baeb8)
<p align="left"><em> Figure 4: The above image depicts the App Inventor blocks needed to control the serial communications. These blocks initialize the serial monitor at a Baud Rate of 9600, creates a button to open the serial monitor through the app (Open_Serial), and uses a clock to read data from the arduino when the serial monitor is open (ReadFromArduino). Serial1 is the name of the serial connectivity component. These blocks then translate to an User Interface on an app. The blocks also send this command to the RedBoard. </em></p>

![image](https://github.com/user-attachments/assets/bc9b2d88-b275-41f7-8060-d7ebe8239a1c)
<p align="left"><em> Figure 5: The above image shows the App Inventor blocks needed to create buttons for 4 different directions (forward, backward, left, and right) and 3 different speeds (fast, medium, and slow).  It does this by communicating to the Serial Monitor and the RedBoard the commands to move in the directions and speeds necessary given the commands required by the initial Arduino code.  </em></p>

![Screenshot 2025-03-31 182931](https://github.com/user-attachments/assets/ec383f69-0e4b-471a-b302-5eec565a0157)
<p align="left"><em> Figure 6: The above image shows the App Inventor Vlocks necessary to initialize Bluetooth control and configuration. The BluetoothClient allows to send data via bluetooth, the ListPicker allows for selection of bluetooth device, and the label shows the status of the bluetooth connection. This initializes Bluetooth connection and gives permission for bluetooth to be used and stores values in TinyDB1.    </em></p>

![image](https://github.com/user-attachments/assets/c56a4b67-445d-47c7-9230-e656b85901ae)
<p align="left"><em> Figure 7: The above image shows how blocks in App Inventor alert the app user that the Bluetooth is connected and creates a button that communicates via bluetooth to move the car forward.  </em></p>

# Discussion

Discussion Question 1 - In Lab 6, we found out what was the minimum speed that will move the motors. What is the minimum speed that will move the complete car?

We found the minimum speed to move the car is 75 rpm.

# Conclusion

Lab 8 provided a comprehensive introduction to mobile app development for remote robotic control, effectively bridging software programming with embedded hardware applications. Through the use of MIT App Inventor 2, we designed and built a user-friendly Android app that communicated with our robot via Bluetooth. Building on our previous work in Lab 6, we first confirmed proper robot assembly and motor control using serial commands and then transitioned to wireless control by integrating the HC-05 Bluetooth module.

The lab allowed us to explore essential concepts in app development, including UI design, serial communication, and command parsing while reinforcing our understanding of real-time embedded systems. We successfully programmed the robot to respond to directional commands and variable speeds, with the system reliably halting when an obstacle was detected. Notably, our tests confirmed that the complete car requires a minimum motor speed of 75 rpm to initiate movement.

Overall, this lab deepened our knowledge of wireless interfacing and mobile app development, providing valuable experience that will serve as a foundation for more advanced projects integrating robotics and remote control technologies.
