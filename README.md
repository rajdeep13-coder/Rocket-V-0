# Rocket V-0

A production-grade, self-calibrating ESP32 flight controller demonstration. Features a high-precision **deterministic 100 Hz control loop** (`micros()`), automatic **sensor offset calibration**, a **non-blocking 5 Hz OLED Telemetry Dashboard (HUD)**, a dual-servo stabilization layout, and full hardware-free local simulation support.

## Repository Contents

- `rocket.ino` - High-performance ESP32 Arduino sketch for sensor fusion, attitude control, OLED HUD telemetry, and servo actuation.
- `rocket-stl/` - 3D-printable airframe, bracket, and nosecone parts.
- `software simulation.md` - High-fidelity local simulation, testing, and debugging workflow.
- `README.md` - Project hardware wiring, dependencies, and build instructions.

## 3D Printing

Recommended starting point:

- Layer height: 0.3 mm
- Supports: not required for the current models

## Hardware

- ESP32 board
- MPU6050
- OLED screen
- 2 servos
- Step-down converter if you are using a battery supply
- USB power or a 9V battery, depending on your setup

## Arduino Libraries

Install these libraries before building the sketch:

- Adafruit MPU6050
- Adafruit Unified Sensor
- ESP32Servo
- Adafruit GFX Library
- Adafruit SSD1306

## Wiring

- MPU6050 and OLED connect to the I2C bus
- Servo pins are `18` and `19`
- The sketch is written for an ESP32-compatible board

## Build

1. Open `rocket.ino` in the Arduino IDE.
2. Select an ESP32 board such as Node32s or another compatible ESP32 target.
3. Install the libraries listed above.
4. Upload the sketch.

