# BAE305-SP25-Lab8
# Laboratory Report for Lab 8 of BAE305 Spring 2025
# Lab 8 - There's an App for That: App Development

* By: Abby Phillips and Audrey Suit
* Submitted: April 2nd, 2025


# Summary  


# Materials


# Assembly Procedures  
![Screenshot 2025-03-31 182353](https://github.com/user-attachments/assets/59a6706f-3cf7-4d69-bb3e-41e1da2baeb8)

![image](https://github.com/user-attachments/assets/bc9b2d88-b275-41f7-8060-d7ebe8239a1c)

![Screenshot 2025-03-31 182931](https://github.com/user-attachments/assets/ec383f69-0e4b-471a-b302-5eec565a0157)

![image](https://github.com/user-attachments/assets/c56a4b67-445d-47c7-9230-e656b85901ae)


# Test Equipment


# Test Procedures


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
<p align="left"><em> Program 2.2: T  </em></p>


# Discussion


# Conclusion
