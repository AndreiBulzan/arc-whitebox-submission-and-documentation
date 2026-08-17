# K129 paired polar endpoint MLMC R1

Date: 2026-08-05

Evidence sought: target-free **component** reconstruction and old-pilot
selection, followed (only after the fixed pilot gate passes) by a sealed
**broad statistical** official-Mini100 regression score.  Any compute or
remote score remains a **projection** until an exact current-meter whole is
built.  No package, upload, submission, or remote action is authorized.

## Question

The confirmed two-trajectory estimator is

```text
y* = y_q0 + 0.5 (y_polar - y_q0).
```

Both complete frames contain the same 129 labelled Kerdock bases.  If
`p_b` and `q_b` are their repaired final per-basis endpoints, then

```text
y_polar - y_q0 = mean_b (p_b - q_b).
```

R1 estimates this mean from a fixed sparse set of the *paired differences*
`d_b = p_b - q_b`, while retaining the already-paid complete q0 arm.  This
is a multilevel/control-variate spelling of the polar correction, not a
sparse approximation of the polar endpoint itself.

## Prior-art boundary

Blocking searches covered `paired basis`, `frame delta`, `endpoint
difference`, `same-index`, `quartet`, `coupling`, `MLMC`, and the exact polar
endpoint call sites.

Nearest controlled work:

- `k129_polar_sparse_arm_20260804` fitted features
  `polar_endpoint_b - global_q0_prediction`; it did not subtract the matched
  `q0_endpoint_b` and therefore retained q0 basis noise in every feature.
- `k129_paired_delta_lowrank_20260804` recursively SVD-compressed the full
  particle-state delta between q0 and right-Gram frames at every layer; it
  did not estimate the final paired endpoint mean.
- the complete-frame coupling/coreset negatives show that four-frame
  cancellation is not reproduced by corresponding atoms in general.  They
  do not test the already selected q0/polar *difference* as a control
  variate.

Outcome: **materially new observable**.  Failure closes sparse matched
q0/polar endpoint MLMC; it does not reopen generic atom coresets or
trajectory low-rank projection.

## Frozen pilot rule

Use only already sealed per-basis endpoint captures on the overlapping
Full16 and Generated16 rows.

1. Associate the mean q0 and polar endpoints with their sealed complete-frame
   predictions.
2. Form all 129 paired endpoint differences.
3. Fit one nested support by pooled target-free orthogonal matching pursuit
   against the complete mean difference.  At every step use one global
   coefficient vector shared by both families and every output coordinate.
4. Evaluate only support sizes `{8, 16, 24, 32, 48, 64}` and the fixed output
   coefficient `0.5`.
5. Price size `m` conservatively as
   `C(m) = 146.1403492625B + 0.999B*m`, preserving the prior sparse-arm model
   and a 5B fixed frame allowance.
6. Select the size minimizing the worse projected adjusted score over the
   old Full16 and Generated16 pilots; ties choose the smaller size.  Freeze
   support, coefficients, and hashes before any new Mini100 endpoint capture.

## Pilot promotion gate

Proceed to official Mini100 only if the selected rule simultaneously has:

- projected adjusted score `<= 1.10e-7` on both old pilot families;
- raw-MSE ratio to q0 `<= 0.80` on both families; and
- at least `10/16` rows improved in each family.

## Mini100 gate

If the pilot passes, capture q0 per-basis endpoints on all official Mini100
rows without reading targets, seal them, and apply the already frozen paired
rule to the existing sealed polar endpoints.  Promote only if:

- central projected adjusted score `<= 1.10e-7`;
- raw ratio `<= 0.80`;
- at least `75/100` rows improve; and
- the paired-bootstrap raw-ratio upper 95% endpoint is `<0.90`.

Passing Mini100 licenses a broad Full100/Generated100 confirmation and an
exact physical graph.  It does not itself establish a measured whole or a
remote-ready candidate.
