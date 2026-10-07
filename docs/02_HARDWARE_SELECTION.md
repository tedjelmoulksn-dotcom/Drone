# 02 — Hardware Selection and Compatibility

## Flight controller

### NXP RDDRONE-FMUK66

Selected because it is the native HoverGames flight management unit and is supported by PX4. It uses a 180 MHz NXP Kinetis K66 Cortex-M4F and follows the Pixhawk FMUv4 design philosophy.

**Role:** IMU acquisition, state estimation, attitude control, failsafes, RC interpretation and actuator commands.

## Propulsion currently available

### RS2205 2300 kV motors

5-inch FPV-class brushless motors. They are materially different from the larger 920 kV motors used in the original 500 mm HoverGames reference airframe. Therefore the project must validate **frame size, propeller, all-up mass and thrust margin** rather than assuming the original HoverGames propulsion sizing applies.

### T-Motor AIR 40 A ESC

40 A ESCs are used between the PDB and motors. The motor's three phase leads connect to the ESC; swapping any two motor phases reverses rotation if required.

### 5-inch propellers

Two CW/CCW pairs are required, matched to the motor/airframe. Propellers stay removed during bench bring-up.

## Power

### LiPo 4S

Nominal 14.8 V source for the propulsion system. The exact capacity is a mass/endurance trade-off.

### PDB

Distributes battery power to the four ESCs. A PDB **does not automatically replace the FMUK66 power module**.

### FMUK66-compatible power module — open compatibility item

The FMUK66 `POWER IN` expects:

| Pin | Function |
|---|---|
| 1 | +5.3 V |
| 2 | +5.3 V |
| 3 | current-sensor analog input |
| 4 | voltage-sensor analog input |
| 5 | GND |
| 6 | GND |

The NXP reference power module regulates the LiPo input to about 5.3 V and provides analog voltage/current sensing. **A digital PM02D should not be treated as a drop-in replacement without an interface redesign.**

## Communications

### Holybro SiK Telemetry Radio V3 — 433 MHz

MAVLink telemetry between FMUK66 and QGroundControl. One radio is connected to the FMU `TELEM` port; the other is connected to the ground computer over USB.

### RC transmitter + receiver

Provides the pilot commands. Exact receiver protocol and pinout must be confirmed from the receiver model before wiring.

## Navigation

### GNSS + compass

Connected to the FMUK66 GPS interface for position/navigation modes. Manual attitude-control bring-up should be completed before relying on GNSS modes.

## Recovery / safety

### VIFLY Finder 2

Independent lost-model buzzer with an internal battery. The photographs in `assets/images/vifly_finder2_user_*.jpg` are actual project hardware.

## Future hardware (not required for M1)

- Raspberry Pi 4-class companion computer;
- Raspberry Pi Camera Module 3;
- dedicated regulated supply for companion computer;
- MAVLink UART between companion computer and FMUK66.
