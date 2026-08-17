# Official Mini100 historical replay R1

Date: 2026-08-04

Purpose: use ARC's fixed, independently downloadable official Mini100 as a
previously missing transfer screen for three frozen numerical candidates. No
support, coefficient, frame, or blend is selected on Mini100 targets.

## Frozen candidates

1. `K141 pooled right-Gram sparse-12`, exactly the support, pooled
   coefficients, and `0.5` blend in
   `candidate_k141_rightgram12_pooled_v091_20260731.py`.
2. `K153 mixed right-Gram/Jacobian-D2 sparse-24`, exactly R3 in
   `capture_mixed_right_d2_sparse24_r3_targetfree_20260731.py`.
3. `K161 mixed right-Gram/Jacobian-D2-right sparse-32`, exactly R14 in
   `capture_mixed_right_d2right_sparse32_r14_targetfree_20260731.py`.

The control is the ordinary K129 q0 prediction returned by the same frozen
CUDA adapter. All four final-layer predictions are captured for all 100 rows
before the Mini100 target member is mapped or read.

## Association checks before Mini100

- q0 and K141 GPU predictions must each match their saved exact physical
  Full7 final layer within `5e-4` maximum absolute error.
- K161 GPU prediction must match its saved exact physical Full7 final layer
  within `5e-4` maximum absolute error.
- All three candidate calls must return the same q0 prediction within
  `1e-10` on every row.

These are component-association checks, not physical receipts for Mini100.

## Fixed scoring

For each candidate report pooled final-layer raw MSE, raw ratio to q0,
fraction of rows improved, median row ratio, maximum row ratio, and a
row-bootstrap 95% interval for the pooled raw ratio. The target scorer may not
alter any prediction.

Promotion screens, fixed before targets:

- K141 is a serious deployment lead if raw ratio is at most `0.90`, at least
  55% of rows improve, and the bootstrap interval's upper endpoint is below
  `1.0`.
- K153 is a serious R31 challenger only if raw ratio is at most `0.83` and the
  bootstrap upper endpoint is below `1.0`; its larger measured graph otherwise
  cannot repay its compute.
- K161 is retained as accuracy-engine provenance only if raw ratio is at most
  `0.80` and the bootstrap upper endpoint is below `1.0`; it remains
  undeployable until wall is repaired.

Passing these screens is broad statistical evidence for raw transfer only.
Adjusted score remains a projection until a current-meter whole receipt is
associated, and no upload or submission is authorized.

