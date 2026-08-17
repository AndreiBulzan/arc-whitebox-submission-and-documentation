# Terminal teacher-forced paired-secant QDEIM R1 preregistration

Date: 2026-08-07

## Singular question

Assume the exact layer-31 packet secant is already known.  Can a basis learned
from 512 distinct-line pilot pairs compress that 512-dimensional paired
secant to rank 16, recover it from 20 QDEIM/gappy coordinates, and preserve at
least 80--85% of the exact finite-pool packet correction after applying the
actual full-width final layer?

This is a deliberately overpowered necessary-condition oracle for the
proposed Canonical-Anchored Paired-Secant QDEIM shadow.  It removes all
32-layer rollout error.  Failure therefore kills the proposed rank-16/q=20
representation before basis construction, canonical-margin plumbing, audit
correction, or contest implementation.

## Prior-art boundary

Capsule searches covered `QDEIM`, `DEIM`, `gappy POD`, hyper-reduction,
paired secants, POD shadows, and trajectory-local reduced models.  No prior
receipt uses canonical-anchored packet secants or selected-neuron gappy
reconstruction.  The complete-state depth R3 receipt is not a reduced model:
it fits a dense output projection independently at each exact depth.  This R1
is novel in the capsule.

## Fixed population

Use the same independent-noise `m=8` packet pool, 33,024 K129 lines, Full rows
`640..647`, Generated rows `88..95`, packet radius, and normalization as the
sealed packet and complete-depth receipts.  Open no benchmark targets.

For line `i`, propagate the canonical anchor pair `(+rho*u_i,-rho*u_i)` and
every candidate pair `(y_is,-y_is)` exactly through layer 31.  Define the
ordered paired activation secant

```text
delta31_is = concat(h31(y_is) - h31(rho*u_i),
                    h31(-y_is) - h31(-rho*u_i)).
```

Positive homogeneity supplies the anchor from a literal canonical trajectory,
but this oracle propagates it explicitly as an association control.

Apply the supplied final weight matrix and ReLU exactly for both the true and
reconstructed secants.  Packet radial normalization is unchanged.  Associate
the exact candidate pool and canonical q0 means with the sealed output-oracle
capture to maximum absolute error `2e-6`.

## Frozen pilot design

Use `PILOT_SEED = 2026080741`.  Draw a fixed random permutation of the 129
Kerdock bases and a fixed random permutation of the 256 lines inside every
basis.  Visit four rounds in that basis order, taking the next unused line
from each basis until exactly 512 distinct lines have been selected.  This
gives 3 or 4 lines per basis.  Draw one uniform candidate label in `0..7` for
each selected line from the same generator.  Use the identical pilot units
for every network.

No other candidate on a pilot line is exposed to basis construction.  Every
reported population fidelity substitutes the exact 512 pilot outputs, as the
proposed production estimator would, but all nonpilot units are reconstructed
from the learned basis.

## Frozen bases

Let `X` be the `512 state coordinates x 512 pilot units` layer-31 secant
matrix and let `Y` be the `256 final pair-average coordinates x 512 pilot
units` exact final-output secant matrix.

Compute an exact float64 SVD for this capacity oracle, which is strictly more
favourable than the proposed production randomized range finder.

Record held-unit state-energy capture for ordinary POD ranks

```text
8, 12, 16, 20, 24.
```

The controlling hybrid rank-16 basis is frozen as:

1. the first 12 left POD modes of `X`;
2. the top eight output scores `T = diag(sigma_Y[:8]) V_Y[:8]^T`;
3. `C = X T^T`;
4. remove the projection of `C` onto the 12 POD modes;
5. append the first four left singular modes of that residual and
   orthonormalize.

Also retain ordinary POD-16 as a diagnostic.

## Frozen projection and QDEIM spells

Evaluate three terminal reconstructions:

- `POD16_PROJECT`: exact coefficients in the ordinary POD-16 basis;
- `HYBRID16_PROJECT`: exact coefficients in the hybrid rank-16 basis;
- `HYBRID16_Q20`: exact values of 20 selected layer-31 activation-secants,
  reconstructed by oversampled gappy POD.

For `HYBRID16_Q20`, run pivoted QR on `U_hybrid^T` with deterministic SciPy
tie behaviour and use the first 20 returned coordinate pivots.  Set

```text
D = U_hybrid @ pinv(U_hybrid[pivots, :])       # 512 x 20.
```

The teacher-forced sampled state is exactly `delta31_is[pivots]`; no reduced
rollout or approximate margin enters.  Map each reconstruction through the
actual block-diagonal final weight and all 512 final ReLUs around the exact
canonical preactivation.  Do not use a low-rank final linear decoder.

## Target-free metric and gates

For a network, let `pool` be the exact normalized `m=8` packet-pool mean,
`q0` the exact normalized canonical mean, and `pool_hat` the reconstructed
mean after exact pilot substitution.  Define packet-correction fidelity

```text
fidelity = 1 - mean((pool_hat - pool)^2) / mean((pool - q0)^2).
```

Pool across networks and output coordinates by summing numerator and
denominator before taking the ratio.  This measures reproduction of the
target-free packet correction; it is not a benchmark raw-MSE score.

`HYBRID16_Q20` passes only if all hold:

- pooled correction fidelity `>=0.85`;
- Full and Generated family fidelity each `>=0.80`;
- at least 12/16 rows have fidelity `>=0.70`;
- maximum `cond(U_hybrid[pivots,:]) <= 1e3`;
- every association, pilot-disjointness, finite-value, and replay control
  passes.

An exact-hybrid projection fidelity below `0.90` or Q20 fidelity below `0.85`
kills the rank-16/q=20 paired-secant shadow.  Do not try memory or local-basis
extensions: those are licensed only when terminal teacher forcing passes and
a later free rollout identifies a specific failure.

A pass licenses only a second target-free oracle that builds bases at every
layer, records exact canonical selected margins, and compares teacher-forced
versus free QDEIM rollout.  It does not license an estimator or a score
projection.

## Production-feasibility caveat

The external cost estimate treats the 20 exact canonical margins per line and
layer as nearly free.  The current R87/R40 chassis compresses and reconstructs
parts of the propagated state; arbitrary QDEIM-selected full-width margins
have not yet been shown to be owned exactly by that graph.  Even after an R1
capacity pass, a source-level owner-map and FlopScope census must price those
margins.  This oracle makes no `10--12B` production claim.

## Evidence boundary

Target-free **component** evidence on the reused Full8/Generated8 bank.  It
uses exact off-frame trajectories only as an oracle.  No target, estimator
logic, FlopScope session, physical row, package, upload, or submission is
authorized.  Controller access is serialized through
`runtime/.benchmark_lane.lock`.
