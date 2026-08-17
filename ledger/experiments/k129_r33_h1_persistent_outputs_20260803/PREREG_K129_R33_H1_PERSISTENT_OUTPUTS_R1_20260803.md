# K129 R33 repaired-H1 persistent-output preregistration

Evidence label: **projection** until an exact archive is run.

## Prior-art check

The capsule was searched for a DPS48 workspace, persistent DPS48 output
destinations, and a cached/reused DPS48 weight-side transform.  The current
R30/R31 kernel materializes both rank banks with `stack`, recomputes the
weight-side L program in both repaired-H1 calls, and copies all 16 P outputs
into a fresh result.  No capsule-native implementation of this exact rewrite
was found.  Existing persistent-destination work is in other contractions and
establishes only the lifecycle pattern.

## Frozen parent

R31 archive:

`runtime/artifacts/k129_r31_h1_dps48_lateleaf_batched4_scale_local_candidate_r2_20260803.tar.gz`

SHA-256:

`520febd128d61a9015cb0c96a9c69a9a4da9ed2848fc5de06cc4f11bda641f3c`

Steady receipt: 124,239,224,002 counted FLOPs, 137,926,260,702 diagnostic
effective compute, prediction SHA-256
`0ab299a9b5a1c9086a701baad61b7a0839186d1fdf42dff26218331066c0e165`.

## Single change

For the repaired-H1 call in one prediction:

1. write the 48 L outputs directly into a persistent weight bank;
2. write the 48 R outputs directly into a persistent activation bank;
3. write the 16 P outputs directly into a persistent result buffer.

Every scalar SLP expression keeps the same AST evaluation order.  The nested
B7/B7 product is unchanged.

## Ceiling and kill rule

The first exact execution established that this K129 graph has one repaired-H1
call, not two.  The corrected counted ceiling is therefore smaller: remove one
large input-bank stack, one small input-bank stack, and 16 final block copies.
This is expected to save materially less than 0.1B counted FLOPs.
The only plausible additional value is reduced request/handle churn.

Promote only if an exact-archive official-runner two-prediction receipt has:

- the R31 prediction hash on both rows;
- lower steady effective compute than R31 by at least 0.05B;
- no count/effective/wall/result-cap failure;
- exactly one H1 call and one weight transform per prediction.

Otherwise kill it immediately.  No targets are opened and no remote action is
authorized.
