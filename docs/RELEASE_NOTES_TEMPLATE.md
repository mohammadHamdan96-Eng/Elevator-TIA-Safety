# GitHub Release template

Release tag:

```text
v1.0.0
```

Release title:

```text
Elevator TIA Portal V20 + SIMATIC Safety — Final Engineering Release
```

Release notes:

```markdown
## Elevator TIA Portal V20 + SIMATIC Safety — v1.0.0

Final engineering release of the five-stop Siemens TIA Portal elevator portfolio project.

### Engineering scope

- CPU 1511F-1 PN / SIMATIC Safety
- WinCC Unified HMI
- five-stop automatic sequence
- direction-aware car and hall-call handling
- structured UDT / FB / FC / DB architecture
- controller / simulation separation
- command / feedback separation
- Maintenance hold-to-run Jog Up / Jog Down
- alarm and controlled recovery logic
- ET 200SP fail-safe I/O / PROFIsafe configuration
- acknowledged E-stop and door-safe release concept
- `F_SafeMotionEnable` integrated at the final motion gate
- structured PLCSIM FAT validation

### Validation

The repository contains a screenshot-backed seven-case FAT report, architecture documentation, safety evidence, HMI/runtime evidence and PLCSIM reproduction notes.

### Scope boundary

This is a portfolio/lab implementation. It is not a certified passenger-elevator controller, physical commissioning record, SIL/PL claim or conformity assessment.

### TIA Portal archive

`Elevator_TIA_V20_Safety_FINAL_2026-08-29.zap20`

Restore/open with Siemens TIA Portal V20.

### PLCSIM access

If TIA Portal requests the configured CPU access password while downloading to PLCSIM, use the dedicated portfolio password supplied here:

`<INSERT PORTFOLIO-ONLY CPU / PLCSIM PASSWORD>`
```

Do not publish a reused personal, work, Siemens-account or real-machine credential. If no password is required, delete the password subsection before publishing.
