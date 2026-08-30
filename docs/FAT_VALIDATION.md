# FAT validation index

The public validation report contains seven executed PLCSIM/lab tests. Each test is documented with the requirement, observed result and retained PLC/HMI evidence.

| FAT | Test | Result | Key evidence |
|---|---|---:|---|
| FAT-01 | Normal travel | PASS | AutoState 50 during Up travel, motion commands active, `F_SafeMotionEnable` TRUE; clean arrival with outputs off |
| FAT-02 | Multi-call sequence | PASS | coherent target/state progression and final command removal at service completion |
| FAT-03 | Door interlock | PASS | tested interlocked condition inhibits movement; deliberate recovery/start restores valid travel |
| FAT-04 | Door-stuck fault / recovery | PASS | AutoState 900, `AlarmWord = 16#0000_0002`, fault summary active, outputs off, deliberate recovery |
| FAT-05 | Maintenance jog | PASS | state 810/820 for Jog Up/Down; release returns to 800 and removes motion commands |
| FAT-06 | Stop / restart | PASS | STOP removes active final motion and disables Auto; deliberate restart is required |
| FAT-07 | Safety integration | PASS | standard controller requests movement while `F_SafeMotionEnable = FALSE`; final motor/drive/brake outputs remain FALSE |

**Evidence report:** [Elevator_FAT_Validation_Report.pdf](Elevator_FAT_Validation_Report.pdf)

## Validation positioning

The report demonstrates controller and configured safety-architecture behaviour in the portfolio PLCSIM/lab environment. It is not a physical passenger-elevator acceptance test or a functional-safety certification record.
