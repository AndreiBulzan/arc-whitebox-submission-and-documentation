# Preregistration: stochastic covariance-factor packet closure R5

Date frozen: 2026-08-07

## Motivation

The exact full-covariance local closure retained 99.78% of packet-correction
energy at layer 32, while every diagonal/block state failed at layer 2.  The
remaining production-shaped possibility is to carry a stochastic factor of
the cross-neuron covariance alongside each packet mean.

R5 tests capacity only.  No FlopScope graph, package, remote score, or wall
claim is authorized.

## Frozen recurrence

For each K129 packet center retain a mean `m` and `r` covariance probes `t_s`.
Initialize

```text
m = rho*u
t_s = tau * xi_s,
```

where every `xi_s` coordinate is an independent Rademacher sign.  At a linear
layer, propagate `m @ W` and every `t_s @ W`.  Estimate each marginal
preactivation variance by the mean of `t_s^2` over probes.

Apply the exact univariate Gaussian ReLU mean/variance.  With gate probability
`p=Phi(m/sigma)`, update each factor by

```text
t'_s = p*t_s + sqrt(post_var - p^2*pre_var) * eta_s,
```

using independent Rademacher `eta_s`.  In expectation this is the established
linearized full-covariance ReLU update: off-diagonal covariance is multiplied
by gate probabilities while the exact marginal variance is restored on the
diagonal.

Use all 66,048 centers, float32 state/arithmetic, float64 layer reductions,
the R2 `epsilon=0.20` packet parameters, and layers `1,2,4,8,16,24,32`.

Frozen probe/seed grid:

```text
r=1: seeds 0..7
r=2: seeds 0..3
r=4: seeds 0..1
r=8: seed 0
base seed 2026080715
```

No result may select a scale, blend, radius, sign construction, or recurrence.

## Evidence and gates

Predictions are sealed target-free before scoring.  These are the already
scored R2 packet rows, so target association is exploratory rather than a new
confirmation split.

The production-relevant `r=1` arm passes only if:

- median-seed pooled raw reduction versus q0 is at least 25%;
- median-seed Full and Generated reductions are each at least 15%;
- at least 6 of 8 seeds improve pooled raw MSE;
- median-seed final correction cosine with the sealed packet correction is at
  least 0.40;
- median-seed final correction fidelity is positive.

If `r=1` fails but a larger `r` retains at least 45% of the true packet target
gain and has positive correction fidelity, report a covariance-rank signal but
do not call it production-compatible.  Otherwise kill the stochastic-factor
family.

A production-relevant pass authorizes only a counted-operation graph and wall
canary.  It does not authorize an estimator change, package, score projection,
upload, or submission.

