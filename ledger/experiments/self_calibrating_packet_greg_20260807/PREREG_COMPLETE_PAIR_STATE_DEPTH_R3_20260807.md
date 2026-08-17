# Complete packet-pair state depth R3 preregistration

Date: 2026-08-07

## Question

At what exact network depth does the complete propagated state of one packet
candidate pair first contain enough *quenched* information to linearly remove
the finite-pool selection error?

Packet GREG R1 killed the universal and first-layer-conditioned NNGP spans.
Sparse-layer-two R2 then found only `12.751%` explained variance with complete
`abs(z1)` plus 32 exact layer-two probes, against the frozen `70%` gate.  The
candidate-output deviations themselves remain low-dimensional (`73.69%` of
their energy is in eight output modes), so the unresolved issue is depth of
information rather than output dimension.

This round is an oracle diagnostic.  It does not implement a production
estimator or price progressive propagation.

## Prior-art boundary

Capsule searches covered depth-of-information, complete candidate pair
states, progressive packet pruning, shadow networks, full layer-two packet
states, and pair-state control variates.  No receipt measures the optimal
within-line output projection of the complete off-frame packet-pair state as
a function of exact depth.  Earlier H1/H2 repairs concern the incumbent frame;
R2 observes only 32 selected layer-two neurons.  This is therefore a new
observable inside the capsule.

## Fixed population and association

Use the identical `m=8`, 33,024-line independent-noise packet pool,
normalization, Full rows `640..647`, and Generated rows `88..95` used by packet
GREG R1 and sparse-layer-two R2.  Recreate all candidate outputs target-free
and associate each finite-pool mean with the sealed output-oracle receipt to
maximum absolute error `2e-6`.

For every candidate `y`, propagate both `y` and `-y` through the supplied
network.  At exact activation depths

```text
1, 2, 4, 8, 12, 16, 24, 32
```

retain the complete ordered pair state

```text
PAIR_FULL_d = concat(h_d(y), h_d(-y))       # 512 features.
```

The order is deterministic: the first half always comes from the stored
candidate `y`, the second from its antipode.  This deliberately obeys the
external warning that `abs(z1)` alone discards branch membership.  Although
the final pair output is invariant under swapping both branches, the oracle is
allowed the complete runtime-observable pair state and therefore gives the
strongest linear capacity ceiling for a GREG control.

Each feature is centred across the eight candidates within its Kerdock line
and scaled by pooled RMS.  A column with RMS at most `1e-14` remains exactly
zero and is recorded; it is never replaced.  Maximum absolute line-sum drift
must be at most `2e-10`.

## Fixed oracle

For each network and depth, fit the exact minimum-norm network-specific
multi-output projection from the complete centred pair state `X_d` to the
centred final pair outputs `V`:

```text
B_star = pinv(X_d' X_d) X_d' V

delta_random = sum ||V||^2 / (256 * M^2 * 8)
delta_d      = sum ||V - X_d B_star||^2 / (256 * M^2 * 8)
R2_d         = 1 - delta_d / delta_random.
```

The eigencut and float32 contraction / float64 solve are inherited unchanged
from packet GREG R1.  No candidate labels, targets, held scores, or selected
production coefficients enter.

Controls:

- depth 32 must give pooled `R2 >= 0.9999` and maximum row residual
  `<=1e-10`, because the pair mean at the final layer is a fixed linear
  function of the complete pair state;
- `delta_random` must agree across all depths to relative `1e-10`;
- the sealed pool association and centring checks above must pass.

## Gates and interpretation

A depth is *capacity-positive* only if all hold:

- pooled `delta_d <= 4.0e-8`;
- Full and Generated `delta_d <= 4.8402381038e-8` separately;
- pooled `R2_d >= 0.70`;
- at least 12/16 rows have `R2_d >= 0.60`.

The earliest capacity-positive depth controls the next action:

- depth 1--2: license a production-shaped complete-H1/H2 GREG feature and
  cross-fit;
- depth 4: license a separately priced progressive `8 -> 4 -> 2 -> 1`
  prototype and a width-8/16 shadow comparison;
- depth 8 or later: do **not** build progressive full propagation routinely;
  use the curve only to define a thin actual-weight shadow-network target;
- no pass before depth 24: treat one-frame self-calibration as requiring
  near-answer information and retire routine GREG/progressive work unless a
  shadow oracle independently clears the same residual gate.

This round does not authorize cross-fitting, pilots, a shadow network,
progressive selection, estimator edits, production-compute claims, physical
rows, packages, uploads, or submissions.

## Evidence boundary

Target-free **component** evidence on a reused 16-network research bank.  All
benchmark-controller access is serialized through
`runtime/.benchmark_lane.lock`.
