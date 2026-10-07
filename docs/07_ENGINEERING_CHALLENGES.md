# 07 — Engineering Challenges, Decisions and Lessons Learned

This file intentionally records the non-trivial parts of the project. It is useful both technically and as evidence of engineering reasoning.

## 1. Legacy platform / current software

The RDDRONE-FMUK66 is a discontinued reference flight controller, while PX4 continues to evolve. Firmware target, board revision, bootloader and QGroundControl compatibility therefore have to be handled explicitly.

### Encountered issue
Initial flashing required J-Link/bootloader work before normal QGroundControl firmware loading was available.

### Decision
Keep the bootloader/firmware procedure documented and preserve a known-good binary/toolchain baseline.

## 2. Power-module compatibility

A six-pin connector does not imply electrical compatibility.

### Finding
The FMUK66 `POWER IN` expects approximately 5.3 V plus **analog** current and voltage sensing. A modern digital/I2C power module cannot be assumed to be a drop-in replacement.

### Engineering lesson
Validate **electrical interface semantics**, not only connector geometry.

## 3. Propulsion differs from the reference HoverGames kit

The original NXP reference kit uses a much larger airframe/motor/propeller combination. The current project uses RS2205 2300 kV FPV-class motors.

### Consequence
The reference assembly guide is valuable for FMU wiring and PX4 architecture, but propulsion sizing must be revalidated for the actual 5-inch platform.

## 4. Separation of concerns

Adding a Raspberry Pi immediately would increase mass, power complexity and software variables before the basic airframe is validated.

### Decision
First obtain a known-good RC-controlled PX4 aircraft. Add the companion computer only after M1 passes.

## 5. Virtual machine / USB device access

Development has involved USB passthrough and serial/J-Link access from a Linux VM. Device enumeration can change (`/dev/ttyACM*`) and USB ownership/passthrough must be checked before diagnosing firmware problems.

## 6. Safety as an engineering requirement

Motor tests without propellers, staged power-up, failsafe verification and logging are treated as verification gates rather than informal precautions.
