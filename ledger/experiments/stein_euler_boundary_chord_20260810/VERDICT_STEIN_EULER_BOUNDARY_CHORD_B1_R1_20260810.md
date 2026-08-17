# Stein--Euler boundary chord B1/R1 verdict

## Verdict

**Kill this collective Gaussian-chord estimator before an all-Mini100 or
physical implementation.**

The identity is exact, but its boundary isolation does not buy enough
variance reduction.  The theorem coefficient greatly amplifies the rare
cell-crossing contribution at small and medium chord scales.  At large
scales it approaches ordinary antithetic Gaussian sampling and loses the
Kerdock design advantage.

## Frozen official-Mini20 result

Evidence is **component** for raw error and **projection** for the 24.967B
production count / adjusted score.

- Same-cost Gaussian plus antithetic pair: pooled raw `3.3313e-6`.
- Best hindsight pooled chord-family cell: ordinary triplet at `h=16`, raw
  `3.5111e-6` (worse than the comparator).
- Public-selected cell: coordinate cross-fit at `h=2`, public raw
  `2.0898e-6` but frozen-holdout raw `6.4468e-6`.
- Required floor gate: `<=1.0e-6`; stretch gate: `<=8.0e-7`.
- Exact R92 on these rows: pooled raw `2.1174e-7`.
- No candidate improved a single row versus R92.

The theorem-fixed boundary arm was much worse.  Its best tested scale was
`h=16`, with pooled raw `5.1913e-6`; at `h=0.25`, its coefficient is 33.49
and pooled raw rises to `1.5706e-3`.

## What was learned

The interior cancellation is real algebraically but not statistically free.
The finite difference replaces smooth-region variance with a sparse,
high-amplitude boundary-crossing variance.  Cross-fitted optimal coefficients
settled near `0.69--1.06`, far below the theorem coefficients at all but the
largest scales; in other words, the data explicitly reject aggressive
boundary isolation.  At the large-scale limit the method becomes an ordinary
Gaussian antithetic estimator, whose error remains several times above the
compute-floor requirement and roughly 16x above R92 on this screen.

This closes the three-trajectory Gaussian-chord spelling.  It does not prove
that every analytic boundary-flux formula is impossible, but any successor
must analytically integrate a boundary measure or exploit the Kerdock design;
merely estimating flux through finite differences is now strongly falsified.

## Evidence

- Preregistration:
  `PREREG_STEIN_EULER_BOUNDARY_CHORD_B1_R1_20260810.md`
- Target-free capture:
  `runtime/artifacts/stein_euler_boundary_chord_b1_r1_targetfree_20260810.npz`
  (`c0e13065f164f55e546475773ed49fa855487c3e208e296fdfc8cd54ff08165c`)
- Capture receipt:
  `runtime/artifacts/stein_euler_boundary_chord_b1_r1_targetfree_20260810.json`
- Post-seal receipt:
  `runtime/artifacts/stein_euler_boundary_chord_b1_r1_postseal_20260810.json`

No FlopScope session, physical row, package, upload, submission, or remote
action occurred.
