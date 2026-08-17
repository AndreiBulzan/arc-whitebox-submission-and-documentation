# Degree-6 support Mini100 R5 preregistration

Date: 2026-08-06

Evidence sought: **broad statistical** rejection/promotion-to-package evidence
only.  This run is an ordinary CUDA reconstruction over all 100 official
Mini100 networks.  It does not satisfy the exact packaged Mini100 gate and
authorizes no FlopScope, package, upload, submission, or remote action.

## Prior-work boundary

The fixed target-free degree-6 support

```text
[3, 11, 15, 17, 27, 34, 36, 45, 54, 64, 66, 70, 78, 82, 111, 115, 120]
```

was selected without targets on 2026-07-29 and propagated through the full
K146/m17 numerical replay.  Its literal final-output raw-MSE ratios were
`0.96349` on project Full100 and `0.96451` on Generated64.  A fixed
orientation-1 coefficient of `0.10`, reported separately after that seal,
gave `0.95584` and `0.96068`.  Neither spelling has been scored on official
Mini100.  D6 scout16, harmonic-frame, and support-weight searches are distinct
experiments and do not answer this support-only question.

## Frozen candidates

Run the old production-aligned numerical replay twice on all Mini100 weights:

1. the unchanged incumbent support and literal `129:17` blend;
2. the fixed degree-6 support above, retaining every other operation.

Record two fixed degree-6 outputs:

- `literal`: the production `129:17` blend;
- `alpha0p10`: `0.90*q0 + 0.10*q1`.

No support, coefficient, row, or output-dependent selection is permitted.
Capture and seal all predictions before reading Mini100 targets.

The exact packaged R31 Mini100 final rows are a frozen target-free reference.
Because the CUDA replay and package can differ slightly numerically, score
both the literal offline prediction and the paired-delta association

```text
exact_R31 + (offline_candidate - offline_incumbent).
```

The association is diagnostic only; an accuracy winner must still be ported
and run as an exact archive over all Mini100 rows.

## Fast gate

Continue to an exact package only if at least one fixed associated candidate
satisfies all of:

```text
central Mini100 raw-MSE ratio to exact R31 <= 0.960
rows improved                           >= 55 / 100
paired-bootstrap 95% ratio upper bound < 1.000
all arrays finite
offline incumbent association max abs  <= 5e-4
```

Report tails and the probability that the raw ratio is at most
`1.10e-7 / 1.1433268804164358e-7`, but do not call the corresponding arithmetic
a remote projection before the exact-archive gate.

