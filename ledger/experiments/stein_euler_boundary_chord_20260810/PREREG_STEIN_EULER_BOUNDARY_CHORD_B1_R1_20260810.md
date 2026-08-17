# Stein--Euler boundary chord B1/R1 preregistration

Date: 2026-08-10

## Question

Can a collective Gaussian chord cancel the piecewise-linear interior of the
supplied bias-free ReLU network and estimate its Gaussian mean with lower
variance per propagated row than ordinary Gaussian/antithetic sampling?

For the final-output map `f`, positive homogeneity and independent
`X,V ~ N(0,I)` give, for `s=sqrt(1+h^2)`,

```
E[f(X+hV)+f(X-hV)-2f(X)] = 2(s-1) E[f(X)].
```

Equivalently, with `U+=(X+hV)/s`, `U-=(X-hV)/s`,

```
C = f(X)
O = (f(U+)+f(U-))/2
T_h = C + s/(s-1) (O-C).
```

Every marginal is standard Gaussian, so `C + alpha*(O-C)` is unbiased for
every fixed `alpha`.  The theorem coefficient `s/(s-1)` annihilates the
interior homogeneous-linear contribution whenever the chord remains in one
activation cell.  A second arm estimates the variance-minimizing alpha from
three independent banks and applies it to the held fourth bank, rotating the
held bank and averaging the four predictions.  This cross-fit is exactly
unbiased because each fitted coefficient is independent of its held bank.

## Evidence boundary and frozen design

- Capture is target-free and reads only `weights.npy` from the official
  Mini100 archive.
- Frozen screen rows are Mini100 public `0..9` and holdout `50..59`.
- Four banks of 1,024 chords are used.  A production candidate at one frozen
  `h` therefore propagates exactly `3*4096=12,288` rows.
- Frozen `h` grid: `0.25, 0.5, 1, 2, 4, 8, 16`.
- Float32 literal-network propagation; float64 sufficient statistics and
  reductions; TF32 disabled.
- Fixed NumPy PCG64DXSM seeds, shared across networks.
- Cross-fitted scalar and coordinate-wise coefficients are clipped to
  `[0,64]`.  No benchmark target enters their construction.

## Comparators

At the same 12,288 propagated rows:

1. theorem boundary coefficient;
2. cross-fitted scalar coefficient;
3. cross-fitted coordinate-wise coefficient;
4. ordinary symmetric triplet coefficient `alpha=2/3`;
5. one independent Gaussian plus one antithetic pair.

The projected count is the existing K24/12,288-path projection, about
24.967B before a capsule-native physical implementation.  This is a
**projection**, not a physical receipt.

## Post-seal protocol

After the prediction archive and receipt are sealed:

- report every fixed arm on public rows and holdout rows separately;
- select at most one `(method,h)` on public rows and report its frozen holdout
  result;
- compare against the 1e-6 raw-MSE floor threshold and the 8e-7 stretch
  threshold;
- report per-bank disagreement and coefficient stability.

Continue to an all-Mini100 capture only if the frozen holdout result is below
`1.0e-6`, or if it gives at least a 2x raw-MSE reduction over the same-cost
Gaussian/antithetic comparator on both halves.  Prefer `<=8.0e-7`.

Kill the family if the theorem arm and both cross-fitted arms fail to improve
the same-cost comparator materially, or if any apparent public gain reverses
on the frozen holdout rows.

## Prior-art preflight

- Exact conditional line Rao--Blackwellization was killed by the cost of
  enumerating and integrating all line breakpoints.  This oracle evaluates
  three ordinary trajectories and never enumerates a breakpoint.
- Scalar gate/source sampling was killed by loss of downstream signed
  cancellation.  Here every chord produces a complete 256-output vector and
  all crossed boundaries are summed before averaging.
- Gaussian closure, shared-covariance Price/Wick, KLPQ continuation and
  final-gate heat controls replace or model the state.  This estimator is an
  exact finite-sample identity for the realized full network.
- Output-sensitive conic atlases killed small explicit polyhedral atlases but
  explicitly left collective analytic boundary-flux estimators open.

No FlopScope session, physical row, package, upload, submission, or remote
action is authorized by this preregistration.
