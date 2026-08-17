# Coupled zonal-H2 residual cloud R1 — preregistration

Date: 2026-07-29

Evidence scope: offline **component** scout only. The experiment may read
weights from the already-used Full rows `{0,1}` and Generated rows `{0,1}`.
It may also read the already sealed target-free direct-K24 predictions from
the coupled first-harmonic R1 capture. The new predictions must be frozen and
hashed before a separate invocation opens those four already-used development
targets. It may not run FlopScope, a physical/timed benchmark, package an
estimator, upload, submit, or perform a remote action.

## One new observable

The killed first-harmonic cloud proved that the unresolved signal is in the
basis-varying even component or a mixed higher angular harmonic. This screen
adds exactly one inference-available component: the rank-one zonal degree-two
harmonic aligned with each live basis/output's odd harmonic.

For each complete antipodal basis, output coordinate, and preactivation,
write

```text
e_i = [z(u_i) + z(-u_i)] / 2
o_i = [z(u_i) - z(-u_i)] / 2
m   = mean_i e_i
r²  = sum_i o_i²
x_i = o_i / r
h2_i = x_i² - 1/d,       d = 256.
```

Because the directions `u_i` form an orthonormal basis, the vector `o_i` is
the sampled trace of a unique linear harmonic `b·u`, `r=||b||`, and
`x=bhat·u`. The fixed second-harmonic coefficient is the target-free
least-squares projection

```text
c = sum_i [(e_i-m) h2_i] / sum_i h2_i².
```

No clipping, shrinkage, fitted coefficient, target statistic, layer
schedule, family rule, or retry is permitted. The surrogate is

```text
g(u) = m + r x + c (x² - 1/d).
```

For uniform `u` on the unit sphere, `x` has the known beta marginal
proportional to `(1-x²)^((d-3)/2)`. The first two moments of `ReLU(g)` are
computed exactly by splitting `[-1,1]` at the real roots of the quadratic
and evaluating incomplete-beta polynomial moments. The coupled moments are

```text
M1 = sphere_mean(ReLU(g))
     + sample_mean(ReLU(z) - ReLU(g))

M2 = sphere_mean(ReLU(g)²)
     + sample_mean(ReLU(z)² - ReLU(g)²).
```

The live basis cloud is affinely matched to these radial first/second moments
before the next weight matrix. Thus the rank-one H2 state changes every
subsequent layer. This is not a final-boundary blend and does not duplicate
the killed marginal Gaussian/full-covariance controls.

## Frozen screen

1. Unit identities:
   - on a purely odd linear first preactivation, `c=0` and the surrogate
     reproduces the sampled preactivation within float tolerance;
   - analytic affine (`c=0`) moments agree with the existing exact
     affine-sphere formula;
   - analytic quadratic moments agree with an independent dense numerical
     integration;
   - returned first/second moments are finite and physical.
2. Target-free K4 smoke on Full row `0`.
3. Exactly one Stage-A acquisition: total `K=24`, Full `{0,1}` and Generated
   `{0,1}`, with exactly:
   - sealed `direct_h1`;
   - `basis_zonal_h2_cv_all`.
4. Freeze the complete all-layer predictions in one NPZ and record its
   SHA-256.
5. Only then may a separate scorer open the four development targets.

No K40 continuation, coefficient sweep, schedule sweep, alternative axis,
or follow-up variant is part of R1.

## Kill/keep gate

The candidate must satisfy all of:

- final candidate/direct MSE ratio `<=0.50` on Full and Generated
  separately;
- final candidate MSE `<=1.2e-6` on Full and Generated separately;
- every trajectory finite;
- conservative current-FlopScope-0.9.1 count projection `<60B`.

If any condition fails, kill this exact zonal-H2 recurrence. A reopen would
require a genuinely multi-axis or cross-basis connected state, not another
coefficient or layer schedule.

## Count projection rule

The K24 dense propagation is conservatively obtained by separating the fixed
`1.31B` closure from the current K162 counted graph and scaling the remaining
cloud work linearly by `24/162`. The H2 projection then adds a deliberately
conservative ceiling for:

- antipodal even/odd decomposition;
- reductions for `m`, `r`, and `c`;
- evaluation of `h2` and the quadratic surrogate;
- exact scalar incomplete-beta moments;
- residual first/second moments and affine matching.

This arithmetic is a **projection**, not a FlopScope receipt. R1 is retained
only if the total remains below `60B`.

