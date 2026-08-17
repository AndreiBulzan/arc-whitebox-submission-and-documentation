# Basis-trajectory student R2 preregistration

Date: 2026-08-04

Evidence sought: **component** only.  This stage does not open
`target_final_premean.npy`, any challenge target, Mini100, FlopScope, a
physical row, a package, an upload, or a submission.

## Question

Can the orientation-zero, per-basis trajectory at the two already-cached
observer checkpoints predict the all-eight-orientation signed-premean
correction well enough to replace most of the seven unpaid frames?

The baseline is the mean of the 129 orientation-zero final signed endpoints.
The target-free teacher is the mean of all `8 x 129` cached signed endpoints.
For each output coordinate, the available sequences are:

1. the 129 orientation-zero final endpoints;
2. the 129 L4 observer means transported through the realised affine
   response to the final premean; and
3. the analogous 129 L8 transported observer means.

This is materially different from the killed endpoint-only students and the
killed aggregate affine response: it retains the canonical basis index and
the matched trajectory value at both checkpoints for every basis.

## Fixed screen

For each coordinate, centre and scale the final-endpoint sequence.  Express
the two transported sequences in the same units.  The fixed feature vector
contains the three 129-vectors, their three pointwise pair-products, and
fixed low-order scalar summaries.  The label is the all-eight correction in
endpoint-scale units.

Train one shared two-hidden-layer SiLU head (`789 -> 128 -> 64 -> 1`) on
whole-network splits only:

- Full-A rows `0:160` train, `160:200` development; apply the selected
  checkpoint unchanged to all 128 Generated rows and the 167 Full-B rows;
- Generated rows `0:96` train, `96:128` development; apply unchanged to all
  167 Full-B rows.

Use seed `20260804`, AdamW (`lr=6e-4`, `weight_decay=3e-3`), batch 2048, and
checkpoints 250/500/1000.  Select solely by source-family development MSE.
At each checkpoint fit one source-training scalar in `[0,1]` multiplying the
predicted correction; this protects against scale error and is frozen before
the opposite family is evaluated.

## Hard gate

Promote only if the fixed cross-family candidate satisfies, independently in
both directions:

- pooled teacher-correction MSE ratio `<= 0.60` versus orientation zero;
- at least `60%` of networks improve; and
- row-ratio p95 `<= 1.20`.

A weaker result is killed without opening challenge targets or Mini100.
Passing licenses a separately sealed target scorer.  It is not validation:
no candidate is considered validated until it subsequently passes the
official independent Mini100 gate.

