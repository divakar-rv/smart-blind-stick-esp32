# IoT-Based Smart Blind Stick — Obstacle Detection System

An IoT-based assistive device for visually impaired persons, built on the ESP32 and simulated using Wokwi. The system uses three HC-SR04 ultrasonic sensors to detect obstacles in the front, left, and right directions, and alerts the user through a buzzer with direction-coded beep patterns and distance-proportional frequency. A lightweight Wi-Fi web dashboard provides real-time monitoring.

## Overview

Visually impaired persons often face difficulty navigating public spaces independently. This project addresses that by implementing a wearable Smart Blind Stick that detects obstacles at approximately 3 feet height in three directions and alerts the user in real time through audio and visual cues. The entire system is built and validated on the ESP32 using the Wokwi online simulator, making it fully reproducible without physical hardware.

## Features

- Detects obstacles in 3 directions (front, left, right) using HC-SR04 ultrasonic sensors
- Buzzer activates when an obstacle is within 3 meters
- Buzzer frequency increases as the obstacle gets closer, and decreases as it moves away
- Direction-coded beep patterns: 1 beep (front), 2 beeps (left), 3 beeps (right)
- Red/Green LED indicators for obstacle vs. clear path
- Real-time web dashboard over Wi-Fi (auto-refreshes every 3 seconds)
- Sub-100ms response latency from sensor read to alert actuation (tested in simulation)

## Hardware Used

| Component | Quantity | Purpose |
|---|---|---|
| ESP32 Dev Board | 1 | Main microcontroller + Wi-Fi host |
| HC-SR04 Ultrasonic Sensor | 3 | Measures obstacle distance in front, left, and right directions (2–400 cm) |
| Buzzer (Active) | 1 | Audio alert with variable frequency |
| Red LED | 1 | Visual alert – obstacle present within 3 m |
| Green LED | 1 | Visual indicator – all directions clear |
| 220Ω Resistors | 2 | Current limiting for LEDs |
| Breadboard & Jumper Wires | 1 set | Circuit connections on Wokwi |
| USB Power Supply (5V) | 1 | Power source for ESP32 |

## Pin Connections (Wokwi Simulation)

| Component | ESP32 GPIO Pin | Notes |
|---|---|---|
| HC-SR04 Front – TRIG | GPIO 5 | Digital OUTPUT |
| HC-SR04 Front – ECHO | GPIO 18 | Digital INPUT |
| HC-SR04 Left – TRIG | GPIO 19 | Digital OUTPUT |
| HC-SR04 Left – ECHO | GPIO 21 | Digital INPUT |
| HC-SR04 Right – TRIG | GPIO 22 | Digital OUTPUT |
| HC-SR04 Right – ECHO | GPIO 23 | Digital INPUT |
| Red LED (Obstacle) | GPIO 2 | Digital OUTPUT via 220Ω |
| Green LED (Clear) | GPIO 15 | Digital OUTPUT via 220Ω |
| Buzzer | GPIO 13 | Digital OUTPUT (PWM for frequency control) |
| VCC (HC-SR04 sensors) | 3.3V / 5V | Sensor power supply |
| GND (All) | GND | Common ground |

## Circuit Diagram



![Hardware Architecture](docs/hardware-architecture.png)


*(Add the Wokwi circuit screenshot to a `docs/` folder and update the path above)*

## Software Architecture

The firmware is written in Arduino C/C++ and structured into clear functional modules:

| Module / Function | Description |
|---|---|
| `setupWiFi()` | Connects ESP32 to Wokwi-GUEST Wi-Fi and starts HTTP server on port 80 |
| `getDistance(trigPin, echoPin)` | Triggers HC-SR04 TRIG pulse, measures ECHO pulse width, converts to cm |
| `handleAlerts(front, left, right)` | Compares all 3 distances to 3 m threshold; drives buzzer/LED outputs |
| `getBuzzerPattern(direction)` | Returns the buzzer beep count: 1 (front), 2 (left), 3 (right) |
| `handleWebClient()` | Serves HTML dashboard with real-time distance readings and alert status |
| `loop()` | Main loop: read sensors → evaluate → actuate → serve web page → delay 500ms |

## How to Run

1. Open the project in [Wokwi](https://wokwi.com/)
2. Upload `blind_stick.ino` to the ESP32 board in the simulator
3. Start the simulation — the ESP32 connects to the `Wokwi-GUEST` network automatically
4. Adjust the HC-SR04 distance sliders to simulate obstacles in each direction
5. Open the printed IP address in a browser to view the live dashboard

## Testing and Results

Tested across 7 scenarios covering single-direction obstacles, multiple obstacles, boundary conditions, and dashboard refresh — all cases passed, with buzzer/LED behavior and web dashboard values matching expected output in every test.

## Future Work

- GPS integration for turn-by-turn navigation guidance
- Voice alerts via text-to-speech module instead of buzzer-only feedback
- Downward-facing sensor for pothole/water detection
- Mobile app integration (Bluetooth/MQTT) for caregiver monitoring
- Edge ML for obstacle classification (human, vehicle, wall, steps)
- Battery optimization via deep sleep mode
- Physical PCB prototype for real-world deployment

## Author

**Divakar V**
B.Tech, Electronics and Communication Engineering, Vellore Institute of Technology, Chennai
