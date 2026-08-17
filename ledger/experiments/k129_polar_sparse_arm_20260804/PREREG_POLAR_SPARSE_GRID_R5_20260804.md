# Preregistration: literal sparse-polar grid R5

Date: 2026-08-04

Evidence sequence: target-free **component** capture on the already used
Full16 and Generated16 pilot rows, post-seal pilot scoring, then (only if a
candidate passes) one frozen **broad statistical** official-Mini100 gate.
No FlopScope row, package, upload, submission, or remote action is permitted.

## Why this is not the killed R4 point

R4 selected `m=32` from a full-frame endpoint linearization.  Its literal
independently repaired arm scored only `0.89065x` q0 on Mini100, so that point
is dead.  The same target-free endpoint fit had already frozen nested
supports at `m=64,80,96,104`, but none of those literal trajectories was ever
run.  This experiment answers only whether the larger supports cross the
nonlinear repair boundary and retain enough of the complete polar arm.

## Fixed grid and selection

- Supports and coefficients: copied without refitting from every matching
  record in `k129_polar_sparse_arm_r4_selection_20260804.json`.
- Sizes: `64,80,96,104`.
- Output shrink: `alpha in {0.25,0.50,0.75}`.
- Literal propagation and endpoint repair are identical to R4.
- Choose the smaller worst-family projected adjusted score on Full16 and
  Generated16; ties prefer lower `m`, then lower `alpha`.

The unchanged conservative projection is

```text
C(m) = 141.1403492625B + 0.999B*m + 5B.
```

Promote exactly one point to Mini100 only if its worst pilot projection is at
most `1.08e-7`, both pilot raw ratios are at most `0.70`, and at least 10/16
rows improve in each family.  These margins are intentionally stronger than
the final `1.1e-7` target because pilot-to-Mini transfer is uncertain.

The frozen point passes Mini100 only if projected adjusted is at most
`1.10e-7`, raw ratio is at most `0.65`, at least 75/100 rows improve, and the
paired-bootstrap ratio upper 95% endpoint is below `0.80`.

