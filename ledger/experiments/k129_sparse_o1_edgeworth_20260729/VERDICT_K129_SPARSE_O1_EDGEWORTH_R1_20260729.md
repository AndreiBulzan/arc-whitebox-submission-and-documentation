# K129 sparse-O1 Edgeworth R1 — killed at 8+8

Date: 2026-07-29

Evidence label: the accuracy results are **component development
diagnostics**. The incremental count is a **projection**. No FlopScope
physical row, estimator edit, package, upload, or remote action occurred.

## Bottom line

The full 17-basis O1 Edgeworth3433 correction remains a real lead, but the
target-free endpoint-selected bases `[70,45]` do not estimate it. The
preregistered tiny-family gate fails decisively, so the lane stops before
Full100+Generated64.

All target-free predictions and raw moments were written before either
family was scored. The fixed blend was:

```text
candidate = q0(lambda=.0075)
          + .0734 * (estimated_O1_Edgeworth3433 - q0)
```

## Eight-row result

| estimate of O1 Edgeworth3433 | Full8 raw ratio | Generated8 corrected raw ratio |
|---|---:|---:|
| existing full 17-basis reference | `0.936166` | `1.016419` |
| direct `[70,45]` moments | `1.040913` | `1.361561` |
| matched-O0 moment control | `1.356879` | `1.461267` |
| matched-O0 readout control | `1.356776` | `1.461138` |

The Full8 full-17 reference shows the expected strong signal. Its
Generated8 reversal is a noisy eight-row diagnostic, not a contradiction
of the existing Generated64 result. In contrast, every sparse spelling is
far outside the preregistered `<=0.975` family-wise gate.

The individual bases are weaker:

| support / spelling | Full8 ratio | Generated8 ratio |
|---|---:|---:|
| direct `[70]` | `1.415915` | `2.620437` |
| direct `[45]` | `1.731258` | `1.151945` |
| moment control `[70]` | `2.083927` | `2.946371` |
| moment control `[45]` | `2.707904` | `1.789756` |

The matched control is not merely unhelpful; it amplifies the error.
Matched O0 and O1 endpoint identity therefore does not provide a
low-variance estimator of the nonlinear third/fourth final moments.

## Decision

Kill this exact sparse support and stop. Do not spend the broad capture,
physical-meter, or integration lane on it. The projected two-basis count
increment is about `1.9501B`; even zero cost could not rescue raw ratios
above one.

The optional four-basis escalation was conditioned on a marginal two-basis
signal. This result is not marginal, so no four-basis search was opened.
A future reopening requires bases selected directly for final raw-moment
reconstruction on a genuinely larger target-free proxy, rather than the
endpoint-selected `[70,45]`.

## Artifacts

- `capture_sparse_o1_edgeworth_r1_targetfree_20260729.py`
- `score_sparse_o1_edgeworth_r1_postseal_20260729.py`
- `k129_sparse_o1_edgeworth_full8_r1_targetfree_20260729.npz`
  (`a99bfc81e41be908d90b00c18a3017273cba052ae93b492ca55eb806cb0e548e`)
- `k129_sparse_o1_edgeworth_generated8_r1_targetfree_20260729.npz`
  (`324d8453e850d672fb16973504a12c4e3ec5cce5237596b1f601075dbd903dcc`)
- the corresponding target-free JSON receipts and post-seal score JSONs.

