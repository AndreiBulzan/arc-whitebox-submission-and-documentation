# K129 L24 positive recombination R1 preregistration

Date: 2026-08-05

## Prior-work boundary

The capsule already killed:

- equal-weight, per-basis moment herding at checkpoint 12;
- whole-basis pruning at checkpoint 16 with response-mean calibration;
- endpoint and multidepth equal-weight herding; and
- static positive cubature over input frames.

This screen is narrower and materially different.  It keeps the complete K129
cloud through layer 24, then replaces its 33,024 antipodal trajectory pairs by
a **global nonnegative weighted subset of individual pairs**.  The weights are
chosen without labels to match the complete layer-24 activation mean, or its
mean and diagonal second moment.  Only the reduced positive measure is run
through layers 25--31.  No whole-basis quota is imposed.

The reason for the late checkpoint is structural: the remaining ReLUs are far
more sign-stable than the middle of the network.  Exact preservation of the
layer-24 first two marginal moments is therefore a plausible sufficient
statistic for a large fraction of the suffix, whereas the prior L12/L16 kills
show it is not sufficient in the middle.

## Frozen target-free spelling

Use the exact hash-gated K146/m17 CUDA adapter only to construct the complete
O0/K129 trajectory through layer 24.  For each of the 129 bases choose a fixed,
deterministic set of `q` directions, with a base-dependent cyclic shift.  Fit
nonnegative simplex weights on those fixed candidates by NNLS against either:

- `m1`: pair-averaged layer-24 activation;
- `m2`: the same activation plus pair-averaged squared activation.

The frozen candidates are:

```text
q2_m1, q4_m1, q4_m2, q8_m2, q16_m2
```

They retain respectively 258, 516, 516, 1,032, and 2,064 antipodal pairs
(twice as many propagated rows).  Subsequent energy screens are recomputed
under the fitted positive row weights.  The final estimate is the same
sphere-scaled ReLU mean under those weights.

## Fast gate

Capture predictions target-free on the first two already-designated Full
pilot rows and first two already-designated Generated pilot rows.  Seal all
predictions before opening targets.  Promote only if one fixed candidate has:

- pooled raw-MSE ratio at most `1.12` on each family;
- at least one improved row in each family; and
- finite nonnegative weights summing to one on every row.

The `1.12` raw gate is deliberately conservative: reducing the full-cloud
suffix to at most 4,128 rows removes almost all of the layer-25--31 row-linear
work, so a positive result has ample compute repayment.  A passing 2+2 result
earns a disjoint 8+8 broad screen; it does not earn Mini100 directly.

## Evidence and actions

This is an offline target-free **component** screen.  Any compute saving before
a current-meter implementation is a **projection**.  It creates no estimator
package and performs no upload, submission, or remote action.
