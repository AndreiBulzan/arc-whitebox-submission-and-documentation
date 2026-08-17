# Gate-aware Schur-pair selector R1

Date: 2026-08-05

## Mechanism

The R84/R85 selector propagates squared-weight relevance with a uniform
factor 0.5 at every layer.  Because the vector is renormalized after each
step, that scalar cancels exactly; the selector therefore uses no actual
downstream ReLU gate information.

Propagate diagonal Gaussian means and variances forward through the fixed
MLP, compute each neuron's marginal gate probability `Phi(mu/sigma)`, and
use the exact squared-Jacobian expectation spelling

```text
s_{l-1} = W_l^2 @ (p_l * s_l)
```

in the backward selector.  Keep the already transferred gamma=0.50 exponent,
polar frame, pair ordering, one-per-pair rule, K129 propagation, and R84 scale
unchanged.

## Prior-art and ceiling

Queries covered gate-aware/gate-probability pullbacks, observability,
sensitivity, Schur, frame, rotation, CDF, and covariance selectors.  The
nearest artifact is the killed sensitivity-directed width fold, which used
gate-aware observability to delete neurons; no artifact uses gate probability
to choose Schur invariant planes.  Outcome: **novel in capsule**.

The added forward diagonal closure is about two matrix-vector products per
layer, only tens of millions of billed operations.  A 0.1% raw gain can repay
it; a discrete change in plane choices gives a materially larger possible
ceiling.  This is not the killed covariance-fusion frame: no state is folded
or reconstructed, and the full K129 trajectory remains intact.

## Sealed pilot

- First 16 rows of the existing sealed Full100 and Generated128 Schur banks.
- Official Mini100 rows 0..15.
- Fixed candidate: gate-aware pullback with gamma=0.50 only.
- Controls: q0 and the sealed R84 gamma=0.50 prediction.

Promote to complete Full100/Generated128/Mini100 only if candidate/R84 raw
ratio is <=1.0 on every family, pooled <=0.995, and at least 8/16 rows improve
in every family.  No tuning, alternate gate exponent, or target-conditioned
choice is permitted after opening the pilot targets.

