# K178 direct signed-pilot multilevel correction R1

Date: 2026-07-28

Evidence sought: target-free offline **component** evidence only. No
challenge target, score, FlopScope physical row, package, upload, submission,
or remote action is permitted.

## Fixed question

The total-K=178 chassis retains 49 orientation-1 bases where K238 retains
109. For every basis, define `P_b` as the signed W31 endpoint mean from the
already-preregistered 16 fixed positive rows
`0,16,...,240` and their 16 exact negatives. Define `F_b` as the pinned
complete-basis signed endpoint mean.

Test the direct multilevel correction

```text
dF = mean(F over S109) - mean(F over S49)
dP = mean(P over S109) - mean(P over S49)
estimated S109 endpoint = mean(F over S49) + beta*dP.
```

This is materially different from the killed trajectory support router: the
pilot is used as a sign-bearing correction, never to choose or change a
support.

## Frozen rows and coefficients

Use the first four Full and first four Generated source indices in the
sealed held arrays of
`k162_16row_antithetic_trajectory_router_targetfree_20260727.npz`.

Report:

1. fixed `beta=1`;
2. one scalar beta fit by least squares on Full4 and evaluated on Generated4;
3. one scalar beta fit on Generated4 and evaluated on Full4.

The exact endpoint teachers may be opened only after all eight pilot arrays
have been computed. No challenge target is opened at any point.

## Gate

Continue only if the cross-family fitted-beta residual-MSE ratio is at most
`0.70` in both directions, all values are finite, and each fitted beta is in
`[0,2]`. Otherwise kill this spelling without scanning rows, row counts,
features, or nonlinear models.

## Economics boundary

The existing 129-basis, 32-particle full-depth pilot projection is
`17.049416815B` operations. S49 overlaps S109 in 47 bases, so a production
K178 correction needs pilot propagation for the 62 missing S109 bases; the
32 selected rows of the retained bases can in principle be accumulated from
the K178 trajectory itself. Proportional pilot work is therefore
`62/129 * 17.049416815B = 8.194B` before integration, movement, calls, and
residual. This is a projection, not a measured whole.
