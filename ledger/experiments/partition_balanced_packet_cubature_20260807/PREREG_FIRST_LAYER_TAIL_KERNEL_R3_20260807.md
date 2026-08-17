# Preregistration: first-layer-conditioned tail-kernel selector R3

Date frozen: 2026-08-07

## Blocking question

R1 proved that one candidate per Kerdock line has ample output-space
capacity.  R2 proved that a universal rank-64/128 input-NNGP landmark slice
does not predict the required cancellation.  R3 asks:

> Does conditioning on the supplied network's exact first ReLU layer make a
> depth-31 NNGP tail kernel sufficiently aligned to recover at least 70% of
> the ideal packet gain with one selected antipodal frame?

This is the expert-prescribed next discriminator.  Passing licenses only an
unchanged disjoint-family numerical test and a separately measured compute
audit.  It does not license estimator implementation, a score projection,
packaging, upload, or submission.

## Prior-art and reopening boundary

The closest capsule negative is the 2026-07-27 H1 gate-co-occupancy kernel.
It herded 33 whole bases toward an S109 gate Gram using only binary
coactivation counts.  It did not use packet candidates, post-ReLU
magnitudes, within-line feature centring, partition-constrained selection,
or a depth-31 tail kernel.  Its basis-support failure therefore does not
control R3.

The R2 kill does control every network-independent rank-64/128 input-NNGP
landmark spelling.  R3 reopens the selector only with a materially new
observable: each candidate's exact positive and negative first-layer
activation vectors under the supplied `W0`.

Outcome: **materially new weight-conditioned observable**.

## Target ceiling

On the reused R1 bank, the `m=4` finite packet pool retained `96.1285%` of
the ideal packet gain and actual-output selection retained `99.9159%` of the
pool gain.  Hence the unavailable-information ceiling comfortably exceeds
the required 25% raw-improvement research gate.  R3 is justified as a
capacity falsifier; no production price is inferred from it.

## Frozen packet construction

Use R1 labels `0..3` only:

```text
M:             33,024 unoriented K129 lines
m:             4 candidates per line
epsilon:       0.20
rho:           cos(0.20)
tau:           sin(0.20)/sqrt(256)
noise seed:    2026080713
```

For candidate `y`, compute one exact first-layer preactivation `z=y@W0` and

```text
hplus(y)  = ReLU(z)
hminus(y) = ReLU(-z).
```

The two states are the exact first-layer states of the selected point and
its antipode; no second matrix product is needed.

## Frozen conditional kernel

Let `K31(a,b)` be the norm-scaled normalized He-ReLU NNGP kernel composed 31
times.  For candidate-pair objects define

```text
kW(y,z) = 0.25 * [
    K31(hplus(y),  hplus(z))  + K31(hplus(y),  hminus(z))
  + K31(hminus(y), hplus(z))  + K31(hminus(y), hminus(z))
].
```

This is the NNGP covariance of the antipodal pair-average after conditioning
on the exact supplied first layer and annealing only the remaining 31-layer
tail.  It is target-free and invariant to permutations of the first hidden
neurons.

Use the same difference-landmark construction as R2, with the fixed line
permutation from `default_rng(2026080739)` and

```text
a_j = j mod 4
b_j = (a_j + 1 + floor(j/4) mod 3) mod 4.
```

Evaluate the conditional difference Gram in float64, retain the specified
positive Nyström modes, whiten, and centre the four candidate features
separately within every line.

The 31-fold scalar-kernel lookup must agree with the literal recurrence to
maximum absolute error `<=2e-9`.  Require Gram symmetry error `<=2e-10` and
no eigenvalue below `-2e-8` times the largest positive eigenvalue.

## Frozen variants and solver

```text
C4P:  p=128, r=64   primary production-shaped spelling
C4D1: p=128, r=128  rank diagnostic
C4D2: p=256, r=128  landmark diagnostic
C4D3: p=256, r=256  high-rank capacity diagnostic
```

For each network and variant, use four restarts and four complete cyclic
coordinate-descent sweeps.  Restart seed is

```text
202608075100 + 1009*restart + 31*network_position + p + r.
```

Ties choose the lowest label.  The best represented residual norm wins.
No supplied tail weight, final prediction, target, R1 output-oracle choice,
or target-derived scalar may enter selection.

After the choices are frozen in memory, propagate only each chosen
`hplus/hminus` pair through the actual remaining layers `W1..W31`, apply the
already sealed packet radial normalization, and write predictions before
opening targets.

## Frozen rows and evidence boundary

Use the already-opened diagnostic rows:

```text
Full1000:      640..647
Generated128:  88..95
```

The target-free capture may compare selected output to the sealed finite-pool
mean and independent-selection reconstruction controls.  Targets are opened
only by the post-seal scorer.  The resulting association is **broad
statistical exploratory on a reused bank**.  Timing is diagnostic only; no
FlopScope or physical row is in scope.

## Frozen gates

The primary C4P passes only if all hold:

- pooled ideal packet gain retention `>=0.70`;
- Full and Generated retention each `>=0.60`;
- at least `12/16` rows improve versus q0;
- geometric-mean actual selected-to-pool reconstruction error is `<=0.30`
  times the R1 median independent one-per-line error;
- no target-dependent scaling, blending, radius, rank, or variant choice.

A diagnostic variant meeting the same gates establishes capacity only and
requires an unchanged disjoint test; it cannot be promoted from this reused
bank.  If all four miss the target-free reconstruction gate, kill the
first-layer-conditioned Nyström family before target scoring.  If C4D3 is
strong while C4P fails, the mechanism is likely too high-rank for the
contest; do not silently price the diagnostic as production-shaped.

