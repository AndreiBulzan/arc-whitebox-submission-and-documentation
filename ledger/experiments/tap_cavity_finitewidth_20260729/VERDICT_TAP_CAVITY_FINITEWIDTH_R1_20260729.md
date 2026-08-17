# Finite-width TAP/cavity lane — R1 verdict

Date: 2026-07-29

Evidence label: **component**. This is a target-open, teacher-forced
layer-30 to layer-31 scout on four already-open Full development MLPs. It is
not a free rollout, broad statistical result, measured whole, physical price,
package, upload, submission, or remote result.

## Verdict

**Kill this cavity state before a Full32 expansion.**

The strongest direct cavity variant reaches `1.2499230e-6` pooled final-mean
MSE, versus `1.3113404e-6` for the exact-source-mean/covariance Gaussian
baseline. That is a real `4.68%` component improvement, but it remains
`10.42x` above the preregistered `1.2e-7` teacher-forced gate.

Giving the method the exact output K3 and asking its cavity mode to supply
only K4 also fails:

| arm | pooled final-mean MSE |
|---|---:|
| exact source mean/cov Gaussian | `1.3113404e-6` |
| best direct cavity | `1.2499230e-6` |
| exact output K3, zero K4 | `1.7669689e-7` |
| exact output K3 + best cavity K4 | `1.7422428e-7` |
| exact output K3+K4 oracle | `2.3537137e-8` |

The cavity K4 therefore recovers only a `1.40%` readout improvement over
exact K3, while the required improvement to the gate is about `32%`. Its
best K4 query correlation is `0.45823`, but relative RMSE is `0.98097`: the
amplitude and geometry needed for the oracle gap are almost entirely absent.
Its implied K3 has the wrong sign geometry (correlation `-0.32790` in the
same strongest raw-weight arm).

Four rows are not population evidence. They are sufficient for this early
falsifier because the best arm misses the gate by an order of magnitude even
with exact teacher source state, and the exact-K3 hybrid remains `45%` above
the gate.

## What was genuinely new

Classic TAP does not directly apply here. The estimator is a feed-forward
DAG and each layer's realised weight matrix is independent of all upstream
activations. There is no recurrent edge or reused coupling through which a
unit perturbs the field that created it, hence no ordinary spin-glass/AMP
self-feedback Onsager term. A formula claiming such a term here would merely
rename the already-killed diagonal cumulant recurrence.

R1 instead tested a lawful, distinct query-conditioned cavity state. For
each downstream weight column `w`, it chose the realised scalar mode

```text
d = w .* Phi(mu / sigma)
t = d^T (z - mu) / sqrt(d^T Sigma d)
```

and evaluated every conditional source ReLU mean analytically:

```text
z_i | t ~ N(mu_i + Cov(z_i,t)t, Var(z_i)-Cov(z_i,t)^2)
g_w(t) = sum_i w_i E[ReLU(z_i) | t].
```

The unexplained projected field was a Gaussian bath with variance

```text
Var(q) - Var_t(g_w(t)).
```

This subtraction is a genuine reaction correction: it prevents the
explicit collective mode from being counted again in the bath. Twelve-node
Gauss-Hermite integration then resummed all powers of that one
output-specific shared gate mode. Unlike Price/tree K3/K4, it never formed a
universal connected tensor; unlike PTCC or endpoint heads, it fitted no
coefficient and used no target at inference.

The negative result is informative: the missing late-layer connected state
is not concentrated in one downstream-linear-response, raw-weight, or
curvature-response Gaussian collective mode. The existing exact-K3/K4
oracle gap is carried by higher-dimensional, non-Gaussian upstream state.
Adding more quadrature nodes cannot repair that state. Reopen only with a
new non-Gaussian factor observable already present in the propagated cloud,
not another Gaussian mean/covariance cavity mode.

## Reproducibility and boundary

```text
source
  screen_query_conditioned_cavity_r1_20260729.py
  526f6f06477f4589b0cd6c89d4109c18c86239123869d3d13f3e4f42ca3de0ed

component receipt
  query_conditioned_cavity_full4_r2_20260729.json
  0cbbfda38dfb910ece66d13b46ae780a527dce71c089e1bf9147b88dac9152cb
```

The scout used zero physical rows, zero FlopScope sessions, zero package or
network actions, and made no deployment, frozen-source, evidence-bank, or
`STATUS.json` changes.
