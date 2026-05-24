# Rocket V-0

A production-grade, self-calibrating ESP32 flight controller demonstration. Features a high-precision **deterministic 100 Hz control loop** (`micros()`), a **PID-based attitude controller**, a **complementary-filter sensor fusion** pipeline, automatic **sensor offset auto-calibration**, a **non-blocking 5 Hz OLED Telemetry Dashboard (HUD)**, a dual-servo stabilization layout, and full hardware-free local simulation support.

## Repository Contents

- `rocket.ino` - High-performance ESP32 Arduino sketch implementing complementary-filter sensor fusion, a PID-based attitude controller, automatic auto-calibration routines, OLED HUD telemetry, and servo actuation.
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

The system utilizes an I2C shared bus for sensor readings and display updates, and two high-speed PWM lines for dual-servo actuation.

### Circuit Schematic

```text
       +---------------------------------------------+
       |                  ESP32 DevKit               |
       |                                             |
       |  [GPIO 21] (SDA) ---+--------------------+  |
       |  [GPIO 22] (SCL) ---|-+                  |  |
       |  [GPIO 18] (PWM) ---|---|-------------+  |  |
       |  [GPIO 19] (PWM) ---|---|---------+   |  |  |
       |  [ 5V or  VIN  ] ---|---|---------|-+-|-|--|--+ (Power Rail)
       |  [     GND     ] ---|-+-|---------|-|-+-|--|--|--+ (Ground Rail)
       +---------------------|-|-----------|-|-|-|-|-|--|--+
                             | |           | | | | | |  |  |
                  +----------+ |           | | | | | |  |  |
                  | +----------+           | | | | | |  |  |
                  | |                      | | | | | |  |  |
            +-----+-----+            +-----+-----+ | |  |  |
            |    SDA    |            |    SDA    | | |  |  |
            |    SCL    |            |    SCL    | | |  |  |
            |    VCC    |------------|----VCC    | | |  |  |
            |    GND    |------------|----GND    | | |  |  |
            +-----------+            +-----------+ | |  |  |
               MPU6050                   OLED      | |  |  |
              (Gyro/Acc)               (Display)   | |  |  |
                                                   | |  |  |
                                  +----------------+ |  |  |
                                  | +----------------+  |  |
                                  | |                   |  |
                            +-----+-----+         +-----+-----+
                            |    PWM    |         |    PWM    |
                            |    VCC    |---------|----VCC    |
                            |    GND    |---------|----GND    |
                            +-----------+         +-----------+
                               Servo X               Servo Y
                               (Pitch)               (Roll)
```

### Connection Table

| Component | Component Pin | ESP32 GPIO / Pin | Connection Type | Description |
| :--- | :--- | :--- | :--- | :--- |
| **MPU6050** | VCC | `5V` (or `3.3V` / `VIN`) | Power Input | 5V/3.3V Power input |
| | GND | `GND` | Ground Reference | Common ground reference |
| | SDA | `GPIO 21` | I2C Data Bus | Hardware I2C Serial Data (shared) |
| | SCL | `GPIO 22` | I2C Clock Bus | Hardware I2C Serial Clock (shared) |
| **OLED (SSD1306)** | VCC | `5V` (or `3.3V` / `VIN`) | Power Input | 5V/3.3V Power input |
| | GND | `GND` | Ground Reference | Common ground reference |
| | SDA | `GPIO 21` | I2C Data Bus | Hardware I2C Serial Data (shared) |
| | SCL | `GPIO 22` | I2C Clock Bus | Hardware I2C Serial Clock (shared) |
| **Servo X (Pitch)** | PWM / Signal | `GPIO 18` | Output PWM | Servo command pulse line |
| | VCC (Red) | `5V` (or `VIN`) | Power Input | 5V high-current supply input |
| | GND (Black/Brown) | `GND` | Ground Reference | Shared system ground |
| **Servo Y (Roll)** | PWM / Signal | `GPIO 19` | Output PWM | Servo command pulse line |
| | VCC (Red) | `5V` (or `VIN`) | Power Input | 5V high-current supply input |
| | GND (Black/Brown) | `GND` | Ground Reference | Shared system ground |

*Note: Depending on the power draw of your servos, powering them directly from the ESP32's `5V`/`VIN` pin might cause brownouts. For production flight, a dedicated external Step-Down regulator powering the servo rail is highly recommended.*

---

## Build

1. Open `Rocket.ino` in the Arduino IDE.
2. Select an ESP32 board such as `Node32s` or another compatible ESP32 target.
3. Install the required libraries listed in the **Arduino Libraries** section.
4. Compile and upload the sketch to the board.

