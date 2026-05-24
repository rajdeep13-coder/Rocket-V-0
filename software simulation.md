# Software Simulation

This project does not have a full physics-accurate rocket simulator built in. The practical way to test the software without hardware is to run the sketch in **simulation mode**, where the MPU6050 readings are replaced with generated values and the servo commands are logged to Serial.

## What simulation mode does

- Replaces live MPU6050 readings with synthetic acceleration and gyro values
- Keeps the PID / complementary-filter control path active
- Logs timestamped flight data to Serial
- Lets you verify that the sketch compiles and that the control logic behaves as expected
- Prevents servo writes when you are testing without hardware

## What simulation mode does not do

- It does not model real aerodynamics
- It does not simulate the actual servos or airframe motion
- It does not replace real hardware testing before flight

## Quick setup

The repository ignores the local file `sim_config.h`, so you can create it on your machine without pushing it to GitHub.

Create a file named `sim_config.h` in the repo root with this content:

```cpp
#define SIMULATION_MODE 1
```

When that file exists, `rocket.ino` switches to simulation mode automatically.

## How to run the simulation

1. Make sure Arduino CLI is installed.
2. Install the required libraries listed in `requirements.txt`.
3. Create the local `sim_config.h` file shown above.
4. Compile the sketch from the repo folder or from a temporary sketch folder.
5. Open Serial Monitor at `115200` baud.
6. Watch the CSV-style output for:
   - timestamp
   - simulation mode flag
   - acceleration values
   - gyro values
   - servo command values
   - sensor health flag

## Example output

```text
timestamp,mode,accelX,accelY,accelZ,gyroX,gyroY,gyroZ,servoX,servoY,healthy
1234,1,0.12,0.98,9.79,0.03,0.01,0.00,91,89,1
```

## Recommended test checks

- Confirm the code compiles cleanly in simulation mode
- Confirm Serial output is updating at a steady rate
- Confirm servo commands stay within `0` to `180`
- Confirm the fail-safe path returns the commands to `90` when sensor data is missing

## After testing

Delete `sim_config.h` when you are done testing if you want to return to the normal hardware path. The file is already ignored by Git, so it will not be pushed accidentally.

## Note on online simulation

If by "online" you mean a browser-based simulator, this sketch is not currently wired for a full web simulator. The closest workable option is the local simulation mode above, which lets you test the control software without connecting the MPU6050, servos, or OLED.
