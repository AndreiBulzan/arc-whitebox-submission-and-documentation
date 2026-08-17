# K129 R75 — cold R27, R71 steady

Date: 2026-08-03

Initial evidence label: **projection**. This is a target-blind lifecycle
policy and performs no remote action.

## Prior art and reopened observable

R67 already implemented the same cold-row policy on R66. Its r2 archive
executed the cold row at `128.668567611B`; that gate failed only because its
projected ledger was low by `16.908288M`. The corrected r3 archive reached the
steady row and was then rejected by the generic gate's inappropriate
requirement that cold R27 and steady R56 predictions be bit-identical. Thus
the policy was not numerically or physically falsified.

R71 now supplies the missing observable: its exact five-worker, 100-row
persistent test passed every row, while its five initialized rows averaged
`51.9141s` and its 95 steady rows averaged `45.6170s`. The remaining risk is
strictly the one initialized row per worker. R75 therefore reuses R67's
policy on the R71 chassis rather than inventing another view rewrite.

## Frozen policy

- The first production prediction in each worker uses the remotely anchored
  R27 estimator after the ordinary deferred constructor has completed.
- Every later production prediction uses the complete R71/R56 correction.
- The branch observes only a monotone per-worker production-call counter. It
  observes no target, seed, row index, prediction, meter state, or grader
  telemetry.
- This does not exploit unmetered computation: each row is billed for the
  exact work it performs and each returned estimate is scored normally.

With five workers, about 5% of rows use R27. Mixing the `1.1936e-7` R27
projection with R71's inherited R56-family projection changes the expected
mean by well under one percent and keeps the Generated-family central
projection near `1.19e-7`.

## Fast gates

1. Bare setup and teardown emit zero FlopScope requests.
2. Cold Full0 equals the exact R27 Full0 hash; the next distinct row equals
   the already-recorded R71 prediction for that row.
3. Cold and steady ledgers are exact and both remain below `240B` effective.
4. Five concurrent cold rows must remain below 60 seconds; R71's completed
   5x20 receipt remains the steady persistence evidence.

No upload or submission is authorized.
