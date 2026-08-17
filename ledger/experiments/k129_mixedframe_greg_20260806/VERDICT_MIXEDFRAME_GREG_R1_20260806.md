# Mixed-frame exact-moment GREG R1 verdict

Date: 2026-08-06

## Verdict

**Killed broadly.**  Do not scan Latin offsets, GREG ridges/shrinks,
alternate low-order moments, or target-fitted coefficients.

This is **broad statistical** evidence from sealed target-free predictions
on Full100 and Generated128.  It is not a literal mixed-cloud capture,
physical receipt, package, or remote result.

## Result

| family | unweighted Latin / q0 | exact-z2 GREG / q0 | GREG p95 | improved rows |
|---|---:|---:|---:|---:|
| Full100 | `1.55945` | `1.59102` | `5.34477` | `31/100` |
| Generated128 | `1.69015` | `1.67980` | `5.89238` | `38/128` |

The fixed GREG step reduced the exact auxiliary mismatch to `0.94572` and
`0.94665` of the unweighted mismatch, but that correction had no useful
association with final error.  It worsened Full and only trivially improved
the already-catastrophic Generated Latin mix.

## Boundary learned

The failure closes the explicit version of the hypothesis that frame mixing
mainly loses because it perturbs a repairable low-degree moment.  The
four-frame teacher remains strong (`0.28981`/`0.26662` raw ratios to q0), but
its cancellation is not retained by a total-K Latin support after exact
second-layer moment calibration.  Together with the earlier exact quartic
defect, Schur moment-descent, signed-crossframe, gate-state, and paired-gate
failures, the missing signal is high-dimensional and network-conditioned
under all currently available cheap observables.

## Receipts

- preregistration: `PREREG_MIXEDFRAME_GREG_R1_20260806.md`
- target-free capture:
  `runtime/artifacts/k129_mixedframe_greg_r1_targetfree_20260806.json`
- broad score:
  `runtime/artifacts/k129_mixedframe_greg_r1_postseal_20260806.json`
