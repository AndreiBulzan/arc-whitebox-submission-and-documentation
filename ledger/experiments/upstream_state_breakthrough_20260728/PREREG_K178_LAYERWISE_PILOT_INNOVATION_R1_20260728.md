# K178 layerwise missing-O1 pilot innovation R1

Date: 2026-07-28

Evidence: a target-free offline capture, frozen before a separate 4+4
post-seal component score. No FlopScope physical row, package, upload,
submission, or remote action is authorized.

## Fixed estimator

Keep the production total-K=178 O0 and O1 predictions. For the O1 arm,
construct one dense offline control with all 512 signed rows of the frozen
49-basis support and one cheap pilot with 13 positive rows per missing S109
basis plus their exact antithetic negatives.

The 13 positive row IDs are fixed as:

```text
floor(j*256/13), j=0,...,12
```

At every layer, before ReLU:

```text
delta_z = mean(z_pilot over S109) - mean(z_pilot over S49)
z_S49   = z_S49 + delta_z
z_miss  = z_miss + delta_z
```

The S49 pilot is a view of the paid complete S49 cloud. The missing pilot is
propagated through the same accumulated shifts. Both paths then apply the
literal ReLU. Coefficient is exactly one. No fitted affine map, feature
regression, clipping, support routing, or target-dependent value is allowed.

Let `delta_q1` be the final radial-scaled mean of the coupled S49 cloud minus
the corresponding unmodified dense S49 control. Attach it to the frozen
production K178 O1 prediction:

```text
q1_hat = q1_K178 + delta_q1
candidate = (129*q0_K178 + 109*q1_hat) / 238.
```

This is hidden-unit permutation equivariant: every statistic is a coordinate
mean, a shared coordinatewise shift, matrix multiplication by the supplied
weights, or coordinatewise ReLU.

## Fixed rows

```text
Full       0, 1, 2, 3
Generated  2, 4, 5, 6
```

During capture, only weights, the frozen supports/bases, and sealed K178
q0/q1 predictions may be opened. The K238 teacher and challenge targets are
forbidden until the candidate archive and manifest are complete.

## Gates

After sealing:

1. candidate/K178 discrepancy MSE to the frozen K238 teacher is `<=0.70` on
   both families;
2. candidate/K178 final-layer target MSE is `<=0.82` on both families;
3. at least two of four rows improve in each family;
4. no row MSE ratio exceeds `2.0`;
5. all outputs and intermediate summaries are finite.

Any failure kills this exact 13-pair, coefficient-one, every-layer
preactivation-innovation spelling. No row-count, clipping, coefficient, or
checkpoint scan is authorized.

## Count projection

For 62 missing bases, 26 particles, width/depth 256/32:

```text
missing dense products                    6,748,012,544
missing ReLU                                13,205,504
shift full S49 cloud                       205,520,896
shift missing pilot                         13,205,504
pilot gathers                               20,447,232
S49/reference reductions                    33,652,736
small combine allowance                         32,768
------------------------------------------------------
projected increment                       7,034,077,184
```

This is an arithmetic projection. Movement, integration, request count,
residual, and wall are unmeasured.
