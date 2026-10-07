# 03 — Electrical Architecture and Wiring

> **Status:** architecture document. Do not energize the aircraft until the exact power-module/connector compatibility is verified.

## 1. High-current power path

```text
4S LiPo
   |
   v
FMUK66-compatible power module
   |-----------------------> POWER IN / FMUK66
   |
   v
PDB
 |     |     |     |
ESC1  ESC2  ESC3  ESC4
 |     |     |     |
M1    M2    M3    M4
```

The PDB carries propulsion current. The FMU power module performs a different function: regulated FMU power plus battery voltage/current measurement.

## 2. ESC control path

The FMUK66 commands the four ESCs from its servo rail. NXP's HoverGames reference mapping for a quad-X is:

| PX4 motor | Physical position |
|---:|---|
| 1 | front right |
| 2 | back left |
| 3 | front left |
| 4 | back right |

This mapping must be checked against the actual PX4 airframe/output configuration used on the installed firmware.

## 3. FMUK66 interfaces

```text
POWER IN (6-pin) <- compatible power module
GPS (10-pin)     <- GNSS / compass
TELEM (6-pin)    <- Holybro SiK telemetry radio
RC IN            <- RC receiver
Servo rail       -> ESC signals
microSD          -> flight logs
USB              <-> QGroundControl / firmware / bench configuration
```

## 4. Power-module compatibility warning

The FMUK66 power input uses analog voltage/current sensing and approximately 5.3 V supply. A module designed around digital I2C power telemetry is not automatically pin-compatible even if it has a six-pin connector.

**Engineering rule:** verify connector type, pin order, supply voltage and sensor interface electrically before mating modules.

## 5. Wiring colours are not specifications

Never infer a pin solely from wire colour. Use the manufacturer pinout and continuity measurements where needed.

## 6. Grounding

The flight controller and ESC signal references require a defined common ground. High-current motor wiring should be physically routed to reduce coupling into sensitive sensor and signal wiring.

## 7. Bench-power checklist

- propellers removed;
- LiPo disconnected while assembling;
- XT60 polarity checked;
- no continuity between battery positive and ground indicating a hard short;
- regulated FMU supply verified before connecting the FMU;
- motor phases insulated;
- ESC signal connectors correctly oriented;
- microSD installed for logging.

See `assets/diagrams/system_architecture.svg` for the visual architecture.
