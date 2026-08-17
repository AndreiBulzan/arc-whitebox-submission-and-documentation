# Packet marginal-variance-field compression V1 R1 preregistration

Date: 2026-08-09

Evidence class: component capacity oracle. No benchmark target is opened, no
FlopScope prediction is run, and no production-cost claim is made.

## Question

The exact full-covariance local Gaussian packet closure accurately reproduces
the packet correction, while covariance-factor and trajectory compression have
failed. This oracle asks a narrower question that those experiments did not
answer:

> Is the per-centre, per-neuron *marginal preactivation variance field* itself
> low dimensional enough that a tiny positive representation preserves the
> centre-averaged final packet correction?

The oracle may use the exact full-covariance marginal variances as a teacher.
It may not use benchmark expectations or packet Monte Carlo outputs to fit the
variance representation. A pass is only representational capacity; it does not
establish a lawful way to compute the factors.

## Frozen evidence

- Four networks: Full rows 640 and 641; Generated rows 88 and 89.
- Sixteen oriented canonical centres already used by the sealed full-covariance
  packet micro oracle.
- The same fixed `rho`, `tau`, 32 supplied layers, Gaussian ReLU covariance
  recurrence, and quadrature order 8.
- The sealed packet teacher is used only for hashes and provenance. The primary
  truth is the recomputed exact full-covariance closure, not Monte Carlo.

## Frozen representations

For each network and layer, form the positive matrix `V` of exact marginal
preactivation variances with shape `16 x 256`. Test:

1. `raw_svd`: rank-r truncated SVD of `V`, clipped positive.
2. `log_svd`: rank-r truncated SVD of `log(V)`, then exponentiated.
3. `log_interaction_svd`: exact row and column means plus a rank-r SVD of the
   doubly-centred log-variance interaction.

Ranks are `0, 1, 2, 4, 8, 12, 16`. Rank zero is the representation's declared
base field (column mean for raw/log SVD; additive row/column field for the
interaction form). Each layer's reconstructed positive variance is teacher
forced into an otherwise ordinary Gaussian marginal-mean recurrence. The mean
is rolled freely through all 32 actual supplied matrices.

## Checks

- Exact rank 16 must reproduce the recomputed full-covariance mean to `1e-9`
  maximum absolute error.
- The recomputed selected-layer exact means must reproduce the sealed
  full-covariance teacher to `1e-8` maximum absolute error after normalization.
- Record singular-energy retention and variance reconstruction error by layer.
- Record pointwise and centre-averaged correction fidelity/cosine, pooled and by
  family, at layers 1, 2, 4, 8, 16, 24, and 32.

## Gates

Primary pass: at least one representation at rank <= 4 has all of:

- final centre-averaged correction fidelity >= 0.90;
- final centre-averaged cosine >= 0.95;
- minimum family centre-averaged fidelity >= 0.80;
- final pointwise correction fidelity >= 0.70.

Fallback research pass: no primary pass, but a rank <= 8 representation has
centre-averaged fidelity >= 0.90 and minimum family fidelity >= 0.75.

Otherwise kill the small marginal-variance-field representation on this
teacher. A failure does not reject full covariance itself, nor a nonlinear
representation that is not a low-rank positive variance field.

## Output policy

Write a hash-pinned JSON receipt and compressed NPZ under `runtime/artifacts/`.
Refuse overwrite. A non-passing status is still a successful execution of the
preregistered falsifier.
