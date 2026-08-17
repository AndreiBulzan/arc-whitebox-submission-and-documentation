# Preregistration: exact-anchor short Gaussian tail R1

Date: 2026-08-08

Evidence sought: target-free **component** reproduction of the existing
canonical K129 particle mean.  This is not an accuracy claim, physical or
effective-compute receipt, Mini100 result, package, upload, submission, or
remote action.

## Question

Can exact Kerdock particles be stopped at a late layer, replaced by their
exact empirical mean and full 256x256 covariance, and propagated for only the
remaining two to eight layers by recursive Gaussian moment matching without
changing the final K129 mean materially?

The purpose is compute barter.  One empirical covariance costs approximately
one `N x 256` by `256 x N` contraction.  Omitting `t` literal particle layers
therefore saves approximately `t-1` particle-layer contractions before the
small `256^3` covariance sandwiches.  A four-layer tail beginning after layer
28 has a first-order net saving near 13B operations.

## Prior-art boundary

Closed global closures Gaussianized the law at or near the input and rolled
that approximation through most of 32 ReLU layers.  Packet closures maintained
one Gaussian law per Kerdock centre and addressed a different smoothed target.
The shared gate-Gram closure replaced centre-specific packet covariances.

R1 here does none of those things.  It propagates the literal complete K129
cloud to the anchor and computes that cloud's exact empirical mean and dense
covariance.  The only question is whether a short tail can reproduce the
already-known continuation of those same particles.  Exact particle tail
means are the labels; no challenge expectation is needed.

## Frozen rows and anchors

Use Full rows `628..635` and Generated rows `80..87`.  Targets remain closed.

Capture exact empirical states after layers

```text
24, 26, 28, 29, 30
```

and the exact K129 mean after every subsequent layer.  All nodes are the
complete antipodal canonical K129 cloud.  Sphere means are compared before
the common exact Gaussian radial multiplier, since that multiplier cancels.

## Two frozen covariance recurrences

For empirical anchor mean `mu` and covariance `C`, a subsequent weight matrix
`W` gives

```text
m = mu W
S = W^T C W.
```

The ReLU marginal mean and variance are exact under the Gaussian assumption:

```text
a_i = m_i / sqrt(S_ii)
mu_i^+ = sqrt(S_ii) phi(a_i) + m_i Phi(a_i)
```

with the standard exact second-moment formula.

R1 evaluates:

1. `full_bivariate`: exact pairwise Gaussian ReLU second moments using the
   same Plackett/Legendre implementation already validated by the packet
   full-covariance oracle;
2. `hermite1_diag`: `C^+ = D S D + diag(r)`, where
   `D=diag(Phi(a))` and the diagonal residual `r` repairs every exact marginal
   variance.

The first is the ceiling for a two-moment Gaussian tail.  The second is the
production-shaped recurrence; it retains the first Hermite/Bussgang term and
the exact diagonal while dropping only the off-diagonal higher-Hermite tail.

No scale, target fit, reset, per-family choice, anchor selection, covariance
shrinkage, or mean calibration is allowed.

## Metrics and gates

For every anchor and method, report at each remaining layer:

- per-output MSE against the exact particle mean;
- RMS and maximum absolute difference;
- cosine of the predicted and exact tail corrections relative to the anchor
  mean;
- both families separately and pooled.

The production-shaped four-layer tail (`anchor=28`, `hermite1_diag`) passes
only if:

1. final pooled mean-vector MSE <= `1.0e-8`;
2. final MSE <= `2.0e-8` in each family;
3. final maximum absolute difference <= `1.0e-3`;
4. the full-bivariate ceiling is no worse than the production recurrence;
5. every covariance is finite, symmetric to `1e-9` relative, and has
   diagonal >= `-1e-10` after roundoff.

An exploratory fallback may be reported for anchors 29 or 30, but it is not
called the four-layer pass.  If full-bivariate anchor 28 exceeds `2.0e-8`
pooled, close the four-layer Gaussian-tail mechanism: recurrence engineering
cannot repair non-Gaussian anchor information already absent from its state.

Passing authorises only a later capsule-native armwise reproduction and a
measured cost audit.  It does not inherit the approximate arithmetic above as
a score receipt.
