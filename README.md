# Rocket V-0

A small ESP32 rocket-control demo with a 3D-printable parts set, MPU6050 sensor input, OLED status display, and dual servo outputs.

## Repository Contents

- `rocket.ino` - ESP32 Arduino sketch for sensor fusion, display output, and servo control
- `rocket-stl/` - printable STL parts
- `README.md` - project overview and build instructions

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

