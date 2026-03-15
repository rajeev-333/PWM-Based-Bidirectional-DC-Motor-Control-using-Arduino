# PWM-Based Bidirectional DC Motor Control using Arduino

## Overview

This project implements **speed and direction control of a DC motor using an Arduino Uno and an H-Bridge motor driver (L293D)**. The system allows the user to control the **motor rotation direction using push buttons** and **adjust motor speed using a potentiometer**.

The Arduino reads the analog signal from the potentiometer and converts it into a **PWM (Pulse Width Modulation) signal** to regulate the motor speed. The **L293D H-bridge motor driver** enables bidirectional motor control by reversing the polarity of the voltage applied across the motor terminals.

This project demonstrates fundamental **embedded systems concepts** including **PWM generation, analog-to-digital conversion (ADC), digital input processing, and motor driver interfacing**.

---

# Project Objectives

* Control **DC motor speed using PWM**
* Control **motor direction (Forward / Reverse)**
* Interface **Arduino with H-Bridge motor driver**
* Demonstrate **embedded system hardware-software integration**

---

# Hardware Components

| Component                 | Description                                                         |
| ------------------------- | ------------------------------------------------------------------- |
| Arduino Uno               | Microcontroller used to process inputs and generate control signals |
| L293D Motor Driver        | H-bridge driver used to control motor direction and speed           |
| DC Motor                  | Actuator whose speed and direction are controlled                   |
| Potentiometer             | Provides variable analog input to control motor speed               |
| Push Buttons (2)          | Used to control motor direction                                     |
| Battery Pack              | External power supply for the motor                                 |
| Breadboard & Jumper Wires | Used for circuit connections                                        |

---

# Microcontroller Used

**Arduino Uno**

Microcontroller: **ATmega328P**

Key Specifications:

| Feature       | Value     |
| ------------- | --------- |
| Architecture  | 8-bit AVR |
| Clock Speed   | 16 MHz    |
| Flash Memory  | 32 KB     |
| SRAM          | 2 KB      |
| Analog Inputs | 6         |
| PWM Outputs   | 6         |

---

# System Architecture

```
Push Button 1 ──┐
                │
Push Button 2 ──┼──► Arduino Uno (ATmega328P)
                │           │
Potentiometer ──┘           │ PWM + Direction Signals
                            ▼
                       L293D Motor Driver
                            │
                      External Battery
                            │
                          DC Motor
```

---

# Circuit Connections

| Arduino Pin | Component         | Function                  |
| ----------- | ----------------- | ------------------------- |
| D8          | L293D Input 1     | Forward direction control |
| D12         | L293D Input 2     | Reverse direction control |
| D11         | L293D Enable Pin  | PWM speed control         |
| D2          | Push Button 1     | Forward command           |
| D3          | Push Button 2     | Reverse command           |
| A0          | Potentiometer     | Speed input               |
| 5V          | L293D Logic Power | Driver logic supply       |
| GND         | Ground            | Common ground             |

---

# Working Principle

### 1. Speed Control (PWM)

The Arduino generates a **PWM signal** on pin **D11** using the `analogWrite()` function. The duty cycle of the PWM determines the speed of the motor.

PWM range:

```
0 → Motor OFF
255 → Maximum Speed
```

---

### 2. Potentiometer Input (ADC)

The potentiometer produces an **analog voltage between 0V and 5V**. Arduino converts this voltage into a digital value using:

```cpp
analogRead(A0);
```

Range:

```
0 – 1023
```

This value is mapped to PWM range:

```cpp
speed = map(pot, 0, 1023, 0, 255);
```

---

### 3. Direction Control

Two push buttons determine the motor rotation direction:

| Button State     | Motor Action     |
| ---------------- | ---------------- |
| Button 1 pressed | Forward rotation |
| Button 2 pressed | Reverse rotation |
| Both released    | Motor stop       |

The Arduino sends HIGH/LOW signals to the **L293D H-bridge inputs** to change motor polarity.

---

# H-Bridge Motor Driver Logic

| IN1 | IN2 | Motor State |
| --- | --- | ----------- |
| 0   | 0   | Stop        |
| 1   | 0   | Forward     |
| 0   | 1   | Reverse     |
| 1   | 1   | Brake       |

The L293D driver allows the motor to rotate in both directions by **reversing current flow through the motor**.

---

# Arduino Code

```cpp
// declare variables
const int forwardPin = 8;
const int backwardPin = 12;
const int delayTime = 1000;
const int but1pin = 2;
const int but2pin = 3;
const int potpin = A0;
const int speedpin = 11;

int but1;
int but2;
int pot;
int speed;

void setup() {
  pinMode(forwardPin, OUTPUT);
  pinMode(backwardPin, OUTPUT);
  pinMode(but1pin, INPUT_PULLUP);
  pinMode(but2pin, INPUT_PULLUP);
}

void loop() {

  but1 = digitalRead(but1pin);
  but2 = digitalRead(but2pin);
  pot = analogRead(potpin);

  speed = map(pot,0,1023,0,255);
  analogWrite(speedpin,speed);

  if(but1 == HIGH){
    digitalWrite(forwardPin,LOW);
  }
  else{
    digitalWrite(forwardPin,HIGH);
  }

  if(but2 == HIGH){
    digitalWrite(backwardPin,LOW);
  }
  else{
    digitalWrite(backwardPin,HIGH);
  }

}
```

---

# Key Embedded System Concepts Demonstrated

* Pulse Width Modulation (PWM)
* Analog-to-Digital Conversion (ADC)
* Digital Input Handling
* H-Bridge Motor Control
* Microcontroller-based Motor Drive
* Hardware-Software Integration

---

# Applications

* Robotics
* Automated Vehicles
* Conveyor Belt Systems
* Industrial Motor Control
* Smart Automation Systems
* Educational Embedded Projects

---

# Possible Improvements

Future upgrades can include:

* Motor speed feedback using encoders
* PID-based speed control
* Bluetooth / WiFi control
* LCD speed display
* Motor current protection
* Soft start acceleration control

---

# Project Structure

```
Arduino-DC-Motor-Control
│
├── Arduino_Code
│   └── motor_control.ino
│
├── Circuit_Diagram
│   └── circuit.png
│
├── Images
│   └── project_setup.png
│
└── README.md
```

---

# Author

Rajeev Gupta

---

# License

This project is released for educational and learning purposes.
