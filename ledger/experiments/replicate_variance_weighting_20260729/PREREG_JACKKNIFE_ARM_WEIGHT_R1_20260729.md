# Within-arm jackknife weighting R1 preregistration

Date: 2026-07-29

## Claim and boundary

This is one target-free, cache-only **component** falsifier of the
still-open within-arm replicate-variance observable.  It tests whether the
separate per-basis signed endpoints already produced by the K162 geometry
can improve the final blend of its complete O0 arm and fixed m33 O1 arm.

No challenge target, exact signed target, prior endpoint error, arm
disagreement, family label, learned coefficient, FlopScope session,
physical/timed row, package, upload, submission, or remote action may be
used to form a prediction.

## Frozen inputs and rows

Read only:

- `indices.npy` and `final_basis_premean.npy` from the pinned Full200 and
  Generated128 eight-orientation atlases;
- `support_033` from the pinned lower-K support freeze.

Use cache positions `0..11` in Full and positions `0..11` in Generated.
The rule is identical in both families.  Each selected input slice and
prediction array must be hash-recorded.  Predictions must be written and
the capture process must exit before a separate scorer may open
`target_final_premean.npy`.

## Exactly one rule

For every row and output coordinate independently, let `X0` be the 129
per-basis final signed endpoints in orientation 0 and `X1` the 33 endpoints
in orientation 1 selected by the frozen `support_033`.

Define arm means `m0`, `m1` and unbiased per-basis variances `s0^2`, `s1^2`
(`ddof=1`).  The ordinary delete-one jackknife variance estimate of each
arm mean is

```text
v0 = s0^2 / 129
v1 = s1^2 / 33
```

and the candidate is the inverse-variance blend

```text
w0 = v1 / (v0 + v1)
w1 = v0 / (v0 + v1)
candidate = w0*m0 + w1*m1.
```

If `v0+v1` is exactly zero, use the fixed K162 weight `w0=129/162` for that
coordinate.  There is no clipping, shrinkage, scalar/trace alternative,
covariance correction, block-size choice, or second variant.

The baseline is the literal equal-particle K162 blend

```text
baseline = (129*m0 + 33*m1) / 162.
```

## Post-seal score and hard gate

After prediction sealing, score against the corresponding exact signed
target.  Report rowwise and pooled signed-preactivation MSE separately for
Full and Generated.

Continue this mechanism only if both families independently satisfy:

- pooled candidate/baseline MSE ratio `<= 0.90`;
- row-ratio p95 `<= 1.10`;
- row-ratio maximum `<= 1.50`; and
- at least `55%` of rows improve.

Any nonfinite value, shape/support/hash drift, premature target access, or
failed condition kills this exact rule.  A pass would license only a broader
component gate; it would not license a physical row or deployment claim.

