# NXP FMUK66 Quadcopter — PX4 Integration and Engineering

PX4 firmware bring-up and system integration for an NXP FMUK66 quadcopter.

## Current scope

The immediate objective is reliable RC-controlled flight. Bootloader and firmware bring-up work is recorded, including J-Link and Linux virtual-machine USB access. Manual-flight validation remains the next integration milestone; GNSS, telemetry extensions, companion-computer vision and autonomy are later stages.

This is a system-integration repository, not an independently developed autopilot stack.

## Architecture

![System architecture](assets/diagrams/system_architecture.svg)

The flight controller handles the time-sensitive sensor and actuator path. A proposed Raspberry Pi/camera layer is reserved for later vision work, after the base aircraft has been validated.

## Engineering documents

| Document | Content |
|---|---|
| [System overview](docs/01_SYSTEM_OVERVIEW.md) | Objectives, architecture and milestones |
| [Hardware selection](docs/02_HARDWARE_SELECTION.md) | Components, compatibility and unresolved choices |
| [Electrical architecture](docs/03_ELECTRICAL_ARCHITECTURE.md) | Power distribution, sensing and FMU connections |
| [Flight physics](docs/04_FLIGHT_PHYSICS.md) | Thrust, moments, mass and propulsion sizing |
| [PX4 software](docs/05_PX4_SOFTWARE.md) | Firmware, QGroundControl and development concepts |
| [Integration and test plan](docs/06_INTEGRATION_AND_TEST_PLAN.md) | Bring-up sequence and verification gates |
| [Engineering challenges](docs/07_ENGINEERING_CHALLENGES.md) | Encountered issues and design decisions |
| [References](docs/08_REFERENCES.md) | Source documentation and further reading |

## Important integration decisions

- **Legacy firmware compatibility:** preserve the known board revision, bootloader, firmware target and toolchain baseline.
- **Power-interface semantics:** the documented FMUK66 power input uses approximately 5.3 V and analogue voltage/current signals. Connector shape alone does not establish compatibility with a digital/I²C power module.
- **Actual propulsion hardware:** RS2205 2300 kV motors differ from the larger reference HoverGames configuration. Recalculate thrust margin and power demand for the actual airframe and propellers.
- **Staged integration:** validate RC mapping, sensor calibration, ESC/motor assignment and failsafes before introducing the companion computer.

## Reviewing the project

```bash
git clone https://github.com/tedjelmoulksn-dotcom/Drone.git
cd Drone
```

Read the overview first, then the electrical architecture and test plan. Firmware source is maintained separately in the [personal PX4 fork](https://github.com/tedjelmoulksn-dotcom/PX4-Autopilot); upstream PX4 retains its own authorship and licence.

Bench motor tests are performed without propellers until assignment, direction and arming behaviour are verified. Power polarity and supply compatibility are checked before energising the system.

## Evidence and next milestone

The project records firmware bring-up and the engineering decisions needed to integrate the actual aircraft. The next milestone connects RC input, calibrated sensors and motor output in a manual-flight test, with configuration snapshots and logs supporting diagnosis.

The design documentation follows the aircraft from component choices through firmware bring-up and integration gates. Manual-flight validation is the current milestone; later vision/autonomy work depends on that stable baseline.

## Attribution and licence

PX4, NXP and third-party hardware documentation remain attributed to their respective authors. No project-wide licence has been defined for this documentation repository.
