# Quadrotor Drone — Flight Controller Integration

Engineering documentation for a quadrotor project developed incrementally, from hardware selection and electrical integration to radio control, testing and future onboard processing.

## Current configuration — DJI NAZA-M Lite (October 2026)

The bench prototype is being reconfigured around a **DJI NAZA-M Lite** flight controller, **DJI VU** power module, **DJI LED/USB** interface and **FlySky FS-iA6B** radio receiver. The Matek PDB, ESCs and brushless propulsion hardware are retained.

**Current status:** Windows 11 detects a USB serial device associated with the bench connection (COM3), but successful communication with **NAZA-M Lite Assistant V1.00**, RC calibration and motor tests are **not yet confirmed**. This is **not a flight-ready configuration**.

**Read first:** [NAZA-M Lite migration, wiring and test log](docs/07_NAZA_M_LITE_MIGRATION.md).

## Previous architecture — NXP FMUK66 / PX4

The original design was based on the **NXP RDDRONE-FMUK66** and **PX4/QGroundControl**. Its architecture, hardware choices and staged verification plan remain in this repository for reference. **Do not apply FMUK66-specific wiring instructions to the NAZA-M Lite.**

## Documentation

| Location | Contents |
|---|---|
| [NAZA migration log](docs/07_NAZA_M_LITE_MIGRATION.md) | Current controller, intended wiring, tests performed, open issues and next steps |
| [System overview — original](docs/01_SYSTEM_OVERVIEW.md) | Initial NXP/PX4 design and longer-term objectives |
| [Hardware selection — original](docs/02_HARDWARE_SELECTION.md) | Hardware analysis for the NXP/PX4 architecture |
| [Integration test plan — original](docs/06_INTEGRATION_AND_TEST_PLAN.md) | Original staged testing methodology; PX4-specific steps must be adapted |
| [assets/](assets/) | Project images and diagrams |

## Safety

All bench electrical, radio and motor tests are conducted **with propellers removed**. Power wiring and voltage levels must be checked before connecting the LiPo. The receiver and NAZA input ports must never be fed directly from the battery voltage.

## Future development

The immediate goal remains a reliable RC-controlled quadrotor. Raspberry Pi/camera integration is a future phase and requires a separate compatibility assessment for the NAZA-M Lite.
