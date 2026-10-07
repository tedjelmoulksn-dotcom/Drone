# 05 — PX4 Software Architecture

## Flight stack

PX4 runs on the RDDRONE-FMUK66 under NuttX. The FMUK66 is responsible for sensor acquisition, estimation, control and actuator output.

## Build target

For the classic Rev. C/D FMUK66 target used in the HoverGames documentation:

```bash
make nxp_fmuk66-v3_default
```

The exact target must match the board revision.

## QGroundControl workflow

QGroundControl is used for:

- firmware loading;
- airframe selection;
- sensor calibration;
- radio calibration;
- actuator/motor verification;
- flight-mode setup;
- parameter configuration;
- telemetry and status monitoring.

## uORB

PX4 modules exchange data using **uORB**, a publish/subscribe middleware. A sensor driver or module publishes a topic; other modules subscribe to it without requiring direct coupling between the producer and consumer.

Example conceptual flow:

```text
Gyroscope driver -> sensor_gyro topic -> estimator/controller/subscriber
```

The NXP HoverGames tutorials use uORB subscription/publishing examples as an introduction to writing custom PX4 modules.

## Why `poll()` is useful in a uORB subscriber

A subscriber should not continuously spin the CPU waiting for new sensor data. `px4_poll()` blocks until data is available or a timeout occurs. This reduces useless CPU load while still allowing the program to detect missing data.

## Development roadmap

1. Validate stock PX4 flight behaviour.
2. Collect flight logs and establish a known-good baseline.
3. Add custom PX4 modules only after the aircraft is stable.
4. Future companion computer communicates through MAVLink rather than replacing the PX4 stabilization loop.
