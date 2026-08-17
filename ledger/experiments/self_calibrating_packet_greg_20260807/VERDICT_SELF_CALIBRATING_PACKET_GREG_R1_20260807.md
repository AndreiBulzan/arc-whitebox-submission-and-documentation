# Self-calibrating partition GREG R1 verdict

Date: 2026-08-07

## Verdict

**Kill calibration, pilots, probability balancing, and cross-fitted GREG for
the existing universal and exact-first-layer-conditioned NNGP/Nystrom feature
spans.  License the preregistered sparse exact layer-two probe oracle.**

The expert's change of objective was mathematically valid and worth testing:
candidate classification was unnecessarily strong, while a block-centred
generalized-difference estimator can be design-unbiased for the finite packet
pool.  The decisive feature-span calculation nevertheless failed before
targets.  The relevant output information is not present in the existing
features under *any* linear finite-network metric.

No post-seal target score, FlopScope session, physical row, estimator edit,
package, compute projection, upload, or submission occurred.

## Exact feature-span result

The complete packet candidate outputs were recreated in memory for reused
Full `640..647` and Generated `88..95`.  For each feature family, the optimal
network-specific multi-output map `B_star` was fitted to all within-line
candidate deviations.  This is strictly more information than a production
estimator can use.

| Feature span | m | r | Oracle variance explained | Residual selection variance |
|---|---:|---:|---:|---:|
| universal depth-32 NNGP | 8 | 64 | `2.683%` | `2.0800e-7` |
| universal depth-32 NNGP | 8 | 128 | `2.718%` | `2.0793e-7` |
| exact H1 + conditional NNGP tail | 4 | 64 | `6.119%` | `1.8724e-7` |
| exact H1 + conditional NNGP tail | 4 | 256 | `7.257%` | `1.8497e-7` |

The frozen capacity gates required at least `70%` explained variance and at
most `4.0e-8` pooled residual variance.  Every span missed by a wide margin.
Increasing the exact-first-layer conditional span from 64 to 256 modes gained
only `1.14` percentage points.

This resolves the ambiguity left by the earlier selector failures: the native
NNGP metric was not merely misweighted.  The spans themselves are nearly
orthogonal to the supplied network's within-line output deviations.

## Honest one-candidate cross-fit

Sixteen independent selection seeds and four independent folds were simulated
per network.  Each held-fold correction used coefficients fitted only from
the other folds, so the design-unbiasedness argument was valid.  Cross-fitting
did not rescue any span:

| Feature span | Independent selection MSE | Cross-fitted pool MSE |
|---|---:|---:|
| universal r64 | `2.1489e-7` | `3.7291e-7` |
| universal r128 | `2.1489e-7` | `1.0466e-6` |
| first-layer r64 | `1.8487e-7` | `4.9010e-7` |
| first-layer r256 | `1.8487e-7` | `1.9162e-6` |

One observation per line is formally identifying but practically noisy: the
unobserved line effect contaminates the feature/output cross-moment, and the
high-rank fits amplify it.  A pilot cannot repair the much stronger preceding
feature-span failure, so pilot work is not licensed.

The JSON field `crossfit_oracle_reduction_retention` is non-controlling for
the first-layer variants because their finite 16-seed empirical random MSE
fell slightly below the exact oracle residual, making that sample ratio's
denominator nonpositive.  The literal cross-fit MSE, exact `B_star` capacity,
and every promotion gate remain unambiguous.

## Positive structural result

The block-centred candidate-output deviations are themselves low-dimensional:

| Best output rank | m8 energy captured |
|---:|---:|
| 1 | `37.60%` |
| 4 | `60.50%` |
| 8 | `73.69%` |
| 16 | `85.20%` |
| 32 | `93.09%` |
| 64 | `97.59%` |

This does not supply those modes at runtime, but it sharpens the next target.
A successful lawful feature need not predict all 256 candidate outputs; it
must expose roughly 8--16 *quenched* output directions.  Exact sparse
layer-two probes are now the cheapest materially new observable specified in
the external follow-up.

## Evidence boundary and receipts

This is target-free **component** evidence on a reused 16-network bank.

- `PREREG_SELF_CALIBRATING_PACKET_GREG_R1_20260807.md`
- `capture_self_calibrating_packet_greg_r1_targetfree_20260807.py`
- `runtime/artifacts/self_calibrating_packet_greg_r1_targetfree_20260807.npz`
- `runtime/artifacts/self_calibrating_packet_greg_r1_targetfree_20260807.json`
