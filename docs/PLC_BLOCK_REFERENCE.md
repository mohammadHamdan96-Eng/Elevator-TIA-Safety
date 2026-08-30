# PLC block reference

This is a compact map of the blocks and data boundaries used by the final Elevator architecture.

## Standard application

| Block / DB | Type | Main responsibility |
|---|---|---|
| `FC_HMI_InputMap` | FC | normalize WinCC commands into controller inputs |
| `FB_CallManager` | FB / multi-instance | stored car/hall calls, rising-edge capture, served-call clearing |
| `FC_RequestManager` | FC | direction-aware target recommendation using structured request data |
| `FB_ModeManager` | FB / multi-instance | Auto/Maintenance arbitration and passenger-call permission |
| `FB_AutoSequence` | FB / multi-instance | Auto state, accepted target, service direction, Auto candidate command |
| `FB_Maintenance` | FB / multi-instance | Maintenance state, hold-to-run Jog Up/Down, manual door actions |
| `FB_ElevatorSimulation` | FB | simulated floor/door/drive feedback and named fault injection |
| `FC_SafetyEvaluation` | FC | standard permissives, limits, direction conflict and blocked reason |
| `FC_OutputArbiter` | FC | sole normal final-command writer; applies standard and fail-safe motion gates |
| `FB_AlarmManager` | FB / multi-instance | raw/latched alarms, timeout supervision, reset rules |
| `FC_StatusEvaluation` | FC | stable controller/HMI status model |
| `FC_HMI_OutputMap` | FC | publishes the stable status structure to `DB_HMI.Status` |
| `FB_ElevatorController` | FB | central application coordinator and multi-instance owner |
| `DB_ElevatorController` | instance DB | state for the central controller and its multi-instances |
| `DB_ElevatorSimulation` | instance DB | state/timers for the simulated plant |
| `DB_HMI` | global DB | WinCC command/status contract |
| `DB_IO` | global DB | logical plant boundary: independent feedback + final commands |
| `DB_Config` | global DB | named timing/configuration values |
| `DB_System` | global DB | startup, simulation and stable system/controller state |

## Main structured data types

The TIA redesign uses structured interfaces rather than a large set of floor-specific and LED-style global tags. The source architecture defines 13 organized UDT families, including:

- `UDT_HMI_Command`
- `UDT_OperatorCommand`
- `UDT_RequestStore`
- `UDT_RequestDerived`
- `UDT_DispatchResult`
- `UDT_CallClearRequest`
- `UDT_ActuatorCommand`
- `UDT_PlantFeedback`
- `UDT_SafetyStatus`
- `UDT_AlarmBits`
- `UDT_AlarmStatus`
- `UDT_ElevatorConfig`
- `UDT_HMI_Status`

## Auto state map

| State | Meaning |
|---:|---|
| `0` | Off |
| `10` | Idle |
| `20` | Evaluate target |
| `30` | Close door |
| `40` | Direction decision |
| `50` | Move Up |
| `60` | Move Down |
| `70` | Arrival |
| `80` | Door open |
| `81` | Door dwell |
| `82` | Close/lock after service |
| `90` | issue served-call event |
| `900` | fault / emergency |

## Maintenance state map

| State | Meaning |
|---:|---|
| `0` | Inactive |
| `800` | Maintenance idle |
| `810` | hold-to-run Jog Up |
| `820` | hold-to-run Jog Down |
| `830` | Maintenance Door Open |
| `840` | Maintenance Door Closing / Locking |

## Alarm supervision

The standard Alarm Manager includes latched supervision for:

1. Door Close Timeout
2. Door Cycle Timeout
3. Travel Timeout
4. Motor Direction Conflict
5. Door Interlock
6. Invalid Floor

Reset clears a latched alarm only after its raw cause is absent; an active dangerous cause retains priority over reset.

## Fail-safe program

| Item | Role |
|---|---|
| `FOB_RTG1 [OB123]` | F-runtime execution OB |
| `Main_Safety_RTG1 [FB8]` | main fail-safe logic |
| `Main_Safety_RTG1_DB [DB7]` | safety block instance DB |
| `F_EStop_OK` | fail-safe E-stop safe-state input |
| `F_DoorSafe` | fail-safe door-safe input |
| `Safety_Reset` | acknowledgement input used in lab validation |
| `EStopRelease` | acknowledged E-stop release |
| `DoorRelease` | acknowledged door-safe release |
| `F_SafeMotionEnable` | fail-safe permission passed to final motion arbitration |
