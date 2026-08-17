# Preregistration: local antipodal packet-GP bridge L1/R1

Date: 2026-08-08

Evidence scope: `component`, target-free development oracle.

## Question

Can the exact final preactivations already observed at each antipodal Kerdock
line predict the *local* Gaussian-packet response around that same line well
enough to choose a sparse set of complete packet bases?

This is deliberately narrower than the rejected generic KLPQ continuation.
The packet direction is approximately 0.20 radians from an observed anchor;
no generic off-design integral is attempted here.

## Frozen inputs

- Complete-basis packet capture:
  `runtime/artifacts/grouped_gate_source_basis_g0_r1_targetfree_20260808.npz`.
- Kerdock geometry and supplied weights through the frozen capsule loaders.
- Development rows only: Full rows 640, 641 and Generated rows 88, 89.
- Packet parameters are read from the complete-basis capture and must equal the
  frozen epsilon-0.20 Gaussian packet construction.
- Latent correlation kernel: normalized depth-31 ReLU NNGP kernel `K31`.

No benchmark expectation, Mini100 row, physical benchmark row, package,
upload, or remote action may be opened by the target-free program.

## Fixed predictor

For one line with unit representative `u`, let `z+` and `z-` be the exact
network preactivations immediately before the last ReLU at `u` and `-u`.
For a packet point `y = rho*u + tau*Z`, put `R = ||y||`, `v = y/R`, and
`c = u^T v`.  Define

```
k0 = K31(-1)
kc = K31(c)
km = K31(-c)
det = 1 - k0^2
a = (kc - k0*km) / det
b = (km - k0*kc) / det
s2 = 1 - (kc^2 + km^2 - 2*k0*kc*km) / det
```

The two-anchor latent posterior means at `v` and `-v` are

```
m+ = a*z+ + b*z-
m- = b*z+ + a*z-
```

and their normalized posterior variance is `s2`.  For amplitude `A`, the
predicted antipodal pair activation is

```
R/2 * (E ReLU(N(m+, A*s2)) + E ReLU(N(m-, A*s2))).
```

The Gaussian packet geometry is integrated with a fixed product rule:

- order-8 Gauss-Hermite for the parallel standard normal;
- order-8 generalized Gauss-Laguerre for the transverse chi-square with 255
  degrees of freedom.

Order 12 on two fixed bases is a convergence check, not a selectable model.

## Frozen amplitude variants

The variants are fixed before seeing the exact packet responses:

1. `theory`: constant amplitude `2/256`;
2. `global_energy`: per-output mean squared preactivation over all canonical
   antipodal nodes;
3. `basis_energy`: per-output mean squared preactivation within the current
   complete antipodal basis;
4. `zero_variance`: conditional-mean ReLU only, retained as a diagnostic.

`basis_energy` is the primary variant.  All four are reported; none may be
fit to packet responses or benchmark expectations.

## Algebra and numerical gates

The capture is invalid unless all of the following hold:

1. ReLU of the captured final preactivations reconstructs the already sealed
   canonical complete-basis response with maximum absolute error at most
   `2e-6`.
2. All posterior variances are finite and nonnegative up to `2e-12` numerical
   tolerance.
3. The order-8 versus order-12 primary prediction differs by at most `2e-6`
   in maximum absolute basis-output value on the fixed convergence subset.
4. The K31 checksum at `-1` matches the sealed KLPQ implementation to
   `2e-13` in float64.

## Target-free bridge metrics and continuation rule

After the prediction file is sealed, the exact packet-basis capture may be
used as a *target-free teacher* to measure:

- flattened source-vector cosine and normalized residual
  `R2_source = 1 - ||pred-true||^2 / ||true||^2`;
- cosine and normalized residual of the summed 256-vector correction;
- normalized Gram-matrix disagreement across the 129 basis sources.

The bridge is a strong pass if one non-diagnostic lawful variant has, on both
families, `R2_source >= 0.25` and summed-correction cosine at least `0.50`.

It is an admissible weak pass, allowing one sparse-selector test, if one
lawful variant has strictly positive `R2_source` and strictly positive
summed-correction cosine on both families.  This deliberately conservative
weak gate prevents a possibly useful geometric signal from being discarded
prematurely.

If every lawful variant has nonpositive `R2_source` or nonpositive summed
correction cosine on either family, sparse selection stops.  The sealed
capture remains evidence only against this two-anchor isotropic bridge, not
against packet smoothing or all finite-network local predictors.

## Sparse selector, if the bridge gate passes

Use only predicted basis-source vectors to choose a signed weighted support
of exactly 16 complete bases per network.  Validate the frozen support and
weights against the exact packet source vectors without opening expectation
targets.  Selection must not use exact source vectors, packet replicate
labels, or benchmark errors.

A production-capacity pass requires at least 50% normalized correction
fidelity on both families and stability under the two sealed packet-replicate
halves.  Only then may a separate post-seal scorer open the four fixed
development expectation targets.  The post-seal acceptance threshold is at
least 35% pooled raw-MSE reduction, positive reduction on both families, and
no sign reversal under the half-replicate checks.

## Evidence limits

- A successful four-row result remains `component`, not broad statistical or
  physical evidence.
- Arithmetic costs are projections until a capsule-native whole receipt
  exists.
- No result here authorizes Mini100, physical-row, package, upload, or remote
  work.
