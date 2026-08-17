# K129 R67 — cold R27, steady R56 sidecar

Date: 2026-08-03

Initial evidence label: **projection**.  This is a target-blind lifecycle
policy, not public-row or seed routing.

## Prior-art and legality boundary

Searches covered `cold row`, `cold start`, `initialized row fallback`, `first
prediction fallback`, `warmup prediction`, `lifecycle router`, `stateful
first`, and `skip sidecar`.  No capsule candidate varies the estimator only
to protect the deferred-initialization row.  Existing lifecycle work instead
runs the full statistic on the first prediction.

The challenge scores compute and MSE per MLP before averaging.  The estimator
contract permits persistent worker state, and this policy observes neither a
target, seed identity, row index, prediction error, grader state, nor a
metering omission.  It deterministically uses the same R27 fallback for the
first production prediction in every worker and the same R56 sidecar for all
later predictions.  Outcome: **novel in capsule**, within the ordinary
estimator contract.

## Reason and ceiling

The correctly partitioned R66 five-lane screen measured:

```text
initialized wall  53.909--54.417s
steady wall       48.145--48.752s
```

The steady path is inside the calibrated remote band; the initialized path is
not.  R66's sidecar adds `12.038157570B` steady counted work over R27.  Skipping
only that correction on the cold row should reduce its count from
`140.689816893B` to approximately `128.651659323B`, while retaining a useful
R27 prediction rather than returning zero or a weak analytic fallback.

With five workers over 100 MLPs, only about 5% of rows use R27.  The completed
remote R27/R26 family anchor is near `1.20e-7`; R56's repriced projection is
near `1.17e-7`.  A rowwise mixture therefore has an expected score around
`1.18e-7`, before fresh association uncertainty.  This remains a projection.

## Gates

1. cold and steady ledgers deterministic and below the cap;
2. cold prediction identical to R27 and full steady prediction identical to
   R66 on the same MLP;
3. five-lane initialized and steady rows all below 60s, with calibrated remote
   headroom;
4. persistent multirow state changes exactly once and never reverts;
5. broad score is repriced as a heterogeneous rowwise policy, not asserted
   from R56 alone.

No upload or submission is authorized.
