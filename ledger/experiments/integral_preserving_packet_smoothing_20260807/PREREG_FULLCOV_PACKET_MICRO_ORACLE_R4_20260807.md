# Preregistration: full-covariance packet micro-oracle R4

Date frozen: 2026-08-07

## Purpose

R3 showed that every marginal/block packet closure, including 256 diagonal
variances, loses the true packet correction immediately at layer 2.  R4 asks
the remaining blocking question: does exact full-covariance Gaussian moment
matching preserve a local packet accurately enough through depth 32?

This is a small target-free component oracle.  It is not a production method:
one 256x256 covariance per Kerdock center is far outside the contest budget.

## Frozen support

Use four networks whose packet predictions are already sealed:

```text
Full1000:     640,641
Generated128: 88,89
```

Select eight unoriented K129 lines deterministically from bases
`0,18,36,54,72,90,108,128`, with within-basis row indices
`0,31,63,95,127,159,191,255`.  Include both orientations, giving 16 packet
centers per network.

The true packet mean uses the unchanged `epsilon=0.20` construction with
4,096 independent Gaussian vectors and their antithetic negatives per center
(8,192 samples per center).  Record layers `1,2,4,8,16,24,32` and both
replicate halves.

## Full-covariance closure

For each center independently, initialize

```text
mean = rho*u
covariance = tau^2 I.
```

At every layer propagate `mean @ W` and `W.T @ covariance @ W`, then apply the
exact mean and bivariate second moment of a multivariate Gaussian passed
through coordinatewise ReLU.  Evaluate the bivariate normal CDF with frozen
8-node Plackett/Legendre quadrature.  No scale, reset, covariance shrinkage,
or target-derived parameter is permitted.

## Metrics and gates

For each checkpoint define the zero-noise central packet prediction and true
packet correction.  Full-covariance correction fidelity is

```text
1 - ||fullcov - true_packet||^2 / ||central - true_packet||^2.
```

The mechanism passes only if all hold:

- layer-1 fidelity `>=0.95`;
- layer-8 fidelity `>=0.85`;
- final-layer fidelity `>=0.75`;
- final correction cosine `>=0.85`;
- final Monte Carlo half-to-half discrepancy is at most 5% of the central-to-
  packet correction energy.

Failure at layer 8 kills recursive Gaussian packet closure as the immediate
bridge.  Passing means the mathematics survives but does not solve production
compression; a separate structured covariance representation would still be
required.  Neither outcome authorizes estimator modification, packaging,
score projection, upload, or submission.

