# Coupled closure-axis rank-8 H2 cloud R1 — preregistration

Date: 2026-07-29

Evidence scope: offline **component** scout only. It may read weights from
the already-used Full rows `{0,1}` and Generated rows `{0,1}`, plus the
sealed target-free direct-K24 predictions. New predictions must be frozen
and hashed before a separate invocation opens those four development
targets. It may not run FlopScope, a physical/timed benchmark, package an
estimator, upload, submit, or perform a remote action.

## Why this is a lawful reopen

The one-axis zonal H2 recurrence improved pooled K24 error on both families
but explained only `4.26%--5.66%` of late even energy and reversed on one
row in each family. Its verdict permits one genuinely multi-axis,
cross-basis connected state.

This R1 is not the killed endpoint projected-chaos correction, the
teacher-forced PTCC readout, or the first-order adjoint K3/K4 recurrence.
It never adds an endpoint cumulant. It extracts one rank-8 connected
quadratic state from the live K24 cloud at every ReLU boundary, analytically
anchors its quadrature error, and moment-matches the state before the next
weight matrix.

## Fixed axes

For each MLP, first run the fixed order-eight Price/Gaussian closure using
weights only. At layer `l`, take the top eight eigenvectors of the closure's
preactivation covariance `C_l`, ordered by increasing eigenvalue within the
selected top-eight block and with each sign fixed by its largest-magnitude
entry. Rank is exactly eight at every layer. There is no alternate rank,
random range, downstream schedule, target-conditioned choice, or retry.

These closure-covariance axes `P_l in R^(256x8)` are inference-available and
weight-conditioned. Covariance selects only a coordinate system; all
sign-bearing H2 coefficients are fitted independently from the live cloud.

## Cross-basis connected H2 state

For live antipodal preactivations, define

```text
e_bi = [z_bi(u) + z_bi(-u)] / 2
o_bi = [z_bi(u) - z_bi(-u)] / 2
m_b  = mean_i e_bi.
```

Project the complete odd vector onto the fixed axes:

```text
x_bi = o_bi P_l.
```

Whiten the pooled `K*256` samples by their target-free `8x8` covariance so
that `mean(x x^T)=I/256`. The fixed symmetric H2 feature map contains all
`36` rank-eight components:

```text
diag:     x_a² - 1/256
offdiag:  2 x_a x_b, a<b.
```

One global cross-basis coefficient matrix per output is obtained from the
normal equations with the single fixed ridge

```text
lambda = 1e-8 * trace(F^T F) / 36.
```

The fit target is `e_bi-m_b`; no target label, endpoint error, fitted blend,
coefficient sweep, clipping rule, or family-specific value is used. The
resulting surrogate is

```text
g_bi(±) = m_b ± o_bi + q_bi,
q = F theta.
```

Under the fixed Gaussian closure `x~N(0,I/256)`, the connected quadratic has
known zero mean and variance

```text
Var(q_j) = 2 trace(Theta_j²) / 256².
```

Odd and even components have zero mixed covariance by antipodal parity.
The analytic anchor is therefore the exact marginal Gaussian ReLU
first/second moment with mean `m_b` and variance

```text
mean_i(o_bi²) + Var(q).
```

Layer zero is the unique purely linear boundary: its fitted H2 core is
identically zero and the existing exact affine-sphere H1 match is used
instead of introducing a Gaussian approximation where the spherical
integral is already known exactly. Layers `1..31` use the rank-eight rule
above without a schedule choice.

At every layer, use

```text
M1 = GaussianMean(g)
     + sample_mean[ReLU(z)-ReLU(g)]

M2 = GaussianSecond(g)
     + sample_mean[ReLU(z)²-ReLU(g)²],
```

then affinely match each live basis to the resulting physical radial moments
before the next matrix. The Gaussian law is the analytic control anchor, not
a claim that the live H2 state is Gaussian.

## Frozen screen

1. Unit checks:
   - H1/purely odd input produces zero H2 coefficient;
   - the 36-feature reconstruction and symmetric-core variance formula agree
     with direct matrix evaluation;
   - whitening reaches `I/256`;
   - Gaussian moments and all states are finite and physical.
2. Target-free K4 smoke on Full row `0`.
3. Exactly one K24 acquisition on Full `{0,1}` and Generated `{0,1}`:
   - sealed `direct_h1`;
   - `closure_h2_rank8_cv_all`.
4. Freeze complete all-layer predictions and record their SHA-256.
5. Only then may a separate scorer open the four targets.

No other rank, axes, ridge, coefficient, layer schedule, K, or correction is
authorized in this experiment.

## Fixed continuation gate

Continue this class only if all hold:

- pooled final candidate/direct MSE ratio `<=0.50` on Full;
- pooled final candidate/direct MSE ratio `<=0.50` on Generated;
- every individual-row candidate/direct final-MSE ratio `<=1.50`;
- every trajectory finite;
- conservative current-FlopScope-0.9.1 count projection `<=60B`.

Failure kills this exact rank-eight cross-basis H2 control. No sweep follows.

## Count projection

As in the preceding K24 screens, separate the fixed `1.31B` closure from the
current K162 steady counted graph and scale remaining cloud work by
`24/162`. Add a deliberately conservative `30B` ceiling covering the Price
closure and covariance axes, odd-axis projection, 36-feature construction,
normal-equation fit, H2 evaluation, analytic Gaussian moments, residual
moments, and affine matching.

This gives a predeclared total projection of `54.753301624B`. It is a
**projection**, not a FlopScope receipt.
