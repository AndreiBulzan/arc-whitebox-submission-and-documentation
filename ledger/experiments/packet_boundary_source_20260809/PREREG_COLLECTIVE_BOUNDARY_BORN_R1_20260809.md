# Preregistration: collective packet-boundary Born oracle R1

Date frozen: 2026-08-09

## Question

The full-covariance packet closure is an extremely accurate teacher, but its
covariance state is unaffordable.  Before trying another covariance
approximation, test whether the *mean correction itself* is well represented
as local Gaussian ReLU boundary sources transported collectively through the
canonical supplied network.

This is target-free with respect to benchmark expectations.  It uses only the
already sealed four-network, sixteen-centre full-covariance packet component
oracle and the corresponding supplied weights.  It is a component diagnostic,
not an estimator or score claim.

## Frozen construction

For every centre, propagate the exact full-covariance Gaussian closure.  At
layer `l`, let `c_l` be the canonical preactivation, `s_l^2` the exact teacher
marginal preactivation variance, and `D_l = 1[c_l > 0]`.  Define the local
canonical-margin boundary source

```text
q_l = E[max(c_l + s_l G, 0)] - max(c_l, 0),  G ~ N(0,1).
```

Propagate one collective correction state

```text
delta_l = D_l * (delta_{l-1} @ W_l) + q_l,
delta_0 = 0.
```

Also record two diagnostics:

1. the same canonical-Jacobian recurrence with the boundary source evaluated
   at the full-covariance preactivation mean rather than the canonical margin;
2. an algebraic exact-source checksum whose source is defined as the exact
   full-covariance mean residual after the canonical linear term.

Decompose the canonical-margin result into all 32 transported layer sources.

## Frozen evidence and metrics

Use the exact support and normalization from
`fullcov_packet_micro_oracle_r4_targetfree_20260807.npz`: two Full1000 rows,
two Generated128 rows, sixteen oriented centres, and checkpoints
`1,2,4,8,16,24,32`.

At every checkpoint report unscaled pointwise and centre-averaged correction:

- cosine;
- fidelity `1 - MSE(prediction - teacher) / MSE(teacher)`;
- family-specific centre-averaged fidelity.

The primary gate is the unscaled canonical-margin construction at layer 32.
It passes as a representation lead only if:

- centre-averaged fidelity is at least 0.70;
- centre-averaged cosine is at least 0.90;
- neither family has centre-averaged fidelity below 0.50;
- the exact-source checksum has maximum absolute error below `1e-10`.

Failure kills only the first-Born collective source representation; it does
not contradict the full-covariance closure.  Passing authorizes a new oracle
that judges cheap variance representations by final centre-averaged source
correction, rather than by per-centre covariance reconstruction.

