# R98: exact odd-linear endpoint correction preregistration

**Date:** 2026-08-10  
**Evidence boundary:** target-free candidate seal, then official-Mini100
public selection and untouched-holdout transfer  
**Parent:** exact packaged R92

## Question

For every scalar preactivation,

`ReLU(z) = (z + |z|) / 2`.

Consequently, the final spherical expectation is exactly the sum of a linear
signed-preactivation mean and an even absolute-value mean.  The current R92
endpoint injects the analytic signed-mean proxy by shifting every sampled
preactivation before ReLU.  That also changes the even absolute component,
although the oracle localization says almost all remaining error is in the
signed mean.

R98 tests the algebraically cleaner operation: leave R92's complete ReLU
cloud and its absolute component unchanged, and add only a fraction of the
remaining linear signed-mean discrepancy.

Let `m` be the uniform mean of R90's 129 final signed-preactivation basis
means, `p` its already-computed analytic proxy, `lambda_bf=0.015` R92's
globally recentered BF shift, and `s=1.000025` its output scale.  The frozen
direction is

`d = 0.5 * s * (1 - lambda_bf) * (p - m)`.

Candidates are

`y_beta = max(0, y_R92 + beta * d)`.

This is not another inside-ReLU lambda scan: it changes only the exact odd
linear component and cannot disturb the sampled absolute component.

## Frozen beta grid

`{-0.02,-0.015,-0.01,-0.0075,-0.005,-0.003,-0.002,-0.001,0,
  0.001,0.002,0.003,0.004,0.005,0.006,0.0075,0.01,0.0125,
  0.015,0.02,0.03,0.05,0.075,0.1,0.2,0.5,1}`.

The target-free builder must hash-pin and seal the exact R92 prediction
capture and the existing target-free R90 basis-statistic capture before a
separate scorer opens Mini100 targets.

## Selection and gates

Select one beta solely by minimum mean raw MSE on official Mini100 public
rows `0:50`.  Apply it unchanged to holdout rows `50:100`.

Promote only if all hold:

- public raw reduction at least `0.25%`;
- holdout raw reduction at least `0.25%`;
- pooled raw reduction at least `0.50%`;
- at least `50/100` rows improve;
- all outputs are finite and nonnegative.

The scorer also reports continuous target-aware scalar capacity separately
on public, holdout and pooled rows.  That is diagnostic capacity, never a
production coefficient.

A pass licenses a package and exact physical/Mini gate.  A failure kills this
direct odd-linear spelling only.  No upload or submission is authorized.
