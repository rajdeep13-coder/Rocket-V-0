# Software Simulation & Flight Control

This project features a production-ready, high-precision flight control system designed for the ESP32. To test the software without physical hardware, the sketch supports a dedicated **simulation mode**, where real sensor readings are replaced with generated synthetic telemetry values, and servo command outputs are logged directly to Serial.

## Advanced Production Features

### 1. Deterministic 100 Hz Control Loop
The flight controller implements a highly accurate, jitter-free **100 Hz (10ms) control loop** using microsecond-level timing (`micros()`). By replacing variable delay loops, the PID and complementary filter equations execute inside uniform, precise time slices, which is critical for dynamic flight stability.

### 2. Live OLED Telemetry Dashboard
An asynchronous **5 Hz (200ms) Telemetry HUD** runs on the SSD1306 OLED screen in both Simulation and Hardware modes. It is decoupled from the main control loop to avoid I2C latency bottlenecks.
The dashboard displays:
- **Flight Mode:** `[REAL]` or `[SIM]`
- **Attitude Telemetry:** Real-time fused Pitch & Roll angles in degrees
- **Actuator Commands:** Computed output commands for Servo X & Servo Y

### 3. Automatic Sensor Bias Calibration
In hardware mode, the controller runs a self-calibration sequence at boot to calculate gyroscope and accelerometer offsets. The rocket must remain stationary during this phase, and a beautiful calibration loading bar is rendered on the OLED screen.

### 4. High-Visibility Hardware Fail-Safe
If the sensor health check fails or a timeout occurs, a large-font flashing `SYS FAIL!` error is rendered on the OLED screen, alerting the operator while the system automatically centres the control servos at `90°` as a fail-safe.

---

## What simulation mode does

- Replaces live MPU6050 readings with synthetic, wave-generated acceleration and gyro values.
- Keeps the high-precision PID and complementary-filter control loops fully active.
- Logs real-time timestamped flight data to the Serial terminal.
- Disables physical servo actuation to prevent hardware strain.
- Renders simulated telemetry dynamics directly on the OLED Telemetry Dashboard.

## What simulation mode does not do

- It does not model real external aerodynamic forces.
- It does not simulate the mechanical response delays of physical servos.
- It does not replace full pre-flight hardware testing.

---

## Quick setup

The repository ignores the local file `sim_config.h`, allowing you to toggle simulation mode locally without pushing changes to GitHub.

Create a file named `sim_config.h` in the repo root with this content:

```cpp
#define SIMULATION_MODE 1
```

When this file exists, `rocket.ino` switches to simulation mode automatically.

---

## How to run the simulation

1. Make sure Arduino CLI or Arduino IDE is installed.
2. Install the required libraries listed in `requirements.txt`.
3. Create the local `sim_config.h` file shown above.
4. Compile and upload the sketch to your ESP32 board.
5. Open the Serial Monitor at `115200` baud.
6. Watch the CSV-style telemetry output:
   - timestamp (ms)
   - simulation mode flag (1 for SIM, 0 for REAL)
   - acceleration values (X, Y, Z)
   - gyro values (X, Y, Z)
   - servo command values (X, Y)
   - sensor health flag (1 for OK, 0 for FAIL)

## Example output

```text
timestamp,mode,accelX,accelY,accelZ,gyroX,gyroY,gyroZ,servoX,servoY,healthy
1234,1,0.12,0.98,9.79,0.03,0.01,0.00,91,89,1
```

## Recommended test checks

- **Compile Test:** Confirm the sketch compiles cleanly in both hardware and simulation modes.
- **Timing Check:** Verify the CSV data updates at a steady 100 Hz rate.
- **OLED HUD Check:** Verify the OLED updates attitude angles and servo outputs smoothly at 5 Hz.
- **Fail-Safe Check:** In hardware mode, disconnect the I2C wires. Verify the OLED flashes `SYS FAIL!` and that the servos instantly return to `90°`.

## After testing

Delete `sim_config.h` to return to the real flight hardware path. The file is git-ignored and will not be pushed to GitHub.
