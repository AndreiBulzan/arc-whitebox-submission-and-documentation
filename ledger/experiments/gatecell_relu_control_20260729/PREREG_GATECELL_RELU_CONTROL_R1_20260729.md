# Nonlinear-pilot gate-cell ReLU control R1

Date: 2026-07-29.

Scope: target-free CUDA component capture followed by a separate post-seal
score on two Full and two Generated development MLPs.  No FlopScope,
physical row, package, upload, submission, or remote action.

## Hypothesis

The mean-gated tangent proxy is killed.  R1 replaces it by literal
nonlinear gate geometry.

Use the 256 positive axes of the first production basis as pilot cells.
Propagate them through the same H1-repaired deep graph.  At pilot row `p`,
backpropagate fixed Hadamard output adjoint `p` through the realized ReLU
masks.  This gives one exact local input gradient `a_p` of a scalar
projection of the actual deep output.  Normalize it and define the
known-mean non-smooth control

```text
g_p(u) = ReLU(u dot a_p),
E_sphere[g_p] = 1 / (sqrt(2*pi) E[chi_256]).
```

All 256 controls are fit on even bases and applied only to odd bases, then
swapped and averaged.  Ridge is fixed at `1e-2` times the mean Gram
diagonal.  The pilot uses weights and inference states only; no target,
identity, family coefficient, or cross-network training occurs.

## Fast gate

```text
Full       4, 104
Generated  4, 68
K          16, 32
```

Promote only if one K reaches pooled candidate/literal final MSE `<=0.50`
in both families and every row ratio is `<=1.25`.  Otherwise kill.

The feature/fit work is bounded by the prior `35B` projection at K146, plus
less than `1B` for the 256-pilot forward/adjoint.  Total incremental
ordinary-real work is projected below `36B`, not a FlopScope receipt.
