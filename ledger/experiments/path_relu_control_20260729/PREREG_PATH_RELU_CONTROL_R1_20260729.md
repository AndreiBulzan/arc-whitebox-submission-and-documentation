# Mean-gated path-ReLU control functional R1

Date: 2026-07-29.

Scope: target-free CUDA component capture followed by a separate post-seal
score on two Full and two Generated development MLPs.  No physical,
FlopScope, package, upload, submission, or remote action.

## Distinct mechanism

Polynomial controls are annihilated by the balanced design and the H1
quadratic scout was neutral.  R1 instead uses a non-smooth control with a
known integral.

For each MLP, a weights-only diagonal Gaussian pass supplies gate
probabilities.  Its mean-gated input-to-final Jacobian has 256 realized
columns `a_j`.  Normalize those columns and define

```text
g_j(u) = ReLU(u dot a_j),   u uniform on the unit sphere.
```

Every exact control mean is

```text
E[g_j] = ||a_j|| / (sqrt(2*pi) E[chi_256]).
```

The literal H1-repaired cloud output is regressed on all 256 controls using
even bases and corrected only on odd bases, then vice versa and averaged.
The ridge is fixed at `1e-2` times the mean Gram diagonal.  This is a
cross-fitted control functional, not a learned cross-network correction,
empirical interpretation of the repaired cloud, or target fit.

## Frozen gate

```text
Full       4, 104
Generated  4, 68
K          16, 32
```

Promote only if one K has pooled candidate/literal MSE `<=0.50` in both
families and every row ratio `<=1.25`.  Otherwise kill immediately.

At K146 the feature projection, Gram, and feature-to-output contractions
are projected below `35B` ordinary real operations.  A successful K146
hybrid would therefore remain below the lawful `272B` cap; the count is a
projection, not a FlopScope receipt.
