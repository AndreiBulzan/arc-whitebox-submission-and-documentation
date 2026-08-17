# Final-weight spectral shrink R1 preregistration

Date: 2026-07-29

Evidence scope: offline **component** experiment only.  No estimator,
FlopScope, physical row, package, upload, submission, or remote action is
authorized.

## Question

Can the saved K146 correction from its marginal Gaussian endpoint proxy be
denoised in the canonical output basis of the final weight matrix?

For every MLP, let

```text
Y = frozen K146 prediction
G = saved per-arm Gaussian endpoint readout
d = Y - G
W = W31 = U diag(s) V.T
c = d V
```

The endpoint-grid capture supplies the structurally shared K146 `q0`.
Reconstruct `q1` from the frozen literal blend and define the arm-disagreement
noise probe `n = q1 - q0`, with coefficients `a = n V`.  The fixed
two-sample variance conversion is

```text
nu = 129 * 17 / 146**2.
```

This is an inference-time target-free observable.  Full targets are used
only to fit one global shrink strength and select among the three shapes
below.

## Fixed split and protocol

- Full capture positions `0:60`: train the one scalar strength.
- Full capture positions `60:80`: select exactly one shape.
- Full capture positions `80:100`: untouched Full test.
- Generated64 targets remain unopened until the shape, scalar, Full-test
  predictions, and Generated predictions have been serialized and hashed.
- The fresh Full confirmation bank at indices `>=200` remains unopened.

For a prespecified shape `r`, form `h = (r * c) V.T` and

```text
Y_candidate = Y - eta h.
```

Fit `eta = clip(<Y-T,h>/<h,h>, 0, 1)` once on Full60.  Select the lowest
Full20-development MSE, breaking exact ties in the order listed here:

1. `low_sv_ridge`: shrink low-singular modes with
   `r = 1/(s^2/mean(s^2) + 0.25)`, normalized to maximum one.
2. `high_sv_ridge`: shrink high-singular modes with
   `r = t/(t + 0.25)`, `t=s^2/mean(s^2)`, normalized to maximum one.
3. `arm_wiener_8band`: in each consecutive 32-mode singular-value band,
   set `r = clip(nu * sum(a^2) / sum(c^2), 0, 1)`.

No extra rank, ridge, floor, sign, family-specific coefficient, or retry is
permitted.

## Hard gate

After selection, open the untouched Full20 and Generated64 targets once.
Promotion requires both:

```text
Full20 candidate raw MSE / baseline raw MSE              <= 0.90
Generated64 noise-corrected MSE ratio                     <= 0.90
```

Otherwise stop this spelling.  Generated observed-target ratio, fixed
halves, row-loss summaries, and hashes are descriptive only and cannot
select another spelling.

