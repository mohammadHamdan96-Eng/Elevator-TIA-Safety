# Engineering scope

This repository is a Siemens TIA Portal V20 portfolio/lab implementation of a five-stop elevator controller with WinCC Unified, structured standard-control logic and an integrated SIMATIC Safety motion-permission layer.

## Demonstrated engineering scope

- five-stop automatic sequence;
- car and directional hall-call handling;
- service-direction / physical-motion separation;
- structured UDT/DB interfaces;
- controller / plant-simulation separation;
- command / feedback separation;
- multi-instance controller decomposition;
- single-writer output arbitration;
- Maintenance hold-to-run jogging;
- door, travel and interlock diagnostics;
- WinCC Unified operator and diagnostics screens;
- CPU 1511F-1 PN / SIMATIC Safety project structure;
- F-runtime group configuration;
- ET 200SP fail-safe I/O / PROFIsafe configuration;
- E-stop and door-safe acknowledgement concepts;
- `F_SafeMotionEnable` integrated into the final movement path;
- screenshot-backed PLCSIM FAT / recovery validation.

## Explicit limits

The repository does not claim:

- a certified passenger-elevator controller;
- a SIL/PL result or certified safety function;
- conformity assessment, TÜV approval or legal elevator validation;
- physical passenger-lift commissioning;
- physical validation of safety gear, brake, overspeed, door lock, STO or unintended-car-movement protection;
- real field-device proof testing.

Any hardware architecture shown without corresponding physical test evidence is described as configured architecture / lab work rather than commissioned plant hardware.
