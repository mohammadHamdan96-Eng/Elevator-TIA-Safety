# Running the Elevator project in PLCSIM

This guide documents the **portfolio reproduction flow** for the archived TIA Portal V20 project.

## 1. Obtain the TIA archive

The final `.zap20` archive is distributed through the repository's GitHub Release rather than normal Git history:

[Latest release / TIA archive](https://github.com/mohammadHamdan96-Eng/Elevator-TIA-Safety/releases/latest)

Recommended asset name:

```text
Elevator_TIA_V20_Safety_FINAL_2026-08-29.zap20
```

## 2. Restore in TIA Portal V20

1. Open TIA Portal V20.
2. Retrieve/open the `.zap20` archive.
3. Compile the standard program, F-program and HMI.
4. Start the configured PLCSIM target for the S7-1500F project.
5. Download the program to the simulated CPU.

If the public archive uses a CPU-access password for PLCSIM download, use the password provided in the GitHub Release notes. Do not substitute a password from another project.

## 3. Establish the simulated fail-safe healthy state

A fresh simulation starts without a valid motion release until the fail-safe input conditions are put into their healthy simulated state and acknowledged.

Set/modify the test inputs used by the project:

```text
F_EStop_OK = TRUE
F_DoorSafe = TRUE
```

Then pulse the acknowledgement input:

```text
Safety_Reset: FALSE -> TRUE -> FALSE
```

Expected result:

```text
EStopRelease       = TRUE
DoorRelease        = TRUE
F_SafeMotionEnable = TRUE
```

This is a **PLCSIM test prerequisite**. It is not a procedure for bypassing real safety hardware.

## 4. Start normal operation

After the fail-safe motion release is valid:

1. use the HMI Start command;
2. select a valid floor request;
3. observe target selection and the Auto state;
4. verify Door Closed / Door Locked proof before movement;
5. verify Motor Up/Down, Drive Enable and Brake Release only appear when the final movement path is permitted;
6. observe arrival and removal of final motion commands.

## 5. Maintenance operation

Maintenance uses hold-to-run Jog commands:

- state `810` = Jog Up;
- state `820` = Jog Down;
- releasing the jog returns to Maintenance idle (`800`) and removes movement commands.

Terminal limits, fault/emergency conditions and fail-safe motion permission remain part of the final movement path.

## 6. Safety-integration negative check

The key integration check is not only that `F_SafeMotionEnable` can become TRUE. It is that a standard movement request **cannot reach the final outputs while it is FALSE**.

Expected negative condition:

```text
AutoState requests travel
F_SafeMotionEnable = FALSE

FinalCmd.MotorUp      = FALSE
FinalCmd.MotorDown    = FALSE
FinalCmd.DriveEnable  = FALSE
FinalCmd.BrakeRelease = FALSE
```

This is the behaviour documented by FAT-07.

## 7. Fault/recovery note

For fault cases such as the Door Stuck injection, clear the cause and use the controller's deliberate reset/start sequence. The project does not silently resume travel from the pre-fault movement state.

## 8. Evidence

- [Safety architecture](SAFETY_ARCHITECTURE.md)
- [FAT validation](FAT_VALIDATION.md)
- [Screenshot-rich FAT report](Elevator_FAT_Validation_Report.pdf)
