# 01 — System Overview

## Project intent

The project is not simply an FPV assembly. It is an **embedded control and robotics platform** built in two phases.

### Phase 1 — Reliable flight platform

The first target is a conventional quadrotor that behaves predictably under RC control:

- throttle -> climb/descent;
- roll -> lateral motion;
- pitch -> forward/backward motion;
- yaw -> rotation around the vertical axis;
- stable attitude control through PX4;
- telemetry/logging available for diagnosis.

### Phase 2 — Companion-computer extension

After M1 is validated, a Raspberry Pi and camera can be added for OpenCV/vision algorithms and high-level decisions. PX4 remains responsible for the hard real-time stabilization loop.

## Functional architecture

```text
RC transmitter --RF--> RC receiver ----> FMUK66 / PX4 ----> ESC x4 ----> Motor x4
                                           ^    |
                                           |    +---- flight logs (microSD)
                                           |
GPS / compass -----------------------------+
Telemetry radio <---- MAVLink ------------+

LiPo ----> FMU-compatible power module ----> PDB ----> ESC x4
                    |
                    +----------------------> FMUK66 POWER IN
```

## Why split flight control and future vision?

Flight stabilization requires deterministic, high-rate sensor acquisition and actuator updates. Vision workloads have variable execution time and higher compute/memory requirements. Keeping the FMUK66/PX4 as the flight-critical controller and a future Raspberry Pi as a companion computer creates a clean separation between **real-time control** and **high-level perception**.

## Milestones

- **M0 — Firmware:** bootloader + PX4 firmware operational on FMUK66.
- **M1 — Manual flight:** propulsion, RC, sensors and failsafes validated.
- **M2 — Navigation:** GNSS/compass and assisted flight modes validated.
- **M3 — Telemetry:** reliable MAVLink ground link and log analysis.
- **M4 — Vision:** Raspberry Pi + camera integration.
- **M5 — Autonomy:** high-level perception/decision commands sent to PX4.
