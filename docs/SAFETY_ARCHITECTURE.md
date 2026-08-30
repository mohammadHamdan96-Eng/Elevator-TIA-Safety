# SIMATIC Safety architecture

The Elevator repository includes a practical **S7-1500F / SIMATIC Safety portfolio implementation** integrated with the standard elevator controller. The safety layer is intentionally documented as a lab/PLCSIM implementation; it is not presented as certification of a passenger elevator.

## 1. Functional boundary

The standard PLC program remains responsible for:

- destination and target selection;
- service direction;
- Auto and Maintenance sequence logic;
- door and drive candidate commands;
- standard process permissives;
- alarms and operator status.

The fail-safe program is responsible for one independent result:

> **Is elevator motion released?**

That result is represented by `F_SafeMotionEnable` and is checked again in the final output path.

## 2. F-runtime group

Retained project evidence shows:

| Item | Configuration |
|---|---|
| F-runtime group | `RTG1` |
| F-OB | `FOB_RTG1 [OB123]` |
| Main safety block | `Main_Safety_RTG1 [FB8]` |
| Safety instance DB | `Main_Safety_RTG1_DB [DB7]` |
| F-runtime cycle | 100 ms |
| Warning threshold | 110 ms |
| Maximum cycle | 120 ms |

![F-runtime group](../media/safety/f-runtime-group.png)

## 3. E-stop acknowledgement

The fail-safe E-stop input `F_EStop_OK` is evaluated with acknowledgement required.

```text
F_EStop_OK
    |
    v
ESTOP1 (ACK_NEC = TRUE)
    ^
    |
Safety_Reset
    |
    +--> EStopRelease
```

A recovered safe input is not treated as permission for an automatic restart. The acknowledgement edge is part of the release path.

## 4. Door-safe release

`F_DoorSafe` is supervised separately. A reset/acknowledgement pulse can set the door release only while the safe door input is valid; loss of the safe door condition removes that release.

```text
F_DoorSafe + Safety_Reset edge
              |
              v
          DoorRelease

NOT F_DoorSafe --> release removed
```

## 5. Final fail-safe permission

```text
EStopRelease
      AND
DoorRelease
      |
      v
F_SafeMotionEnable
```

![Safety program focus](../media/safety/f-program-estop-door-safe-motion-focus.png)

The safety program does not choose Up or Down. Its output is an independent permission used by the final movement path.

## 6. Final motion integration

The final command layer requires both the standard motion decision and fail-safe permission.

```text
standard movement request
        AND
standard door / drive / direction permissives
        AND
F_SafeMotionEnable
        |
        v
FC_OutputArbiter
        |
        +--> MotorUp / MotorDown
        +--> DriveEnable
        +--> BrakeRelease
```

![Output Arbiter safety gate](../media/architecture/output-arbiter-safe-motion-focus.png)

FAT-07 is the key integration test: a standard movement state is present while `F_SafeMotionEnable = FALSE`, and all final motion outputs remain FALSE.

## 7. PLCSIM acknowledgement sequence

For a fresh simulation, establish the healthy simulated F-input state and then acknowledge it:

```text
F_EStop_OK = TRUE
F_DoorSafe = TRUE
Safety_Reset: FALSE -> TRUE -> FALSE
```

Expected release:

```text
EStopRelease       = TRUE
DoorRelease        = TRUE
F_SafeMotionEnable = TRUE
```

![Safety release watch table](../media/safety/safety-watch-released-safe-motion-true.png)

This is a simulation-start condition used to exercise the project in PLCSIM. It is **not** a physical safety-bypass procedure.

## 8. F-I/O / PROFIsafe

The project includes ET 200SP fail-safe I/O architecture and PROFIsafe parameterization. F-I/O reintegration is associated with the acknowledgement flow rather than silently accepting a recovered channel.

![F-DI PROFIsafe parameters](../media/safety/fdi-profisafe-parameters.png)

## 9. Scope boundary

The evidence supports discussion of:

- SIMATIC Safety project structure;
- S7-1500F F-runtime configuration;
- E-stop / door-safe acknowledgement concepts;
- fail-safe motion permission;
- F-I/O / PROFIsafe configuration;
- PLCSIM validation of release, inhibition and deliberate restart behaviour.

The repository does **not** claim:

- a certified elevator safety function;
- a SIL or Performance Level result;
- conformity assessment or TÜV approval;
- physical passenger-lift commissioning;
- validated safety gear, brake, overspeed, STO or door-lock hardware.
