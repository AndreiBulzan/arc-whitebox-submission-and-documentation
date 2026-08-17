# Preregistration: calibrated bitplane multilevel cloud

Date: 2026-07-28

Evidence boundary: this document fixes a candidate and its kill gates before
opening any new result. It is a **projection**, not a component result,
measured whole, package, or submission. No remote action is authorized.

## Question

Can a large, lawfully packed low-bit particle cloud plus a small nested
full-precision cloud estimate the current full-precision expectation with
20--30% less counted work and no more than the raw-error increase allowed by
an adjusted score of `1.2e-7` (checkpoint) or `1.0e-7` (promotion)?

This is ordinary mixed-precision numerical analysis. It must use only public
participant operations and their literal published costs. A formulation is
killed if it relies on protocol behavior, missing accounting, undefined
overflow, or a discrepancy between local and remote enforcement.

## Fixed estimator family

For nested input directions, let `F` be the ordinary full-precision particle
trajectory and `L` a low-bit trajectory. At every layer:

1. Quantize the low-cloud activation by a fixed affine integer quantizer and
   the current weight matrix by a fixed per-output affine integer quantizer.
2. Evaluate the integer dot products using packed bitplanes.
3. On the nested high-precision subset, also evaluate the literal
   full-precision dot product.
4. Fit a target-blind affine calibration

   `z_full ~= a * z_low + b`

   independently per output coordinate from the nested pairs. Shrink the
   slope toward one with the fixed rule

   `a = (cov + lambda) / (var_low + lambda)`,

   where `lambda = 0.1 * mean(var_low)` within the layer, and clip
   `a` to `[0.5, 2.0]`.
5. Apply that calibration to every low-cloud preactivation and then apply
   ReLU. The nested full cloud continues from its own exact state.

The final estimate is the multilevel control variate

`mean(L_all) + mean(F_nested - L_nested)`.

The first numerical capture must compare, without retuning:

- signed `int4 activation x int4 weight`;
- unsigned-ReLU `int3 activation x signed int4 weight`;
- the same two formats on the first 24 layers with the last eight restored
  to full precision.

Quantizer clipping is fixed at the empirical `0.001` and `0.999` endpoints
of the applicable row/column on the low cloud or weight matrix. Zero-point
and scale are the ordinary affine-quantization values for those endpoints.
The emulator must accumulate integer products without overflow and dequantize
by the exact affine expansion.

## Fixed support scout

The first scout uses nested supports `(K_L, K_H) = (64, 16)` only. Its
purpose is to measure the coupled residual, not to claim a challenge score.
If a format passes the residual gate on both families, a single allocation
step may evaluate `(130, 20)` and the nearest capsule-native structured
supports that preserve the same ratio.

No support, bit width, depth boundary, clipping rule, or calibration
regularizer may be selected from held target error.

## Exact efficiency diagnostic

For the iid analogue with a nested high subset,

`Var(estimator) / Var(F) = (1-r)/K_L + r/K_H`,

where

`r = Var(F-L) / Var(F)`.

If one low-bit trajectory costs `q` dense trajectories, the optimal
continuous allocation has efficiency factor

`(sqrt(q * (1-r)) + sqrt(r))^2`

relative to full precision. This diagnostic does not replace the actual
structured-cloud score.

Every count projection must include quantization, packing, reductions,
calibration, dequantization, correction, ordinary-result materialization,
and residual time. Public cost rates must be source-pinned.

## Data split and gates

The first result must use whole-MLP-disjoint Full and Generated families.
Support/quantizer/calibration choices are target-free. A held score is a
component or broad-statistical result according to its row count; it is not
a measured whole.

Kill the candidate unless all of the following hold:

1. No overflow, nonfinite result, or exact-affine reconstruction mismatch.
2. The coupled residual ratio `r` is below `0.05` on both families for at
   least one fixed format.
3. The literal multilevel final prediction improves the projected adjusted
   score on both families, including row p95 rather than pooled mean alone.
4. Conservative all-in work is at most `0.80` of its same-support dense
   counterpart for the `1.2e-7` checkpoint, or at most `0.70` for direct
   `1.0e-7` promotion.
5. Using the exact score formula and measured raw error, both-family
   projected adjusted error is at most `1.2e-7`; direct promotion requires
   at most `1.0e-7`.

If raw low-bit propagation diverges but calibrated `r < 0.05`, the
multilevel estimator remains alive. If `r >= 0.05` after the preregistered
calibration, do not tune another quantizer on the same held rows.

