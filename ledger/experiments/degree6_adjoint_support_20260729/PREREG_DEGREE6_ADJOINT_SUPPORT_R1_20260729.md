# Weight-conditioned degree-6 adjoint support R1

Date: 2026-07-29

Evidence scope: target-free support capture followed by a 16+16 endpoint
atlas **component** screen.  No estimator propagation, FlopScope session,
physical row, package, upload, submission, or remote action is permitted.

## Frozen selector

Rows are Full `0..15` and Generated `0..15`.  Orientation 0 remains the
complete 129-basis arm.  R1 selects exactly 33 nonzero orientation-1 bases
independently for each MLP, using weights only.

Let `v_i` be the normalized column `i` of `W0`.  For a Kerdock basis `B`,
use the exact degree-6 zonal harmonic, up to an irrelevant common scale,

```text
H6(t) =
    t^6
  - 15/(d+8) t^4
  + 45/((d+6)(d+8)) t^2
  - 15/((d+4)(d+6)(d+8)).
```

Construct a target-free downstream response from post-layer-0 neurons to
the final signed preactivation:

```text
R = W1 diag(p1) W2 diag(p2) ... W30 diag(p30) W31,
```

where each `p_l` is the Gaussian moment-closure gate probability computed
from the realized weights.  Scalar-normalize `R` after each layer; this does
not change support selection.

Keep the 32 source neurons with largest
`||W0[:,i]|| * ||R[i,:]||`.  Their proxy degree-6 basis endpoint is

```text
Q_B = sum_i ||W0[:,i]|| mean_{b in B} H6(b·v_i) R[i,:].
```

Starting from the complete O0 defect `129 * mean_B Q_B`, greedily add 33
distinct O1 bases from identities `1..128`, each time minimizing the squared
Euclidean norm of the combined proxy defect.  There is no coefficient,
target fit, endpoint access, identity prior, retry, or alternative support
size.

## Gate

The separate scorer compares the selected support against the frozen current
m33 support on exact signed-final-preactivation atlas targets.  Promotion
requires, independently on Full16 and Generated16:

- pooled MSE ratio `<=0.75`;
- row-ratio p95 `<=1.50`;
- at least 60% of rows improved;
- finite predictions and exactly 33 distinct nonzero bases.

A pass licenses one broad atlas replay with the same frozen rule.  A failure
kills this selector; no rank/top-neuron/support-size tuning is allowed.

Projected selector arithmetic is approximately `1.1B` ordinary operations
per MLP (mean-gated response plus two-orientation top-32 harmonic
projections), before FlopScope/call/residual measurement.
