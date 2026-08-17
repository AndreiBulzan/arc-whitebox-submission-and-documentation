# R99 verdict: bank the `matmul(out=)` R94 rewrite

Date: 2026-08-10

## Outcome

R99 passes its preregistered promotion gate and is the new local engineering
bank.  It preserves the exact R94 estimator statistic while reducing both
charged arithmetic and quiet residual cost.

## Exact Mini100 result

The exact packaged R99 archive completed all 100 official Mini rows in five
lanes of 20 rows, with zero failures.  Its predictions are byte-identical to
R94 on every row, layer, and output coordinate.  Consequently, public,
holdout, and pooled raw-MSE ratios are all exactly `1.0`.

The exact per-prediction counts are:

| state | R94 | R99 | reduction |
|---|---:|---:|---:|
| initialized | 125,835,566,424 | 125,815,743,768 | 19,822,656 |
| steady | 123,391,852,159 | 123,372,029,503 | 19,822,656 |

This count removal alone is a `0.01606%` steady-count improvement.  Five-lane
wall and residual measurements are not used for pricing because concurrent
lane contention makes them non-comparable.

## Quiet physical pricing

Two independent isolated `R94/R99/R99/R94` initialized-plus-steady controls
were run under the benchmark lock.  Their median effective-compute ratios
were:

- repeat 1: `0.9921318889512261` (`0.78681%` reduction);
- repeat 2: `0.9935473899959735` (`0.64526%` reduction).

Both repeats clear the preregistered `0.2%` adjusted-score gate.  Using the
less favorable repeat and R94's observed public score
`1.136249153465844e-7` gives a **projection** of
`1.128917380811127e-7`.  This is not a remote receipt.

## Mechanism

R99 changes execution only:

1. persistent destination reuse for the two live late-B7 geometries, removing
   the fringe copy; and
2. destination-backed `matmul(out=)` in place of the dominant R40
   `vecmat(out=)` spelling.

The repaired-H1 product-bank rewrite remains excluded because its independent
component value was too small for roughly 519 MiB of added storage.

## Evidence and boundary

- Exact Mini100 execution: **measured whole** for output/count/lifecycle;
  **broad statistical** for accuracy.
- Quiet A/B repeats: **measured whole**.
- `1.1289e-7` public score: **projection** from the less favorable quiet
  repeat and R94's remote score.
- No upload, submission, or other remote action was taken.

Artifacts:

- package: `runtime/artifacts/k129_r99_r94_matmul_out_local_candidate_r1_20260810.tar.gz`
- target-free Mini capture: `runtime/artifacts/k129_r99_exact_archive_official_mini100_fivelane_r1_targetfree_20260810.npz`
- post-seal score: `runtime/artifacts/k129_r99_exact_archive_official_mini100_r1_postseal_20260810.json`
- quiet controls: `runtime/artifacts/k129_r94_r99_matmul_out_abba_r1_20260810.json` and `runtime/artifacts/k129_r94_r99_matmul_out_abba_repeat_r2_20260810.json`

Decision: bank R99 locally over R94.  A remote submission still requires
explicit authorization.
