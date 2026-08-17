# Preregistration: block/diagonal Gaussian-packet closure R3

Date frozen: 2026-08-07

## Question

Can a production-shaped marginal Gaussian closure reproduce a useful fraction
of the fresh-row true packet gain without carrying packet covariances?

The R2 true packet oracle reduced final raw MSE by 53.57% on fresh Full8 plus
Generated8 rows.  That authorizes this closure scout.  R3 is still ordinary
offline component work; it is not a contest estimator or compute receipt.

## Frozen states

Use the exact R2 packet parameters and the same 66,048 K129 centers.  For each
packet retain its full 256-vector mean and one variance scalar per fixed
contiguous coordinate group.  Test these group counts without tuning:

```text
b = 1, 4, 8, 16, 32, 64, 256.
```

All groups are equal-size contiguous blocks.  `b=256` is the diagonal-
covariance ceiling of this family; it is not a full-covariance closure.

At every layer, for weight matrix `W`:

```text
pre_mean = mean @ W
pre_var[k] = sum_r q[r] * sum_{j in group r} W[j,k]^2
```

Apply the exact univariate Gaussian ReLU mean and variance, then replace each
group's variances by their arithmetic mean.  Layer predictions are the mean
over packet centers, multiplied by the unchanged Gaussian radial factor and
the exact packet normalization.  Use float32 state/arithmetic and float64
terminal reductions, matching the literal packet capture's numerical regime.

Record layers `1,2,4,8,16,24,32`.  No coefficient, scale, grouping, radius,
or shrinkage fit is permitted.

## Seal and evidence limitation

The closure capture reads only weights and the already sealed target-free R2
packet artifact.  It seals every variant prediction before its scorer reads
targets.  The R2 rows' targets have already been opened by the packet scorer,
so target results here are exploratory association, not a fresh confirmation
split.

The target-free primary fidelity at each layer is

```text
1 - ||closure - packet||^2 / ||q0 - packet||^2.
```

The target-open retained gain is

```text
(MSE(q0,target) - MSE(closure,target)) /
(MSE(q0,target) - MSE(packet,target)).
```

## Frozen gates

Two outcomes are distinguished.

`pass_block_packet_closure` requires some `b<=16` to satisfy all of:

- pooled final raw-MSE reduction versus q0 at least `25%`;
- Full and Generated reductions each at least `15%`;
- retained true-packet target gain at least `45%`;
- target-free final correction cosine at least `0.50`;
- target-free final correction fidelity greater than zero.

If no `b<=16` passes but `b=256` has at least 25% pooled raw reduction and
positive target-free fidelity, report `diagonal_only_packet_signal`; this
keeps covariance-compression research alive but rejects the proposed cheap
block state.

Otherwise report `kill_block_and_diagonal_packet_closure`.  Failure does not
mathematically kill a full-covariance packet closure, but it makes that route
unlikely to be contest-compatible.

A pass still authorizes only a fresh-row confirmation and an honest
instrumented cost model.  It does not authorize estimator modification,
packaging, score projection, upload, or submission.

