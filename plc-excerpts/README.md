# Browser-readable PLC excerpts

These files are short, representative excerpts transcribed from the retained TIA Portal V20 implementation views included in this repository.

They are provided so a GitHub reviewer can inspect the key control ideas without opening TIA Portal:

- [`FB_CallManager_edge_capture_excerpt.scl`](FB_CallManager_edge_capture_excerpt.scl) — rising-edge request capture and single-owner stored-call handling.
- [`FB_CallManager_edge_memory_excerpt.scl`](FB_CallManager_edge_memory_excerpt.scl) — Maintenance clear priority and previous-button memory updates that prevent a held button from becoming a false new request.
- [`FB_ElevatorController_safety_arbiter_boundary_excerpt.scl`](FB_ElevatorController_safety_arbiter_boundary_excerpt.scl) — standard movement-permissive evaluation followed by the single final-command writer with `iSafeMotionEnable` as a separate input.

These are **not complete native TIA block exports**. The matching TIA screenshots remain the implementation evidence; the full engineering project is distributed as the `.zap20` archive through GitHub Releases.
