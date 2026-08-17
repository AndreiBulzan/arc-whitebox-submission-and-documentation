# Preregistration: target-free NNGP packet selector R2

Date frozen: 2026-08-07

## Blocking question

The unavailable-information R1 output oracle proved that a practical
one-candidate-per-line solver can retain essentially all of a finite packet
pool's gain.  R2 asks the production-relevant question:

> Can a network-independent, block-centred depth-32 He-ReLU NNGP feature map
> choose one packet candidate per K129 line while retaining at least 70% of
> the ideal packet gain after exactly one selected antipodal frame is
> propagated?

Passing licenses a fresh disjoint-family validation of the frozen universal
support.  It does not license an estimator edit, a compute or remote-score
projection, packaging, upload, or submission.

## Prior-art and reopening boundary

The prior-art search and distinction recorded in
`PREREG_OUTPUT_ORACLE_R1_20260807.md` control here.  The closest prior deep
ReLU-kernel support experiment ranked whole alternative bases by a scalar
cross-energy and collapsed exactly to degree-six support.  It did not form
within-line packet deviations, did not solve a partition-constrained vector
balancing problem, and did not approximate a conditional packet mixture.

The nearest static coreset, same-index, optimal-transport, Latin-allocation,
and mixed-frame failures selected or redistributed existing rotated basis
blocks.  R2's new observable is the block-centred feature deviation among
several local Gaussian-packet candidates attached to every individual
unoriented Kerdock line.  R1 invalidated the one-independent-sample negative:
actual-output global balancing reduced selected-to-pool reconstruction error
to about `4e-4` of an independent one-per-line draw.

Outcome: **materially new observable**.  This is a surrogate test of the R1
capacity mechanism, not a renamed support-ranking experiment.

## Frozen geometry and packet construction

Use the exact R1 construction and seals:

```text
M:             33,024 unoriented K129 lines
dimension:     256
epsilon:       0.20
rho:           cos(0.20)
tau:           sin(0.20)/sqrt(256)
noise seed:    2026080713
m=4 labels:    0..3
m=8 labels:    0..7
```

For line `i` and Gaussian noise vector `z_iq`, labels `2q,2q+1` are
`rho*u_i + tau*z_iq` and `rho*u_i - tau*z_iq`.  A chosen point is always
propagated with its exact antipode.  The selector may use only this fixed
geometry, the analytic depth-32 He-ReLU kernel, and fixed selector seeds.  It
must not read challenge weights, predictions, targets, row identifiers, or
the R1 output-oracle choices.

## Frozen kernel

Let

```text
kappa(t) = [sqrt(1-t^2) + (pi-acos(t))*t] / pi
K32(x,y) = ||x|| ||y|| kappa^32(<x,y>/(||x||||y||))
kplus(x,y) = 0.5 * [K32(x,y) + K32(x,-y)].
```

`kplus` is the NNGP covariance of the antipodal pair-average feature, up to a
common positive scale that cannot change the selected labels.

Use difference landmarks rather than raw landmarks.  A fixed permutation of
the 33,024 line IDs is produced with NumPy `default_rng(2026080739)`; the
first 256 positions form a nested landmark bank.  Landmark `j` is

```text
delta_j = phi(y[line_j,a_j]) - phi(y[line_j,b_j]),
a_j = j mod m,
b_j = (a_j + 1 + floor(j/m) mod (m-1)) mod m.
```

The difference Gram is evaluated from `kplus`, eigendecomposed in float64,
and candidate Nyström features are whitened by the retained positive
eigenvalues.  Candidate features are then centred by subtracting the `m`
candidate mean separately inside every line.

Kernel lookup/interpolation is permitted only after validation against the
literal 32-fold recurrence on a fixed dense and endpoint-heavy audit grid.
Require maximum absolute kernel error `<=2e-9`, Gram symmetry error
`<=2e-10`, and no eigenvalue below `-2e-8` times the largest positive
eigenvalue.

## Frozen variants and solver

The two primary expert-specified variants are:

```text
P4: m=4, p=128 difference landmarks, r=64 features
P8: m=8, p=64  difference landmarks, r=64 features
```

Two preregistered rank diagnostics may explain a primary failure but cannot
be promoted on the reused R1 target bank without a subsequent unchanged
disjoint test:

```text
D8a: m=8, p=128, r=64
D8b: m=8, p=128, r=128
```

For every variant, use four deterministic restarts and four complete cyclic
coordinate-descent sweeps.  Restart seed is
`202608074100 + 1009*restart + 17*m + p + r`; ties choose the lowest label.
Retain the restart with the smallest represented residual norm.

The resulting label arrays and exact selected positive nodes are sealed
before any supplied network weights are opened.  Since a pure input-NNGP
selector is network-independent, any eventual runtime design should treat
these as universal precomputed algorithmic constants.  R2 does not yet claim
that they fit the package or can be loaded within the measured budget.

## Target-free network capture

After the universal choices are sealed, propagate their exact selected point
and antipode through the same already-used R1 diagnostic rows:

```text
Full1000:      640..647
Generated128:  88..95
```

The capture may read weights and the sealed target-free R1 packet/output
capture for association and reconstruction diagnostics.  It may not read
targets.  Timing is diagnostic only.  No FlopScope or physical row is in
scope.

These targets were already opened in R1/R2.  The post-seal result is
therefore **broad statistical exploratory on a reused bank**, never a fresh
holdout or remote projection.

## Frozen gates

For each variant define ideal-gain retention as

```text
(MSE_q0 - MSE_selected) / (MSE_q0 - MSE_ideal_packet_unbiased).
```

Primary mechanism pass requires at least one of P4/P8 to satisfy all of:

- pooled ideal-gain retention `>=0.70`;
- Full and Generated ideal-gain retention each `>=0.60`;
- selected prediction improves at least `12/16` rows versus q0;
- actual selected-to-finite-pool output reconstruction MSE is at most `0.30`
  times the corresponding R1 median independent one-per-line reconstruction
  MSE, pooled geometrically across rows;
- no target-dependent scale, blend, radius, feature rank, or variant choice.

The `0.70` gate is the expert's production-retention threshold.  Failure of
both primary variants kills the pure rank-64 input-NNGP spelling.  A D8
diagnostic pass establishes only that more NNGP rank helps; it licenses an
unchanged disjoint test, not production implementation.  If output-oracle
capacity remains strong while every NNGP variant fails, the next lawful
oracle is a first-layer-conditioned kernel, not another target-tuned input
feature sweep.

