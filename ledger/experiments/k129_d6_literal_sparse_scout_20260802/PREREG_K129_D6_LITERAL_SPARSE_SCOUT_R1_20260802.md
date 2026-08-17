# K129 literal sparse depth-6 scout R1

Evidence class: **component** until predictions are sealed and challenge
targets are opened.  Compute figures in this screen are **projections**.

## Question

The broad R6 depth-6 scout improved raw MSE by `19.51%` on Full100 and
`14.19%` on Generated128, but its nominal `1.096024x` support-layer price
assumed that the 64 selected frame/basis atoms can be propagated on their
own.  The R6 capture actually repaired each atom inside a complete 129-basis
alternate frame.  Both the layer-0 compact repair and H2 repair pool across
the whole arm, so the existing numerical result is not yet a deployable
64-basis statistic.

This screen asks whether the frozen 64 R6 frame/basis slots retain their
teacher signal when run as one literal, independently repaired mixed arm.
It also asks whether the arm can retain only `192`, `160`, or `128` of its
256 matched antipodal directions after H2, because only its depth-6
per-basis mean and diagonal variance are consumed.

## Blocking prior-art preflight

Queries covered `sparse depth-6`, `literal d6`, `independent repair`,
`direction thinning`, `phase thinning`, `checkpoint moment herding`,
`repair_h2`, `compact_repair`, and the exact R6 capture/build call sites.

Nearest controlled work:

- `direction_thinning_20260728/RESULT_20260728.md` killed direction deletion
  for the final K146 estimator after layer 12.
- `checkpoint_moment_herding_20260729/` killed checkpoint-12 moment herding
  for that same final estimator.
- `k129_o1_shadow_width_20260729/VERDICT_K129_O1_SHADOW_WIDTH_R1_20260729.md`
  killed direction thinning of the full O1 accuracy arm.
- `fullframe_weight_rotation_20260731/capture_fullframe_right_sparse12_*`
  and `deep_pullback_frame_20260731/capture_mixed_right_d2_sparse24_*`
  establish that a deployable sparse alternate arm must be independently
  repaired.
- `k129_fourframe_h2_mlmc_20260802/build_sparse_d6_scout_r4_*` and
  `build_direct_sparse_d6_scout_r6_*` compressed complete-frame proxy atoms
  post hoc; neither captured the selected mixed arm through its literal
  repair graph, and neither thinned a scout whose sole observable is a
  depth-6 mean/variance.

Outcome: **materially new observable**.  The old thinning negatives concern
the scored nonlinear endpoint.  Here the thinned cloud is a sidecar used
only to estimate a smooth depth-6 mean/variance before diagonal-Gaussian
continuation.  The independent-repair correction also closes a real R6
deployment mismatch rather than renaming prior work.

## Frozen pilot

- Rows: the existing Full16 (`7,17,...,157`) and Generated16 (`0..15`).
- Slots: the exact 64 `(frame, basis)` pairs sealed by broad R6.
- Frames: canonical right-Gram, Jacobian-d2, and Jacobian-d2-right.
- Direction variants: `m in {256,192,160,128}`.  Positive and negative
  directions use identical IDs.  For `m<256`, each slot uses the fixed
  balanced schedule
  `id = (73 * ((frame*129+basis)*m+j) + 19) mod 256`.
- Prefix: literal mixed-slot compact repair, literal H2 repair, then exact
  layers 2--5 including the existing L4 snap generalized only to the frozen
  direction cardinality.
- Suffix: the already-defined per-slot diagonal Gaussian continuation from
  depth 6 to depth 32.
- Reference feature: the complete q0 depth-6 diagonal-proxy mean already
  sealed target-free in R3.
- Teacher: the complete four-frame endpoint mean already sealed target-free.
- No challenge target is read during capture, ridge selection, direction
  selection, or coefficient fitting.

Four-fold, family-balanced teacher reconstruction selects one direction
variant and one fixed ridge from `{1e-4, 1e-2, 1}`.  For each variant, keep
the ridge with the lowest worst-family out-of-fold reconstruction MSE.  Then
select the variant minimizing
`worst reconstruction MSE * projected effective-compute ratio`, breaking
ties toward fewer directions.  This is the target-free analogue of the
adjusted-score objective.  Raw challenge targets may be opened only after
that candidate is sealed.

## Target ceiling

Using the conservative Generated128 R6 raw ratio `0.858124`, R27's
`1.1936e-7` remote projection, and charging about `1.4B` extra operations for
the q0 plus 64-slot diagonal continuations:

- `m=256` is approximately `1.13e-7` before further exact contraction;
- `m=160` is approximately `1.11e-7`;
- `m=128` is approximately `1.10e-7`, and the already-audited `~1.195B`
  metered exact-contraction saving would put it near `1.09e-7`.

The pilot promotes only if the independently repaired statistic improves
both families and its conservative price-attached projection is at most
`1.11e-7`.  Otherwise stop without a broad capture.  A promoted pilot still
requires broad statistical confirmation, an exact FlopScope implementation,
and a measured whole; this file authorizes none of those automatically.
