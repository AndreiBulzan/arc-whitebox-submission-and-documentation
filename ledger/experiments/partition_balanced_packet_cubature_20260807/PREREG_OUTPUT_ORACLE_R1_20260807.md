# Preregistration: partition-balanced packet output oracle R1

Date frozen: 2026-08-07

## Blocking question

Can one select exactly one Gaussian-packet candidate and its antipode from
every unoriented K129 line, keeping exactly 66,048 propagated trajectories,
while retaining at least 90% of the accuracy gain of a finite `m=8`
candidate-pool average?

This is an intentionally unavailable-information capacity oracle.  Candidate
selection may use the actual final-layer output vectors of every candidate.
It cannot become a contest estimator.  Passing licenses a separate target-
free block-centred NNGP/Nystrom selector; it licenses no estimator edit,
package, score projection, upload, or submission.

## Prior-art boundary

Capsule searches covered `colorful`, `vector balancing`, `partition`,
`one per line`, `candidate pool`, `Nystrom`, `MMD`, `Latin allocation`,
`same-index`, `coreset`, and `mixed frame`.

Nearest negatives are materially different:

- four-frame static coresets selected existing rotated endpoints and reversed;
- same-index coupling, optimal-transport pairing, and complete-basis Latin
  allocation did not reconstruct complete-frame cancellation;
- the fusion-frame coordinate descent optimized aggregate covariance after
  dropping axes, not conditional packet outputs;
- the prior deep-ReLU-kernel support rule produced a scalar basis ordering
  identical to degree-six support, not block-centred within-line selection;
- one independent packet sample per line is conditional-noise dominated.

R1 introduces a new observable: the actual vector deviation of each packet
candidate from its own line's finite-pool mean.  Global selection explicitly
balances these deviations across all 33,024 partitions.  This invalidates the
independence assumption behind the one-sample negative.

The mathematical capacity motivation is Ambrus and Bozzai, *Colorful vector
balancing*, Mathematika 70(4), 2024, DOI `10.1112/mtk.12274`.  For centred
families bounded by Euclidean norm `B` in dimension `D`, a one-per-family
selection exists with summed norm at most `B*sqrt(D)`.  Applied here, the
per-coordinate MSE of the selected mean relative to the pool mean is bounded
by `B^2 / M^2`.  The theorem is existential; R1 separately tests whether a
practical coordinate solver finds a useful selection.

## Frozen data and packet construction

Use the same weight-only rows as the already sealed R2 packet truth so that a
64-replicate packet reference is available without opening any target inside
the capture process:

```text
Full1000:      640..647
Generated128:  88..95
M:             33,024 unoriented K129 lines
D:             256
epsilon:        0.20
rho:            cos(0.20)
tau:            sin(0.20)/sqrt(256)
noise seed:     2026080713
```

For each line representative `u_i`, generate four independent Gaussian
vectors `z_iq`, `q=0..3`.  The nested eight candidate labels are

```text
y_i,2q   = rho*u_i + tau*z_iq
y_i,2q+1 = rho*u_i - tau*z_iq
```

Every label is propagated together with its exact antipode.  Candidate pair
output is the radial-normalized average of the two final-layer activations.
The `m=4` arm uses labels `0..3`; `m=8` uses all labels.  No target, reference
mean, row error, or network identifier may enter candidate generation or
selection.

## Frozen actual-output solver

For each network and each `m` independently:

1. subtract each line's `m`-candidate output mean;
2. run four deterministic restarts with seeds
   `202608071300 + 1009*restart + 17*m + network_position`;
3. initialize one uniformly random label per line;
4. run four complete cyclic coordinate-descent sweeps, each with a frozen
   seeded permutation of the line order;
5. at a coordinate, choose the label minimizing the squared norm of the new
   global residual; ties take the lowest label;
6. retain the restart with the smallest final residual norm.

Also record 64 independent uniformly selected one-per-line controls, the
colorful bound parameter `B`, achieved reconstruction MSE, selected labels,
and pool means.  Float64 is used for global sums and solver residuals;
candidate activations may be captured in float32.

## Evidence boundary

The output capture and selection are target-free.  These rows' targets were
opened by the preceding packet experiment, so subsequent target association
is explicitly **broad statistical exploratory on a reused bank**, not a new
holdout.  Timing is diagnostic only.  No FlopScope session or physical row is
in scope.

## Post-seal gates

Let `G_pool(m)` be `(MSE_q0 - MSE_pool(m))`, and let `G_ideal` use the sealed
64-replicate packet mean.  All denominators must be positive.

Oracle A, finite-pool ceiling:

- `m=4`: pooled `G_pool/G_ideal >= 0.80`;
- `m=8`: pooled `G_pool/G_ideal >= 0.90`;
- `m=8`: each family `G_pool/G_ideal >= 0.80`;
- `m=8` pool improves at least `12/16` rows versus q0.

Oracle B, actual-output selection:

- `m=8`: pooled `(MSE_q0-MSE_selected)/G_pool >= 0.90`;
- `m=8`: each family retains at least `0.80` of finite-pool gain;
- `m=8`: selected rule improves at least `12/16` rows versus q0;
- achieved target-free selected-to-pool reconstruction MSE is no worse than
  `0.05` times the median random-control reconstruction MSE;
- the output-oracle result must not rely on target-dependent scaling or
  blending.

Failure of Oracle A kills that candidate-pool size.  Oracle A pass with
Oracle B failure kills the frozen practical solver, not the existence result.
Both pass license only the NNGP/Nystrom feature-surrogate oracle on an
unchanged construction and disjoint network families.

