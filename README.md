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

## 📦 Code File

See [`ARDUINO_OBSTACLE_AVOIDING_CAR.ino`](ARDUINO_OBSTACLE_AVOIDING_CAR.ino.txt) for the full source code.

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
