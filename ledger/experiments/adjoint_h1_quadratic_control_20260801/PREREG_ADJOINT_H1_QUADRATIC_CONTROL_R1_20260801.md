# Adjoint H1 quadratic control R1

Date: 2026-08-01

Evidence class: **component**. This is a 16-row public-development
falsifier. It is not broad statistical evidence, a physical receipt, a
package, or a remote candidate.

## Mechanism

The production H1 repair fixes every first-layer marginal mean and variance,
but not the off-diagonal second moments. The complete first-layer matrix

```text
E[h1 h1^T]
```

is nevertheless known exactly from the spherical arc-cosine kernel. For each
MLP, estimate the mean-gated downstream Jacobian from `h1` to the final
preactivation and post-ReLU output. Take its leading left singular vectors
`v_r`, and form one exactly calibrated non-smooth quadratic control per
direction:

```text
g_r(h1) = (v_r^T h1)^2,
E[g_r]  = v_r^T E[h1 h1^T] v_r.
```

The controls are averaged separately inside each of the 129 complete Kerdock
frames. Minimum-norm calibration weights then correct the already-captured
per-frame final endpoints. This is not the previous diagonal-`z2^2` GREG:
the new controls contain downstream-selected off-diagonal H1 covariance.

The downstream gate fractions use a fixed target-free 4,096-row balanced
probe selected from the repaired H1 cloud. Candidate families are final-pre,
final-post, and their concatenation, with `p in {1,2,4,8,16}` leading
directions and fixed damping `eta in {0.25,0.5,1.0}`.

## Gate

Promote to a disjoint Full/Generated screen only if a fixed candidate has:

1. pooled MSE ratio `<= 0.92` against the uniform K129 endpoint;
2. at least `10/16` rows improved;
3. maximum row ratio `<= 1.75`;
4. finite controls, weights, and predictions.

Failure kills this exact adjoint-quadratic calibration spelling. It does not
kill other exactly-known nonlinear controls or a nonperturbative connected
state.
