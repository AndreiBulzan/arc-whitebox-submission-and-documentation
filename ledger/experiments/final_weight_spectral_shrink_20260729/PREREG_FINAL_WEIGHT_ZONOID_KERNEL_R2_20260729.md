# Final-weight zonoid-kernel R2 preregistration

Date: 2026-07-29

Evidence scope: offline **component** experiment only.  No estimator,
FlopScope, physical row, package, upload, submission, or remote action is
authorized.

R1 shrinks coordinates in the right singular basis of `W31`; it does not
test smoothness of the output observable over final-weight directions.  R2
tests that distinct mechanism.

For a final column `w`, the exact identity

```text
E ReLU(w·h) = 0.5 w·E[h] + 0.5 E|w·h|
```

makes the nonlinear part an even zonoid support function on the sphere.
Normalize the saved endpoint correction by `||w||`, and kernel-smooth its
values at the 256 final-weight directions.  Test exactly two fixed kernels:

1. `absolute_gaussian`: the centered covariance kernel of absolute Gaussian
   projections,

   ```text
   k(c) = [(2/pi)(sqrt(1-c^2) + c asin(c)) - 2/pi] / (1 - 2/pi).
   ```

2. `degree2_even`: `k(c)=c^2`.

For each MLP and kernel, set the ridge without targets.  In normalized
coordinates, estimate

```text
noise = (129*17/146^2) mean((q1-q0)^2)
signal = max(mean(d^2)-noise, 0.05 mean(d^2))
lambda = noise/signal.
```

Use `S=K(K+lambda I)^-1`, map the smoothed correction back by `||w||`, and
fit only one global interpolation scalar `eta in [0,1]` between K146 and
that fixed smoother.

The split and sealing protocol is identical to R1: Full positions `0:60`
fit `eta`, `60:80` select one kernel, `80:100` are untouched within R2, and
Generated predictions are sealed before this stage reads Generated targets.
No alternate ridge, centering, kernel bandwidth, harmonic degree, negative
eta, or retry is permitted.

Promotion requires candidate/baseline raw-MSE ratio `<=0.90` on both the
Full20 test and noise-corrected Generated64.  Otherwise hard-kill these two
even-kernel spellings.  Generated is an established development family, not
a globally virgin bank.

