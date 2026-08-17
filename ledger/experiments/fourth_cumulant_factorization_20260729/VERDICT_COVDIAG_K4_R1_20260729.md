# Covariance-diagonal K4 R1 — fast-screen verdict

Date: 2026-07-29

Evidence label: **component**.  This reused one frozen target-free production
capture and eight already-open development rows.  No estimator, FlopScope
session, physical/timed row, benchmark lane, package, upload, submission, or
remote action was run.

## Decision

**Kill this endpoint K4 correction immediately.  Keep one narrower
representation result: the final all-distinct K4 query is already observable
very accurately from the current cloud, so endpoint tensor reconstruction is
not our missing breakthrough.**

The new constrained factorization was deliberately small: eight amplitudes
instead of PTCC's 330-parameter symmetric rank-eight core.  On four Full
rows it reconstructed the exact, radial-demixed and final-normalized
preactivation K4 query with:

```text
covariance-diagonal core       correlation 0.988512   relative RMSE 0.150031
direct current-cloud K4        correlation 0.998031   relative RMSE 0.067464
```

Thus the low-rank factor model is real, not numerical noise.  It is also
strictly worse than the scalar K4 query the current final preactivation
cloud already supplies directly.

Replacing the direct query by the factorized query then failed both
families:

| frozen four-row family | candidate/current pooled MSE | rows improved |
|---|---:|---:|
| Full | `1.005413` | `1/4` |
| Generated, noise-corrected | `1.004737` | `1/4` |

Every value was finite and bounded, and the worst row ratio was only
`1.0177`; this is a clean signal failure rather than a blowup.

## Exact formula tested

Let `P` be the top eight covariance eigenvectors of one selected hidden arm.
The symmetric K4 core was constrained to

```text
K4(i,j,k,l) = sum_s lambda_s P[i,s] P[j,s] P[k,s] P[l,s].
```

One target-free joint ridge fit used both repeated connected views:

```text
c31(i,j) ~= sum_s lambda_s P[i,s]^3 P[j,s]
c22(i,j) ~= sum_s lambda_s P[i,s]^2 P[j,s]^2.
```

Each block was normalized by its own RMS, with fixed damping `1e-3`.
The realized next-weight query was

```text
kappa4[o] = sum_s lambda_s (P[:,s]^T w_o)^4.
```

The arm prediction was the current gamma readout plus the bounded Edgeworth
difference between this query and the direct empirical K4 query, followed by
the literal `129:17` blend.  No target-fitted coefficient, pairwise
contraction, independent SVD-triplet interpretation, rank sweep, or damping
sweep was used.

## Economics

Once `cov/c22/c31` exist, the eight-parameter solve and 256 output queries
are below `0.01B` ordinary operations.  Producing those views from the
current signed K146 cloud controls the price:

```text
one width-176 two-arm Gram                 4.630973952B
covariance + central22 + central31        13.892921856B
remote 320262 effective anchor                  170.3B
anchor plus the three Grams                    184.193B   projection
```

At `184.193B`, adjusted `1.2e-7` requires raw `<=1.7721e-7`; adjusted
`1e-7` requires raw `<=1.4767e-7`.  R1 instead worsened raw error.  It also
cannot satisfy the requested `<=55B` economics while sourcing its state from
the K146 cloud.

## What this closes, and what it does not

This closes covariance-diagonal, endpoint-only K4 reconstruction from the
current production cloud.  More generally, it removes the motivation for
another endpoint K4 factorization: the unchanged cloud's direct K4 query is
already better than this compact reconstruction and is within `6.75%`
relative error on the four-row Full component.

It does **not** prove that a compact K4 state cannot help a genuinely
lower-compute solver.  Reopening requires a different execution point:
propagate the factorized connected state upstream while replacing most of
the particle cloud, or use mixed polarization observations that change the
upstream mean estimate.  Merely refactoring/replacing the final K4 query
cannot attack the observed dominant error, which remains the structured
quadrature estimate of the signed final mean.

## Reproducible artifacts

```text
preregistration
  PREREG_COVDIAG_K4_R1_20260729.md
  b0cf9eedaf6b346f05bd4309331e7d4cb386cdc7658438582ea520e7e4d00ff8

screen source
  screen_covdiag_k4_r1_20260729.py
  da07aabca707cdf2bc58e28fe4c2387109ae4eda2785f5589aac04489ceb6d43

receipt
  covdiag_k4_r1_postseal_r4_20260729.json
  a8fe0b239ab41c63701d93a0f48f35f27360a68e7d2f40b2f9d15233529b025d
```

