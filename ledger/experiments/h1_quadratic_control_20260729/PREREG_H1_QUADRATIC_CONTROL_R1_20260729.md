# Query-directed H1 quadratic control R1

Date: 2026-07-29

Scope: target-free CUDA component capture followed by a separate post-seal
score on two Full and two Generated development MLPs.  No FlopScope
session, physical row, package, upload, submission, or remote action.

## Hypothesis

The production design already removes the first-layer marginal mean and
variance error, but it does not integrate every cross-neuron first-layer
covariance direction exactly.  Those covariance errors can be observed
without interpreting the repaired deep cloud as an empirical probability
law.

For each MLP:

1. propagate the literal balanced Kerdock cloud with the exact H1 affine
   repair and otherwise literal dense ReLU layers;
2. choose the 32 H1 coordinates with largest squared-weight downstream
   observability, computed from weights only;
3. form 16 fixed Hadamard projections in that subspace;
4. use the squared centered projections as controls.  Their exact
   unit-sphere expectations are computed from the analytic bivariate
   first-layer arc-cosine kernel;
5. fit the final-output control coefficient on even bases and apply it only
   to odd bases, then swap and average.

The ridge is fixed to `1e-3` times the mean feature Gram diagonal.  There is
no target, seed, row identity, fitted cross-network coefficient, or
family-specific choice.

## Frozen rows and gate

```text
Full       4, 104
Generated  4, 68
K          16, 32
```

Promote only if one K clears all of:

- pooled final MSE ratio `candidate / literal <= 0.50` in both families;
- maximum individual-row ratio `<= 1.25` in both families;
- finite predictions and exact target-free/post-seal separation.

Otherwise kill this control-functional spelling immediately.

## Cost boundary

At production K146, the additional dense arithmetic is projected below
`1B` ordinary real operations: one H1 covariance Gram, 16 feature
projections, and the two cross-fitted `G.T @ Y` contractions.  It does not
add a network propagation.  This is an arithmetic projection, not a
FlopScope receipt.
