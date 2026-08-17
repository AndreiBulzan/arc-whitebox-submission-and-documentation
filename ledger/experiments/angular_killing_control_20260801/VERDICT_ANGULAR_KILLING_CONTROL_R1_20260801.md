# Angular Killing-field control R1 verdict

Date: 2026-08-01

Evidence label: **component**. This was a sealed 2 Full + 2 Generated fast
screen. It used no FlopScope row, package, upload, submission, or remote
action.

## Verdict

**Kill this exact four-plane/cross-fit spelling.** The divergence-free
rotation identity is numerically sound, but the controls do not explain the
structured complete-basis integration error.

The independent `32768`-point IID check passed:

```text
median |z(mean control)|   0.226
p95    |z(mean control)|   1.768
max    |z(mean control)|   2.837
```

The stable point-cross-fitted spelling was essentially neutral. Its best
Full member was K32/four fields at ratio `0.98348`, while the corresponding
Generated noise-corrected ratio was `1.01062`. No fixed member approached
the preregistered `0.85 / 0.85` gate. Complete-basis fitting was unstable
and worsened both families.

This result closes generic rank-two Killing fields selected from leading
`W0` and end-to-end singular directions. Reopening requires a nonlocal
pair/four-point observable, not more planes or coefficient tuning.

Authoritative artifacts:

- `angular_killing_control_r1_targetfree_20260801.npz`
  (`6c12ef6a52bc52fc454d47b6b0432aeae9cd0fbc1e4b86c11748b57dc0c48224`)
- `angular_killing_control_r1_postseal_20260801.json`
