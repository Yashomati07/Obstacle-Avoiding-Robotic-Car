# 🚗 Obstacle Avoiding Robotic Car using Arduino UNO



## 📌 Overview

This project presents a **self-driving robotic car** built using an Arduino UNO, ultrasonic sensor, and motor driver shield. The robot autonomously navigates by detecting and avoiding obstacles in its path, making it a foundational project in autonomous robotics and embedded systems.

---

## 🎯 Objective

To design and develop an **autonomous robotic vehicle** that:
- Detects obstacles using an ultrasonic sensor
- Navigates by making decisions in real-time
- Avoids collisions using motor control logic

---

## 🧠 Working Principle

The car uses an **HC-SR04 ultrasonic sensor** to continuously emit sound pulses. When it detects a reflected pulse from an obstacle, the **Arduino UNO** calculates the distance and:
- Moves forward if the path is clear
- Moves backward and turns left/right if an obstacle is detected
- Continuously scans for safer directions using a **servo motor** to pivot the sensor

Motor speed is smoothly controlled using **PWM (Pulse Width Modulation)** to avoid battery strain.

---

## 🔩 Hardware Components

| Component               | Description                                                      |
|------------------------|------------------------------------------------------------------|
| Arduino UNO            | Microcontroller board for logic and control                     |
| L293D Motor Driver     | Dual H-bridge IC for controlling DC motors                       |
| DC Motors ×4           | For 4-wheel differential drive                                   |
| HC-SR04 Ultrasonic     | Obstacle distance detection (range up to 200 cm)                 |
| Servo Motor            | Rotates the ultrasonic sensor to scan left and right             |
| 18650 Li-ion Battery   | Power supply                                                     |
| Jumper Wires           | For internal connections                                         |
| Chassis & Wheels       | Mechanical structure                                             |

---

## 🧠 Software & Libraries

Ensure the following libraries are installed in the Arduino IDE:

```
AFMotor       → https://learn.adafruit.com/adafruit-motor-shield/library-install
NewPing       → https://github.com/livetronic/Arduino-NewPing
Servo         → https://github.com/arduino-libraries/Servo.git
```

Install via:  
`Sketch > Include Library > Manage Libraries`

---

## 🧪 How It Works (Code Logic)

1. The servo motor positions the ultrasonic sensor forward, left, and right.
2. The `NewPing` library reads the distances.
3. If the distance is greater than 15 cm, the car moves forward.
4. If an obstacle is detected:
   - The car stops, moves backward briefly, and scans left and right.
   - It compares distances on both sides and turns toward the side with more clearance.
5. Speed is ramped up gradually using PWM to avoid sudden current draw.

---

## 📦 Code 
//ARDUINO OBSTACLE AVOIDING CAR//

#include  <AFMotor.h>  
#include <NewPing.h>
#include <Servo.h> 

#define TRIG_PIN  A0 
#define ECHO_PIN A1 
#define MAX_DISTANCE 200 
#define MAX_SPEED 190  // sets speed of DC  motors
#define MAX_SPEED_OFFSET 20

NewPing sonar(TRIG_PIN,  ECHO_PIN, MAX_DISTANCE); 

AF_DCMotor motor1(1, MOTOR12_1KHZ); 
AF_DCMotor  motor2(2, MOTOR12_1KHZ);
AF_DCMotor motor3(3, MOTOR34_1KHZ);
AF_DCMotor motor4(4,  MOTOR34_1KHZ);
Servo myservo;   

boolean goesForward=false;
int distance  = 100;
int speedSet = 0;

void setup() {

  myservo.attach(10);  
  myservo.write(115); 
  delay(2000);
  distance = readPing();
  delay(100);
  distance = readPing();
  delay(100);
  distance = readPing();
  delay(100);
  distance = readPing();
  delay(100);
}

void loop() {
 int distanceR  = 0;
 int distanceL =  0;
 delay(40);
 
 if(distance<=15)
 {
  moveStop();
  delay(100);
  moveBackward();
  delay(300);
  moveStop();
  delay(200);
  distanceR = lookRight();
  delay(200);
  distanceL = lookLeft();
  delay(200);

  if(distanceR>=distanceL)
  {
    turnRight();
    moveStop();
  }else
  {
    turnLeft();
    moveStop();
  }
 }else
 {
  moveForward();
 }
 distance = readPing();
}

int lookRight()
{
    myservo.write(50); 
    delay(500);
    int distance = readPing();
    delay(100);
    myservo.write(115); 
    return distance;
}

int  lookLeft()
{
    myservo.write(170); 
    delay(500);
    int distance  = readPing();
    delay(100);
    myservo.write(115); 
    return distance;
    delay(100);
}

int readPing() { 
  delay(70);
  int cm = sonar.ping_cm();
  if(cm==0)
  {
    cm = 250;
  }
  return cm;
}

void moveStop()  {
  motor1.run(RELEASE); 
  motor2.run(RELEASE);
  motor3.run(RELEASE);
  motor4.run(RELEASE);
  } 
  
void moveForward() {

 if(!goesForward)
  {
    goesForward=true;
    motor1.run(FORWARD);      
    motor2.run(FORWARD);
    motor3.run(FORWARD); 
    motor4.run(FORWARD);     
   for (speedSet =  0; speedSet < MAX_SPEED; speedSet +=2) // slowly bring the speed up to avoid loading  down the batteries too quickly
   {
    motor1.setSpeed(speedSet);
    motor2.setSpeed(speedSet);
    motor3.setSpeed(speedSet);
    motor4.setSpeed(speedSet);
    delay(5);
   }
  }
}

void moveBackward() {
    goesForward=false;
    motor1.run(BACKWARD);      
    motor2.run(BACKWARD);
    motor3.run(BACKWARD);
    motor4.run(BACKWARD);  
  for (speedSet = 0; speedSet < MAX_SPEED; speedSet +=2) // slowly bring the  speed up to avoid loading down the batteries too quickly
  {
    motor1.setSpeed(speedSet);
    motor2.setSpeed(speedSet);
    motor3.setSpeed(speedSet);
    motor4.setSpeed(speedSet);
    delay(5);
  }
}  

void turnRight() {
  motor1.run(FORWARD);
  motor2.run(FORWARD);
  motor3.run(BACKWARD);
  motor4.run(BACKWARD);     
  delay(500);
  motor1.run(FORWARD);      
  motor2.run(FORWARD);
  motor3.run(FORWARD);
  motor4.run(FORWARD);      
} 
 
void turnLeft() {
  motor1.run(BACKWARD);     
  motor2.run(BACKWARD);  
  motor3.run(FORWARD);
  motor4.run(FORWARD);   
  delay(500);
  motor1.run(FORWARD);     
  motor2.run(FORWARD);
  motor3.run(FORWARD);
  motor4.run(FORWARD);
}  


---

## 📈 Results

- The robot successfully avoids obstacles in real-time.
- Smooth motion and responsive turning based on ultrasonic input.
- Capable of operating in moderately cluttered indoor environments.

---

## ✅ Applications

- Automated vacuum cleaners (like Roomba)
- Lawn mowing robots
- Basic pathfinding robots
- Educational robotics kits

---

## ➕ Advantages

- Fully autonomous navigation
- Safe for indoor testing
- Modular and easily expandable
- Great for learning embedded systems and mechatronics

---

## ⚠️ Limitations

- Limited sensing range (~2 meters)
- Not suitable for outdoor or high-speed applications
- No memory or mapping — works purely reactively

---

## 🧾 References

- 📹 [YouTube Demo](https://youtu.be/1n_KjpMfVT0?si=SVeUZ91fRAABHC2Q)
- 📄 [Final Report](https://www.slideshare.net/shubhamthakur614/final-report-obstacle-avoiding-roboat)

---

## 🙌 Acknowledgments

Developed by **Yashomati Vasantrao Bawane (2021BEC093)** under guidance of  
**Dr. A. I. Tamboli**  
📍 Shri Guru Gobind Singhji Institute of Engineering & Technology, Nanded

---
