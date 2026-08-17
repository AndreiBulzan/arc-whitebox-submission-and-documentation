# K129 trajectory-to-true-premean R6 preregistration

Date: 2026-08-06

Evidence sought: **component**.  This is a supervised offline capacity test
on already-frozen per-basis trajectory caches.  It authorizes no FlopScope
run, estimator change, package, upload, submission, or remote action.

## Prior-work distinction

- Learned endpoint summaries, weight-only routers, and K32 aggregate recurrent
  cells failed cross-family.
- `trajectory_premean` R2 fed the complete ordered O0 per-basis endpoint plus
  L4/L8 transported observer sequences to a shared head, but trained it to
  imitate the all-eight-orientation teacher.  It failed that teacher task.
- Gate-state four-frame distillation likewise targeted a multi-frame teacher.

No recorded experiment trains this frozen trajectory-resolved observable
against the actual signed final-preactivation mean.  That is a different
question: the four-frame correction need not equal the true q0 error.

## Frozen test

Reuse R2's exact feature construction and model without a grid:

```text
features: ordered q0 final per-basis sequence,
          matched L4 and L8 response-transport sequences,
          their three pointwise products and 15 summaries
model:    shared 789 -> 128 -> 64 -> 1 SiLU head
steps:    250, 500, 1000; source-development checkpoint selection only
```

Replace only the label by `target_final_premean - q0_baseline`, normalized by
the per-output standard deviation of q0 basis endpoints.

Whole-network splits remain identical to R2:

- Full-A `0:160` train, `160:200` development; evaluate the frozen result on
  all Generated128 and Full-B167;
- Generated `0:96` train, `96:128` development; evaluate unchanged on
  Full-B167.

No MLP identity, seed, challenge final target, Mini100, or remote row is an
input.  The exact signed-premean target is used only as supervised label and
held-family score.

## Gate

Continue only if both cross-family directions satisfy:

```text
pooled signed-premean MSE ratio <= 0.80
whole networks improved        >= 55%
row-MSE ratio p95              <= 1.40
```

Also require the Full-trained model to improve both Generated and Full-B.
Failure kills this exact feature/head/true-target spelling.  Passing licenses
a separate inference-economics and final-ReLU composition test; it is not a
candidate or a remote-score claim.

