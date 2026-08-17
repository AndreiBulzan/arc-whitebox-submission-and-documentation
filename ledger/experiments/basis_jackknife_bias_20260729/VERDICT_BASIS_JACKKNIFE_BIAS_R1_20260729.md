# Verdict — K146 delete-one-basis jackknife bias R1

Status: **killed target-free**.

Evidence label: **component**.  No targets, GPU, FlopScope, physical row,
package, network, upload, or remote action were used.

## Result

The proposed classical bias correction is an exact-arithmetic identity for
the actual K146 final readout.

Both production arm weights are uniform:

```text
O0: 1 / 129
O1: 1 / 17
```

At lambda zero, all nonlinear ReLU work has already happened inside each
captured per-basis endpoint.  The remaining arm statistic is its arithmetic
mean.  Therefore

```text
mean_i q(S \ {i}) = q(S)
q_J = K q(S) - (K - 1) mean_i q(S \ {i}) = q(S).
```

The production gamma path does not change this conclusion:
`weighted_final - equal_final` is zero under uniform weights, so gamma
multiplies zero.  Any observed effect is reduction roundoff, not a statistical
bias estimate.

## Literal replay

The production-compatible gamma reduction and every leave-one readout were
replayed on the existing actual K146 endpoint capture:

```text
family       rows   full/control max abs   jackknife/full max abs
Full           4        8.88e-16                 3.97e-13
Generated      4        8.88e-16                 4.62e-13
```

The corresponding jackknife/full RMSEs were `4.80e-14` and `4.29e-14`.
These are only float64 reduction-association effects.  The preregistered
`<=0.90` raw-ratio gate therefore fails at the target-independent
exact-arithmetic ratio `1.0`; opening development targets cannot rescue the
mechanism and was skipped.

## Economics

A literal endpoint-side implementation is projected below `0.1B` counted
operations, and a sufficient-statistics implementation below `0.01B`, both
inside the `2B` ceiling.  This is a projection, not a measured whole.  Cost is
irrelevant because the correction contains no accuracy signal.

## Evidence

- Target-free receipt:
  `basis_jackknife_bias_r1_targetfree_20260729.json`
- Frozen prediction/replay arrays:
  `basis_jackknife_bias_r1_targetfree_20260729.npz`
- Reproducer:
  `capture_basis_jackknife_bias_r1_targetfree_20260729.py`

This result closes only the delete-one jackknife applied after the actual
per-basis final endpoints.  It does not close a delete-one rerun of the entire
nonlinear propagation graph, which would be a different and far more
expensive estimator.

