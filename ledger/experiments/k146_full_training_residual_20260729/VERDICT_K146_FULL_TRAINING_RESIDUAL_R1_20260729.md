# K146 expanded-training endpoint residual R1 — verdict

Date: 2026-07-29

Evidence label: **component development diagnostic**. No FlopScope session,
physical row, package, upload, submission, or remote action occurred.

## Decision

**Hard stop.** Increasing the endpoint-correction training population from
dozens to hundreds of official MLPs removes the apparent signal rather than
stabilizing it.

The target-free extension captured 275 new K146 endpoint trajectories and
merged them with the existing 100-row capture. The exact frozen lower-K
confirmation set had zero intersection with the 375-row population. Only
226 Full training and 77 Full development targets were opened. The 72 Full
held targets, all Generated targets, and all confirmation targets remained
unopened because the development gate failed.

The strongest frozen ridge member was the 52-feature linear-moment subset at
penalty 10:

```text
Full development pooled raw ratio       0.99962698
Full development row-ratio p95          1.22067260
rows improved                           48.05%
required pooled ratio                  <=0.92
```

Normalized moment and weight-conditioned feature sets were worse. This
confirms that the prior roughly `0.87x` fit was small-network selection
error, not a transferable estimator correction.

The projected inference arithmetic was below the declared `2B` ceiling, but
accuracy killed the lane before a meter ledger or prediction seal was
justified.

## Artifacts

- `PREREG_K146_FULL_TRAINING_RESIDUAL_R1_20260729.md`
- `capture_k146_final_moments_nonconfirm_r1_20260729.py`
- `k146_final_moments_nonconfirm_lt400_r1_20260729.npz`
- `k146_final_moments_nonconfirm_lt400_r1_20260729.json`
- `train_seal_k146_full_training_residual_r1_20260729.py`
- `k146_full_training_residual_r1_train_seal_20260729.json`

Independent audit:

`../k146_full_training_residual_critic_20260729/AUDIT_K146_FULL_TRAINING_RESIDUAL_R1_20260729.md`
