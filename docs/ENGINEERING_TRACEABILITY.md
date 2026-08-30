# Engineering traceability

This matrix links important behaviour to its software owner, runtime evidence and FAT coverage.

| Engineering requirement | Primary implementation | Observable result | Public evidence |
|---|---|---|---|
| Store passenger calls once per button event | `FB_CallManager` | held button does not recreate a served call every scan | README migration / held-button regression |
| Direction-aware target recommendation | `FC_RequestManager` | target + Service Direction selected from car/hall requests | README request-handling section, hall-call HMI |
| One Auto-state owner | `FB_AutoSequence` | AutoState/target/candidate command owned in one sequence | architecture docs, FAT-01/02 |
| Independent plant feedback | `FB_ElevatorSimulation` → `DB_IO.Feedback` | floor/door/drive proof changes independently from command | architecture + HMI diagnostics |
| Single final actuator writer | `FC_OutputArbiter` | only accepted Auto/Maint candidate command reaches `FinalCmd` | Output Arbiter screenshot, architecture docs |
| Door proof before motion | standard movement-permissive evaluation + Output Arbiter | unsafe/not-proven door condition inhibits travel | FAT-03 |
| Door-stuck supervision | `FB_AlarmManager` | state 900, alarm latched, final outputs removed | FAT-04 |
| Hold-to-run Maintenance | `FB_Maintenance` | state 810/820 while held; release returns to 800 and removes motion | FAT-05 |
| STOP removes movement | mode/controller path + Output Arbiter | system disabled / AutoState 0 / final motion off | FAT-06 |
| E-stop release requires acknowledgement | `Main_Safety_RTG1` | `EStopRelease` only after safe state + acknowledgement | safety watch evidence |
| Door-safe release requires safe input + acknowledgement | `Main_Safety_RTG1` | `DoorRelease` removed when door unsafe and restored deliberately | safety architecture evidence |
| Fail-safe motion permission | `EStopRelease AND DoorRelease` | `F_SafeMotionEnable` TRUE only when both releases are valid | F-program screenshot + safety watch |
| F-program cannot be bypassed by standard movement request | `FC_OutputArbiter` + `F_SafeMotionEnable` | AutoState requests movement while final motion remains FALSE | FAT-07 |
| Stable HMI contract | `FC_StatusEvaluation` + `FC_HMI_OutputMap` | WinCC consumes status instead of internal instance state | HMI / cross-reference evidence |

## FAT summary

| FAT | Requirement under test | Result |
|---|---|---|
| FAT-01 | normal automatic travel and arrival | PASS |
| FAT-02 | multi-call target/state progression | PASS |
| FAT-03 | door interlock inhibits movement | PASS |
| FAT-04 | door-stuck fault, latching and controlled recovery | PASS |
| FAT-05 | Maintenance Jog Up / Jog Down hold-to-run behaviour | PASS |
| FAT-06 | Stop removes motion; deliberate restart required | PASS |
| FAT-07 | fail-safe motion permission gates final outputs | PASS |

See [Elevator_FAT_Validation_Report.pdf](Elevator_FAT_Validation_Report.pdf) for screenshot-backed validation pages.
