# Coupled harmonic residual cloud R1 — preregistration

Date: 2026-07-29

Evidence scope: offline **component** scout only. This experiment may read
weights from the already-used Full rows `{0,1}` and Generated rows `{0,1}`.
Predictions must be frozen and hashed before a separate process opens those
four already-used development targets. It may not run FlopScope, a physical
benchmark, package an estimator, upload, submit, or perform a remote action.

## New mechanism

This is not the previously killed independent Gaussian/full-covariance
control variate. It extracts an exact, weight-conditioned first spherical
harmonic from the *live residual cloud at every layer*.

For each complete antipodal Kerdock basis and each preactivation coordinate,
write the paired values as

```text
e(u) = (z(u) + z(-u))/2
o(u) = (z(u) - z(-u))/2.
```

The fixed surrogate is

```text
g(u) = mean_u e(u) + o(u).
```

On one complete orthonormal basis, `o(u)` is exactly the sampled value of a
linear spherical harmonic `b·u`; its spherical variance is exactly
`mean_u o(u)^2`. Therefore the uniform-sphere first and second moments of
`ReLU(g)` are available analytically from the beta-law marginal, without
fitting targets or constructing `b`.

At every layer, candidate moments are

```text
M1 = exact_sphere_mean(ReLU(g))
     + sample_mean(ReLU(z) - ReLU(g))

M2 = exact_sphere_mean(ReLU(g)^2)
     + sample_mean(ReLU(z)^2 - ReLU(g)^2).
```

The live basis cloud is then affinely matched to these radial first/second
moments before the next weight matrix. Thus the analytic closure and cloud
are coupled through the state at every layer. There is no endpoint blend,
learned coefficient, target-fitted scalar, independent Price state, or
square-root orientation choice.

## Frozen experiment

1. Unit identities:
   - at layer zero, where preactivation is exactly odd linear, residuals must
     vanish within float tolerance;
   - analytic moments must satisfy the physical first/second-moment bounds;
   - fixed zero residual must reproduce the analytic beta-law moments.
2. Target-free smoke: Full row `0`, total `K=4`, direct and candidate finite.
3. Stage A: total `K=24`, Full `{0,1}` and Generated `{0,1}`, with exactly
   two methods:
   - `direct_h1`;
   - `basis_harmonic_cv_all`.
4. Freeze the complete all-layer prediction arrays in NPZ and record their
   SHA-256 before scoring.
5. A separate scorer may then open the four development targets.

No coefficient or schedule sweep is permitted. The correction is applied at
all 32 layers, with coefficient exactly one. H1 is included through the same
formula rather than a special target-derived repair.

## Sequential gate

The primary viability bar is final raw MSE `<=3.0e-7` on Full and Generated
separately at a projected current-meter total `<=50B`.

Stage A is killed immediately unless both conditions hold:

- candidate/direct final-MSE ratio `<=0.50` on each family; and
- candidate final MSE `<=1.2e-6` on each family.

Only if both pass may one prespecified Stage B be run at total `K=40` on the
same four rows. Stage B must meet `<=3.0e-7` on both families, or show a
two-point degradation law that conservatively projects `<=3.0e-7` before
`50B`. No additional K, coefficient, layer schedule, clipping, support,
family-specific rule, or retry follows.

## Compute projection rule

The literal K40 propagation is conservatively priced by linear scaling from
the current K162 counted graph. The harmonic correction adds only
elementwise paired sums/differences, reductions, beta-law scalar operations,
and affine matching. It does not require an extra dense cloud transform.
Before promotion, the report must show a conservative arithmetic ceiling
`<=50B`; this remains a **projection**, not a FlopScope receipt.

## Decision

- **Keep** only if the frozen two-family accuracy gate and projected compute
  gate pass.
- Otherwise **kill** this exact harmonic residual-cloud recurrence. A later
  reopen would require a genuinely higher/mixed harmonic observable, not a
  coefficient or schedule search.

