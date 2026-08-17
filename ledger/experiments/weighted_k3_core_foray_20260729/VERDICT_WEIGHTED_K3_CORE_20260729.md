# Compact weighted K3 core: kill verdict

Date: 2026-07-29

Evidence label: **component**. This is an already-open Full32,
teacher-forced final-transition experiment. It is not a free rollout,
measured whole, FlopScope receipt, package, upload, submission, remote result,
or deployable estimator.

## Verdict

**Kill the rank-8/12/16 PTCC K3 core as a deployable direct moment
readout. Keep PTCC K4 rank 8 as a separate component.**

The no-exact-K3 gate required pooled final-mean MSE at most `1.4e-7`.
The best tested direct result was:

```text
covariance PTCC K3 rank 16 + covariance PTCC K4 rank 8
pooled final-mean MSE = 3.400267e-7
```

That misses the gate by `2.43x`. The K3 rank trend is real but already
strongly diminishing:

| K3 rank | query rRMSE | Edgeworth-weighted query rRMSE | with exact K4 | with PTCC-K4 r8 |
|---:|---:|---:|---:|---:|
| 8  | `0.057260` | `0.568094` | `3.792677e-7` | `4.347565e-7` |
| 12 | `0.046174` | `0.490166` | `2.880217e-7` | `3.577493e-7` |
| 16 | `0.041998` | `0.474083` | `2.638869e-7` | `3.400267e-7` |

Going from rank 12 to 16 buys only `5.5%` in the metric that matters and
`5.0%` in direct final-mean MSE. Reaching the gate from rank 16 requires a
further `58.8%` final-mean reduction. This is not a compact-rank continuation
of the observed curve.

## The new basis did not rescue it

The tested target-free alternative standardizes source coordinates by their
marginal standard deviations and chooses the rank-`r` subspace from the
equal-trace sum of:

1. the hidden correlation matrix; and
2. the downstream-weight Gram weighted by squared analytic Edgeworth K3
   sensitivity.

It has no fitted MLP coefficient and uses only state available before the
next ReLU mean. It failed decisively:

```text
sensitivity-standardized K3 query rRMSE
rank 8   0.373617
rank 12  0.416108
rank 16  0.456995
```

The associated final-mean errors were `1.77e-5` or worse when paired with
PTCC-K4 rank 8. The standardization/query metric discards the globally
consistent covariance directions that make the original PTCC fit useful;
it is not a production seam.

## K4 survives, but rank 8 remains the readout choice

With exact K3 supplied at score time:

| K4 spelling | final-mean MSE |
|---|---:|
| PTCC rank 8  | **`1.096968e-7`** |
| PTCC rank 12 | `1.159916e-7` |
| PTCC rank 16 | `2.398419e-7` |
| exact K4 | `1.977727e-8` |

Rank 12 has slightly better unweighted K4 query correlation than rank 8 but
worse final-mean MSE; rank 16 reverses sharply in the sensitive directions.
This independently confirms that unweighted `0.99+` query correlation is
not a promotion statistic.

## What this closes

Do not spend production work on:

- widening the same covariance-basis PTCC K3 core a few ranks;
- marginal standardization of the repeated K3 views;
- swapping covariance modes for a sensitivity-weighted downstream-weight
  subspace; or
- widening PTCC K4 beyond rank 8 under the current LSQR spelling.

The surviving new information is narrower: the rank-8 K4 representation has
teacher-forced capacity, but it must be paired with an independently accurate
K3/structured-cloud channel. A genuinely new K3 observable, rather than a
larger version of this repeated-view core, is required.

## Reproducible artifacts

```text
source
  run_weighted_k3_core_20260729.py
  SHA-256 5cd0b9f4e9af2b992b1540bb9e45cd4a2b0a0ad789d495afd9e898c0939b2d13

arrays
  weighted_k3_core_full32_r1_20260729.npz
  SHA-256 9fb72d2a871aad618c2df4012658b3f31f8f8f2c036aa7fb533f5e92be963b9e

receipt
  weighted_k3_core_full32_r1_20260729.json
  SHA-256 1413d6299cd6129f12832f68fd6473f17f47209432616ab7b77f4c56395261c4
```

No frozen source, `STATUS.json`, candidate, package, remote state, or
submission was changed.
