# 06 — Integration and Test Plan

The integration strategy is deliberately incremental. A subsystem must pass its verification gate before the next potentially hazardous subsystem is enabled.

## Gate A — Mechanical

- motors securely mounted;
- no screw contacts motor windings;
- frame rigid;
- wiring restrained away from rotating parts;
- no propellers installed.

## Gate B — Electrical, no FMU

- battery/PDB polarity verified;
- no hard short across battery input;
- power module output verified against its specification;
- ESC power wiring inspected.

## Gate C — FMUK66 bench bring-up

- FMUK66 powers normally;
- PX4 boots;
- QGroundControl connects;
- microSD logging works;
- accelerometer/gyro orientation is coherent.

## Gate D — RC

- transmitter/receiver bound;
- roll/pitch/yaw/throttle channels identified;
- direction and endpoints calibrated in QGroundControl;
- arm/disarm behaviour understood;
- RC-loss failsafe configured.

## Gate E — Motors, **without propellers**

For each motor:

1. command the motor from the actuator test;
2. verify that the expected physical motor responds;
3. verify rotation direction;
4. correct phase order/configuration if needed;
5. repeat for all four motors.

## Gate F — Propellers

Install only after Gate E passes. Confirm each propeller produces thrust in the correct direction and is mechanically secured.

## Gate G — First restrained/low-risk flight campaign

- open area appropriate for testing;
- short hover attempt;
- verify roll/pitch/yaw responses;
- land immediately on abnormal oscillation, motor sound or control reversal;
- inspect logs and hardware after the test.

## Acceptance criteria for M1

- repeatable arming/disarming;
- stable hover without persistent strong correction;
- correct stick directions;
- predictable landing;
- RC failsafe demonstrated safely;
- no excessive motor/ESC heating;
- logs available for post-flight analysis.
