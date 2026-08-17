# K129 upstream covariance innovation R1 — fast kill

Date: 2026-07-29

Evidence label: **component**. Cost is a **projection**. This was an offline
screen against the frozen Full100 and Generated64 K129/O0 banks. No physical
benchmark, estimator package, upload, submission, or remote action occurred.

## Decision

**Kill this exact response-conditioned L8 covariance innovation.**

It misses the required `3%` raw-MSE gain on both families by orders of
magnitude:

| family | baseline raw MSE | candidate raw MSE | candidate / baseline |
|---|---:|---:|---:|
| Full100 | `2.532021e-7` | `2.852568e-5` | `112.659743` |
| Generated64 | `3.105450e-7` | `2.754048e-5` | `88.684340` |

The direction contains only a weak real signal. Allowing a forbidden
target-open scalar after the fact gives:

| family | oracle scale | oracle / baseline |
|---|---:|---:|
| Full100 | `0.00598195` | `0.987869` |
| Generated64 | `0.00337756` | `0.997085` |

Thus even oracle rescaling removes only `1.21%` of Full error and `0.29%` of
Generated error. It cannot clear the `<=0.97` two-family gate.

## Exact target-free construction

For the 129 orientation-zero basis trajectories, let `X_b` be the post-ReLU
L8 basis centroid, `Y_b` its final signed endpoint, `mu` the analytic L8
mean, and `R` the realised mean-gated response from L8 to the output. The
screen formed

```text
T_b      = (X_b - mean_b X_b) R
beta_j   = clip(Cov_b(T_bj, Y_bj) / Var_b(T_bj), 0, 2)
delta    = beta * ((mu - mean_b X_b) R)
candidate = max(0, frozen_K129_O0 + delta).
```

This is a response-weighted control innovation: the within-basis covariance
sets the sensitivity, while the discrepancy between the analytic and
empirical L8 means supplies the signed action. No target enters `beta` or
`delta`.

The failure is a scale mismatch. The correction RMS is about `7.3--7.6e-3`,
whereas useful K129 corrections are on the `1e-4` scale. The target-open
scales near `0.003--0.006` expose that mismatch directly. Correction/residual
cosines are only `0.0904` Full and `0.0445` Generated, so no derived fixed
normalisation can turn this carrier into a factor-scale result.

## Count projection

Composing a dense mean-gated L8-to-output response costs at most
`0.738198B`; projecting 129 basis centroids and forming the innovation adds
about `0.0172B`. A conservative total is `<0.8B`, below the requested `2B`
ceiling. Accuracy, not cost, kills the lane.

## Boundary

Do not retry this carrier with a fitted coefficient, ridge sweep, alternate
checkpoint, or additional response rank. Its forbidden oracle capacity is
already below the required gain on Generated. A viable new mechanism must
carry non-Gaussian, design-phase information whose coefficient-one scale is
physically correct; another covariance/mean-gated innovation is closed.

Authoritative offline result:
`k129_upstream_covariance_innovation_r1_20260729.json`.
