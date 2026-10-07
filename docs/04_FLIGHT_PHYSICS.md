# 04 — Flight Physics and Control

## Force balance

A quadrotor hovers when total upward thrust approximately balances weight:

```text
T1 + T2 + T3 + T4 = m g
```

A practical aircraft needs thrust margin above hover for disturbances and manoeuvres. Therefore the relevant design quantity is not only motor kV, but the **measured motor + propeller + battery thrust curve versus all-up mass**.

## Motor kV

Motor kV is approximately the unloaded speed constant in rpm/V. A 2300 kV motor on a 4S pack is a high-speed FPV-class combination; propeller diameter/pitch and current draw must therefore be selected conservatively.

## Control axes

### Throttle
All four motor thrusts are changed in the same general direction to climb or descend.

### Roll
PX4 creates a thrust difference between the left and right sides, generating a moment around the longitudinal axis.

### Pitch
PX4 creates a thrust difference between front and rear, generating a moment around the lateral axis.

### Yaw
Two motors rotate CW and two CCW. Differential reaction torque produces yaw while largely preserving total thrust.

## Why opposite motor directions?

If all four motors rotated in the same direction, their reaction torques would add and the airframe would tend to spin. Pairing CW and CCW motors makes the nominal reaction torques cancel.

## Centre of gravity

The battery, FMU and later companion computer should be positioned so that the centre of gravity stays near the geometric centre of the four motors. Large offsets increase the steady correction required from the controller and reduce actuator margin.

## Vibration

Propeller imbalance, loose fasteners and frame resonance contaminate IMU measurements. Mechanical integrity is therefore part of the control problem, not merely an assembly concern.

## Measurements planned

- all-up mass;
- individual component masses;
- static thrust per motor/propeller at several throttle points;
- current draw;
- hover throttle;
- battery voltage sag;
- vibration metrics from PX4 logs;
- flight time and thermal behaviour.
