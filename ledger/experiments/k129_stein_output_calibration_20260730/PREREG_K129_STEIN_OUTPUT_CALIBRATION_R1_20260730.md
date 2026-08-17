# K129 output-column Stein/Hermite calibration R1 — preregistration

Date: 2026-07-30.

Evidence scope: one target-free CUDA **component** acquisition followed by
one fixed post-seal Full/Generated development falsifier. Count is a
**projection**. This lane does not run FlopScope, edit an estimator, build a
package, upload, submit, or take any remote action.

## Exact identities

Let `H` be the random layer-30 hidden activation under a standard Gaussian
network input, let `w ~ N(0, tau^2 I)` be one independent He-Gaussian final
weight column, and define

```text
t(w) = E_H ReLU(w' H).
```

Gaussian conditioning and Stein's identity give

```text
E_w[t(w)]
  = tau / sqrt(2*pi) * E_H ||H||

E_w[w_i t(w)]
  = tau^2 / 2 * E_H H_i

E_w[(w_i w_j - tau^2 delta_ij) t(w)]
  = tau^3 / sqrt(2*pi) * E_H[H_i H_j / ||H||].
```

Consequently, for

```text
R = E_H[H H' / ||H||],
```

the exact degree-two output-column Hermite component is

```text
g2_R(w)
  = (w' R w - tau^2 trace(R)) / (2*tau*sqrt(2*pi)).
```

The first two identities follow directly from the conditional centered
Gaussian ReLU mean and first-order Stein integration. The final identity
uses the distributional Hessian
`d_ij ReLU(w'H) = H_i H_j delta(w'H)`.

## Duplication audit

- `final_mean_signal_scout_20260729` closes scalar/common-output endpoint
  corrections.
- `k129_output_ensemble_moment_anchor_20260730` used only the degree-zero
  identity above and was killed target-free because its coefficient-one
  shift violated output nonnegativity.
- R21's existing `lambda=0.0075` correction already shrinks the degree-one
  signed preactivation mean toward its analytic closure proxy.
- `learned_weight_residual_20260729` fitted output-equivariant endpoint and
  weight features and reversed on disjoint Full and Generated. It did not
  impose this exact degree-two population identity or use a target-free
  shrinkage coefficient.
- `deep_stein_control_foray_20260729` used input-sphere derivative controls,
  not output-column Gaussian identities.

R1 therefore tests only the distinct degree-two channel. It does not reopen
the closed degree-zero, degree-one, fitted-residual, or input-Stein lanes.

## Fixed estimator

The baseline is the literal K129/O0 R21 prediction with endpoint
`lambda=0.0075`.

For the actual final hidden cloud, interpret the existing final-176 plus
omitted-sample-mean restoration as a 200-coordinate effective cloud:
selected coordinates retain each node value and omitted coordinates equal
their shared sample mean. Its exact angular `R` query is formed from the
already-computed final preactivation array and effective node norms. The
Gaussian-input radial factor is the exact width-256 chi mean.

The target matrix uses the already-computed analytic closure state at layer
30. If its mean and covariance are `mu_G, Sigma_G`, set

```text
M_G = Sigma_G + mu_G mu_G'
R_G = M_G / sqrt(trace(M_G)).
```

This is the fixed high-dimensional norm-concentration approximation to
`E[H H'/||H||]`; no target or fitted coefficient enters it.

Let `g2_b` be the exact empirical degree-two query separately for each of
the 129 complete Kerdock bases, `g2_Q = mean_b g2_b`, and `g2_G` the closure
query. The sole target-free Ledoit-Wolf shrinkage scalar is

```text
noise = sum_j Var_b(g2_bj) / 129
distance = sum_j (g2_Gj - g2_Qj)^2
alpha = clip(noise / max(distance, tiny), 0, 1)
delta = alpha * (g2_G - g2_Q)
candidate = max(0, baseline + delta).
```

There is no grid, fitted coefficient, row router, post-score choice, or
output scaling. Projection to the nonnegative orthant cannot increase
squared error against the challenge's nonnegative target.

## Fixed tiny gate

Use these positions in the already frozen endpoint-grid order:

```text
Full positions       76:80 -> indices [157, 158, 160, 162]
Generated positions  60:64 -> indices [121, 123, 124, 126]
```

Baseline and candidate arrays, indices, shrinkage scalars, and diagnostics
must be serialized and hashed before either target or label-noise member is
opened.

The mechanism passes only if both hold:

```text
Full pooled raw-MSE ratio                    <= 0.970
Generated noise-corrected pooled ratio       <= 0.970
```

Any family reversal or either ratio above `0.970` kills this exact mechanism
immediately. No coefficient, normalization, clipping rule, row set, or
correction surface may change after scoring.

## Economics boundary

The empirical query reuses the final preactivation array. Effective hidden
norms, basis reductions, one `200 x 200` closure matrix, and the quadratic
queries are projected below `0.1B` counted operations. The hard ceiling for
this lane is `1B`. This is not a physical receipt.
